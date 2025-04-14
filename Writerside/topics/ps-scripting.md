# Scripting no PowerShell

> "Scripts são a arte de automatizar o impossível."

## Fundamentos de Scripting

### Estrutura Básica
```powershell
# Cabeçalho do script
<#
.SYNOPSIS
    Descrição breve do script
.DESCRIPTION
    Descrição detalhada do script
.PARAMETER Nome
    Descrição do parâmetro
.EXAMPLE
    .\Script.ps1 -Nome "Exemplo"
#>
param(
    [string]$Nome,
    [int]$Idade = 30
)

# Configurações iniciais
Set-StrictMode -Version Latest
$ErrorActionPreference = 'Stop'
```

## Controle de Fluxo

### Condicionais
```powershell
# If-ElseIf-Else
if ($valor -gt 100) {
    "Valor alto"
} elseif ($valor -gt 50) {
    "Valor médio"
} else {
    "Valor baixo"
}

# Switch
switch ($opcao) {
    "A" { "Opção A selecionada" }
    "B" { "Opção B selecionada" }
    default { "Opção inválida" }
}
```

### Loops
```powershell
# ForEach
foreach ($item in $colecao) {
    $item.ProcessarAlgo()
}

# While
while ($condicao) {
    # Processamento
}

# Do-While
do {
    # Executa pelo menos uma vez
} while ($condicao)

# For
for ($i = 0; $i -lt 10; $i++) {
    # Loop tradicional
}
```

## Funções

### Funções Avançadas
```powershell
function Get-UserInfo {
    [CmdletBinding()]
    param(
        [Parameter(Mandatory, Position=0)]
        [string]$Username,
        
        [Parameter()]
        [switch]$Detailed
    )
    
    begin {
        Write-Verbose "Iniciando busca..."
    }
    
    process {
        # Lógica principal
    }
    
    end {
        Write-Verbose "Busca finalizada."
    }
}
```

### Parâmetros Avançados
```powershell
function Test-Parameters {
    param(
        [ValidateNotNullOrEmpty()]
        [string]$Nome,
        
        [ValidateRange(0,120)]
        [int]$Idade,
        
        [ValidateSet("A", "B", "C")]
        [string]$Opcao,
        
        [ValidateScript({Test-Path $_})]
        [string]$Arquivo
    )
}
```

## Tratamento de Erros

### Try-Catch
```powershell
try {
    # Código que pode gerar erro
    $resultado = 10 / $divisor
} catch [System.DivideByZeroException] {
    Write-Error "Divisão por zero!"
} catch {
    Write-Error $_.Exception.Message
} finally {
    # Sempre executa
    Write-Host "Operação finalizada"
}
```

### Logging
```powershell
function Write-Log {
    param(
        [string]$Message,
        [string]$Level = "INFO"
    )
    
    $logLine = "$(Get-Date -Format 'yyyy-MM-dd HH:mm:ss')|$Level|$Message"
    Add-Content -Path ".\app.log" -Value $logLine
}
```

## Módulos

### Criação de Módulos
```powershell
# Module Manifest
New-ModuleManifest -Path .\MyModule.psd1 `
    -ModuleVersion "1.0" `
    -Author "Seu Nome" `
    -Description "Descrição do módulo" `
    -FunctionsToExport @('Get-Something', 'Set-Something')

# Module Script
$functions = @(
    'Public\*.ps1'
    'Private\*.ps1'
)

foreach ($function in $functions) {
    . $PSScriptRoot\$function
}
```

## Boas Práticas

### Organização
```powershell
# Estrutura de diretórios
MyModule/
  ├── Public/
  │   └── Export-Function.ps1
  ├── Private/
  │   └── Helper-Function.ps1
  ├── Tests/
  │   └── Module.Tests.ps1
  ├── MyModule.psd1
  └── MyModule.psm1
```

### Documentação
```powershell
function Get-Something {
    <#
    .SYNOPSIS
        Breve descrição
    .DESCRIPTION
        Descrição detalhada
    .PARAMETER Name
        O que é este parâmetro
    .EXAMPLE
        Get-Something -Name "Test"
        Exemplo de uso
    .NOTES
        Informações adicionais
    #>
    param($Name)
}
```

## Testes

### Pester Tests
```powershell
Describe "Função Get-Something" {
    Context "Parâmetros válidos" {
        It "Retorna resultado esperado" {
            $result = Get-Something -Name "Test"
            $result | Should -Not -BeNullOrEmpty
        }
        
        It "Gera erro com input inválido" {
            { Get-Something -Name "" } | 
                Should -Throw
        }
    }
}
```

## Debugging

### Técnicas de Debug
```powershell
# Breakpoints
Set-PSBreakpoint -Script .\script.ps1 -Line 10
Set-PSBreakpoint -Variable $varName -Mode Write

# Debug output
Write-Debug "Valor: $valor"
Write-Verbose "Processando item: $item"
```

## Exercícios Práticos

1. Script de Automação
   ```powershell
   # Crie um script que:
   # - Aceita parâmetros
   # - Valida input
   # - Trata erros
   # - Gera logs
   # - Produz output formatado
   ```

2. Módulo Utilitário
   ```powershell
   # Desenvolva um módulo com:
   # - Funções públicas e privadas
   # - Documentação completa
   # - Testes unitários
   # - Manifesto
   ```

3. Script de Monitoramento
   ```powershell
   # Implemente monitoramento que:
   # - Coleta métricas do sistema
   # - Processa dados
   # - Gera alertas
   # - Executa ações corretivas
   ```

## Próximos Passos

1. [PowerShell Avançado](ps-advanced.md)
2. [Módulos e Extensões](ps-modules.md)
3. [Integração DevOps](ps-devops-integration.md)

---
_"Scripts são a ponte entre ideias e automação."_