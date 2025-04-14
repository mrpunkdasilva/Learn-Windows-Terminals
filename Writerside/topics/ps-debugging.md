# Debugging no PowerShell

> "Debug é como ser um detetive em um crime que você mesmo cometeu."

## Fundamentos de Debugging

### Breakpoints
```powershell
# Breakpoint em linha
Set-PSBreakpoint -Script .\script.ps1 -Line 10

# Breakpoint em comando
Set-PSBreakpoint -Command "Get-Process"

# Breakpoint em variável
Set-PSBreakpoint -Variable "errorCount" -Mode Write
```

## Debug Interativo

### Comandos de Debug
```powershell
# Iniciar sessão de debug
Write-Debug "Iniciando processamento..."
$DebugPreference = "Continue"

# Comandos no debugger
s   # Step into
v   # Step over
o   # Step out
c   # Continue
q   # Quit
k   # Stack trace
```

## Logging e Trace

### Técnicas de Logging
```powershell
# Verbose output
Write-Verbose "Processando item $i de $total"

# Warning para condições especiais
Write-Warning "Arquivo não encontrado: $file"

# Informação para usuário
Write-Information "Operação completada com sucesso"

# Trace de comandos
Trace-Command -Name ParameterBinding -Expression {
    Get-Process
} -PSHost
```

## Ferramentas de Debug

### ISE e VSCode
```powershell
# Debug no ISE
F9      # Toggle breakpoint
F5      # Continue
F10     # Step over
F11     # Step into

# Debug no VSCode
# Launch.json configuration
{
    "type": "PowerShell",
    "request": "launch",
    "name": "PowerShell Debug Script",
    "script": "${file}",
    "args": []
}
```

## Análise de Performance

### Medição e Profiling
```powershell
# Medir tempo de execução
Measure-Command {
    Get-Process | Sort-Object CPU -Descending
}

# Profile de memória
$before = [System.GC]::GetTotalMemory($true)
# ... código a ser analisado ...
$after = [System.GC]::GetTotalMemory($true)
$used = $after - $before
```

## Remote Debugging

### Debug em Sessões Remotas
```powershell
# Iniciar sessão remota com debug
Enter-PSSession -ComputerName "servidor01" -EnableNetworkAccess

# Debug remoto
Invoke-Command -ComputerName "servidor01" -ScriptBlock {
    Set-PSBreakpoint -Script C:\Scripts\Remote.ps1 -Line 5
}
```

## Troubleshooting Comum

### Padrões de Debug
```powershell
# Função helper para debug
function Debug-ScriptBlock {
    param(
        [Parameter(Mandatory)]
        [scriptblock]$Code
    )
    
    $ErrorActionPreference = 'Stop'
    $VerbosePreference = 'Continue'
    
    try {
        & $Code
    }
    catch {
        Write-Error "Erro: $_"
        Write-Debug "Stack trace: $($_.ScriptStackTrace)"
    }
}
```

## Boas Práticas

### Estratégias de Debug
```powershell
# Estrutura recomendada
function Do-Something {
    [CmdletBinding()]
    param()
    
    Write-Verbose "Iniciando operação"
    
    try {
        Write-Debug "Parâmetros validados"
        # código principal
        Write-Verbose "Operação concluída"
    }
    catch {
        Write-Error $_
        throw
    }
}
```

## Exercícios Práticos

1. Debug Básico
   ```powershell
   # Pratique:
   # - Configurar breakpoints
   # - Usar stepping
   # - Inspecionar variáveis
   ```

2. Debug Avançado
   ```powershell
   # Implemente:
   # - Debug remoto
   # - Análise de performance
   # - Logging estruturado
   ```

## Próximos Passos

1. [Testes Automatizados](ps-testing.md)
2. [Performance Tuning](ps-performance.md)
3. [Error Handling](ps-error-handling.md)

---
_"Todo bug é uma oportunidade de aprendizado... ou de questionar suas escolhas de carreira."_