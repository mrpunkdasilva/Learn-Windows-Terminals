# Azure PowerShell Integration

> "Automatize e gerencie recursos Azure com PowerShell."

## Configuração Inicial

### Instalação
```powershell
# Instalar Azure PowerShell
Install-Module -Name Az -AllowClobber -Scope CurrentUser

# Verificar instalação
Get-InstalledModule -Name Az -AllVersions
```

### Autenticação
```powershell
# Login interativo
Connect-AzAccount

# Login não-interativo
$credential = Get-Credential
Connect-AzAccount -Credential $credential

# Service Principal
$sp = @{
    SubscriptionId = "your-sub-id"
    TenantId = "your-tenant-id"
    ApplicationId = "your-app-id"
    CertificateThumbprint = "cert-thumbprint"
}
Connect-AzAccount -ServicePrincipal @sp
```

## Operações Comuns

### Resource Groups
```powershell
# Criar Resource Group
New-AzResourceGroup -Name "MyRG" -Location "eastus2"

# Listar Resource Groups
Get-AzResourceGroup | Select-Object ResourceGroupName, Location

# Remover Resource Group
Remove-AzResourceGroup -Name "MyRG" -Force
```

### Virtual Machines
```powershell
# Criar VM
$vmParams = @{
    ResourceGroupName = "MyRG"
    Name = "MyVM"
    Location = "eastus2"
    Size = "Standard_DS2_v2"
    Image = "Win2019Datacenter"
}
New-AzVM @vmParams

# Gerenciar estado
Stop-AzVM -ResourceGroupName "MyRG" -Name "MyVM"
Start-AzVM -ResourceGroupName "MyRG" -Name "MyVM"
```

### Storage
```powershell
# Criar Storage Account
$storageParams = @{
    ResourceGroupName = "MyRG"
    Name = "mystorageaccount"
    Location = "eastus2"
    SkuName = "Standard_LRS"
}
New-AzStorageAccount @storageParams

# Upload de blob
$context = (Get-AzStorageAccount -ResourceGroupName "MyRG" -Name "mystorageaccount").Context
Set-AzStorageBlobContent -Container "mycontainer" -File "myfile.txt" -Context $context
```

## Automação e DevOps

### ARM Templates
```powershell
# Deploy ARM template
$templateParams = @{
    ResourceGroupName = "MyRG"
    TemplateFile = "template.json"
    TemplateParameterFile = "parameters.json"
}
New-AzResourceGroupDeployment @templateParams
```

### Azure Functions
```powershell
# Deploy Function App
$functionParams = @{
    ResourceGroupName = "MyRG"
    Name = "MyFunctionApp"
    Runtime = "PowerShell"
    StorageAccountName = "mystorageaccount"
}
New-AzFunctionApp @functionParams
```

## Monitoramento

### Azure Monitor
```powershell
# Obter métricas
Get-AzMetric -ResourceId $resourceId -MetricName "CPU" -TimeGrain 00:01:00

# Configurar alertas
$alertParams = @{
    ResourceGroupName = "MyRG"
    Name = "HighCPUAlert"
    Location = "eastus2"
    TargetResourceId = $resourceId
    MetricName = "Percentage CPU"
    Operator = "GreaterThan"
    Threshold = 90
}
New-AzMetricAlertRule @alertParams
```

## Boas Práticas

### 1. Gerenciamento de Credenciais
```powershell
# Usar Key Vault
$secret = Get-AzKeyVaultSecret -VaultName "MyVault" -Name "MySecret"
$credential = New-Object PSCredential "user", $secret.SecretValue
```

### 2. Error Handling
```powershell
try {
    $result = New-AzResourceGroup -Name "MyRG" -Location "eastus2"
    Write-Output "Resource Group criado: $($result.ResourceGroupName)"
}
catch {
    Write-Error "Falha ao criar Resource Group: $_"
    throw
}
finally {
    Disconnect-AzAccount
}
```

### 3. Logging
```powershell
# Configurar logging
Enable-AzContextAutosave
Start-Transcript -Path ".\AzureDeployment.log"

# Executar operações
# ...

Stop-Transcript
```

## Scripts de Exemplo

### 1. Backup Automatizado
```powershell
function Backup-AzureVM {
    param(
        [string]$ResourceGroupName,
        [string]$VMName
    )
    
    # Criar snapshot do disco
    $vm = Get-AzVM -ResourceGroupName $ResourceGroupName -Name $VMName
    $disk = Get-AzDisk -ResourceGroupName $ResourceGroupName -DiskName $vm.StorageProfile.OsDisk.Name
    
    $snapshotConfig = New-AzSnapshotConfig -SourceUri $disk.Id -Location $disk.Location -CreateOption Copy
    $snapshot = New-AzSnapshot -ResourceGroupName $ResourceGroupName -SnapshotName "$VMName-snap-$(Get-Date -Format 'yyyyMMdd')" -Snapshot $snapshotConfig
    
    return $snapshot
}
```

### 2. Deployment Automatizado
```powershell
function Deploy-AzureEnvironment {
    param(
        [string]$Environment,
        [string]$Location
    )
    
    # Criar infraestrutura base
    $rgName = "RG-$Environment"
    New-AzResourceGroup -Name $rgName -Location $Location
    
    # Deploy recursos
    $resources = @(
        @{ Type = "Storage"; Name = "storage$Environment" }
        @{ Type = "AppService"; Name = "app$Environment" }
        @{ Type = "SQL"; Name = "sql$Environment" }
    )
    
    foreach ($resource in $resources) {
        Write-Output "Deploying $($resource.Type): $($resource.Name)"
        # Lógica de deployment específica para cada tipo
    }
}
```

## Próximos Passos

1. [AWS Integration](ps-aws-integration.md)
2. [DevOps Integration](ps-devops-integration.md)
3. [Security Best Practices](ps-security.md)

## Recursos Adicionais

- [Azure PowerShell Documentation](https://docs.microsoft.com/powershell/azure/)
- [Azure Resource Manager Templates](https://docs.microsoft.com/azure/azure-resource-manager/templates/)
- [Azure DevOps](https://docs.microsoft.com/azure/devops/)

---
_"Azure e PowerShell: a combinação perfeita para automação cloud."_