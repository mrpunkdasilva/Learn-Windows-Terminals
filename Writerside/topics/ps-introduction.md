# Introdução ao PowerShell

> "Bem-vindo ao PowerShell, onde tudo é um objeto e nada é apenas texto."

## História e Evolução

### Da Necessidade à Inovação
- 2006: Lançamento inicial (PowerShell 1.0)
- 2009: Windows PowerShell 2.0 (Remoting)
- 2012: Integração profunda com Windows
- 2016: PowerShell Core (Multiplataforma)
- 2020: PowerShell 7 (Unificação)

## Instalação e Configuração

### Windows 10/11
```powershell
# Verificar versão atual
$PSVersionTable.PSVersion

# Instalar última versão
winget install Microsoft.PowerShell
```

### Configuração Inicial
```powershell
# Criar perfil pessoal
if (!(Test-Path -Path $PROFILE)) {
    New-Item -ItemType File -Path $PROFILE -Force
}

# Configurar política de execução
Set-ExecutionPolicy RemoteSigned -Scope CurrentUser
```

## Conceitos Fundamentais

### 1. Cmdlets
```powershell
# Estrutura: Verbo-Substantivo
Get-Process    # Lista processos
Stop-Service   # Para serviço
New-Item      # Cria item

# Descobrir comandos
Get-Command -Verb Get
Get-Command -Noun Process
```

### 2. Pipeline
```powershell
# Encadeamento de comandos
Get-Process | 
    Where-Object { $_.CPU -gt 50 } |
    Sort-Object CPU -Descending |
    Select-Object Name, CPU, Memory
```

### 3. Objetos
```powershell
# Tudo é objeto
$processo = Get-Process chrome
$processo.Kill()  # Método
$processo.Name    # Propriedade

# Explorar objeto
$processo | Get-Member
```

## Primeiros Passos

### Hello, PowerShell!
```powershell
# Saída básica
Write-Host "Hello, PowerShell!" -ForegroundColor Green

# Variáveis
$nome = "Dev"
Write-Output "Olá, $nome!"

# Input do usuário
$resposta = Read-Host "Qual seu nome?"
```

### Navegação Básica
```powershell
# Sistema de arquivos
Set-Location C:\    # cd
Get-Location        # pwd
Get-ChildItem       # ls/dir
```

## Ajuda e Documentação

### Sistema de Ajuda
```powershell
# Atualizar documentação
Update-Help

# Obter ajuda
Get-Help Get-Process
Get-Help Get-Process -Examples
Get-Help Get-Process -Online
```

## Comparação com CMD

### Comandos Equivalentes
| CMD | PowerShell | Descrição |
|-----|------------|-----------|
| `dir` | `Get-ChildItem` | Lista arquivos |
| `cd` | `Set-Location` | Muda diretório |
| `copy` | `Copy-Item` | Copia arquivos |
| `del` | `Remove-Item` | Remove itens |

## Ambiente de Desenvolvimento

### Ferramentas Recomendadas
- Visual Studio Code + PowerShell Extension
- Windows Terminal
- PowerShell ISE (integrado)

### Extensões Úteis
```powershell
# Instalar módulos populares
Install-Module PSReadLine
Install-Module Terminal-Icons
Install-Module PSFzf
```

## Dicas Práticas

### Aliases e Atalhos
```powershell
# Aliases comuns
Set-Alias -Name k -Value kubectl
Set-Alias -Name g -Value git

# Ver aliases existentes
Get-Alias | Where-Object { $_.Definition -like "*git*" }
```

### Perfil Personalizado
```powershell
# Adicionar ao $PROFILE
function prompt {
    $location = Get-Location
    "PS $location> "
}

# Funções úteis
function which($cmd) {
    Get-Command $cmd | Select-Object -ExpandProperty Source
}
```

## Próximos Passos

1. [Cmdlets Básicos](ps-cmdlets.md)
   - Comandos essenciais
   - Parâmetros comuns
   - Exemplos práticos

2. [Pipeline e Objetos](ps-pipeline.md)
   - Manipulação de dados
   - Filtragem
   - Transformação

3. [Scripting Básico](ps-scripting.md)
   - Variáveis
   - Funções
   - Controle de fluxo

## Recursos Adicionais

- [Documentação Oficial](https://docs.microsoft.com/powershell)
- [GitHub PowerShell](https://github.com/PowerShell/PowerShell)
- [PowerShell Gallery](https://www.powershellgallery.com)

## Exercícios Práticos

1. Configure seu ambiente PowerShell
2. Explore comandos básicos
3. Crie seu primeiro script
4. Experimente com pipeline
5. Personalize seu perfil

---
_"O primeiro passo para a automação é entender suas ferramentas."_