# PowerShell DevOps Integration

> "Automatize seu pipeline DevOps com o poder do PowerShell."

## CI/CD Integration

### Azure DevOps
```powershell
# Configurar Azure DevOps CLI
az extension add --name azure-devops

# Criar pipeline
$pipelineYaml = @'
trigger:
- main

pool:
  vmImage: 'windows-latest'

steps:
- task: PowerShell@2
  inputs:
    targetType: 'inline'
    script: |
      Write-Host "Executando pipeline..."
'@

az pipelines create --name "MyPipeline" --yaml-path /azure-pipelines.yml
```

### Jenkins
```powershell
# Script para Jenkins pipeline
function Invoke-JenkinsPipeline {
    param(
        [string]$JenkinsUrl,
        [string]$JobName,
        [PSCredential]$Credential
    )
    
    $auth = [Convert]::ToBase64String(
        [Text.Encoding]::ASCII.GetBytes(
            "$($Credential.UserName):$($Credential.GetNetworkCredential().Password)"
        )
    )
    
    $headers = @{
        Authorization = "Basic $auth"
    }
    
    Invoke-RestMethod -Uri "$JenkinsUrl/job/$JobName/build" -Headers $headers -Method Post
}
```

## Infrastructure as Code

### Terraform Integration
```powershell
function Deploy-TerraformInfra {
    param(
        [string]$WorkingDirectory,
        [string]$Environment
    )
    
    Push-Location $WorkingDirectory
    
    try {
        # Inicializar Terraform
        terraform init
        
        # Selecionar workspace
        terraform workspace select $Environment
        
        # Planejar mudanças
        terraform plan -out=tfplan
        
        # Aplicar mudanças
        terraform apply tfplan
    }
    finally {
        Pop-Location
    }
}
```

### ARM Templates
```powershell
function Deploy-ArmTemplate {
    param(
        [string]$ResourceGroupName,
        [string]$TemplateFile,
        [hashtable]$Parameters
    )
    
    New-AzResourceGroupDeployment `
        -ResourceGroupName $ResourceGroupName `
        -TemplateFile $TemplateFile `
        -TemplateParameterObject $Parameters `
        -Mode Incremental
}
```

## Container Orchestration

### Docker Integration
```powershell
function Build-DockerImage {
    param(
        [string]$DockerfilePath,
        [string]$ImageName,
        [string]$Tag = "latest"
    )
    
    docker build -t "${ImageName}:${Tag}" -f $DockerfilePath .
    
    if ($LASTEXITCODE -eq 0) {
        Write-Output "Image built successfully: ${ImageName}:${Tag}"
    }
    else {
        throw "Docker build failed"
    }
}
```

### Kubernetes Management
```powershell
function Deploy-KubernetesApp {
    param(
        [string]$Namespace,
        [string]$DeploymentFile
    )
    
    # Verificar conexão com cluster
    kubectl cluster-info
    if ($LASTEXITCODE -ne 0) {
        throw "Kubernetes cluster não acessível"
    }
    
    # Criar/atualizar namespace
    kubectl create namespace $Namespace --dry-run=client -o yaml | kubectl apply -f -
    
    # Aplicar deployment
    kubectl apply -f $DeploymentFile -n $Namespace
}
```

## Monitoramento e Logging

### Prometheus Integration
```powershell
function Get-PrometheusMetrics {
    param(
        [string]$PrometheusUrl,
        [string]$Query
    )
    
    $endpoint = "$PrometheusUrl/api/v1/query"
    $response = Invoke-RestMethod -Uri "$endpoint?query=$Query"
    
    return $response.data.result
}
```

### Grafana Dashboard
```powershell
function New-GrafanaDashboard {
    param(
        [string]$GrafanaUrl,
        [string]$ApiKey,
        [string]$DashboardJson
    )
    
    $headers = @{
        Authorization = "Bearer $ApiKey"
        'Content-Type' = 'application/json'
    }
    
    Invoke-RestMethod `
        -Uri "$GrafanaUrl/api/dashboards/db" `
        -Method Post `
        -Headers $headers `
        -Body $DashboardJson
}
```

## Boas Práticas

### 1. Versionamento
- Use controle de versão para scripts
- Mantenha documentação atualizada
- Implemente testes automatizados
- Siga convenções de nomenclatura

### 2. Segurança
- Utilize secrets management
- Implemente RBAC
- Audite execuções
- Valide inputs

### 3. Automação
- Crie pipelines reutilizáveis
- Automatize testes
- Implemente validações
- Configure monitoramento

## Próximos Passos

1. [Azure Integration](ps-azure-integration.md)
2. [AWS Integration](ps-aws-integration.md)
3. [Security Best Practices](ps-security.md)

## Recursos Adicionais

- [Azure DevOps Documentation](https://docs.microsoft.com/azure/devops)
- [Jenkins Documentation](https://www.jenkins.io/doc)
- [Terraform Documentation](https://www.terraform.io/docs)
- [Kubernetes Documentation](https://kubernetes.io/docs)

---
_"DevOps é sobre cultura, automação é apenas uma ferramenta para alcançá-la."_