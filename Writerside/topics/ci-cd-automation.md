# Automação CI/CD Híbrida

> "Automatize seu pipeline de desenvolvimento com a flexibilidade de CMD e PowerShell."

## Visão Geral

Sistema de CI/CD que combina:
- Scripts CMD para compatibilidade e operações básicas
- PowerShell para operações avançadas e integrações
- Suporte multi-ambiente
- Monitoramento em tempo real

## Estrutura do Pipeline

### Configuração Base
```batch
@echo off
:: Pipeline Config - CMD
set "PROJECT_ROOT=%~dp0"
set "BUILD_NUMBER=%1"
set "ENVIRONMENT=%2"
set "ARTIFACTS_DIR=%PROJECT_ROOT%artifacts"

:: Validação básica
if "%BUILD_NUMBER%"=="" (
    echo Erro: Build number não especificado
    exit /b 1
)

:: Setup inicial
call :setupEnvironment || exit /b 1
call :validateRequirements || exit /b 1

:: Executa pipeline PowerShell
powershell.exe -File "%PROJECT_ROOT%\pipeline.ps1" ^
    -BuildNumber "%BUILD_NUMBER%" ^
    -Environment "%ENVIRONMENT%" ^
    -ArtifactsDir "%ARTIFACTS_DIR%"

exit /b %ERRORLEVEL%

:setupEnvironment
    mkdir "%ARTIFACTS_DIR%" 2>nul
    echo Ambiente configurado: %ENVIRONMENT%
    exit /b 0

:validateRequirements
    where git >nul 2>nul || (
        echo Git não encontrado
        exit /b 1
    )
    exit /b 0
```

### Pipeline Principal
```powershell
# pipeline.ps1
param(
    [Parameter(Mandatory=$true)]
    [string]$BuildNumber,
    
    [Parameter(Mandatory=$true)]
    [string]$Environment,
    
    [string]$ArtifactsDir
)

class Pipeline {
    [string]$BuildNumber
    [string]$Environment
    [string]$Status = "Running"
    [System.Collections.ArrayList]$Stages
    
    Pipeline([string]$bn, [string]$env) {
        $this->BuildNumber = $bn
        $this->Environment = $env
        $this->Stages = @()
    }
    
    [void]AddStage([string]$name, [scriptblock]$action) {
        $this->Stages.Add(@{
            Name = $name
            Action = $action
            Status = "Pending"
        })
    }
    
    [bool]Execute() {
        foreach ($stage in $this->Stages) {
            try {
                Write-Host "Executando $($stage.Name)..."
                & $stage.Action
                $stage.Status = "Success"
            }
            catch {
                $stage.Status = "Failed"
                $this->Status = "Failed"
                return $false
            }
        }
        $this->Status = "Success"
        return $true
    }
}
```

## Estágios do Pipeline

### 1. Build
```powershell
function Start-BuildStage {
    # Compilação
    try {
        dotnet build --configuration Release
        
        # Build legado (CMD)
        $buildCmd = @"
            @echo off
            call build.bat
            if errorlevel 1 exit /b 1
"@
        $buildCmd | Out-File "temp_build.bat" -Encoding ASCII
        cmd /c "temp_build.bat"
        Remove-Item "temp_build.bat"
    }
    catch {
        throw "Erro no build: $_"
    }
}
```

### 2. Testes
```powershell
function Start-TestStage {
    # Testes unitários
    dotnet test --no-build --configuration Release
    
    # Testes de integração
    $testScript = @"
        @echo off
        echo Executando testes legados...
        call integration_tests.bat
        if errorlevel 1 (
            echo Falha nos testes legados
            exit /b 1
        )
"@
    $testScript | Out-File "temp_test.bat" -Encoding ASCII
    cmd /c "temp_test.bat"
    Remove-Item "temp_test.bat"
}
```

### 3. Deploy
```powershell
function Start-DeployStage {
    param(
        [string]$Environment,
        [string]$Version
    )
    
    switch ($Environment) {
        "dev" { 
            Deploy-ToDev -Version $Version 
        }
        "staging" { 
            Deploy-ToStaging -Version $Version 
        }
        "prod" { 
            Deploy-ToProduction -Version $Version 
        }
    }
}

function Deploy-ToProduction {
    param([string]$Version)
    
    # Validação de produção
    $validation = @"
        @echo off
        echo Validando ambiente de produção...
        call validate_prod.bat
        if errorlevel 1 (
            echo Ambiente de produção não está pronto
            exit /b 1
        )
"@
    
    # Deploy com rollback
    try {
        # Backup
        Backup-Production
        
        # Deploy
        Execute-ProductionDeploy -Version $Version
        
        # Smoke tests
        Test-Deployment -Environment "prod"
    }
    catch {
        Write-Warning "Erro no deploy: $_"
        Restore-Production
        throw
    }
}
```

## Monitoramento e Logs

### Sistema de Logging
```powershell
class PipelineLogger {
    [string]$LogPath
    [string]$CurrentBuild
    
    PipelineLogger([string]$path, [string]$build) {
        $this->LogPath = $path
        $this->CurrentBuild = $build
    }
    
    [void]Log([string]$message, [string]$level = "INFO") {
        $timestamp = Get-Date -Format "yyyy-MM-dd HH:mm:ss"
        $logEntry = "[$timestamp][$level][Build $($this->CurrentBuild)] $message"
        
        # Log em arquivo
        Add-Content -Path $this->LogPath -Value $logEntry
        
        # Log no console
        Write-Host $logEntry
    }
    
    [void]LogError([string]$message) {
        $this->Log($message, "ERROR")
    }
}
```

### Métricas de Pipeline
```powershell
class PipelineMetrics {
    [DateTime]$StartTime
    [hashtable]$StageMetrics = @{}
    
    [void]StartStage([string]$name) {
        $this->StageMetrics[$name] = @{
            StartTime = Get-Date
            EndTime = $null
            Duration = $null
            Status = "Running"
        }
    }
    
    [void]EndStage([string]$name, [bool]$success) {
        $metrics = $this->StageMetrics[$name]
        $metrics.EndTime = Get-Date
        $metrics.Duration = $metrics.EndTime - $metrics.StartTime
        $metrics.Status = if ($success) { "Success" } else { "Failed" }
    }
    
    [hashtable]GetReport() {
        return @{
            TotalDuration = (Get-Date) - $this->StartTime
            Stages = $this->StageMetrics
            SuccessRate = $this->CalculateSuccessRate()
        }
    }
}
```

## Integração com Ferramentas

### Jenkins Integration
```powershell
function Connect-Jenkins {
    param(
        [string]$JenkinsUrl,
        [PSCredential]$Credential
    )
    
    # Autenticação
    $auth = [Convert]::ToBase64String(
        [Text.Encoding]::ASCII.GetBytes(
            "$($Credential.UserName):$($Credential.GetNetworkCredential().Password)"
        )
    )
    
    # Headers
    $script:JenkinsHeaders = @{
        Authorization = "Basic $auth"
    }
    
    # Test connection
    try {
        Invoke-RestMethod -Uri "$JenkinsUrl/api/json" -Headers $script:JenkinsHeaders
        return $true
    }
    catch {
        Write-Error "Falha na conexão com Jenkins: $_"
        return $false
    }
}
```

### Azure DevOps Integration
```powershell
function Start-AzureDevOpsPipeline {
    param(
        [string]$Organization,
        [string]$Project,
        [string]$PipelineId
    )
    
    # Configuração Azure CLI
    az devops configure --defaults organization=$Organization project=$Project
    
    # Trigger pipeline
    az pipelines run --id $PipelineId --branch main
}
```

## Uso do Sistema

### Execução Básica
```batch
@echo off
:: Executa pipeline
pipeline.bat "1.0.0" "staging"
```

### Pipeline Avançado
```powershell
# Cria pipeline
$pipeline = [Pipeline]::new("1.0.0", "staging")

# Adiciona estágios
$pipeline.AddStage("Build", { Start-BuildStage })
$pipeline.AddStage("Test", { Start-TestStage })
$pipeline.AddStage("Deploy", { Start-DeployStage -Environment "staging" -Version "1.0.0" })

# Executa
$pipeline.Execute()
```

## Próximos Passos

1. Implementar análise de qualidade de código
2. Adicionar testes de segurança automatizados
3. Expandir integrações com outras ferramentas
4. Implementar deploy blue-green

## Recursos Adicionais

- [Jenkins API Documentation](https://www.jenkins.io/doc/book/using/remote-access-api/)
- [Azure DevOps REST API](https://docs.microsoft.com/rest/api/azure/devops)
- [PowerShell Pipeline Documentation](ps-reference.md)

---
_"Automatize com confiança, deploy com segurança."_