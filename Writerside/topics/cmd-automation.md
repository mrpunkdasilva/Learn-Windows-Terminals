```
           _         _                        _   _             
     /\   | |       | |                      | | (_)            
    /  \  | |_ ___  | |_ __ ___   __ _  __ _| |_ _  ___  _ __  
   / /\ \ | __/ _ \ | | '_ ` _ \ / _` |/ _` | __| |/ _ \| '_ \ 
  / ____ \| || (_) || | | | | | | (_| | (_| | |_| | (_) | | | |
 /_/    \_\\__\___/ |_|_| |_| |_|\__,_|\__,_|\__|_|\___/|_| |_|
```

# Automação Avançada: Além dos Scripts

> "A verdadeira produtividade está na automação inteligente."

## Automação de Build

```batch
:: Build automatizado
@echo off
setlocal enabledelayedexpansion

:: Configurações
set BUILD_DIR=build
set SRC_DIR=src
set TIMESTAMP=%date:~-4%%date:~3,2%%date:~0,2%_%time:~0,2%%time:~3,2%

:: Limpeza
rmdir /s /q %BUILD_DIR%
mkdir %BUILD_DIR%

:: Build
call npm run build
if errorlevel 1 (
    echo Build falhou
    exit /b 1
)

:: Arquivamento
7z a %BUILD_DIR%\build_%TIMESTAMP%.zip %BUILD_DIR%\*
```

## Monitoramento Automatizado

```batch
:: Monitor de sistema
:monitor
cls
echo === Status do Sistema ===
echo Time: %time%

:: CPU e Memória
wmic cpu get loadpercentage
wmic OS get FreePhysicalMemory

:: Processos
tasklist | findstr /i "node npm"

:: Portas
netstat -ano | findstr "LISTENING"

timeout /t 10 > nul
goto monitor
```

## Integração com Git

```batch
:: Auto-commit e push
:gitSync
set BRANCH=main
set COMMIT_MSG="Auto-sync %date% %time%"

:: Verifica alterações
git status | findstr "nothing to commit" > nul
if not errorlevel 1 (
    echo Nada para sincronizar
    exit /b 0
)

:: Commit e push
git add .
git commit -m %COMMIT_MSG%
git push origin %BRANCH%
```

## Deployment Automatizado

```batch
:: Deploy multi-ambiente
:deploy
set ENV=%1
if "%ENV%"=="" set ENV=dev

:: Configurações por ambiente
if "%ENV%"=="prod" (
    set DEPLOY_DIR=\\prod-server\deploy
    set BACKUP_DIR=\\backup-server\prod
) else (
    set DEPLOY_DIR=\\dev-server\deploy
    set BACKUP_DIR=\\backup-server\dev
)

:: Backup
xcopy /s /e %DEPLOY_DIR% %BACKUP_DIR%\backup_%date%

:: Deploy
robocopy dist %DEPLOY_DIR% /MIR /MT:8
```

## Automação de Testes

```batch
:: Suite de testes
:testSuite
echo === Iniciando Testes ===

:: Testes unitários
call npm test
if errorlevel 1 goto :testFailed

:: Testes E2E
call npm run e2e
if errorlevel 1 goto :testFailed

echo === Testes Concluídos ===
exit /b 0

:testFailed
echo === Falha nos Testes ===
exit /b 1
```

## Automação de Logs

```batch
:: Rotação de logs
:rotateLogs
set LOG_DIR=logs
set MAX_LOGS=5

:: Remove logs antigos
forfiles /p %LOG_DIR% /m *.log /d -%MAX_LOGS% /c "cmd /c del @path"

:: Compacta logs
for %%f in (%LOG_DIR%\*.log) do (
    7z a %LOG_DIR%\%%~nf.zip %%f
    del %%f
)
```

## Integração com APIs

```batch
:: Chamadas REST
:checkAPI
curl -s -o response.tmp http://api.exemplo.com/status
findstr "OK" response.tmp > nul

if errorlevel 1 (
    echo API offline
    call :notifyTeam
) else (
    echo API operacional
)
```

## Scripts de Manutenção

```batch
:: Manutenção automática
:maintenance
echo === Iniciando Manutenção ===

:: Limpeza
call :cleanTemp
call :rotateLogs
call :backupDB

:: Verificações
call :checkDiskSpace
call :checkServices
call :checkCertificates

echo === Manutenção Concluída ===
```

## Troubleshooting Automático

```batch
:: Diagnóstico automático
:diagnose
set REPORT=diagnostic_%date%.txt

:: Coleta informações
systeminfo >> %REPORT%
tasklist >> %REPORT%
netstat -ano >> %REPORT%
dir /s /b >> %REPORT%

:: Análise
findstr "Error" *.log >> %REPORT%
```

## Próximos Passos

1. [CI/CD Integration](cmd-cicd.md)
2. [PowerShell Migration](ps-introduction.md)
3. [Cloud Integration](cmd-cloud.md)

---
_"Automatize o presente, controle o futuro."_