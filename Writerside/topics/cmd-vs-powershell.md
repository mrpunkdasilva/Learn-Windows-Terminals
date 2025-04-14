# CMD vs PowerShell: Duelo de Terminais

> "Tradição encontra modernidade no mundo dos terminais Windows."

## Comparação Rápida

| Aspecto | CMD | PowerShell |
|---------|-----|------------|
| Lançamento | 1981 (MS-DOS) | 2006 |
| Base | Texto | Objetos .NET |
| Scripting | Batch (.bat/.cmd) | PowerShell (.ps1) |
| Case Sensitivity | Não | Sim (parcial) |
| Pipeline | Texto | Objetos |

## Pontos Fortes

### CMD
```batch
:: Vantagens CMD
- Simplicidade
- Baixo consumo
- Compatibilidade legada
- Scripts batch simples
- Presente em todo Windows
```

### PowerShell
```powershell
# Vantagens PowerShell
- Orientado a objetos
- Mais poderoso
- Integração .NET
- Remoting nativo
- Módulos extensíveis
```

## Casos de Uso

### Quando Usar CMD
- Scripts legados
- Tarefas simples
- Sistemas limitados
- Compatibilidade máxima
- Performance crítica

### Quando Usar PowerShell
- Automação complexa
- Administração Windows
- DevOps/Cloud
- Scripting avançado
- Integração sistemas

## Exemplos Comparativos

### Listagem de Processos
```batch
:: CMD
tasklist

# PowerShell
Get-Process | Select-Object Name, CPU, Memory
```

### Manipulação de Arquivos
```batch
:: CMD
dir /s /b *.txt > files.txt

# PowerShell
Get-ChildItem -Recurse -Filter *.txt | Select-Object FullName | Export-Csv files.csv
```

## Migração CMD para PowerShell

### Equivalências Comuns
| CMD | PowerShell |
|-----|------------|
| `dir` | `Get-ChildItem` |
| `copy` | `Copy-Item` |
| `del` | `Remove-Item` |
| `type` | `Get-Content` |

---
_"CMD é como um canivete: simples e confiável. PowerShell é como um canivete suíço: versátil e poderoso."_