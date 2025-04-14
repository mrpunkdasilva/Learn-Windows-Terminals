# Cmdlets: Os Comandos do PowerShell

> "Cmdlets são as ferramentas que fazem o PowerShell poderoso."

## Estrutura dos Cmdlets

### Anatomia
```powershell
# Formato: Verbo-Substantivo
Get-Process
Set-Location
New-Item

# Parâmetros comuns
-WhatIf      # Simulação
-Confirm     # Confirmação
-Verbose     # Saída detalhada
-Debug       # Informações de debug
```

### Verbos Comuns
| Verbo | Uso | Exemplo |
|-------|-----|---------|
| `Get` | Obtém recursos | `Get-Service` |
| `Set` | Modifica recursos | `Set-Content` |
| `New` | Cria recursos | `New-Item` |
| `Remove` | Remove recursos | `Remove-Item` |
| `Start` | Inicia operação | `Start-Process` |
| `Stop` | Para operação | `Stop-Service` |

## Cmdlets Essenciais

### Sistema de Arquivos
```powershell
# Navegação
Set-Location C:\      # Muda diretório
Get-Location          # Mostra diretório atual
Push-Location        # Salva localização atual
Pop-Location         # Retorna à localização salva

# Manipulação de arquivos
Get-ChildItem        # Lista conteúdo
New-Item -Type File  # Cria arquivo
Copy-Item           # Copia
Move-Item           # Move
Remove-Item         # Remove
```

### Processos e Serviços
```powershell
# Processos
Get-Process         # Lista processos
Start-Process      # Inicia processo
Stop-Process       # Para processo
Wait-Process       # Aguarda processo

# Serviços
Get-Service        # Lista serviços
Start-Service     # Inicia serviço
Stop-Service      # Para serviço
Restart-Service   # Reinicia serviço
```

### Rede
```powershell
# Conectividade
Test-Connection    # Ping avançado
Test-NetConnection # Teste TCP
Get-NetAdapter     # Info adaptadores
Get-NetIPAddress   # Endereços IP

# Web
Invoke-WebRequest  # Requisição web
Invoke-RestMethod  # API REST
```

### Sistema
```powershell
# Informações
Get-ComputerInfo   # Info do sistema
Get-Disk           # Info de discos
Get-EventLog       # Logs do sistema
Get-HotFix         # Updates instalados

# Gerenciamento
Restart-Computer   # Reinicia
Stop-Computer      # Desliga
Clear-Host         # Limpa tela
```

## Parâmetros Avançados

### Parâmetros Comuns
```powershell
# Controle de execução
-WhatIf            # Simula execução
-Confirm           # Pede confirmação
-Verbose           # Saída detalhada
-Debug             # Modo debug
-ErrorAction       # Tratamento de erro
```

### Pipeline Input
```powershell
# Aceita input do pipeline
Get-Process | Stop-Process
Get-ChildItem *.txt | Remove-Item

# Filtragem
Get-Service | Where-Object Status -eq 'Running'
Get-Process | Sort-Object CPU -Descending
```

## Descoberta e Ajuda

### Encontrar Cmdlets
```powershell
# Busca por verbo
Get-Command -Verb Get
Get-Command -Verb Set

# Busca por substantivo
Get-Command -Noun Process
Get-Command -Noun Item

# Busca por módulo
Get-Command -Module Microsoft.PowerShell.Security
```

### Sistema de Ajuda
```powershell
# Ajuda básica
Get-Help Get-Process
Get-Help Get-Service -Detailed
Get-Help New-Item -Full
Get-Help About_Variables

# Exemplos
Get-Help Get-Process -Examples
```

## Módulos e Extensões

### Gerenciamento de Módulos
```powershell
# Listar e instalar
Get-Module -ListAvailable
Install-Module -Name PSReadLine
Update-Module -Name PSReadLine
Remove-Module -Name PSReadLine

# Importar e exportar
Import-Module Az
Export-ModuleMember -Function Get-Something
```

## Boas Práticas

### Segurança
```powershell
# Verificar antes de executar
Get-Process -Name "notepad" | Stop-Process -WhatIf

# Confirmar ações perigosas
Remove-Item -Path "C:\Important" -Confirm

# Tratar erros
Get-Process -ErrorAction SilentlyContinue
```

### Performance
```powershell
# Otimizar pipeline
Get-Process | 
    Where-Object {$_.CPU -gt 50} |
    Sort-Object CPU -Descending |
    Select-Object -First 5

# Usar parâmetros específicos
Get-Process -Name "chrome" # Melhor que filtrar depois
```

## Exercícios Práticos

1. Gerenciamento de Arquivos
   - Liste todos arquivos .txt recursivamente
   - Copie arquivos com confirmação
   - Mova arquivos usando pipeline

2. Processos e Serviços
   - Liste processos por uso de CPU
   - Gerencie serviços específicos
   - Use WhatIf para simular ações

3. Descoberta
   - Encontre cmdlets por verbo
   - Explore ajuda e exemplos
   - Pratique com parâmetros comuns

## Próximos Passos

1. [Pipeline Avançado](ps-pipeline.md)
2. [Objetos PowerShell](ps-objects.md)
3. [Scripting](ps-scripting.md)

---
_"Um bom artesão conhece suas ferramentas."_