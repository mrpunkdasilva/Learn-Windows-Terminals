# Exercícios PowerShell

> "A prática leva à perfeição. Vamos praticar!"

## Exercícios Básicos

### 1. Manipulação de Arquivos
```powershell
# Exercício: Sistema de Backup
# Crie um script que:
# 1. Liste todos arquivos .txt em um diretório
# 2. Copie apenas arquivos modificados nas últimas 24h
# 3. Comprima os arquivos em um zip
# 4. Registre as operações em um log

# Exemplo de solução
$sourceDir = "C:\Dados"
$backupDir = "C:\Backup"
$logFile = "C:\Logs\backup.log"

Get-ChildItem -Path $sourceDir -Filter "*.txt" |
    Where-Object { $_.LastWriteTime -gt (Get-Date).AddDays(-1) } |
    ForEach-Object {
        Copy-Item $_.FullName -Destination $backupDir
        Add-Content $logFile "$(Get-Date) - Copiado: $($_.Name)"
    }
```

### 2. Processos e Serviços
```powershell
# Exercício: Monitor de Recursos
# Crie um script que:
# 1. Monitore uso de CPU e memória
# 2. Alerte quando limites forem excedidos
# 3. Registre estatísticas em CSV
# 4. Permita configuração de limites

# Exemplo de solução
function Monitor-SystemResources {
    param(
        [int]$cpuThreshold = 80,
        [int]$memThreshold = 90
    )
    
    $stats = Get-Process | Measure-Object -Property CPU, WS -Sum
    $cpuUsage = $stats.Sum / $env:NUMBER_OF_PROCESSORS
    $memUsage = (Get-Counter '\Memory\% Committed Bytes In Use').CounterSamples.CookedValue
    
    [PSCustomObject]@{
        DateTime = Get-Date
        CPU = $cpuUsage
        Memory = $memUsage
        Alert = ($cpuUsage -gt $cpuThreshold) -or ($memUsage -gt $memThreshold)
    }
}
```

## Exercícios Intermediários

### 3. Automação de Rede
```powershell
# Exercício: Diagnóstico de Rede
# Crie um script que:
# 1. Teste conectividade com múltiplos hosts
# 2. Verifique portas específicas
# 3. Meça latência
# 4. Gere relatório HTML

# Exemplo de solução
function Test-NetworkHealth {
    param(
        [string[]]$computers,
        [int[]]$ports = @(80, 443, 3389)
    )
    
    $results = foreach ($computer in $computers) {
        $ping = Test-Connection $computer -Count 1 -ErrorAction SilentlyContinue
        $portTests = foreach ($port in $ports) {
            $test = Test-NetConnection $computer -Port $port -WarningAction SilentlyContinue
            [PSCustomObject]@{
                Port = $port
                Open = $test.TcpTestSucceeded
            }
        }
        
        [PSCustomObject]@{
            Computer = $computer
            Online = $null -ne $ping
            ResponseTime = $ping.ResponseTime
            Ports = $portTests
        }
    }
    
    $results | ConvertTo-Html | Out-File "NetworkReport.html"
}
```

### 4. Gerenciamento de Usuários
```powershell
# Exercício: Sistema de Usuários
# Crie um módulo que:
# 1. Adicione/remova usuários
# 2. Gerencie grupos e permissões
# 3. Gere relatórios de acesso
# 4. Implemente logging

# Exemplo de solução
function New-UserEnvironment {
    param(
        [string]$username,
        [string]$department,
        [string[]]$groups
    )
    
    try {
        New-LocalUser -Name $username -Description "Auto created"
        foreach ($group in $groups) {
            Add-LocalGroupMember -Group $group -Member $username
        }
        
        New-Item -Path "C:\Users\$username\Documents" -ItemType Directory
        Set-Acl -Path "C:\Users\$username\Documents" -AclObject $acl
        
        Write-EventLog -LogName "User Management" -Source "UserCreation" -EntryType Information -Message "User $username created successfully"
    }
    catch {
        Write-Error "Failed to create user environment: $_"
        Write-EventLog -LogName "User Management" -Source "UserCreation" -EntryType Error -Message "Failed: $_"
    }
}
```

## Exercícios Avançados

### 5. Integração com APIs
```powershell
# Exercício: Cliente REST
# Crie um módulo que:
# 1. Interaja com uma API REST
# 2. Implemente autenticação
# 3. Processe respostas JSON
# 4. Trate erros adequadamente

# Exemplo de solução
function Invoke-ApiRequest {
    param(
        [string]$endpoint,
        [string]$method = "GET",
        [hashtable]$headers,
        [object]$body
    )
    
    $params = @{
        Uri = $endpoint
        Method = $method
        ContentType = "application/json"
        Headers = $headers
    }
    
    if ($body) {
        $params.Body = $body | ConvertTo-Json
    }
    
    try {
        $response = Invoke-RestMethod @params
        return $response
    }
    catch {
        Write-Error "API request failed: $($_.Exception.Message)"
        throw
    }
}
```

### 6. Projeto Final
```powershell
# Exercício: Sistema de Monitoramento
# Desenvolva um sistema completo que:
# 1. Monitore múltiplos servidores
# 2. Colete métricas de performance
# 3. Gere alertas e relatórios
# 4. Implemente interface web simples

# Exemplo de estrutura
.\MonitoringSystem\
    ├── Modules\
    │   ├── Monitoring.psm1
    │   ├── Reporting.psm1
    │   └── WebInterface.psm1
    ├── Config\
    │   ├── servers.json
    │   └── alerts.json
    ├── Scripts\
    │   ├── Start-Monitoring.ps1
    │   └── Generate-Report.ps1
    └── Web\
        ├── index.html
        └── styles.css
```

## Desafios Extra

### 7. Otimização
```powershell
# Desafio: Otimize um script existente
# 1. Identifique gargalos
# 2. Implemente parallel processing
# 3. Reduza uso de memória
# 4. Melhore tempo de execução

# Exemplo de otimização
$data = 1..1000
$results = $data | ForEach-Object -ThrottleLimit 10 -Parallel {
    # Processamento pesado aqui
    Start-Sleep -Milliseconds 100
    $_
}
```

### 8. Segurança
```powershell
# Desafio: Secure Coding
# 1. Implemente validação de input
# 2. Use credenciais seguras
# 3. Aplique princípio do menor privilégio
# 4. Adicione logging de segurança

# Exemplo de implementação segura
function Invoke-SecureOperation {
    [CmdletBinding()]
    param(
        [Parameter(Mandatory)]
        [ValidateNotNullOrEmpty()]
        [string]$Action,
        
        [Parameter(Mandatory)]
        [System.Management.Automation.PSCredential]
        [System.Management.Automation.Credential()]
        $Credential
    )
    
    try {
        Start-Transcript -Path ".\SecurityLog.txt" -Append
        
        $secureAction = [System.Web.HttpUtility]::HtmlEncode($Action)
        Write-Verbose "Executing secure action: $secureAction"
        
        # Operação principal aqui
        
        Stop-Transcript
    }
    catch {
        Write-Error "Security violation: $_"
        throw
    }
}
```

## Recursos Adicionais

1. [PowerShell Gallery](https://www.powershellgallery.com)
2. [Pester Testing Framework](https://pester.dev)
3. [PSScriptAnalyzer](https://github.com/PowerShell/PSScriptAnalyzer)

## Próximos Passos

1. [Módulos Avançados](ps-modules.md)
2. [DevOps Integration](ps-devops-integration.md)
3. [Segurança Avançada](ps-security.md)

---
_"A melhor maneira de aprender é fazendo. A segunda melhor é quebrando algo e tendo que consertar."_