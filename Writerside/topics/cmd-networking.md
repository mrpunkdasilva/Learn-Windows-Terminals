# Networking no CMD: Dominando a Rede

> "A rede é como um labirinto: você precisa conhecer os caminhos certos para não se perder."

## Comandos Básicos de Rede

### Configuração IP
```batch
:: Informações de rede
ipconfig                    :: Info básica
ipconfig /all              :: Info detalhada
ipconfig /release          :: Libera IP
ipconfig /renew            :: Renova IP
ipconfig /flushdns         :: Limpa cache DNS
```

### Diagnóstico de Conectividade
```batch
:: Testes básicos
ping google.com            :: Teste básico
ping -t 8.8.8.8           :: Ping contínuo
tracert github.com        :: Trace route
pathping servidor         :: Path + ping
nslookup dominio.com     :: Consulta DNS
```

## Monitoramento de Rede

### Conexões Ativas
```batch
:: Monitoramento de portas
netstat -an               :: Todas conexões
netstat -b               :: Processos + portas
netstat -ano            :: PIDs
netstat -sp TCP        :: Estatísticas TCP
```

### Análise de Tráfego
```batch
:: Captura de pacotes
netsh trace start capture=yes    :: Inicia captura
netsh trace stop                :: Para captura
netsh trace show status        :: Status
```

## Configuração de Rede

### Interface de Rede
```batch
:: Configuração de interface
netsh interface show interface           :: Lista interfaces
netsh interface ip show config           :: Mostra config
netsh interface ip set address "Local" static IP MASK GATEWAY
netsh interface ip set dns "Local" static DNS_SERVER
```

### Firewall
```batch
:: Gerenciamento do firewall
netsh advfirewall show allprofiles      :: Status
netsh advfirewall set allprofiles state on
netsh advfirewall firewall add rule name="App" dir=in action=allow program="C:\app.exe"
```

## Compartilhamento e Recursos

### Compartilhamentos
```batch
:: Gerenciamento de shares
net share                     :: Lista shares
net share PASTA=C:\Pasta     :: Cria share
net share PASTA /delete      :: Remove share
net use Z: \\servidor\share  :: Mapeia drive
```

### Recursos de Rede
```batch
:: Recursos disponíveis
net view                     :: Lista recursos
net view \\servidor          :: Recursos do servidor
net session                 :: Sessões ativas
net use                    :: Conexões ativas
```

## Troubleshooting

### Diagnóstico Avançado
```batch
:: Ferramentas avançadas
route print                  :: Tabela de rotas
arp -a                      :: Cache ARP
netsh wlan show networks    :: Redes Wi-Fi
netsh diag show adapter    :: Diagnóstico
```

### Script de Diagnóstico
```batch
@echo off
echo === Diagnóstico de Rede ===
ipconfig /all
echo.
echo === Teste de Conectividade ===
ping -n 1 8.8.8.8
ping -n 1 google.com
echo.
echo === Portas Ativas ===
netstat -an | findstr "ESTABLISHED"
```

## Automação de Rede

### Monitor de Conexão
```batch
:: Monitor de conectividade
@echo off
:check
cls
echo Monitorando conexão...
ping -n 1 8.8.8.8 | findstr "TTL=" > nul
if errorlevel 1 (
    echo Conexão perdida!
    echo %date% %time% - Falha >> network.log
) else (
    echo Conexão OK
)
timeout /t 5 > nul
goto check
```

### Reset de Rede
```batch
:: Script de reset
@echo off
echo Resetando configurações de rede...
ipconfig /release
ipconfig /flushdns
netsh winsock reset
netsh int ip reset
ipconfig /renew
echo Reset completo. Reinicie o computador.
```

## Melhores Práticas

### Segurança
- Use firewall adequadamente
- Monitore conexões suspeitas
- Mantenha logs de atividade
- Limite compartilhamentos

### Performance
- Otimize configurações TCP/IP
- Monitore latência
- Gerencie largura de banda
- Mantenha DNS atualizado

## Próximos Passos

1. [Segurança de Rede](cmd-security.md)
2. [Automação Avançada](cmd-automation.md)
3. [Integração com Cloud](cmd-cloud-integration.md)

---
_"Na rede, cada comando é uma ponte para algum lugar."_