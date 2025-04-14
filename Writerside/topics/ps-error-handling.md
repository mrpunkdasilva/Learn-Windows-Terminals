# Tratamento de Erros no PowerShell

> "Erros são inevitáveis, tratá-los adequadamente é essencial."

## Fundamentos

### Tipos de Erros
- Erros de terminação (Terminating)
- Erros de não-terminação (Non-terminating)
- Exceções gerenciadas (.NET)

### ErrorActionPreference
```powershell
# Configurações globais
$ErrorActionPreference = 'Stop'        # Para em qualquer erro
$ErrorActionPreference = 'Continue'    # Continua após erro
$ErrorActionPreference = 'SilentlyContinue' # Ignora erros
$ErrorActionPreference = 'Inquire'     # Pergunta o que fazer
```

## Try-Catch-Finally

### Estrutura Básica
```powershell
try {
    # Código que pode gerar erro
    Get-Process -Name "NonExistentProcess"
} catch {
    # Tratamento do erro
    Write-Error "Processo não encontrado: $_"
} finally {
    # Sempre executa
    Write-Host "Operação finalizada"
}
```

### Tipos Específicos
```powershell
try {
    throw [System.IO.FileNotFoundException]::new(
        "Arquivo não encontrado"
    )
} catch [System.IO.FileNotFoundException] {
    Write-Error "Arquivo não existe"
} catch [System.Exception] {
    Write-Error "Erro genérico: $_"
}
```

## Logging

### Sistema de Log
```powershell
function Write-Log {
    [CmdletBinding()]
    param(
        [Parameter(Mandatory)]
        [string]$Message,
        
        [ValidateSet('INFO','WARN','ERROR')]
        [string]$Level = 'INFO',
        
        [string]$LogPath = ".\app.log"
    )
    
    $timestamp = Get-Date -Format "yyyy-MM-dd HH:mm:ss"
    $logLine = "$timestamp [$Level] $Message"
    Add-Content -Path $LogPath -Value $logLine
    
    if ($Level -eq 'ERROR') {
        Write-Error $Message
    }
}
```

## Debug e Troubleshooting

### Ferramentas de Debug
```powershell
# Break points
Set-PSBreakpoint -Script .\script.ps1 -Line 10
Set-PSBreakpoint -Command Get-Process

# Trace
Trace-Command -Name ParameterBinding -Expression {
    Get-Process
} -PSHost
```

### Verbose Output
```powershell
function Get-SystemInfo {
    [CmdletBinding()]
    param()
    
    Write-Verbose "Coletando informações..."
    Get-ComputerInfo
    Write-Verbose "Coleta finalizada"
}

Get-SystemInfo -Verbose
```

## Boas Práticas

### Validação de Entrada
```powershell
function Set-UserProfile {
    [CmdletBinding()]
    param(
        [Parameter(Mandatory)]
        [ValidateNotNullOrEmpty()]
        [string]$Username,
        
        [ValidateRange(0,120)]
        [int]$Age,
        
        [ValidateScript({Test-Path $_})]
        [string]$ProfilePath
    )
}
```

### Tratamento Robusto
```powershell
function Invoke-RiskyOperation {
    [CmdletBinding()]
    param($Path)
    
    begin {
        Write-Log "Iniciando operação" -Level INFO
    }
    
    process {
        try {
            # Operação principal
            Write-Verbose "Processando $Path"
            
            if (-not (Test-Path $Path)) {
                throw "Caminho não existe: $Path"
            }
            
            # Mais operações...
            
        } catch {
            Write-Log $_.Exception.Message -Level ERROR
            throw
        }
    }
    
    end {
        Write-Log "Operação finalizada" -Level INFO
    }
}
```

## Exercícios Práticos

1. Sistema de Log
   ```powershell
   # Implemente um sistema de log que:
   # - Registra em arquivo
   # - Suporta níveis diferentes
   # - Inclui stack trace
   ```

2. Tratamento Avançado
   ```powershell
   # Crie uma função que:
   # - Valida entradas
   # - Trata múltiplos erros
   # - Usa logging
   # - Limpa recursos
   ```

## Próximos Passos

1. [Advanced PowerShell](ps-advanced.md)
2. [Debugging](ps-debugging.md)
3. [Best Practices](best-practices.md)

---
_"Erros bem tratados são oportunidades de melhoria."_