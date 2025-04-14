# Referência PowerShell

> "PowerShell: onde comandos encontram objetos."

## Cmdlets Essenciais

### Navegação
```powershell
# Sistema de arquivos
Get-Location      # pwd
Set-Location      # cd
Get-ChildItem     # ls/dir
```

### Manipulação
```powershell
# Arquivos e objetos
Get-Content       # cat
Set-Content      # >
Copy-Item        # cp
Move-Item        # mv
Remove-Item      # rm
```

### Pipeline
```powershell
# Operações
Select-Object    # select
Where-Object    # filter
ForEach-Object  # loop
Sort-Object     # sort
Group-Object    # group
```

## Tipos de Dados

### Básicos
```powershell
# Tipos comuns
[string]        # Texto
[int]           # Inteiro
[bool]          # Boolean
[datetime]      # Data/hora
[array]         # Array
```

### Complexos
```powershell
# Estruturas
[hashtable]     # Dictionary
[PSCustomObject] # Objeto
[xml]           # XML
[scriptblock]   # Code block
```

## Operadores

### Comparação
```powershell
-eq  # Igual
-ne  # Diferente
-gt  # Maior
-lt  # Menor
-like # Pattern
-match # Regex
```

### Lógicos
```powershell
-and # AND
-or  # OR
-not # NOT
-xor # XOR
```

## Fluxo de Controle

### Condicionais
```powershell
if ($condition) {
    # code
}
elseif ($other) {
    # code
}
else {
    # code
}

switch ($value) {
    condition { code }
    default { code }
}
```

### Loops
```powershell
foreach ($item in $collection) {}
while ($condition) {}
do {} while ($condition)
for ($i=0; $i -lt 10; $i++) {}
```

## Error Handling

### Try-Catch
```powershell
try {
    # código arriscado
}
catch [Exception] {
    # tratamento
}
finally {
    # cleanup
}
```

### Preferências
```powershell
$ErrorActionPreference = 'Stop'
$WarningPreference = 'SilentlyContinue'
$VerbosePreference = 'Continue'
```

## Módulos

### Gestão
```powershell
Get-Module
Import-Module
Install-Module
Update-Module
```

### Comuns
- PSReadLine
- Az
- AWS.Tools
- PSScriptAnalyzer

## Perfis

### Localizações
```powershell
$PROFILE.CurrentUserCurrentHost
$PROFILE.CurrentUserAllHosts
$PROFILE.AllUsersCurrentHost
$PROFILE.AllUsersAllHosts
```

## Segurança

### Execution Policy
```powershell
Get-ExecutionPolicy
Set-ExecutionPolicy RemoteSigned
```

### Assinatura
```powershell
Set-AuthenticodeSignature
Get-AuthenticodeSignature
```

---
_"Com grande poder vem grande responsabilidade de documentar."_