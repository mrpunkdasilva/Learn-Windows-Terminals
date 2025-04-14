# DevOps Tools

> "Ferramentas PowerShell essenciais para automação e integração DevOps."

## Visão Geral
Conjunto de ferramentas PowerShell para automatizar e gerenciar processos DevOps, incluindo CI/CD, containers, infraestrutura e monitoramento.

## Recursos Principais

### Pipeline CI/CD
```powershell
function Start-CIPipeline {
    param(
        [string]$SourcePath,
        [string]$BuildConfig = "Release",
        [string[]]$TestCategories = @("Unit", "Integration")
    )
    
    # Validação de código
    Write-Output "Executando análise de código..."
    Invoke-ScriptAnalyzer -Path $SourcePath
    
    # Execução de testes
    Write-Output "Executando testes..."
    foreach ($category in $TestCategories) {
        Invoke-Pester -Path "$SourcePath/tests/$category" -OutputFormat NUnitXml
    }
    
    # Build do projeto
    Write-Output "Iniciando build..."
    dotnet build $SourcePath --configuration $BuildConfig
}
```

### Gerenciamento de Containers
```powershell
function Manage-DockerEnvironment {
    [CmdletBinding()]
    param(
        [Parameter(Mandatory)]
        [ValidateSet('Build', 'Run', 'Stop', 'Clean')]
        [string]$Action,
        
        [string]$ImageName,
        [string]$ContainerName,
        [string]$DockerfilePath = "./Dockerfile"
    )
    
    switch ($Action) {
        'Build' {
            docker build -t $ImageName -f $DockerfilePath .
        }
        'Run' {
            docker run -d --name $ContainerName $ImageName
        }
        'Stop' {
            docker stop $ContainerName
        }
        'Clean' {
            docker system prune -f
        }
    }
}
```

### Automação de Deploy
```powershell
function Deploy-Application {
    param(
        [string]$Environment,
        [string]$Version,
        [switch]$EnableRollback
    )
    
    try {
        # Backup pré-deploy
        if ($EnableRollback) {
            Backup-Environment -Environment $Environment
        }
        
        # Validação de pré-requisitos
        Test-DeploymentPrerequisites
        
        # Deploy da aplicação
        Write-Output "Iniciando deploy v$Version em $Environment..."
        
        # Atualização de configurações
        Update-AppConfig -Environment $Environment
        
        # Deploy dos artefatos
        Copy-DeploymentArtifacts -Version $Version
        
        # Restart dos serviços
        Restart-ApplicationServices
        
        # Validação pós-deploy
        Test-DeploymentHealth
    }
    catch {
        if ($EnableRollback) {
            Write-Warning "Erro no deploy. Iniciando rollback..."
            Restore-Environment -Environment $Environment
        }
        throw
    }
}
```

### Monitoramento de Infraestrutura
```powershell
function Watch-Infrastructure {
    param(
        [int]$IntervalSeconds = 300,
        [string[]]$Services,
        [hashtable]$Thresholds
    )
    
    while ($true) {
        # Métricas do sistema
        $cpu = Get-Counter '\Processor(_Total)\% Processor Time'
        $memory = Get-Counter '\Memory\Available MBytes'
        $disk = Get-Counter '\LogicalDisk(_Total)\% Free Space'
        
        # Verificação de serviços
        $serviceStatus = Get-Service $Services | 
            Select-Object Name, Status, DisplayName
        
        # Verificação de logs
        $recentErrors = Get-EventLog -LogName Application -EntryType Error -Newest 10
        
        # Alertas baseados em thresholds
        if ($cpu.CounterSamples.CookedValue -gt $Thresholds.CPU) {
            Send-Alert -Type "CPU" -Value $cpu.CounterSamples.CookedValue
        }
        
        # Geração de relatório
        $report = @{
            Timestamp = Get-Date
            CPU = $cpu.CounterSamples.CookedValue
            Memory = $memory.CounterSamples.CookedValue
            Disk = $disk.CounterSamples.CookedValue
            Services = $serviceStatus
            Errors = $recentErrors
        }
        
        Export-MetricsToDatabase $report
        
        Start-Sleep -Seconds $IntervalSeconds
    }
}
```

## Integração com Ferramentas

### Azure DevOps
```powershell
function Connect-AzureDevOps {
    param(
        [string]$Organization,
        [string]$Project,
        [string]$PersonalAccessToken
    )
    
    $base64AuthInfo = [Convert]::ToBase64String(
        [Text.Encoding]::ASCII.GetBytes(":$PersonalAccessToken")
    )
    
    $headers = @{
        Authorization = "Basic $base64AuthInfo"
    }
    
    $global:AzureDevOpsContext = @{
        BaseUrl = "https://dev.azure.com/$Organization/$Project"
        Headers = $headers
    }
}
```

### Jenkins
```powershell
function Invoke-JenkinsJob {
    param(
        [string]$JobName,
        [hashtable]$Parameters,
        [switch]$WaitForCompletion
    )
    
    $jenkinsUrl = $env:JENKINS_URL
    $credential = Get-JenkinsCredential
    
    $job = Invoke-RestMethod `
        -Uri "$jenkinsUrl/job/$JobName/build" `
        -Method Post `
        -Credential $credential
        
    if ($WaitForCompletion) {
        Wait-JenkinsJob -JobId $job.Id
    }
}
```

## Boas Práticas

### Logging e Telemetria
```powershell
function Write-DevOpsLog {
    param(
        [string]$Message,
        [ValidateSet('Info', 'Warning', 'Error')]
        [string]$Level = 'Info',
        [hashtable]$Properties
    )
    
    $logEntry = @{
        Timestamp = Get-Date
        Level = $Level
        Message = $Message
        Properties = $Properties
        User = $env:USERNAME
        Computer = $env:COMPUTERNAME
    }
    
    # Log local
    $logEntry | ConvertTo-Json | 
        Add-Content -Path "logs/devops.log"
    
    # Telemetria
    Send-ApplicationInsightsTelemetry @logEntry
}
```

### Segurança
```powershell
function Protect-DevOpsSecrets {
    param(
        [hashtable]$Secrets,
        [string]$KeyVaultName
    )
    
    foreach ($key in $Secrets.Keys) {
        $secureString = ConvertTo-SecureString $Secrets[$key] -AsPlainText -Force
        Set-AzKeyVaultSecret `
            -VaultName $KeyVaultName `
            -Name $key `
            -SecretValue $secureString
    }
}
```

## Exemplos de Uso

### Pipeline Completo
```powershell
# Configuração do ambiente
Connect-AzureDevOps -Organization "MyOrg" -Project "MyProject"
Initialize-DevOpsTools

# Pipeline CI/CD
Start-CIPipeline -SourcePath "./src"
Deploy-Application -Environment "Staging" -Version "1.0.0" -EnableRollback

# Monitoramento
Watch-Infrastructure -Services @("WebApp", "API", "Database")
```

## Próximos Passos
1. Implementar automação de testes de segurança
2. Adicionar suporte para múltiplas clouds
3. Desenvolver dashboards personalizados
4. Expandir integrações com outras ferramentas

## Recursos Adicionais
- [Azure DevOps REST API](https://docs.microsoft.com/rest/api/azure/devops)
- [Jenkins API](https://www.jenkins.io/doc/book/using/remote-access-api/)
- [Docker CLI Reference](https://docs.docker.com/engine/reference/commandline/cli/)

---
_"Automatize tudo o que puder, mas mantenha o controle do que importa."_