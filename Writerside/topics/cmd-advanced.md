# Técnicas Avançadas de CMD

> "Para os mestres do terminal, onde cada comando conta uma história."

## Manipulação Avançada de Strings

### Substrings e Parsing
```batch
:: Extração de substrings
set str=Hello World
set sub=%str:~0,5%     :: Retorna "Hello"
set sub=%str:~6%       :: Retorna "World"
set sub=%str:~-5%      :: Retorna "World"

:: Substituição de strings
set result=%str:Hello=Hi% :: Substitui "Hello" por "Hi"
set result=%str:o=0%     :: Substitui todos "o" por "0"
```

### Processamento de Texto
```batch
:: Manipulação avançada
for /f "tokens=1,2 delims=," %%a in (data.csv) do (
    echo First: %%a, Second: %%b
)

:: Parsing de saída de comando
for /f "tokens=* usebackq" %%i in (`dir /b`) do (
    echo File: %%i
)
```

## Controle de Fluxo Avançado

### Estruturas Complexas
```batch
:: Loop aninhado com condicionais
for %%i in (*.txt) do (
    if exist "backup\%%i" (
        echo Arquivo existe
        call :ProcessFile "%%i"
    ) else (
        echo Novo arquivo
        copy "%%i" "backup\%%i"
    )
)

:: Goto dinâmico
set "action=Process"
goto %action%_Section 2>nul || echo Ação inválida

:Process_Section
echo Processando...
exit /b
```

### Tratamento de Erros
```batch
:: Manipulação de erros
setlocal EnableExtensions EnableDelayedExpansion
set "err_level=0"

call :RiskyFunction || (
    set "err_level=!errorlevel!"
    goto :ErrorHandler
)

:ErrorHandler
echo Erro %err_level% ocorreu
exit /b %err_level%
```

## Funções Avançadas

### Biblioteca de Funções
```batch
:: Funções reusáveis
:Logger
echo [%date% %time%] %~1 >> log.txt
exit /b

:BackupFile
if not exist "backup" mkdir "backup"
copy "%~1" "backup\%~nx1.bak"
call :Logger "Backup criado: %~1"
exit /b

:GetFileSize
set "size="
for %%A in ("%~1") do set "size=%%~zA"
exit /b
```

### Callbacks e Eventos
```batch
:: Sistema de callbacks
set "callback=ProcessResult"
call :%callback% "dados"

:ProcessResult
echo Processando: %~1
exit /b
```

## Integração com Sistema

### Manipulação de Registry
```batch
:: Operações de registro
reg query "HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion"
reg add "HKCU\Environment" /v Path /t REG_EXPAND_SZ /d "%path%;C:\NewPath"
reg export "HKLM\SOFTWARE\MyApp" backup.reg
```

### Gerenciamento de Serviços
```batch
:: Controle avançado de serviços
sc config MyService start= delayed-auto
sc failure MyService reset= 86400 actions= restart/60000/restart/60000//
sc triggerinfo MyService start/port/80
```

## Automação Avançada

### Monitoramento de Eventos
```batch
:: Monitor de mudanças
:WatchDir
dir /b > dir_new.txt
fc dir_old.txt dir_new.txt > nul
if errorlevel 1 (
    call :ProcessChanges
)
move /y dir_new.txt dir_old.txt
timeout /t 5 > nul
goto WatchDir
```

### Tasks Agendadas
```batch
:: Criação de tarefas
schtasks /create /tn "MyTask" /tr "C:\script.bat" /sc daily /st 09:00
schtasks /change /tn "MyTask" /enable
schtasks /query /tn "MyTask" /fo list /v
```

## Networking Avançado

### Diagnóstico de Rede
```batch
:: Ferramentas avançadas
netsh trace start capture=yes report=yes
netsh interface ipv4 show subinterfaces
netsh advfirewall firewall add rule name="MyApp" dir=in action=allow
```

### Monitoramento de Conexões
```batch
:: Monitor de conexões
:MonitorConnections
netstat -nao | findstr "ESTABLISHED" > conn_new.txt
fc conn_old.txt conn_new.txt > nul
if errorlevel 1 (
    call :AlertNewConnection
)
move /y conn_new.txt conn_old.txt
timeout /t 10 > nul
goto MonitorConnections
```

## Dicas de Performance

### Otimização
- Use `setlocal EnableExtensions EnableDelayedExpansion`
- Evite chamadas desnecessárias a comandos externos
- Utilize variáveis em vez de comandos repetidos
- Minimize uso de `goto` em loops

### Debug e Profiling
```batch
:: Timer simples
set "start=%time%"
call :HeavyFunction
set "end=%time%"
call :CalcDuration "%start%" "%end%"
```

## Próximos Passos

1. [Scripting Avançado](cmd-batch-scripting.md)
2. [Integração com DevOps](cmd-integration.md)
3. [PowerShell Migration](ps-introduction.md)

---
_"A verdadeira maestria vem da compreensão dos detalhes."_