# Automação de Deploy com CMD

## Visão Geral
Sistema automatizado de deployment usando CMD, com suporte a múltiplos ambientes, rollback e monitoramento.

## Pré-requisitos
- Windows Server 2016+
- Acesso administrativo
- Git instalado
- Conexão com servidores de destino

## Estrutura do Projeto
```batch
deployment/
│   deploy.bat
│   rollback.bat
│   config.bat
│   
├───scripts/
│       pre-deploy.bat
│       post-deploy.bat
│       health-check.bat
│       backup.bat
│
├───environments/
│       dev.bat
│       staging.bat
│       prod.bat
│
└───logs/
        deploy.log
        error.log
```

## Implementação Core

### 1. Configuração Base
Arquivo `config.bat`:

```batch
@echo off
:: Configurações Globais
set "DEPLOY_ROOT=C:\deployments"
set "BACKUP_DIR=%DEPLOY_ROOT%\backups"
set "LOG_DIR=%DEPLOY_ROOT%\logs"
set "SCRIPT_DIR=%~dp0\scripts"

:: Configurações de Ambiente
set "DEV_SERVER=\\dev-server\deploy"
set "STAGING_SERVER=\\staging-server\deploy"
set "PROD_SERVER=\\prod-server\deploy"

:: Configurações de Aplicação
set "APP_NAME=minha-aplicacao"
set "APP_VERSION=1.0.0"
set "BACKUP_RETENTION=5"

:: Configurações de Notificação
set "SLACK_WEBHOOK=https://hooks.slack.com/services/xxx"
set "TEAMS_WEBHOOK=https://outlook.office.com/webhook/xxx"
```

### 2. Script Principal de Deploy
Arquivo `deploy.bat`:

```powershell
@echo off
setlocal enabledelayedexpansion

:: Carrega configurações
call config.bat

:: Parâmetros
set "ENV=%~1"
set "VERSION=%~2"

:: Validações
if "%ENV%"=="" (
    echo Ambiente não especificado
    echo Uso: deploy.bat [dev^|staging^|prod] [version]
    exit /b 1
)

:: Log inicial
echo [%date% %time%] Iniciando deploy para %ENV% >> "%LOG_DIR%\deploy.log"

:: Executa pre-deploy checks
call :preDeployChecks || exit /b 1

:: Backup
call :backupCurrentVersion || exit /b 1

:: Deploy
call :deployNewVersion || exit /b 1

:: Verificação
call :healthCheck || (
    call :rollback
    exit /b 1
)

:: Finalização
call :postDeployTasks
exit /b 0

:preDeployChecks
    echo Executando verificações pre-deploy...
    
    :: Verifica espaço em disco
    for /f "tokens=3" %%a in ('dir %DEPLOY_ROOT% ^| find "bytes free"') do (
        set "FREE_SPACE=%%a"
        if !FREE_SPACE! LSS 1000000000 (
            echo Espaço insuficiente em disco
            exit /b 1
        )
    )
    
    :: Verifica conectividade
    ping -n 1 %ENV_SERVER% >nul
    if errorlevel 1 (
        echo Servidor não acessível
        exit /b 1
    )
    
    exit /b 0

:backupCurrentVersion
    echo Criando backup...
    
    set "BACKUP_NAME=%APP_NAME%_%date:~-4%%date:~3,2%%date:~0,2%_%time:~0,2%%time:~3,2%"
    robocopy "%DEPLOY_ROOT%\current" "%BACKUP_DIR%\%BACKUP_NAME%" /MIR /R:3 /W:10
    
    :: Limpa backups antigos
    forfiles /p "%BACKUP_DIR%" /m *.* /d -%BACKUP_RETENTION% /c "cmd /c rmdir /s /q @path"
    
    exit /b 0

:deployNewVersion
    echo Realizando deploy...
    
    :: Stop da aplicação
    call "%SCRIPT_DIR%\stop-service.bat"
    
    :: Copia arquivos
    robocopy "%DEPLOY_ROOT%\releases\%VERSION%" "%DEPLOY_ROOT%\current" /MIR /MT:8
    
    :: Start da aplicação
    call "%SCRIPT_DIR%\start-service.bat"
    
    exit /b 0

:healthCheck
    echo Verificando saúde da aplicação...
    
    :: Aguarda inicialização
    timeout /t 30 /nobreak > nul
    
    :: Verifica endpoints
    call "%SCRIPT_DIR%\health-check.bat"
    if errorlevel 1 (
        echo Verificação de saúde falhou
        exit /b 1
    )
    
    exit /b 0

:rollback
    echo Iniciando rollback...
    
    :: Para a aplicação
    call "%SCRIPT_DIR%\stop-service.bat"
    
    :: Restaura backup
    robocopy "%BACKUP_DIR%\%BACKUP_NAME%" "%DEPLOY_ROOT%\current" /MIR
    
    :: Reinicia aplicação
    call "%SCRIPT_DIR%\start-service.bat"
    
    :: Notifica equipe
    call :notifyTeam "Deploy falhou, rollback executado"
    
    exit /b 0

:postDeployTasks
    echo Executando tarefas post-deploy...
    
    :: Limpa cache
    del /s /q "%DEPLOY_ROOT%\temp\*.*"
    
    :: Notifica sucesso
    call :notifyTeam "Deploy concluído com sucesso"
    
    exit /b 0

:notifyTeam
    set "MESSAGE=%~1"
    
    :: Slack
    curl -X POST -H "Content-Type: application/json" ^
         --data "{\"text\":\"%MESSAGE%\"}" ^
         %SLACK_WEBHOOK%
    
    :: Teams
    curl -X POST -H "Content-Type: application/json" ^
         --data "{\"text\":\"%MESSAGE%\"}" ^
         %TEAMS_WEBHOOK%
    
    exit /b 0
```

## Monitoramento e Logs

### Sistema de Logs
```batch
:: Configuração de logging
:logMessage
set "LEVEL=%~1"
set "MESSAGE=%~2"
echo [%date% %time%] [%LEVEL%] %MESSAGE% >> "%LOG_DIR%\deploy.log"
if "%LEVEL%"=="ERROR" (
    echo [%date% %time%] [%LEVEL%] %MESSAGE% >> "%LOG_DIR%\error.log"
)
```

### Monitoramento em Tempo Real
```batch
:: Monitor de deploy
:monitorDeploy
cls
echo === Status do Deploy ===
type "%LOG_DIR%\deploy.log" | find /i "[ERROR]"
type "%LOG_DIR%\deploy.log" | find /i "[INFO]"
timeout /t 5 > nul
goto monitorDeploy
```

## Sistema de Monitoramento e Alertas

### Monitoramento de Performance
```batch
@echo off
setlocal enabledelayedexpansion

:: Configurações de monitoramento
set "PERF_LOG=%LOG_DIR%\performance.log"
set "ALERT_THRESHOLD=90"
set "CHECK_INTERVAL=300"

:monitorLoop
    :: CPU Usage
    for /f "skip=1" %%p in ('wmic cpu get loadpercentage') do (
        set CPU_USAGE=%%p
        if !CPU_USAGE! GTR %ALERT_THRESHOLD% (
            call :alertTeam "CPU Usage crítico: !CPU_USAGE!%%"
        )
    )

    :: Memory Usage
    for /f "tokens=4" %%m in ('systeminfo ^| find "Physical Memory"') do (
        set MEM_USAGE=%%m
        if !MEM_USAGE! GTR %ALERT_THRESHOLD% (
            call :alertTeam "Memória crítica: !MEM_USAGE!%%"
        )
    )

    :: Disk Space
    for /f "tokens=3" %%d in ('dir %DEPLOY_ROOT% ^| find "bytes free"') do (
        set "FREE_SPACE=%%d"
        if !FREE_SPACE! LSS 1000000000 (
            call :alertTeam "Espaço em disco crítico"
        )
    )

    timeout /t %CHECK_INTERVAL% > nul
    goto monitorLoop
```

### Sistema de Métricas
```batch
:: Coleta de métricas de deploy
:collectMetrics
set "DEPLOY_START=%time%"
set "SUCCESS_COUNT=0"
set "FAILURE_COUNT=0"
set "ROLLBACK_COUNT=0"

:: Métricas de tempo
:calculateDeployTime
set "DEPLOY_END=%time%"
call :getTimeDiff "%DEPLOY_START%" "%DEPLOY_END%" deploy_duration
echo Deploy Duration: %deploy_duration% seconds >> "%LOG_DIR%\metrics.log"

:: Métricas de sucesso
:updateMetrics
if %ERRORLEVEL% EQU 0 (
    set /a SUCCESS_COUNT+=1
) else (
    set /a FAILURE_COUNT+=1
    if defined ROLLBACK_EXECUTED set /a ROLLBACK_COUNT+=1
)
```

### Integração com APM
```batch
:: Envio de métricas para APM
:sendToApm
set "APM_ENDPOINT=https://apm.example.com/api/metrics"
set "APM_KEY=your-api-key"

:: Prepara payload
set "PAYLOAD={"
set "PAYLOAD=%PAYLOAD%\"deployDuration\":%deploy_duration%,"
set "PAYLOAD=%PAYLOAD%\"successCount\":%SUCCESS_COUNT%,"
set "PAYLOAD=%PAYLOAD%\"failureCount\":%FAILURE_COUNT%,"
set "PAYLOAD=%PAYLOAD%\"rollbackCount\":%ROLLBACK_COUNT%"
set "PAYLOAD=%PAYLOAD%}"

:: Envia para APM
curl -X POST ^
     -H "Content-Type: application/json" ^
     -H "X-API-Key: %APM_KEY%" ^
     -d "%PAYLOAD%" ^
     %APM_ENDPOINT%
```

### Dashboard de Status
```batch
:: Dashboard em tempo real
:showDashboard
cls
echo ===================================
echo    DASHBOARD DE DEPLOY - %date%
echo ===================================
echo.
echo Status Atual: %DEPLOY_STATUS%
echo Ambiente: %ENV%
echo Versão: %VERSION%
echo.
echo Métricas:
echo - Deploys Sucesso: %SUCCESS_COUNT%
echo - Deploys Falha: %FAILURE_COUNT%
echo - Rollbacks: %ROLLBACK_COUNT%
echo.
echo Performance:
echo - CPU: %CPU_USAGE%%%
echo - Memória: %MEM_USAGE%%%
echo - Disco: %FREE_SPACE% bytes livres
echo ===================================
timeout /t 5 > nul
goto showDashboard
```

## Automação de Testes

### Testes Pré-Deploy
```batch
:: Suite de testes automatizados
:runTests
echo Executando suite de testes...

:: Testes de conectividade
call :testConnectivity || exit /b 1

:: Testes de configuração
call :testConfig || exit /b 1

:: Testes de dependências
call :testDependencies || exit /b 1

exit /b 0

:testConnectivity
    ping -n 1 %ENV_SERVER% >nul
    if errorlevel 1 (
        call :logMessage "ERROR" "Falha na conectividade"
        exit /b 1
    )
    exit /b 0

:testConfig
    if not exist "config.bat" (
        call :logMessage "ERROR" "Arquivo de configuração não encontrado"
        exit /b 1
    )
    exit /b 0

:testDependencies
    where robocopy >nul 2>nul
    if errorlevel 1 (
        call :logMessage "ERROR" "Robocopy não encontrado"
        exit /b 1
    )
    exit /b 0
```

### Testes Pós-Deploy
```batch
:: Verificações pós-deploy
:postDeployTests
echo Executando verificações pós-deploy...

:: Verifica serviços
sc query %SERVICE_NAME% | find "RUNNING"
if errorlevel 1 (
    call :logMessage "ERROR" "Serviço não está rodando"
    exit /b 1
)

:: Verifica endpoints
curl -f -s http://localhost:%PORT%/health
if errorlevel 1 (
    call :logMessage "ERROR" "Endpoint de saúde não responde"
    exit /b 1
)

:: Verifica logs
findstr /i "error exception" "%LOG_DIR%\app.log"
if not errorlevel 1 (
    call :logMessage "WARN" "Erros encontrados nos logs"
)

exit /b 0
```

## Integração com DevOps

### Pipeline CI/CD
```batch
:: Integração com pipeline
:cicdDeploy
echo Iniciando deploy via CI/CD...

:: Variáveis do pipeline
set "BUILD_ID=%1"
set "COMMIT_HASH=%2"
set "BRANCH=%3"

:: Logging especial
call :logMessage "INFO" "Deploy iniciado via CI/CD - Build: %BUILD_ID%"
call :logMessage "INFO" "Commit: %COMMIT_HASH% - Branch: %BRANCH%"

:: Deploy com tracking
call :deployWithTracking || exit /b 1

exit /b 0

:deployWithTracking
    :: Início do tracking
    set "DEPLOY_START=%time%"
    
    :: Executa deploy
    call :deployNewVersion
    set "DEPLOY_RESULT=%errorlevel%"
    
    :: Fim do tracking
    set "DEPLOY_END=%time%"
    
    :: Métricas
    call :calculateDeployTime
    call :updateMetrics
    call :sendToApm
    
    exit /b %DEPLOY_RESULT%
```

## Documentação e Referências

### Comandos Úteis
```batch
:: Lista de comandos úteis
echo Comandos disponíveis:
echo deploy.bat [env] [version] - Realiza deploy
echo rollback.bat [env] [version] - Executa rollback
echo monitor.bat - Inicia dashboard
echo metrics.bat - Exibe métricas
```

### Troubleshooting
1. **Logs não aparecem**
   - Verificar permissões do diretório de logs
   - Confirmar variáveis de ambiente
   - Validar rotação de logs

2. **Falha no Backup**
   - Verificar espaço em disco
   - Confirmar permissões
   - Validar nomenclatura

3. **Erro no Deploy**
   - Consultar logs detalhados
   - Verificar conectividade
   - Validar configurações

### Manutenção
1. **Limpeza Regular**
   - Rotação de logs
   - Limpeza de backups
   - Remoção de arquivos temporários

2. **Atualizações**
   - Scripts de automação
   - Dependências
   - Configurações

3. **Monitoramento**
   - Performance
   - Espaço em disco
   - Logs de erro
```

## Uso do Sistema

### Deploy Básico
```batch
:: Deploy para desenvolvimento
deploy.bat dev 1.0.0

:: Deploy para produção
deploy.bat prod 1.0.0
```

### Rollback Manual
```batch
:: Rollback para versão específica
rollback.bat prod 0.9.9
```

## Melhores Práticas

### Checklist de Deploy
1. Verificar configurações de ambiente
2. Executar testes pré-deploy
3. Confirmar backup
4. Monitorar logs
5. Validar funcionalidade

### Segurança
1. Usar credenciais seguras
2. Limitar acesso aos scripts
3. Validar origem do código
4. Manter logs de auditoria

## Resolução de Problemas

### Problemas Comuns
1. **Falha de Conexão**
   - Verificar firewall
   - Testar credenciais
   - Validar VPN

2. **Erro de Permissão**
   - Confirmar privilégios
   - Verificar ACLs
   - Validar service account

3. **Falha no Serviço**
   - Verificar dependências
   - Checar portas
   - Validar configurações

## Próximos Passos

### Melhorias Futuras
1. Implementar deploy blue-green
2. Adicionar testes automatizados
3. Integrar com pipeline CI/CD
4. Implementar métricas detalhadas

### Integrações
1. [Docker Integration](cmd-docker-integration.md)
2. [Cloud Integration](cmd-cloud-integration.md)


## Referências
- [Documentação Completa](reference.md)
- [Guia de Troubleshooting](troubleshooting.md)
- [Best Practices](best-practices.md)

## Segurança e Compliance

### Controle de Acesso
```batch
:: Verificação de permissões
:checkPermissions
echo Verificando permissões de acesso...

:: Verifica grupo de admin
net localgroup Administrators | find "%USERNAME%" >nul
if errorlevel 1 (
    call :logMessage "ERROR" "Usuário sem permissões administrativas"
    exit /b 1
)

:: Verifica permissões de rede
dir \\%ENV_SERVER%\deploy$ >nul 2>&1
if errorlevel 1 (
    call :logMessage "ERROR" "Sem acesso ao servidor de deploy"
    exit /b 1
)

exit /b 0
```

### Criptografia e Secrets
```batch
:: Gerenciamento de secrets
:manageSecrets
setlocal EnableDelayedExpansion

:: Carrega secrets criptografados
if exist "%DEPLOY_ROOT%\secrets.enc" (
    :: Descriptografa usando Windows Data Protection API
    powershell -Command "$encrypted = Get-Content '%DEPLOY_ROOT%\secrets.enc'; ^
                        $secure = ConvertTo-SecureString $encrypted; ^
                        $bstr = [System.Runtime.InteropServices.Marshal]::SecureStringToBSTR($secure); ^
                        $secrets = [System.Runtime.InteropServices.Marshal]::PtrToStringAuto($bstr); ^
                        Set-Content '%TEMP%\secrets.dec' $secrets"
    
    :: Carrega variáveis
    for /f "tokens=1,* delims==" %%a in (%TEMP%\secrets.dec) do (
        set "%%a=%%b"
    )
    
    :: Limpa arquivo temporário
    del /f /q "%TEMP%\secrets.dec"
)
```

## Recuperação de Desastres

### Backup Automatizado
```batch
:: Sistema de backup completo
:fullBackup
echo Iniciando backup completo...

:: Define timestamp
set "TIMESTAMP=%date:~-4%%date:~3,2%%date:~0,2%_%time:~0,2%%time:~3,2%"
set "BACKUP_FILE=backup_%TIMESTAMP%.zip"

:: Compacta arquivos
powershell -Command "Compress-Archive -Path '%DEPLOY_ROOT%\current\*' -DestinationPath '%BACKUP_DIR%\%BACKUP_FILE%'"

:: Upload para armazenamento externo
if defined AZURE_STORAGE_CONNECTION (
    echo Enviando backup para Azure Storage...
    azcopy copy "%BACKUP_DIR%\%BACKUP_FILE%" "%AZURE_STORAGE_CONNECTION%/backups/"
) else if defined AWS_S3_BUCKET (
    echo Enviando backup para AWS S3...
    aws s3 cp "%BACKUP_DIR%\%BACKUP_FILE%" "s3://%AWS_S3_BUCKET%/backups/"
)

:: Mantém histórico de backups
echo %TIMESTAMP%,%BACKUP_FILE%>> "%LOG_DIR%\backup_history.csv"
```

### Plano de Recuperação
```batch
:: Recuperação de desastre
:disasterRecovery
echo Iniciando procedimento de recuperação...

:: Para todos os serviços
call :stopAllServices

:: Restaura último backup válido
call :findLastValidBackup
if errorlevel 1 (
    call :logMessage "ERROR" "Nenhum backup válido encontrado"
    exit /b 1
)

:: Reconstrói ambiente
call :rebuildEnvironment

:: Reinicia serviços
call :startAllServices

exit /b 0

:findLastValidBackup
    :: Procura último backup na nuvem
    if defined AZURE_STORAGE_CONNECTION (
        azcopy ls "%AZURE_STORAGE_CONNECTION%/backups/" | sort /r | findstr /i ".zip" > "%TEMP%\backups.txt"
    ) else if defined AWS_S3_BUCKET (
        aws s3 ls "s3://%AWS_S3_BUCKET%/backups/" | sort /r | findstr /i ".zip" > "%TEMP%\backups.txt"
    )
    
    :: Verifica integridade
    for /f "tokens=4*" %%a in (%TEMP%\backups.txt) do (
        call :validateBackup "%%a"
        if not errorlevel 1 (
            set "LATEST_BACKUP=%%a"
            exit /b 0
        )
    )
    exit /b 1
```

## Integração com Ferramentas Externas

### Jenkins Integration
```batch
:: Integração com Jenkins
:jenkinsIntegration
echo Executando integração com Jenkins...

:: Parâmetros do Jenkins
set "JENKINS_JOB=%~1"
set "BUILD_NUMBER=%~2"

:: Notifica início do deploy
curl -X POST ^
     -H "Content-Type: application/json" ^
     -d "{\"status\":\"started\",\"job\":\"%JENKINS_JOB%\",\"build\":%BUILD_NUMBER%}" ^
     %JENKINS_CALLBACK_URL%

:: Executa deploy
call :deployNewVersion
set "DEPLOY_STATUS=%errorlevel%"

:: Notifica resultado
curl -X POST ^
     -H "Content-Type: application/json" ^
     -d "{\"status\":\"%DEPLOY_STATUS%\",\"job\":\"%JENKINS_JOB%\",\"build\":%BUILD_NUMBER%}" ^
     %JENKINS_CALLBACK_URL%

exit /b %DEPLOY_STATUS%
```

### Jira Integration
```batch
:: Integração com Jira
:updateJiraTicket
set "TICKET_ID=%~1"
set "STATUS=%~2"
set "COMMENT=%~3"

:: Atualiza ticket
curl -X POST ^
     -H "Content-Type: application/json" ^
     -H "Authorization: Basic %JIRA_AUTH%" ^
     -d "{\"update\":{\"comment\":[{\"add\":{\"body\":\"%COMMENT%\"}}]},\"transition\":{\"id\":\"%STATUS%\"}}" ^
     "%JIRA_URL%/rest/api/2/issue/%TICKET_ID%/transitions"
```

### Slack Integration
```batch
:: Notificações avançadas Slack
:notifySlackDetailed
set "STATUS=%~1"
set "DETAILS=%~2"

:: Prepara mensagem rica
set "PAYLOAD={"
set "PAYLOAD=%PAYLOAD%\"attachments\":[{"
set "PAYLOAD=%PAYLOAD%\"color\":\"%STATUS%\","
set "PAYLOAD=%PAYLOAD%\"title\":\"Deploy Update\","
set "PAYLOAD=%PAYLOAD%\"text\":\"%DETAILS%\","
set "PAYLOAD=%PAYLOAD%\"fields\":["
set "PAYLOAD=%PAYLOAD%{\"title\":\"Environment\",\"value\":\"%ENV%\",\"short\":true},"
set "PAYLOAD=%PAYLOAD%{\"title\":\"Version\",\"value\":\"%VERSION%\",\"short\":true},"
set "PAYLOAD=%PAYLOAD%{\"title\":\"Deployed by\",\"value\":\"%USERNAME%\",\"short\":true},"
set "PAYLOAD=%PAYLOAD%{\"title\":\"Duration\",\"value\":\"%deploy_duration%s\",\"short\":true}"
set "PAYLOAD=%PAYLOAD%]"
set "PAYLOAD=%PAYLOAD%}]"
set "PAYLOAD=%PAYLOAD%}"

:: Envia para Slack
curl -X POST ^
     -H "Content-Type: application/json" ^
     -d "%PAYLOAD%" ^
     %SLACK_WEBHOOK%
```

## Orquestração Multi-Ambiente

### Gerenciamento de Configuração
```batch
:: Sistema de configuração dinâmica
:loadEnvironmentConfig
set "ENV=%~1"
echo Carregando configuração para ambiente: %ENV%

:: Carrega configuração base
call "%DEPLOY_ROOT%\environments\base.bat"

:: Sobrescreve com configurações específicas do ambiente
if exist "%DEPLOY_ROOT%\environments\%ENV%.bat" (
    call "%DEPLOY_ROOT%\environments\%ENV%.bat"
) else (
    call :logMessage "ERROR" "Configuração não encontrada para %ENV%"
    exit /b 1
)

:: Valida configurações obrigatórias
call :validateRequiredConfig || exit /b 1

exit /b 0

:validateRequiredConfig
    :: Lista de variáveis obrigatórias
    set "REQUIRED_VARS=APP_NAME APP_VERSION DB_CONNECTION CACHE_SERVER"
    
    for %%v in (%REQUIRED_VARS%) do (
        if not defined %%v (
            call :logMessage "ERROR" "Variável obrigatória não definida: %%v"
            exit /b 1
        )
    )
    exit /b 0
```

### Deployment em Cascata
```batch
:: Deploy sequencial em múltiplos ambientes
:cascadeDeployment
set "VERSION=%~1"
echo Iniciando deployment em cascata da versão %VERSION%

:: Define ordem dos ambientes
set "DEPLOY_SEQUENCE=dev staging prod"

:: Loop através dos ambientes
for %%e in (%DEPLOY_SEQUENCE%) do (
    echo Preparando deploy para ambiente: %%e
    
    :: Carrega configuração do ambiente
    call :loadEnvironmentConfig "%%e"
    if errorlevel 1 goto :deployError
    
    :: Executa validações específicas do ambiente
    call :validateEnvironment "%%e"
    if errorlevel 1 goto :deployError
    
    :: Realiza o deploy
    call :deployToEnvironment "%%e" "%VERSION%"
    if errorlevel 1 goto :deployError
    
    :: Aguarda estabilização
    call :waitForStabilization "%%e"
    if errorlevel 1 goto :deployError
    
    echo Deploy concluído com sucesso em %%e
    timeout /t 30 /nobreak > nul
)

echo Deployment em cascata concluído com sucesso
exit /b 0

:deployError
set "FAILED_ENV=%%e"
echo Falha no deployment em %FAILED_ENV%
call :notifyTeam "Deployment em cascata falhou em %FAILED_ENV%"
exit /b 1

:validateEnvironment
set "ENV=%~1"
echo Validando ambiente %ENV%

:: Verifica disponibilidade de recursos
call :checkResources "%ENV%" || exit /b 1

:: Verifica dependências
call :checkDependencies "%ENV%" || exit /b 1

:: Verifica integrações
call :checkIntegrations "%ENV%" || exit /b 1

exit /b 0

:waitForStabilization
set "ENV=%~1"
set "ATTEMPTS=0"
set "MAX_ATTEMPTS=30"

:checkStability
set /a "ATTEMPTS+=1"
echo Verificando estabilidade (%ATTEMPTS%/%MAX_ATTEMPTS%)...

:: Verifica métricas de saúde
call :checkHealthMetrics
if errorlevel 1 (
    if %ATTEMPTS% lss %MAX_ATTEMPTS% (
        timeout /t 10 /nobreak > nul
        goto :checkStability
    ) else (
        call :logMessage "ERROR" "Timeout aguardando estabilização"
        exit /b 1
    )
)

exit /b 0
```

### Load Balancing e Blue-Green Deployment
```batch
:: Implementação de Blue-Green Deployment
:blueGreenDeploy
set "VERSION=%~1"
set "ENV=%~2"

:: Identifica ambiente atual (blue/green)
call :getCurrentEnvironment
set "CURRENT_ENV=%errorlevel%"

if %CURRENT_ENV%==1 (
    set "DEPLOY_ENV=green"
    set "STANDBY_ENV=blue"
) else (
    set "DEPLOY_ENV=blue"
    set "STANDBY_ENV=green"
)

echo Iniciando Blue-Green Deployment
echo Ambiente atual: %STANDBY_ENV%
echo Novo ambiente: %DEPLOY_ENV%

:: Deploy no ambiente inativo
call :deployToEnvironment "%DEPLOY_ENV%" "%VERSION%"
if errorlevel 1 goto :bgDeployError

:: Testes no novo ambiente
call :validateDeployment "%DEPLOY_ENV%"
if errorlevel 1 goto :bgDeployError

:: Switch de tráfego
call :switchTraffic "%DEPLOY_ENV%"
if errorlevel 1 goto :bgDeployError

:: Mantém ambiente anterior por período de grace
start /b cmd /c call :gracePeriod "%STANDBY_ENV%"

exit /b 0

:switchTraffic
set "TARGET_ENV=%~1"
echo Redirecionando tráfego para %TARGET_ENV%

:: Atualiza load balancer
call :updateLoadBalancer "%TARGET_ENV%"

:: Verifica switch
call :validateTraffic "%TARGET_ENV%"
if errorlevel 1 (
    call :logMessage "ERROR" "Falha no switch de tráfego"
    exit /b 1
)

exit /b 0

:gracePeriod
set "OLD_ENV=%~1"
echo Iniciando período de grace para %OLD_ENV%

:: Aguarda período de grace
timeout /t 3600 /nobreak > nul

:: Se não houver rollback, limpa ambiente antigo
if not exist "%DEPLOY_ROOT%\rollback_flag" (
    call :cleanupEnvironment "%OLD_ENV%"
)

exit /b 0
```

### Monitoramento Avançado
```batch
:: Sistema de monitoramento avançado
:advancedMonitoring
echo Iniciando monitoramento avançado...

:: Coleta métricas
start /b cmd /c call :collectMetrics
start /b cmd /c call :monitorLogs
start /b cmd /c call :checkAlerts

:: Dashboard em tempo real
call :updateDashboard

exit /b 0

:collectMetrics
:loop
    :: Performance
    wmic cpu get loadpercentage > "%TEMP%\cpu.txt"
    wmic memorychip get capacity > "%TEMP%\mem.txt"
    
    :: Aplicação
    call :getAppMetrics > "%TEMP%\app.txt"
    
    :: Banco de dados
    call :getDbMetrics > "%TEMP%\db.txt"
    
    :: Consolida métricas
    call :consolidateMetrics
    
    timeout /t 60 /nobreak > nul
goto :loop

:checkAlerts
:: Define thresholds
set "CPU_THRESHOLD=80"
set "MEMORY_THRESHOLD=90"
set "RESPONSE_THRESHOLD=2000"

:alertLoop
    :: Verifica métricas
    for /f %%a in (%TEMP%\cpu.txt) do (
        if %%a gtr %CPU_THRESHOLD% (
            call :triggerAlert "CPU" "Alto uso de CPU: %%a%%"
        )
    )
    
    :: Continua monitoramento
    timeout /t 30 /nobreak > nul
goto :alertLoop
```

### Automação de Testes
```batch
:: Framework de testes automatizados
:automatedTesting
echo Executando suite de testes...

:: Testes unitários
call :runUnitTests || exit /b 1

:: Testes de integração
call :runIntegrationTests || exit /b 1

:: Testes de carga
call :runLoadTests || exit /b 1

:: Testes de segurança
call :runSecurityTests || exit /b 1

exit /b 0

:runLoadTests
echo Executando testes de carga...

:: Configura JMeter
set "JMETER_HOME=%TOOLS_DIR%\jmeter"
set "TEST_PLAN=%DEPLOY_ROOT%\tests\load\main.jmx"

:: Executa testes
call "%JMETER_HOME%\bin\jmeter" -n -t "%TEST_PLAN%" ^
    -l "%LOG_DIR%\load-test.jtl" ^
    -e -o "%LOG_DIR%\load-test-report"

:: Analisa resultados
call :analyzeLoadTestResults
exit /b %errorlevel%

:runSecurityTests
echo Executando testes de segurança...

:: OWASP ZAP
set "ZAP_HOME=%TOOLS_DIR%\zap"
set "TARGET_URL=http://%DEPLOY_ENV%.example.com"

:: Scan de segurança
call "%ZAP_HOME%\zap.bat" -cmd ^
    -quickurl "%TARGET_URL%" ^
    -quickout "%LOG_DIR%\security-report.html"

:: Analisa vulnerabilidades
call :analyzeSecurityResults
exit /b %errorlevel%
```

## Documentação Adicional

### Guia de Resolução de Problemas
1. **Problemas de Blue-Green Deployment**
   - Verificar estado dos ambientes
   - Validar configurações de load balancer
   - Confirmar sincronização de dados

2. **Falhas de Monitoramento**
   - Verificar conectividade com agentes
   - Validar thresholds
   - Analisar histórico de alertas

3. **Problemas de Performance**
   - Análise de bottlenecks
   - Otimização de queries
   - Ajuste de recursos

### Melhores Práticas
1. **Deployment**
   - Sempre usar blue-green
   - Manter período de grace
   - Automatizar rollback

2. **Monitoramento**
   - Métricas em tempo real
   - Alertas proativos
   - Dashboards informativos

3. **Testes**
   - Cobertura completa
   - Automação contínua
   - Validação de segurança
