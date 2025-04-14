# Projetos Híbridos CMD/PowerShell

> "Combine o melhor dos dois mundos para criar soluções poderosas."

## Introdução

Projetos híbridos utilizam tanto CMD quanto PowerShell para criar soluções mais robustas e flexíveis. Esta abordagem permite:
- Manter compatibilidade com sistemas legados
- Aproveitar recursos modernos do PowerShell
- Otimizar performance em diferentes cenários
- Criar soluções mais versáteis

## Projeto 1: Sistema de Migração

### Objetivo
Automatizar a migração de aplicações legadas para ambientes modernos.

### Implementação
```batch
@echo off
setlocal enabledelayedexpansion

:: Configuração inicial - CMD
set "SOURCE_DIR=%~1"
set "TARGET_DIR=%~2"
set "LOG_FILE=%~dp0migration.log"

:: Validação de diretórios legados
if not exist "%SOURCE_DIR%" (
    echo Diretório fonte não encontrado
    exit /b 1
)

:: Chamada do PowerShell para processamento moderno
powershell.exe -File "%~dp0process-migration.ps1" -Source "%SOURCE_DIR%" -Target "%TARGET_DIR%"
```

```powershell
# process-migration.ps1
param(
    [string]$Source,
    [string]$Target
)

function Start-Migration {
    try {
        # Análise moderna de compatibilidade
        $compatibility = Test-MigrationCompatibility -Path $Source
        
        # Transformação de dados
        $data = Convert-LegacyData -Path $Source
        
        # Deploy no novo ambiente
        Deploy-ModernApplication -Data $data -Target $Target
    }
    catch {
        Write-Error "Erro na migração: $_"
        exit 1
    }
}
```

## Projeto 2: Monitor de Infraestrutura

### Objetivo
Sistema de monitoramento que combina coleta de dados legada com análise moderna.

### Implementação
```batch
@echo off
:: Monitor Legacy - CMD
set "PERF_LOG=%temp%\perfmon.log"
set "INTERVAL=60"

:: Coleta dados de performance legados
typeperf "\Processor(_Total)\% Processor Time" -si %INTERVAL% -o "%PERF_LOG%"
```

```powershell
# Modern Analysis - PowerShell
function Start-HybridMonitoring {
    # Configuração de alertas modernos
    $alertConfig = @{
        CPU = 80
        Memory = 90
        Disk = 85
    }

    # Processamento em tempo real
    while ($true) {
        # Lê dados legados
        $legacyData = Import-Csv $env:temp\perfmon.log
        
        # Análise moderna
        $analysis = New-ModernAnalysis -Data $legacyData
        
        # Alertas inteligentes
        if ($analysis.RequiresAction) {
            Send-ModernAlert -Analysis $analysis
        }
        
        Start-Sleep -Seconds 60
    }
}
```

## Projeto 3: Automação de Backup

### Objetivo
Sistema de backup que utiliza CMD para operações básicas e PowerShell para features avançadas.

### Implementação
```batch
@echo off
:: Backup Básico - CMD
set "BACKUP_ROOT=D:\Backups"
set "DATE=%date:~-4,4%%date:~-10,2%%date:~-7,2%"

:: Criação de estrutura básica
md "%BACKUP_ROOT%\%DATE%" 2>nul

:: Chamada PowerShell para features avançadas
powershell.exe -File "%~dp0advanced-backup.ps1" -BackupRoot "%BACKUP_ROOT%" -Date "%DATE%"
```

```powershell
# advanced-backup.ps1
param(
    [string]$BackupRoot,
    [string]$Date
)

function Start-AdvancedBackup {
    # Compressão avançada
    $compression = @{
        Level = "Optimal"
        Algorithm = "LZMA2"
    }

    # Backup com verificação
    try {
        # Snapshot VSS
        $snapshot = New-VssSnapshot
        
        # Backup com deduplição
        Backup-DataWithDedup -Source $snapshot -Target "$BackupRoot\$Date"
        
        # Validação de integridade
        Test-BackupIntegrity -Path "$BackupRoot\$Date"
    }
    finally {
        Remove-VssSnapshot $snapshot
    }
}
```

## Melhores Práticas

### Integração CMD/PowerShell
1. Use CMD para:
   - Operações simples de arquivo
   - Compatibilidade legada
   - Scripts batch existentes
   - Comandos do sistema básicos

2. Use PowerShell para:
   - Processamento complexo
   - Manipulação de objetos
   - Integrações modernas
   - Features avançadas

### Tratamento de Erros
```powershell
# Função de wrapper para comandos CMD
function Invoke-CmdCommand {
    param([string]$Command)
    
    $result = cmd /c "$Command 2>&1"
    if ($LASTEXITCODE -ne 0) {
        throw "Erro no comando CMD: $result"
    }
    return $result
}
```

### Logging Híbrido
```batch
:: CMD Logging
echo [%date% %time%] Iniciando operação >> "%LOG_FILE%"
```

```powershell
# PowerShell Logging
function Write-HybridLog {
    param(
        [string]$Message,
        [string]$LogFile
    )
    
    $timestamp = Get-Date -Format "yyyy-MM-dd HH:mm:ss"
    Add-Content -Path $LogFile -Value "[$timestamp] $Message"
}
```

## Próximos Passos

1. [CI/CD Híbrido](ci-cd-automation.md)
2. [Gestão Multi-ambiente](cloud-management.md)
3. [Segurança Integrada](security-automation.md)

## Recursos Adicionais

- [Documentação CMD](cmd-reference.md)
- [Documentação PowerShell](ps-reference.md)
- [Guia de Integração](integration-guide.md)

---
_"A verdadeira força está na união das ferramentas certas para cada tarefa."_