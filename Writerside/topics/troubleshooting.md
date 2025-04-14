# Troubleshooting Guide

> "Problemas são oportunidades de aprendizado disfarçadas."

## Problemas Comuns

### Acesso Negado
```powershell
# Sintomas
- "Access Denied"
- Error 5
- UnauthorizedAccess

# Soluções
1. Verificar permissões
2. Executar como admin
3. Checar ACLs
4. Validar tokens
```

### Comando Não Encontrado
```batch
# Sintomas
- "'X' is not recognized"
- "Command not found"

# Soluções
1. Verificar PATH
2. Confirmar instalação
3. Checar extensão
4. Validar ambiente
```

## Diagnóstico

### Logs
```powershell
# Locais importantes
- Event Viewer
- System logs
- App logs
- Security logs
```

### Ferramentas
```powershell
# Diagnóstico
- Process Monitor
- Network Monitor
- Resource Monitor
- Performance Monitor
```

## Network Issues

### Conectividade
```powershell
# Testes básicos
Test-NetConnection
ping
tracert
nslookup
```

### Portas
```powershell
# Verificação
netstat -ano
Test-NetConnection -Port
Get-NetTCPConnection
```

## Performance

### CPU
```powershell
# Análise CPU
Get-Process | Sort CPU -Descending
typeperf "\Processor(_Total)\% Processor Time"
```

### Memória
```powershell
# Memória
Get-Process | Sort WS -Descending
Get-Counter '\Memory\Available MBytes'
```

## Segurança

### Permissões
```powershell
# Verificação
Get-Acl
icacls
whoami /all
```

### Certificados
```powershell
# Certificados
Get-ChildItem Cert:\
certutil
```

## Scripts

### Debugging
```powershell
# Técnicas
Set-PSBreakpoint
Write-Debug
$DebugPreference = 'Continue'
```

### Logging
```powershell
# Logging estruturado
Start-Transcript
Write-EventLog
Add-Content log.txt
```

## Recovery

### Backup
```powershell
# Backup
- System Restore
- File History
- Shadow Copy
- Export settings
```

### Restore
```powershell
# Restore
- Safe Mode
- Last Known Good
- Recovery Console
- System Reset
```

## Checklist

### Básico
1. Verificar erros óbvios
2. Consultar logs
3. Testar reprodução
4. Documentar passos

### Avançado
1. Análise root cause
2. Implementar fix
3. Testar solução
4. Prevenir recorrência

## Recursos

### Documentação
- Microsoft Docs
- PowerShell Docs
- TechNet
- Stack Overflow

### Ferramentas
- Sysinternals
- PowerShell ISE
- VSCode
- Process Explorer

---
_"O melhor troubleshooting é aquele que previne o problema."_