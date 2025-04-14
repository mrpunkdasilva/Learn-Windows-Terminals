# Segurança no PowerShell

> "Segurança não é uma característica, é uma necessidade."

## Políticas de Execução

### Configuração de Políticas
```powershell
# Verificar política atual
Get-ExecutionPolicy -List

# Configurar política
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
Set-ExecutionPolicy -ExecutionPolicy AllSigned -Scope LocalMachine

# Bypass temporário
powershell.exe -ExecutionPolicy Bypass -File .\script.ps1
```

## Assinatura Digital

### Certificados e Assinaturas
```powershell
# Criar certificado auto-assinado
$cert = New-SelfSignedCertificate -Subject "CN=PowerShell Code Signing" `
    -Type CodeSigning -CertStoreLocation "Cert:\CurrentUser\My"

# Assinar script
Set-AuthenticodeSignature -FilePath .\script.ps1 `
    -Certificate $cert
```

## JEA (Just Enough Administration)

### Configuração JEA
```powershell
# Criar arquivo de configuração
New-PSSessionConfigurationFile -Path .\JEAConfig.pssc `
    -SessionType RestrictedRemoteServer `
    -VisibleCmdlets "Get-Process", "Stop-Process" `
    -VisibleFunctions "Get-CustomInfo"

# Registrar configuração
Register-PSSessionConfiguration -Path .\JEAConfig.pssc `
    -Name "RestrictedSession"
```

## Criptografia

### Proteção de Dados
```powershell
# Criptografar string
$secureString = ConvertTo-SecureString "SenhaSecreta" `
    -AsPlainText -Force
$encrypted = ConvertFrom-SecureString $secureString

# Descriptografar
$secure = ConvertTo-SecureString $encrypted
$BSTR = [System.Runtime.InteropServices.Marshal]::SecureStringToBSTR($secure)
$plaintext = [System.Runtime.InteropServices.Marshal]::PtrToStringAuto($BSTR)
```

## HTTPS e SSL/TLS

### Configuração de Comunicação Segura
```powershell
# Configurar HTTPS para remoting
winrm quickconfig -transport:https

# Verificar certificados
Get-ChildItem Cert:\LocalMachine\My

# Configurar listener HTTPS
New-WSManInstance winrm/config/Listener `
    -SelectorSet @{Transport="HTTPS"; Address="*"} `
    -ValueSet @{CertificateThumbprint="ABC123..."}
```

## Auditoria e Logging

### Monitoramento de Atividades
```powershell
# Habilitar logging detalhado
$logSettings = @{
    LogPipelineExecutionDetails = $true
    LogCommandLifecycleEvents = $true
}
Set-PSSessionConfiguration -Name Microsoft.PowerShell `
    -PSVersion "7.0" -SecurityDescriptorSddl $sddl @logSettings

# Verificar logs
Get-WinEvent -LogName "Windows PowerShell"
Get-WinEvent -LogName "Microsoft-Windows-PowerShell/Operational"
```

## Hardening de Ambiente

### Configurações Recomendadas
```powershell
# Desabilitar histórico de comandos
Set-PSReadLineOption -HistorySaveStyle SaveNothing

# Limitar módulos carregados
$env:PSModulePath = "C:\ApprovedModules"

# Configurar AMSI
[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12
```

## Boas Práticas

### Checklist de Segurança
```powershell
# Função de verificação de segurança
function Test-SecurityCompliance {
    $results = @{
        ExecutionPolicy = Get-ExecutionPolicy
        PSVersion = $PSVersionTable.PSVersion
        TLSVersion = [Net.ServicePointManager]::SecurityProtocol
        AMSIEnabled = Get-MpComputerStatus | Select-Object AMServiceEnabled
    }
    return $results
}
```

## Exercícios Práticos

1. Configuração de Ambiente Seguro
   ```powershell
   # Implemente:
   # - Política de execução apropriada
   # - Assinatura de scripts
   # - Logging adequado
   ```

2. Implementação JEA
   ```powershell
   # Desenvolva:
   # - Configuração JEA personalizada
   # - Roles e capacidades
   # - Testes de segurança
   ```

## Próximos Passos

1. [Remoting Seguro](ps-remote.md)
2. [Compliance e Auditoria](ps-compliance.md)
3. [DevSecOps](ps-devsecops.md)

---
_"A segurança é como uma cebola: tem várias camadas e pode fazer você chorar."_