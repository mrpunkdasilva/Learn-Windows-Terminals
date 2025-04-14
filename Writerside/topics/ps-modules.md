# Módulos PowerShell

> "Módulos são a base da reutilização e distribuição de código no PowerShell."

## Fundamentos de Módulos

### Estrutura Básica
```powershell
# Estrutura recomendada de módulo
MyModule/
  ├── Public/          # Funções exportadas
  ├── Private/         # Funções internas
  ├── Tests/           # Testes unitários
  ├── MyModule.psd1    # Manifesto
  └── MyModule.psm1    # Script do módulo
```

## Criação de Módulos

### Manifesto do Módulo
```powershell
New-ModuleManifest -Path .\MyModule.psd1 `
    -ModuleVersion "1.0.0" `
    -Author "Seu Nome" `
    -Description "Descrição do módulo" `
    -FunctionsToExport @('Get-Something', 'Set-Something')
```

### Script do Módulo
```powershell
# MyModule.psm1
$Public = @( Get-ChildItem -Path $PSScriptRoot\Public\*.ps1 -Recurse )
$Private = @( Get-ChildItem -Path $PSScriptRoot\Private\*.ps1 -Recurse )

foreach($import in @($Public + $Private)) {
    try {
        . $import.FullName
    } catch {
        Write-Error "Falha ao importar função $($import.FullName): $_"
    }
}

Export-ModuleMember -Function $Public.BaseName
```

## Gerenciamento de Módulos

### Comandos Essenciais
```powershell
# Listar módulos
Get-Module -ListAvailable
Get-Module # módulos carregados

# Instalar e atualizar
Install-Module -Name PSReadLine
Update-Module -Name PSReadLine

# Importar e remover
Import-Module MyModule
Remove-Module MyModule
```

## Publicação

### PowerShell Gallery
```powershell
# Publicar módulo
Publish-Module -Name MyModule -NuGetApiKey $key

# Configurar repositório
Register-PSRepository -Name LocalRepo `
    -SourceLocation "\\server\share" `
    -InstallationPolicy Trusted
```

## Boas Práticas

### Organização
- Separe funções públicas e privadas
- Use versionamento semântico
- Documente todas as funções
- Implemente testes unitários
- Mantenha compatibilidade

### Documentação
```powershell
function Get-Something {
    <#
    .SYNOPSIS
        Breve descrição
    .DESCRIPTION
        Descrição detalhada
    .PARAMETER Name
        Descrição do parâmetro
    .EXAMPLE
        Get-Something -Name "Test"
    #>
    param($Name)
}
```

## Exercícios Práticos

1. Módulo Básico
   ```powershell
   # Crie um módulo simples com:
   # - 2 funções públicas
   # - 1 função privada
   # - Manifesto completo
   ```

2. Módulo Avançado
   ```powershell
   # Desenvolva um módulo com:
   # - Pipeline support
   # - Error handling
   # - Logging
   # - Testes Pester
   ```

## Próximos Passos

1. [Error Handling](ps-error-handling.md)
2. [Advanced PowerShell](ps-advanced.md)
3. [DevOps Integration](ps-devops-integration.md)

---
_"Bons módulos são a fundação de código sustentável."_