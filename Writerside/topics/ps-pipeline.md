# Pipeline: O Poder do Encadeamento

> "O pipeline é o que torna o PowerShell verdadeiramente poderoso."

## Conceitos Fundamentais

### O que é Pipeline?
```powershell
# Estrutura básica
Comando1 | Comando2 | Comando3

# Exemplo prático
Get-Process | Sort-Object CPU -Descending | Select-Object -First 5
```

### Fluxo de Objetos
```powershell
# Cada comando passa objetos, não texto
Get-Service |
    Where-Object Status -eq 'Running' |
    Select-Object Name, Status

# Verificar tipo de objeto
Get-Process | Get-Member
```

## Operadores de Pipeline

### Pipeline Básico
```powershell
# Operador |
Get-ChildItem | Sort-Object Length
Get-Process | Export-Csv processos.csv
Dir | Where-Object Length -gt 1MB
```

### Pipeline com Atribuição
```powershell
# Operador |>
$processos = Get-Process |
    Sort-Object CPU -Descending |
    Select-Object -First 10
```

## Cmdlets de Pipeline

### Filtragem
```powershell
# Where-Object
Get-Service | Where-Object {$_.Status -eq 'Running'}
Get-Process | Where-Object CPU -gt 10

# Select-Object
Get-Process |
    Select-Object Name, CPU, WorkingSet |
    Where-Object CPU -gt 10
```

### Ordenação
```powershell
# Sort-Object
Get-ChildItem |
    Sort-Object Length -Descending

# Group-Object
Get-Process |
    Group-Object Company |
    Sort-Object Count -Descending
```

### Transformação
```powershell
# ForEach-Object
Get-Process |
    ForEach-Object {
        $_.Name + " uses " + [math]::Round($_.CPU,2) + "% CPU"
    }

# Select-Object com expressões
Get-Process |
    Select-Object Name,
        @{N='Memory(MB)';E={$_.WorkingSet/1MB}},
        @{N='CPU%';E={$_.CPU}}
```

## Técnicas Avançadas

### Pipeline com Funções
```powershell
# Função que aceita pipeline
function Convert-ToMB {
    param(
        [Parameter(ValueFromPipeline=$true)]
        $bytes
    )
    process {
        $mb = $bytes/1MB
        [math]::Round($mb, 2)
    }
}

# Uso
Get-ChildItem |
    Select-Object Name,
        @{N='Size(MB)';E={$_.Length | Convert-ToMB}}
```

### Processamento em Lote
```powershell
# Begin, Process, End
function Process-Items {
    begin {
        $total = 0
    }
    process {
        $total += $_.Length
    }
    end {
        "Total: $($total/1MB) MB"
    }
}

Get-ChildItem | Process-Items
```

## Otimização

### Performance
```powershell
# Evitar pipeline desnecessário
# Ruim
Get-Process | Where-Object {$_.Name -eq "chrome"}

# Melhor
Get-Process -Name "chrome"

# Pipeline eficiente
Get-Process |
    Where-Object CPU -gt 10 |
    Sort-Object CPU -Descending |
    Select-Object -First 5
```

### Memória
```powershell
# Streaming vs Coleção
# Alto uso de memória
$todos = Get-ChildItem -Recurse
$todos | Where-Object {$_.Length -gt 1MB}

# Streaming eficiente
Get-ChildItem -Recurse |
    Where-Object {$_.Length -gt 1MB}
```

## Depuração

### Debug de Pipeline
```powershell
# Tee-Object para debug
Get-Process |
    Tee-Object -Variable processos |
    Where-Object CPU -gt 10

# Verbose output
Get-Process -Verbose |
    Where-Object {$_.CPU -gt 10} -Verbose
```

### Tratamento de Erros
```powershell
# ErrorAction no pipeline
Get-Process |
    Stop-Process -WhatIf -ErrorAction SilentlyContinue

# Try/Catch com pipeline
try {
    Get-Process |
        Where-Object {$_.CPU -gt 10} |
        Stop-Process -ErrorAction Stop
} catch {
    Write-Warning "Erro ao parar processos: $_"
}
```

## Exercícios Práticos

1. Manipulação de Arquivos
   ```powershell
   # Liste arquivos grandes
   Get-ChildItem -Recurse |
       Where-Object Length -gt 100MB |
       Select-Object FullName, @{N='Size(MB)';E={$_.Length/1MB}}
   ```

2. Análise de Processos
   ```powershell
   # Top consumidores de memória
   Get-Process |
       Sort-Object WorkingSet -Descending |
       Select-Object -First 10 |
       Format-Table Name,
           @{N='Memory(MB)';E={[math]::Round($_.WorkingSet/1MB,2)}},
           CPU
   ```

3. Relatório de Serviços
   ```powershell
   # Status dos serviços
   Get-Service |
       Group-Object Status |
       Select-Object Name, Count |
       Sort-Object Count -Descending
   ```

## Próximos Passos

1. [Objetos PowerShell](ps-objects.md)
2. [Scripting Avançado](ps-scripting.md)
3. [Módulos e Funções](ps-modules.md)

---
_"O pipeline é como LEGO: pequenas peças se conectam para criar algo maior."_