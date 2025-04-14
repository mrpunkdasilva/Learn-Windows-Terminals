# Objetos no PowerShell

> "No PowerShell, tudo é um objeto - essa é a chave para sua potência."

## Fundamentos de Objetos

### O que são Objetos?
```powershell
# Objetos têm propriedades e métodos
$processo = Get-Process chrome
$processo.Name        # Propriedade
$processo.Kill()      # Método

# Tipos de objetos
$processo.GetType()
[System.Diagnostics.Process]
```

### Explorando Objetos
```powershell
# Get-Member
Get-Process | Get-Member
Get-Service | Get-Member -MemberType Property
Get-ChildItem | Get-Member -MemberType Method

# Tipos específicos
$data = Get-Date
$data | Get-Member
```

## Propriedades e Métodos

### Trabalhando com Propriedades
```powershell
# Acessando propriedades
$arquivo = Get-Item ".\exemplo.txt"
$arquivo.Name
$arquivo.Length
$arquivo.LastWriteTime

# Propriedades calculadas
$arquivo | Select-Object Name,
    @{N='SizeKB';E={$_.Length/1KB}},
    @{N='Age';E={(Get-Date) - $_.CreationTime}}
```

### Usando Métodos
```powershell
# Métodos comuns
$texto = "PowerShell"
$texto.ToUpper()
$texto.Replace("Power", "Super")

# Métodos com parâmetros
$data = Get-Date
$data.AddDays(7)
$data.ToString("yyyy-MM-dd")
```

## Criando Objetos

### Objetos Personalizados
```powershell
# PSCustomObject
$usuario = [PSCustomObject]@{
    Nome = "João"
    Idade = 30
    Ativo = $true
}

# Adicionando propriedades
$usuario | Add-Member -MemberType NoteProperty -Name "Email" -Value "joao@email.com"
```

### Classes Personalizadas
```powershell
# Definindo uma classe
class Pessoa {
    [string]$Nome
    [int]$Idade
    
    Pessoa([string]$nome, [int]$idade) {
        $this.Nome = $nome
        $this.Idade = $idade
    }
    
    [string]ToString() {
        return "$($this.Nome) ($($this.Idade) anos)"
    }
}

# Usando a classe
$pessoa = [Pessoa]::new("Maria", 25)
```

## Manipulação de Objetos

### Filtragem e Seleção
```powershell
# Where-Object
Get-Process | 
    Where-Object {$_.CPU -gt 10} |
    Select-Object Name, CPU, WorkingSet

# Select-Object avançado
Get-Service |
    Select-Object Name, Status,
        @{N='RunTime';E={(Get-Date) - $_.StartTime}}
```

### Transformação
```powershell
# ForEach-Object
Get-Process |
    ForEach-Object {
        [PSCustomObject]@{
            Nome = $_.Name
            MemoriaMB = [math]::Round($_.WorkingSet/1MB, 2)
            CPU = $_.CPU
        }
    }
```

## Coleções de Objetos

### Arrays
```powershell
# Array de objetos
$processos = @(Get-Process)
$processos[0]
$processos.Count

# Filtragem de array
$processos.Where({$_.CPU -gt 10})
```

### ArrayList
```powershell
# Criando ArrayList
$lista = New-Object System.Collections.ArrayList
$lista.Add([PSCustomObject]@{
    Nome = "Item 1"
    Valor = 100
})

# Manipulando
$lista.RemoveAt(0)
$lista.Clear()
```

### Hashtables
```powershell
# Hashtable como objeto
$config = @{
    Servidor = "localhost"
    Porta = 8080
    SSL = $true
}

# Convertendo para PSObject
$configObj = [PSCustomObject]$config
```

## Tipos Avançados

### Tipos Genéricos
```powershell
# List<T>
$lista = New-Object System.Collections.Generic.List[string]
$lista.Add("PowerShell")
$lista.AddRange(@("Objetos", "Pipeline"))

# Dictionary<K,V>
$dict = New-Object 'System.Collections.Generic.Dictionary[string,int]'
$dict.Add("Um", 1)
$dict.Add("Dois", 2)
```

### Conversão de Tipos
```powershell
# Conversão explícita
[int]"42"
[datetime]"2024-01-01"
[bool]1

# Conversão de objetos
$json = $objeto | ConvertTo-Json
$xml = $objeto | ConvertTo-Xml
```

## Boas Práticas

### Tipagem Forte
```powershell
# Declaração com tipo
[string]$nome = "PowerShell"
[int]$idade = 30
[datetime]$data = Get-Date

# Parâmetros tipados
function Get-Usuario {
    param(
        [string]$nome,
        [int]$idade
    )
    # ...
}
```

### Validação
```powershell
# Validação de propriedades
class Usuario {
    [ValidateNotNullOrEmpty()]
    [string]$Nome

    [ValidateRange(0,120)]
    [int]$Idade

    [ValidatePattern("^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$")]
    [string]$Email
}
```

## Exercícios Práticos

1. Criação de Objetos
   ```powershell
   # Crie uma classe Livro
   class Livro {
       [string]$Titulo
       [string]$Autor
       [int]$Ano
       [double]$Preco

       [string]ToString() {
           return "$($this.Titulo) por $($this.Autor)"
       }
   }
   ```

2. Manipulação de Coleções
   ```powershell
   # Crie uma biblioteca
   $biblioteca = [System.Collections.Generic.List[Livro]]::new()
   $biblioteca.Add([Livro]@{
       Titulo = "PowerShell in Action"
       Autor = "Bruce Payette"
       Ano = 2017
       Preco = 59.99
   })
   ```

3. Transformação de Dados
   ```powershell
   # Relatório de livros
   $biblioteca |
       Select-Object Titulo, Autor,
           @{N='PrecoFormatado';E={"R$ " + $_.Preco}},
           @{N='Idade';E={(Get-Date).Year - $_.Ano}}
   ```

## Próximos Passos

1. [Pipeline Avançado](ps-pipeline.md)
2. [Scripting com Objetos](ps-scripting.md)
3. [Módulos e Classes](ps-modules.md)

---
_"Objetos são a linguagem do PowerShell - aprenda a falar fluentemente."_