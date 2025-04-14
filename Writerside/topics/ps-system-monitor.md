# System Monitor

> "Monitor de sistema em tempo real com PowerShell"

## Visão Geral
Sistema de monitoramento completo que coleta, analisa e apresenta métricas do sistema em tempo real, com capacidade de alertas e relatórios.

## Recursos
- Dashboard em tempo real
- Monitoramento de CPU, memória e disco
- Alertas configuráveis
- Logging e histórico
- Relatórios automatizados
- Interface web básica

## Estrutura do Projeto
```powershell
SystemMonitor/
  ├── src/
  │   ├── Modules/
  │   │   ├── Monitoring.psm1
  │   │   ├── Alerting.psm1
  │   │   ├── Reporting.psm1
  │   │   └── WebInterface.psm1
  │   ├── Config/
  │   │   ├── settings.json
  │   │   └── thresholds.json
  │   └── Scripts/
  │       ├── Start-Monitor.ps1
  │       └── Generate-Report.ps1
  ├── tests/
  │   └── Unit/
  └── web/
      ├── index.html
      └── styles.css
```

## Implementação Principal

### Módulo de Monitoramento
```powershell
function Get-SystemMetrics {
    [CmdletBinding()]
    param(
        [int]$SampleInterval = 5,
        [string]$ComputerName = $env:COMPUTERNAME
    )
    
    process {
        $cpu = Get-Counter '\Processor(_Total)\% Processor Time'
        $memory = Get-Counter '\Memory\% Committed Bytes In Use'
        $disk = Get-Counter '\LogicalDisk(_Total)\% Free Space'
        
        [PSCustomObject]@{
            Timestamp = Get-Date
            CPU = $cpu.CounterSamples.CookedValue
            Memory = $memory.CounterSamples.CookedValue
            DiskSpace = $disk.CounterSamples.CookedValue
            ComputerName = $ComputerName
        }
    }
}

function Start-SystemMonitor {
    [CmdletBinding()]
    param(
        [int]$RefreshInterval = 5,
        [string]$LogPath = ".\logs\system.log"
    )
    
    begin {
        Write-Verbose "Iniciando monitoramento..."
        $null = New-Item -Path $LogPath -ItemType File -Force
    }
    
    process {
        while ($true) {
            $metrics = Get-SystemMetrics
            
            # Log métricas
            $metrics | Export-Csv -Path $LogPath -Append -NoTypeInformation
            
            # Verificar alertas
            Test-MetricThresholds -Metrics $metrics
            
            # Atualizar dashboard
            Update-Dashboard -Data $metrics
            
            Start-Sleep -Seconds $RefreshInterval
        }
    }
}
```

### Sistema de Alertas
```powershell
function Test-MetricThresholds {
    [CmdletBinding()]
    param(
        [Parameter(Mandatory)]
        [PSCustomObject]$Metrics,
        
        [int]$CpuThreshold = 90,
        [int]$MemoryThreshold = 85,
        [int]$DiskThreshold = 10
    )
    
    if ($Metrics.CPU -gt $CpuThreshold) {
        Send-Alert -Type "CPU" -Value $Metrics.CPU -Threshold $CpuThreshold
    }
    
    if ($Metrics.Memory -gt $MemoryThreshold) {
        Send-Alert -Type "Memory" -Value $Metrics.Memory -Threshold $MemoryThreshold
    }
    
    if ($Metrics.DiskSpace -lt $DiskThreshold) {
        Send-Alert -Type "Disk" -Value $Metrics.DiskSpace -Threshold $DiskThreshold
    }
}
```

### Geração de Relatórios
```powershell
function New-SystemReport {
    [CmdletBinding()]
    param(
        [DateTime]$StartDate,
        [DateTime]$EndDate,
        [string]$ReportPath = ".\reports\"
    )
    
    process {
        $data = Import-Csv ".\logs\system.log" | Where-Object {
            $_.Timestamp -ge $StartDate -and $_.Timestamp -le $EndDate
        }
        
        $report = @{
            Period = "$StartDate to $EndDate"
            Metrics = @{
                CPU = @{
                    Average = ($data.CPU | Measure-Object -Average).Average
                    Max = ($data.CPU | Measure-Object -Maximum).Maximum
                }
                Memory = @{
                    Average = ($data.Memory | Measure-Object -Average).Average
                    Max = ($data.Memory | Measure-Object -Maximum).Maximum
                }
                DiskSpace = @{
                    Current = $data[-1].DiskSpace
                    Trend = ($data.DiskSpace | Measure-Object -Average).Average
                }
            }
            Alerts = Get-AlertHistory -StartDate $StartDate -EndDate $EndDate
        }
        
        $report | ConvertTo-Html | Out-File "$ReportPath\report_$(Get-Date -f 'yyyyMMdd').html"
    }
}
```

## Uso

### Instalação
```powershell
# Clone o repositório
git clone https://github.com/user/SystemMonitor.git

# Instale dependências
Install-Module -Name PSFramework -Force
```

### Configuração
```powershell
# Configure thresholds
$config = @{
    CPU = 90
    Memory = 85
    Disk = 10
    RefreshInterval = 5
    AlertRecipients = @("admin@domain.com")
}

$config | ConvertTo-Json | Out-File ".\config\settings.json"
```

### Execução
```powershell
# Inicie o monitor
.\Scripts\Start-Monitor.ps1

# Gere relatório
.\Scripts\Generate-Report.ps1 -Days 7
```

## Testes

### Testes Unitários
```powershell
Describe "System Monitor" {
    Context "Metric Collection" {
        It "Returns valid metrics" {
            $metrics = Get-SystemMetrics
            $metrics | Should -Not -BeNullOrEmpty
            $metrics.CPU | Should -BeGreaterOrEqual 0
            $metrics.Memory | Should -BeGreaterOrEqual 0
        }
    }
}
```

## Próximos Passos
1. Implementar interface web avançada
2. Adicionar suporte a múltiplos servidores
3. Integrar com sistemas de monitoramento externos
4. Implementar machine learning para detecção de anomalias

---
_"Monitoramento eficiente é a chave para sistemas confiáveis."_