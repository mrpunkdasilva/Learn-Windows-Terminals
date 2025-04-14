# Cloud Automation

> "Automatize operações multi-cloud com PowerShell"

## Visão Geral
Sistema de automação para gerenciamento de recursos em múltiplas clouds (AWS, Azure, GCP) usando PowerShell, com foco em otimização de custos e governança.

## Recursos
- Provisionamento automático
- Gerenciamento de recursos
- Otimização de custos
- Backup e recuperação
- Monitoramento multi-cloud
- Relatórios consolidados

## Estrutura do Projeto
```powershell
CloudAutomation/
  ├── src/
  │   ├── Providers/
  │   │   ├── AWS/
  │   │   │   ├── EC2Management.psm1
  │   │   │   ├── S3Operations.psm1
  │   │   │   └── CostManagement.psm1
  │   │   ├── Azure/
  │   │   │   ├── ResourceManagement.psm1
  │   │   │   ├── StorageOperations.psm1
  │   │   │   └── CostAnalysis.psm1
  │   │   └── GCP/
  │   │       ├── ComputeManagement.psm1
  │   │       └── StorageOperations.psm1
  │   ├── Core/
  │   │   ├── Authentication.psm1
  │   │   ├── ResourceMapping.psm1
  │   │   └── CostOptimization.psm1
  │   └── Reports/
  │       ├── CostReports.psm1
  │       └── UsageReports.psm1
  ├── config/
  │   ├── credentials.json
  │   └── settings.json
  └── scripts/
      ├── Deploy-Resources.ps1
      └── Optimize-Costs.ps1
```

## Implementação Principal

### Módulo de Autenticação Multi-Cloud
```powershell
function Connect-CloudProvider {
    [CmdletBinding()]
    param(
        [Parameter(Mandatory)]
        [ValidateSet('AWS', 'Azure', 'GCP')]
        [string]$Provider,
        
        [Parameter(Mandatory)]
        [PSCredential]$Credentials,
        
        [string]$Region = 'us-east-1'
    )
    
    switch ($Provider) {
        'AWS' {
            Set-AWSCredentials -AccessKey $Credentials.UserName -SecretKey $Credentials.GetNetworkCredential().Password
            Set-DefaultAWSRegion -Region $Region
        }
        'Azure' {
            Connect-AzAccount -Credential $Credentials
        }
        'GCP' {
            gcloud auth activate-service-account --key-file=$Credentials.Password
        }
    }
}
```

### Gerenciamento de Recursos
```powershell
function New-CloudResource {
    [CmdletBinding()]
    param(
        [Parameter(Mandatory)]
        [string]$Provider,
        
        [Parameter(Mandatory)]
        [string]$ResourceType,
        
        [Parameter(Mandatory)]
        [hashtable]$Parameters,
        
        [string]$Region
    )
    
    $resourceMap = @{
        'AWS' = @{
            'VM' = 'New-EC2Instance'
            'Storage' = 'New-S3Bucket'
        }
        'Azure' = @{
            'VM' = 'New-AzVM'
            'Storage' = 'New-AzStorageAccount'
        }
    }
    
    $command = $resourceMap[$Provider][$ResourceType]
    & $command @Parameters
}
```

### Otimização de Custos
```powershell
function Optimize-CloudResources {
    [CmdletBinding()]
    param(
        [string[]]$Providers = @('AWS', 'Azure'),
        [int]$DaysToAnalyze = 30
    )
    
    foreach ($provider in $Providers) {
        # Análise de uso
        $usage = Get-ResourceUsage -Provider $provider -Days $DaysToAnalyze
        
        # Identificar recursos ociosos
        $idleResources = $usage | Where-Object {
            $_.Utilization -lt 10 -and $_.Cost -gt 50
        }
        
        # Recomendações
        foreach ($resource in $idleResources) {
            [PSCustomObject]@{
                Provider = $provider
                ResourceId = $resource.Id
                CurrentCost = $resource.Cost
                Recommendation = if ($resource.Utilization -eq 0) {
                    "Terminate"
                } else {
                    "Downsize"
                }
                PotentialSavings = $resource.Cost * 0.7
            }
        }
    }
}
```

### Backup Multi-Cloud
```powershell
function Backup-CloudResources {
    [CmdletBinding()]
    param(
        [string]$Provider,
        [string[]]$ResourceIds,
        [string]$BackupLocation,
        [string]$RetentionDays = 30
    )
    
    $backupJobs = foreach ($id in $ResourceIds) {
        switch ($Provider) {
            'AWS' {
                $snapshot = New-EC2Snapshot -VolumeId $id
                Wait-EC2Snapshot -SnapshotId $snapshot.SnapshotId
                
                [PSCustomObject]@{
                    Provider = 'AWS'
                    ResourceId = $id
                    BackupId = $snapshot.SnapshotId
                    Status = 'Completed'
                }
            }
            'Azure' {
                $snapshot = New-AzSnapshot -SourceId $id
                
                [PSCustomObject]@{
                    Provider = 'Azure'
                    ResourceId = $id
                    BackupId = $snapshot.Id
                    Status = 'Completed'
                }
            }
        }
    }
    
    return $backupJobs
}
```

## Uso

### Configuração Inicial
```powershell
# Instalar módulos necessários
Install-Module -Name AWS.Tools.Common -Force
Install-Module -Name Az -Force
Install-Module -Name GoogleCloud -Force

# Configurar credenciais
$config = Get-Content .\config\credentials.json | ConvertFrom-Json
Connect-CloudProvider -Provider AWS -Credentials $config.AWS
Connect-CloudProvider -Provider Azure -Credentials $config.Azure
```

### Exemplos de Uso
```powershell
# Provisionar recursos
$vmParams = @{
    Name = 'webserver'
    Size = 't2.micro'
    Image = 'ami-12345678'
}
New-CloudResource -Provider 'AWS' -ResourceType 'VM' -Parameters $vmParams

# Otimizar custos
$savings = Optimize-CloudResources -Providers @('AWS', 'Azure')
$savings | Export-Csv -Path ".\reports\cost_savings.csv"

# Backup
$resources = Get-CloudResources -Type 'VM'
Backup-CloudResources -Provider 'AWS' -ResourceIds $resources.Id
```

## Monitoramento e Relatórios

### Dashboard de Custos
```powershell
function Get-CloudCostDashboard {
    [CmdletBinding()]
    param(
        [DateTime]$StartDate,
        [DateTime]$EndDate
    )
    
    $costs = @{
        AWS = Get-AWSCostAndUsage -StartDate $StartDate -EndDate $EndDate
        Azure = Get-AzConsumptionUsageDetail -StartDate $StartDate -EndDate $EndDate
    }
    
    $dashboard = @{
        TotalCost = ($costs.Values | Measure-Object -Sum).Sum
        ByProvider = $costs
        TopServices = $costs.Values | 
            Group-Object ServiceName |
            Sort-Object Count -Descending |
            Select-Object -First 5
    }
    
    return $dashboard
}
```

## Próximos Passos
1. Implementar suporte completo para GCP
2. Adicionar automação de compliance
3. Desenvolver previsão de custos com ML
4. Integrar com ferramentas de IaC

## Recursos Adicionais
- [AWS PowerShell Documentation](https://docs.aws.amazon.com/powershell/)
- [Azure PowerShell Documentation](https://docs.microsoft.com/powershell/azure/)
- [GCP PowerShell Documentation](https://cloud.google.com/tools/powershell/)

---
_"Automatize uma vez, execute em qualquer lugar."_