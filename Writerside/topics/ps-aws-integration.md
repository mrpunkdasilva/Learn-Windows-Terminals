# AWS PowerShell Integration

> "Gerencie sua infraestrutura AWS com o poder do PowerShell."

## Configuração Inicial

### Instalação
```powershell
# Instalar AWS Tools
Install-Module -Name AWS.Tools.Installer
Install-AWSToolsModule AWS.Tools.EC2, AWS.Tools.S3, AWS.Tools.Common

# Verificar instalação
Get-AWSPowerShellVersion
```

### Autenticação
```powershell
# Configurar credenciais
Set-AWSCredential -AccessKey "YOUR-ACCESS-KEY" -SecretKey "YOUR-SECRET-KEY" -StoreAs "default"

# Usar perfil
Initialize-AWSDefaultConfiguration -ProfileName "default" -Region "us-east-1"
```

## Serviços Principais

### EC2 (Elastic Compute Cloud)
```powershell
# Listar instâncias
Get-EC2Instance

# Criar instância
$launchParams = @{
    ImageId = "ami-12345678"
    InstanceType = "t2.micro"
    KeyName = "my-key-pair"
    SecurityGroupId = "sg-12345678"
}
New-EC2Instance @launchParams

# Gerenciar estado
Stop-EC2Instance -InstanceId "i-1234567890abcdef0"
Start-EC2Instance -InstanceId "i-1234567890abcdef0"
```

### S3 (Simple Storage Service)
```powershell
# Criar bucket
New-S3Bucket -BucketName "my-unique-bucket"

# Upload de arquivo
Write-S3Object -BucketName "my-bucket" -File "local-file.txt" -Key "remote-file.txt"

# Download de arquivo
Read-S3Object -BucketName "my-bucket" -Key "remote-file.txt" -File "local-copy.txt"
```

### Lambda
```powershell
# Criar função
$functionCode = @'
exports.handler = async (event) => {
    return {
        statusCode: 200,
        body: JSON.stringify('Hello from Lambda!')
    };
};
'@

$function = New-AWSFunction `
    -FunctionName "MyFunction" `
    -Runtime "nodejs14.x" `
    -Role "arn:aws:iam::123456789012:role/lambda-role" `
    -Code_ZipFile ([System.Text.Encoding]::UTF8.GetBytes($functionCode))

# Invocar função
Invoke-LMFunction -FunctionName "MyFunction"
```

## Automação e DevOps

### CloudFormation
```powershell
# Deploy stack
New-CFNStack `
    -StackName "MyStack" `
    -TemplateBody (Get-Content "template.json" -Raw) `
    -Parameter @(
        @{ ParameterKey="Environment"; ParameterValue="Production" }
    )

# Verificar status
Get-CFNStack -StackName "MyStack"
```

### Systems Manager
```powershell
# Executar comando
$command = Send-SSMCommand `
    -InstanceId "i-1234567890abcdef0" `
    -DocumentName "AWS-RunShellScript" `
    -Parameter @{
        commands = @("echo Hello World")
    }

# Obter resultado
Get-SSMCommandInvocation `
    -CommandId $command.CommandId `
    -Details $true
```

## Monitoramento

### CloudWatch
```powershell
# Obter métricas
$metric = Get-CWMetricStatistic `
    -Namespace "AWS/EC2" `
    -MetricName "CPUUtilization" `
    -Dimension @{Name="InstanceId";Value="i-1234567890abcdef0"} `
    -StartTime (Get-Date).AddHours(-1) `
    -EndTime (Get-Date) `
    -Period 300 `
    -Statistic "Average"

# Criar alarme
Write-CWMetricAlarm `
    -AlarmName "HighCPU" `
    -MetricName "CPUUtilization" `
    -Namespace "AWS/EC2" `
    -Statistic "Average" `
    -Period 300 `
    -Threshold 90 `
    -ComparisonOperator "GreaterThanThreshold" `
    -EvaluationPeriods 2
```

## Scripts de Exemplo

### 1. Backup Automatizado EC2
```powershell
function Backup-EC2Instance {
    param(
        [string]$InstanceId,
        [string]$Description
    )
    
    try {
        # Criar snapshot de todos os volumes
        $volumes = Get-EC2Volume | 
            Where-Object { $_.Attachments.InstanceId -eq $InstanceId }
        
        foreach ($volume in $volumes) {
            $snapshot = New-EC2Snapshot `
                -VolumeId $volume.VolumeId `
                -Description "$Description-$(Get-Date -Format 'yyyyMMdd')"
            
            Write-Output "Created snapshot: $($snapshot.SnapshotId)"
        }
    }
    catch {
        Write-Error "Backup failed: $_"
        throw
    }
}
```

### 2. Deployment Multi-Region
```powershell
function Deploy-MultiRegion {
    param(
        [string[]]$Regions,
        [string]$StackName,
        [string]$TemplatePath
    )
    
    foreach ($region in $Regions) {
        Set-DefaultAWSRegion -Region $region
        
        Write-Output "Deploying to $region..."
        New-CFNStack `
            -StackName "$StackName-$region" `
            -TemplateBody (Get-Content $TemplatePath -Raw) `
            -Parameter @(
                @{ ParameterKey="Region"; ParameterValue=$region }
            )
    }
}
```

## Boas Práticas

### 1. Segurança
```powershell
# Usar roles e políticas IAM
# Evitar hardcoding de credenciais
# Implementar princípio do menor privilégio
```

### 2. Error Handling
```powershell
try {
    $result = New-EC2Instance @launchParams
}
catch [Amazon.EC2.AmazonEC2Exception] {
    Write-Error "EC2 error: $_"
    throw
}
catch {
    Write-Error "Unexpected error: $_"
    throw
}
```

### 3. Logging
```powershell
# Configurar logging AWS
Set-AWSLogging -LogMetrics -LogResponses

# Logging local
Start-Transcript -Path ".\AWSDeployment.log"
```

## Próximos Passos

1. [Azure Integration](ps-azure-integration.md)
2. [DevOps Integration](ps-devops-integration.md)
3. [Security Best Practices](ps-security.md)

## Recursos Adicionais

- [AWS Tools for PowerShell Documentation](https://docs.aws.amazon.com/powershell/)
- [AWS CloudFormation](https://aws.amazon.com/cloudformation/)
- [AWS Systems Manager](https://aws.amazon