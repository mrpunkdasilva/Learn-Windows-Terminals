# Comandos do Sistema: Controlando o Windows

> "O sistema é seu. Você só precisa saber como controlá-lo."

## Gerenciamento de Processos

### Listagem e Monitoramento
```batch
:: Processos ativos
tasklist                    :: Lista todos processos
tasklist /fi "imagename eq chrome.exe"  :: Filtra processo
tasklist /v                :: Modo detalhado
wmic process get name,processid,priority :: Info detalhada

:: Monitoramento
tasklist /m                :: Mostra DLLs usadas
tasklist /svc             :: Mostra serviços
wmic process where "name='chrome.exe'" get processid,commandline
```

### Controle de Processos
```batch
:: Manipulação de processos
taskkill /PID 1234        :: Mata por PID
taskkill /IM chrome.exe /F :: Força fechamento
wmic process where name="chrome.exe" delete :: Alternativa
start "" notepad.exe      :: Inicia processo
```

## Serviços do Sistema

### Gerenciamento de Serviços
```batch
:: Controle de serviços
sc query                   :: Lista serviços
net start                  :: Serviços ativos
net start "Windows Update" :: Inicia serviço
net stop "Print Spooler"   :: Para serviço
sc config service_name start= disabled :: Desabilita
```

### Status e Configuração
```batch
:: Informações de serviços
sc queryex service_name    :: Info detalhada
sc qc service_name        :: Mostra config
wmic service list brief   :: Lista resumida
```

## Informações do Sistema

### Hardware
```batch
:: Info de hardware
systeminfo                :: Info completa
wmic cpu get name         :: Processador
wmic memorychip get capacity :: Memória
wmic diskdrive get size,model :: Discos
wmic bios get version     :: BIOS
```

### Software e Sistema
```batch
:: Info de software
ver                      :: Versão Windows
winver                   :: Janela de versão
wmic os get version      :: Versão detalhada
wmic product get name    :: Programas instalados
```

## Gerenciamento de Energia

### Controle de Energia
```batch
:: Energia e shutdown
shutdown /s /t 60        :: Desliga em 60s
shutdown /r /t 0         :: Reinicia agora
shutdown /h              :: Hiberna
powercfg /energy        :: Diagnóstico
powercfg /batteryreport :: Relatório bateria
```

## Rede e Conectividade

### Diagnóstico de Rede
```batch
:: Ferramentas de rede
ipconfig /all           :: Config completa
netstat -ano           :: Conexões ativas
route print            :: Tabela de rotas
nslookup google.com    :: DNS lookup
tracert 8.8.8.8       :: Trace route
```

### Configuração de Rede
```batch
:: Config de rede
netsh interface show interface :: Interfaces
netsh wlan show networks      :: Redes Wi-Fi
netsh advfirewall set allprofiles state off :: Firewall
```

## Scripts de Diagnóstico

### Monitor de Sistema
```batch
@echo off
:monitor
cls
echo === Monitor do Sistema ===
echo Data: %date% Hora: %time%

:: CPU e Memória
wmic cpu get loadpercentage
wmic OS get FreePhysicalMemory

:: Processos críticos
tasklist | findstr /i "chrome firefox node"

:: Rede
netstat -ano | findstr "ESTABLISHED"

timeout /t 5 > nul
goto monitor
```

### Verificação de Saúde
```batch
:: Script de diagnóstico
@echo off
echo === Diagnóstico do Sistema ===

:: Verificações básicas
sfc /scannow
chkdsk /f
dism /online /cleanup-image /restorehealth

:: Logs de erro
wevtutil qe System /c:5 /f:text
wevtutil qe Application /c:5 /f:text
```

## Melhores Práticas

### Segurança
- Use `runas` para elevação de privilégios
- Verifique processos desconhecidos
- Monitore portas abertas
- Mantenha logs de alterações

### Performance
- Limite processos em background
- Monitore uso de recursos
- Agende manutenção regular
- Documente configurações

## Próximos Passos

1. [Automação de Sistema](cmd-automation.md)
2. [Scripting Avançado](cmd-batch-scripting.md)
3. [Networking](cmd-networking.md)

---
_"Com grandes poderes vêm grandes responsabilidades... e muitos comandos para lembrar."_