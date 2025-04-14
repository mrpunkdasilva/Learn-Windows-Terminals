# PowerShell Avançado

> "Domine os recursos avançados para elevar seu domínio do PowerShell."

## Scripting Avançado

### Módulos e Funções
```powershell
# Estrutura de módulo
New-ModuleManifest -Path .\MyModule.psd1 -ModuleVersion "1.0" `
    -Author "Seu Nome" -Description "Descrição do módulo"

# Funções avançadas
function Get-SystemInfo {
    [CmdletBinding()]
    param(
        [Parameter(Mandatory)]
        [string]$ComputerName,
        
        [switch]$IncludeNetwork
    )
    
    begin {
        Write-Verbose "Iniciando coleta de informações..."
    }
    
    process {
        # Lógica principal
    }
    
    end {
        Write-Verbose "Coleta finalizada."
    }
}
```

## Programação Orientada a Objetos

### Classes e Tipos
```powershell
# Definição de classe
class ServerConfig {
    [string]$Hostname
    [int]$Port
    [bool]$SSL
    
    # Construtor
    ServerConfig([string]$host, [int]$port) {
        $this.Hostname = $host
        $this.Port = $port
        $this.SSL = $false
    }
    
    # Método
    [bool]Test() {
        # Lógica de teste
        return $true
    }
}
```

## Manipulação Avançada de Dados

### Expressões Regulares
```powershell
# Regex avançado
$pattern = '(?<protocol>https?):\/\/(?<domain>[\w\.-]+)(?<path>\/.*)?'
$url = "https://github.com/powershell"

if ($url -match $pattern) {
    $protocol = $matches['protocol']
    $domain = $matches['domain']
    $path = $matches['path']
}
```

### XML e JSON
```powershell
# Manipulação XML
[xml]$config = Get-Content .\config.xml
$config.SelectNodes("//server[@enabled='true']")

# JSON avançado
$json = @{
    servers = @(
        @{
            name = "prod"
            ip = "10.0.0.1"
        }
    )
} | ConvertTo-Json -Depth 10
```

## Automação Avançada

### Jobs e Workflows
```powershell
# Jobs em background
Start-Job -ScriptBlock {
    Get-Process | Where-Object CPU -gt 50
} -Name "MonitorCPU"

# Workflow
workflow Backup-Infrastructure {
    parallel {
        InlineScript { Backup-Database }
        InlineScript { Backup-FileSystem }
    }
}
```

### Remoting Avançado
```powershell
# Sessões persistentes
$session = New-PSSession -ComputerName "server01"
Invoke-Command -Session $session -ScriptBlock {
    # Comandos remotos
}

# Multi-threading
$servers = "server01", "server02", "server03"
$servers | ForEach-Object -ThrottleLimit 10 -Parallel {
    Get-Service -ComputerName $_
}
```

## Segurança Avançada

### Criptografia
```powershell
# Certificados
$cert = Get-PfxCertificate -FilePath .\cert.pfx
$encrypted = $cert.PublicKey.Key.Encrypt($bytes, $true)

# Secure String
$secureString = ConvertTo-SecureString "senha" -AsPlainText -Force
$encrypted = ConvertFrom-SecureString $secureString
```

### JEA (Just Enough Administration)
```powershell
# Configuração JEA
New-PSSessionConfigurationFile -Path .\JEAConfig.pssc `
    -SessionType RestrictedRemoteServer `
    -VisibleCmdlets "Get-Process", "Stop-Process" `
    -VisibleFunctions "Get-SystemInfo"
```

## Debugging Avançado

### Técnicas de Debug
```powershell
# Pontos de interrupção
Set-PSBreakpoint -Script .\script.ps1 -Line 10
Set-PSBreakpoint -Command "Get-Process" -Action {
    Write-Host "Breakpoint atingido!"
}

# Trace
Trace-Command -Name ParameterBinding -Expression {
    Get-Process
} -PSHost
```

## Otimização e Performance

### Medição de Performance
```powershell
# Benchmark
Measure-Command {
    1..1000 | ForEach-Object { Get-Random }
}

# Profile
$timer = [System.Diagnostics.Stopwatch]::StartNew()
# código a medir
$timer.Stop()
$timer.Elapsed
```

### Boas Práticas
```powershell
# Pipeline eficiente
$processes = Get-Process # Melhor que repetir chamadas
$processes | Where-Object { $_.CPU -gt 50 }

# Uso de memória
Get-ChildItem -Recurse | ForEach-Object {
    # Processamento em streaming
} # Melhor que carregar tudo na memória
```

## Integração com APIs

### REST e Web Services
```powershell
# Chamadas REST
$headers = @{
    'Authorization' = "Bearer $token"
    'Content-Type' = 'application/json'
}

$response = Invoke-RestMethod -Uri $apiUrl `
    -Method Post `
    -Headers $headers `
    -Body ($body | ConvertTo-Json)
```

## Exercícios Avançados

1. Automação Completa
   ```powershell
   # Crie um script que:
   # - Monitora recursos do sistema
   # - Envia alertas por email
   # - Registra logs
   # - Executa ações corretivas
   ```

2. Framework de Testes
   ```powershell
   # Implemente testes unitários
   Describe "Módulo de Sistema" {
       Context "Função Get-SystemInfo" {
           It "Retorna objeto válido" {
               $result = Get-SystemInfo
               $result | Should -Not -BeNullOrEmpty
           }
       }
   }
   ```

3. Módulo Profissional
   ```powershell
   # Desenvolva um módulo completo com:
   # - Manifesto
   # - Funções avançadas
   # - Documentação
   # - Testes
   # - CI/CD
   ```

## Próximos Passos

1. [Scripting Avançado](ps-scripting.md)
2. [Módulos e Extensões](ps-modules.md)
3. [DevOps Integration](ps-devops-integration.md)

---
_"O verdadeiro poder do PowerShell está nos detalhes avançados."_