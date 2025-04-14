# Analisador de Logs em CMD

## Visão Geral
Sistema avançado para análise e processamento de logs usando CMD, com recursos de filtragem, agregação e geração de relatórios.

## Implementação Base

### Parser de Logs
```batch
@echo off
setlocal enabledelayedexpansion

:: Configurações
set "LOG_DIR=logs"
set "REPORT_DIR=reports"
set "DATE_FORMAT=%date:~-4%%date:~3,2%%date:~0,2%"

:parseLog
echo === Log Parser v1.0 ===
echo Data: %date% Hora: %time%

:: Cria diretório de relatórios se não existir
if not exist "%REPORT_DIR%" mkdir "%REPORT_DIR%"

:: Loop através dos arquivos de log
for %%f in (%LOG_DIR%\*.log) do (
    echo Processando: %%f
    call :processLogFile "%%f"
)

exit /b 0

:processLogFile
set "LOG_FILE=%~1"
set "OUTPUT_FILE=%REPORT_DIR%\report_%DATE_FORMAT%.txt"

:: Contadores
set "ERROR_COUNT=0"
set "WARN_COUNT=0"
set "INFO_COUNT=0"

:: Processa linha por linha
for /f "tokens=1,2,* delims=[]" %%a in ('type "%LOG_FILE%"') do (
    set "LOG_LEVEL=%%b"
    set "MESSAGE=%%c"
    
    :: Classifica por nível
    if "!LOG_LEVEL!"=="ERROR" set /a ERROR_COUNT+=1
    if "!LOG_LEVEL!"=="WARN" set /a WARN_COUNT+=1
    if "!LOG_LEVEL!"=="INFO" set /a INFO_COUNT+=1
    
    :: Armazena mensagens de erro para análise
    if "!LOG_LEVEL!"=="ERROR" (
        echo %%a [ERROR] !MESSAGE! >> "%REPORT_DIR%\errors.txt"
    )
)

:: Gera relatório
echo === Relatório: %LOG_FILE% === >> "%OUTPUT_FILE%"
echo Erros: %ERROR_COUNT% >> "%OUTPUT_FILE%"
echo Avisos: %WARN_COUNT% >> "%OUTPUT_FILE%"
echo Informações: %INFO_COUNT% >> "%OUTPUT_FILE%"
echo. >> "%OUTPUT_FILE%"

exit /b 0
```

### Análise de Padrões
```batch
:: Detector de padrões
:patternAnalysis
set "PATTERN_FILE=patterns.txt"
set "LOG_FILE=%~1"

echo Analisando padrões em %LOG_FILE%...

:: Carrega padrões conhecidos
for /f "tokens=1,* delims==" %%a in (%PATTERN_FILE%) do (
    set "PATTERN_NAME=%%a"
    set "PATTERN_REGEX=%%b"
    
    :: Busca padrão no log
    findstr /r /c:"!PATTERN_REGEX!" "%LOG_FILE%" > nul
    if not errorlevel 1 (
        echo Padrão detectado: !PATTERN_NAME!
        call :alertPattern "!PATTERN_NAME!" "%LOG_FILE%"
    )
)

exit /b 0

:alertPattern
set "PATTERN=%~1"
set "SOURCE=%~2"
echo [ALERT] Padrão %PATTERN% detectado em %SOURCE% >> "%REPORT_DIR%\alerts.txt"
exit /b 0
```

### Agregação e Estatísticas
```batch
:: Gerador de estatísticas
:generateStats
set "LOG_FILE=%~1"
set "STATS_FILE=%REPORT_DIR%\statistics.csv"

:: Cabeçalho do CSV
if not exist "%STATS_FILE%" (
    echo Data,Arquivo,Erros,Avisos,Infos,TotalLinhas > "%STATS_FILE%"
)

:: Calcula estatísticas
for /f %%a in ('type "%LOG_FILE%" ^| find /c /v ""') do set "TOTAL_LINES=%%a"
for /f %%a in ('findstr /i "error" "%LOG_FILE%" ^| find /c /v ""') do set "ERROR_COUNT=%%a"
for /f %%a in ('findstr /i "warn" "%LOG_FILE%" ^| find /c /v ""') do set "WARN_COUNT=%%a"
for /f %%a in ('findstr /i "info" "%LOG_FILE%" ^| find /c /v ""') do set "INFO_COUNT=%%a"

:: Adiciona linha ao CSV
echo %date%,%LOG_FILE%,%ERROR_COUNT%,%WARN_COUNT%,%INFO_COUNT%,%TOTAL_LINES% >> "%STATS_FILE%"

exit /b 0
```

## Funcionalidades Avançadas

### Monitor em Tempo Real
```batch
:: Monitor live de logs
:liveMonitor
set "TARGET_LOG=%~1"
set "REFRESH_RATE=2"

:monitorLoop
cls
echo === Monitor de Logs em Tempo Real ===
echo Arquivo: %TARGET_LOG%
echo Pressione Ctrl+C para sair
echo.

:: Exibe últimas linhas
tail -n 20 "%TARGET_LOG%"

:: Estatísticas rápidas
call :quickStats "%TARGET_LOG%"

timeout /t %REFRESH_RATE% > nul
goto monitorLoop

:quickStats
set "LOG=%~1"
echo.
echo === Estatísticas Rápidas ===
for /f %%a in ('findstr /i "error" "%LOG%" ^| find /c /v ""') do echo Erros (última hora): %%a
for /f %%a in ('findstr /i "warn" "%LOG%" ^| find /c /v ""') do echo Avisos (última hora): %%a
exit /b 0
```

### Sistema de Alertas
```batch
:: Configuração de alertas
set "ALERT_THRESHOLD_ERROR=10"
set "ALERT_THRESHOLD_WARN=50"
set "NOTIFICATION_CMD=notify.bat"

:checkAlerts
set "LOG_FILE=%~1"

:: Verifica thresholds
call :countErrors "%LOG_FILE%"
if %ERROR_COUNT% gtr %ALERT_THRESHOLD_ERROR% (
    call :triggerAlert "Alto número de erros: %ERROR_COUNT%"
)

call :countWarnings "%LOG_FILE%"
if %WARN_COUNT% gtr %ALERT_THRESHOLD_WARN% (
    call :triggerAlert "Alto número de avisos: %WARN_COUNT%"
)

exit /b 0

:triggerAlert
set "MESSAGE=%~1"
echo [ALERT] %date% %time% - %MESSAGE% >> "%REPORT_DIR%\alerts.log"
call "%NOTIFICATION_CMD%" "%MESSAGE%"
exit /b 0
```

## Uso do Sistema

### Execução Básica
```batch
:: Análise simples
analyzer.bat "app.log"

:: Monitoramento contínuo
analyzer.bat --monitor "app.log"

:: Geração de relatório
analyzer.bat --report "app.log"
```

### Configuração Avançada
```batch
:: Configurações personalizadas
set "CUSTOM_PATTERNS=patterns.txt"
set "ALERT_CONFIG=alerts.conf"
set "REPORT_TEMPLATE=template.html"

:: Execução com configurações
analyzer.bat --config "%CUSTOM_PATTERNS%" --alerts "%ALERT_CONFIG%" --template "%REPORT_TEMPLATE%" "app.log"
```

## Melhores Práticas

1. **Organização de Logs**
   - Rotação automática
   - Compressão de histórico
   - Nomenclatura padronizada

2. **Monitoramento**
   - Verificações regulares
   - Alertas proativos
   - Backup de relatórios

3. **Performance**
   - Filtragem eficiente
   - Processamento otimizado
   - Limpeza periódica

## Troubleshooting

### Problemas Comuns
1. **Logs Inacessíveis**
   - Verificar permissões
   - Confirmar caminhos
   - Validar formato

2. **Alertas Falsos**
   - Ajustar thresholds
   - Refinar padrões
   - Validar regras

3. **Performance Baixa**
   - Limitar tamanho de logs
   - Otimizar expressões
   - Aumentar intervalo de análise