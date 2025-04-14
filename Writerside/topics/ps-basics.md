# Conceitos Básicos do PowerShell

> "Entenda os fundamentos, domine o PowerShell."

## Sintaxe Básica

### Estrutura de Comandos
```powershell
# Formato básico: Verbo-Substantivo
Get-Process
Set-Location
New-Item

# Com parâmetros
Get-Process -Name chrome
New-Item -Path "C:\temp" -ItemType Directory
```

### Variáveis
```powershell
# Declaração e uso
$nome = "PowerShell"
$idade = 30
$lista = @(1, 2, 3)
$hash = @{
    Chave = "Valor"
    Numero = 42
}

# Tipagem
[string]$texto = "123"
[int]$numero = 456
[datetime]$data = "2024-01-01"
```

## Tipos de Dados

### Tipos Básicos
```powershell
# Strings
$texto = "PowerShell"
$texto.ToUpper()
$texto.Length

# Números
$inteiro = 42
$decimal = 3.14
$resultado = $inteiro + $decimal

# Booleanos
$verdadeiro = $true
$falso = $false
```

### Arrays e Coleções
```powershell
# Arrays
$frutas = @("maçã", "banana", "laranja")
$frutas[0]
$frutas.Count

# ArrayList
$lista = New-Object System.Collections.ArrayList
$lista.Add("item")
$lista.Remove("item")

# HashTable
$config = @{
    Servidor = "localhost"
    Porta = 8080
    Ativo = $true
}
```

## Operadores

### Comparação
```powershell
# Operadores básicos
-eq  # Igual
-ne  # Diferente
-gt  # Maior que
-lt  # Menor que
-ge  # Maior ou igual
-le  # Menor ou igual

# Exemplos
$idade -gt 18
"texto" -eq "TEXTO"  # Case-insensitive por padrão
```

### Lógicos
```powershell
# AND e OR
$true -and $false
$true -or $false

# NOT
-not $true
!$false

# Combinados
($idade -gt 18) -and ($nome -eq "Admin")
```

## Estruturas de Controle

### Condicionais
```powershell
# If-ElseIf-Else
if ($idade -lt 18) {
    "Menor de idade"
} elseif ($idade -eq 18) {
    "Acabou de completar 18"
} else {
    "Maior de idade"
}

# Switch
switch ($cor) {
    "vermelho" { "Pare" }
    "amarelo" { "Atenção" }
    "verde" { "Siga" }
    default { "Cor inválida" }
}
```

### Loops
```powershell
# ForEach
foreach ($item in $lista) {
    Write-Output $item
}

# For
for ($i = 0; $i -lt 10; $i++) {
    Write-Output $i
}

# While
while ($true) {
    "Loop infinito"
    break
}

# Do-While
do {
    "Executa pelo menos uma vez"
} while ($false)
```

## Entrada e Saída

### Input
```powershell
# Leitura básica
$nome = Read-Host "Digite seu nome"

# Leitura segura de senha
$senha = Read-Host "Digite sua senha" -AsSecureString
```

### Output
```powershell
# Diferentes formas de saída
Write-Output "Saída normal"
Write-Host "Saída colorida" -ForegroundColor Green
Write-Warning "Aviso importante"
Write-Error "Erro crítico"
```

## Manipulação de Texto

### Strings
```powershell
# Concatenação
$primeiro = "Power"
$segundo = "Shell"
$completo = $primeiro + $segundo

# Formatação
$nome = "Dev"
$mensagem = "Olá, {0}!" -f $nome

# Substituição
$texto = "Hello World"
$texto.Replace("Hello", "Hi")
```

## Boas Práticas

### Nomenclatura
- Use PascalCase para funções
- Use camelCase para variáveis
- Prefixe variáveis privadas com _
- Use nomes descritivos

### Organização
```powershell
# Estrutura de script
# 1. Parâmetros
param(
    [string]$Nome,
    [int]$Idade
)

# 2. Funções
function Get-UserInfo {
    param($Nome)
    # ...
}

# 3. Lógica principal
```

## Exercícios Práticos

1. Crie um script que:
   - Receba input do usuário
   - Use diferentes tipos de dados
   - Implemente estruturas de controle
   - Produza output formatado

2. Manipule coleções:
   - Crie e modifique arrays
   - Trabalhe com hashtables
   - Use loops para processamento

3. Pratique com strings:
   - Concatenação
   - Formatação
   - Manipulação

## Próximos Passos

1. [Cmdlets em Detalhes](ps-cmdlets.md)
2. [Pipeline Avançado](ps-pipeline.md)
3. [Objetos e Classes](ps-objects.md)

---
_"A base sólida é o segredo do sucesso."_