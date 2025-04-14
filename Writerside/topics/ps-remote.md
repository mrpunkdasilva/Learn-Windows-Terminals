# PowerShell Remoting

> "Gerencie qualquer servidor Windows, de qualquer lugar."

## Fundamentos do Remoting

### Configuração Básica
```powershell
# Habilitar remoting no servidor
Enable-PSRemoting -Force

# Configurar TrustedHosts no cliente
Set-Item WSMan:\localhost\Client\TrustedHosts -Value "*" -Force

# Verificar configuração
Test-WSMan
Get-PSSessionConfiguration
```

## Sessões Remotas

### Conexões Básicas
```powershell
# Sessão interativa
Enter-PSSession -ComputerName "servidor01"

# Comando único
Invoke-Command -ComputerName "servidor01" -ScriptBlock {
    Get-Process
}

# Múltiplos servidores
Invoke-Command -ComputerName "srv01","srv02" -ScriptBlock {
    Get-Service | Where-Object Status -eq "Running"
}
```

### Sessões Persistentes
```powershell
# Criar sessão
$session = New-PSSession -ComputerName "servidor01"

# Usar sessão
Invoke-Command -Session $session -ScriptBlock {
    $processes = Get-Process
    $services = Get-Service
    return @{
        Processes = $processes
        Services = $services
    }
}

# Fechar sessão
Remove-PSSession $session
```

## Autenticação e Segurança

### Credenciais
```powershell
# Criar credencial
$cred = Get-Credential

# Usar credencial
Enter-PSSession -ComputerName "servidor01" -Credential $cred

# Certificados
Enter-PSSession -ComputerName "servidor01" `
    -CertificateThumbprint "ABC123..." `
    -UseSSL
```

### Configurações de Segurança
```powershell
# Configurar HTTPS
Set-WSManQuickConfig -UseSSL

# Configurar endpoints
Register-PSSessionConfiguration -Name "AdminEndpoint" `
    -SecurityDescriptorSddl $sddl
```

## Transferência de Arquivos

### Cópia Remota
```powershell
# Copiar para servidor remoto
Copy-Item -Path "C:\local\arquivo.txt" `
    -Destination "C:\remoto\" `
    -ToSession $session

# Copiar do servidor remoto
Copy-Item -Path "C:\remoto\logs\" `
    -Destination "C:\local\" `
    -FromSession $session
```

## Gerenciamento em Massa

### Execução Paralela
```powershell
# Executar em paralelo
$servers = "srv01","srv02","srv03"
$jobs = Invoke-Command -ComputerName $servers `
    -ScriptBlock {
        Get-WindowsUpdate
    } -AsJob

# Aguardar resultados
$results = $jobs | Wait-Job | Receive-Job
```

### Fan-out Remoting
```powershell
# Configurar sessões
$sessions = New-PSSession -ComputerName $servers

# Executar comandos
Invoke-Command -Session $sessions -ScriptBlock {
    param($updateType)
    Install-WindowsUpdate -Type $updateType
} -ArgumentList "Security"
```

## Troubleshooting

### Diagnóstico
```powershell
# Testar conectividade
Test-NetConnection -ComputerName "servidor01" -Port 5985

# Verificar configuração
Get-PSSessionConfiguration
Get-Item WSMan:\localhost\Client\*

# Logs do WinRM
Get-WinEvent -LogName "Microsoft-Windows-WinRM/Operational"
```

## Boas Práticas

### Gerenciamento de Sessões
```powershell
# Função helper
function Start-RemoteOperation {
    param(
        [string[]]$ComputerName,
        [scriptblock]$Operation
    )
    
    try {
        $sessions = New-PSSession -ComputerName $ComputerName
        Invoke-Command -Session $sessions -ScriptBlock $Operation
    }
    catch {
        Write-Error "Falha na operação remota: $_"
    }
    finally {
        $sessions | Remove-PSSession
    }
}
```

### Otimização
```powershell
# Minimizar tráfego de rede
$session = New-PSSession -ComputerName "servidor01"
Invoke-Command -Session $session -ScriptBlock {
    $data = Get-Process
    $filtered = $data | Where-Object CPU -gt 50
    return $filtered
}
```

## Exercícios Práticos

1. Configuração Básica
   ```powershell
   # Configure remoting em:
   # - Servidor local
   # - Servidor remoto
   # - Autenticação segura
   ```

2. Gerenciamento Multi-servidor
   ```powershell
   # Desenvolva script para:
   # - Monitorar múltiplos servidores
   # - Coletar métricas
   # - Gerar relatório consolidado
   ```

3. Automação Remota
   ```powershell
   # Crie workflow para:
   # - Atualização de servidores
   # - Backup distribuído
   # - Verificação de saúde
   ```

## Próximos Passos

1. [Segurança PowerShell](ps-security.md)
2. [Debugging Remoto](ps-debugging.md)
3. [DevOps Integration](ps-devops-integration.md)

---
_"Com grande poder remoto vem grande responsabilidade."_