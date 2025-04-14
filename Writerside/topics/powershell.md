# PowerShell: A Evolução do Terminal Windows

> "PowerShell não é apenas um shell, é uma plataforma de automação."

## O que é PowerShell?

PowerShell é uma plataforma de automação e shell de comando que combina:
- Shell de linha de comando
- Linguagem de scripting
- Framework de automação
- Gerenciamento de configuração

## Por que PowerShell?

### Vantagens sobre CMD
```powershell
# CMD: Saída como texto
dir | findstr ".txt"

# PowerShell: Saída como objetos
Get-ChildItem | Where-Object { $_.Extension -eq '.txt' }
```

### Recursos Exclusivos
- Pipeline orientado a objetos
- Cmdlets consistentes (Verb-Noun)
- Integração profunda com Windows
- Suporte multiplataforma (Core)
- Extensibilidade via módulos

## Arquitetura Básica

### Componentes Principais
```powershell
# Verificar versão e edição
$PSVersionTable

# Listar comandos disponíveis
Get-Command

# Explorar tipos de objetos
Get-ChildItem | Get-Member
```

### Pipeline & Objetos
```powershell
# Exemplo de pipeline poderoso
Get-Process |
    Where-Object { $_.CPU -gt 100 } |
    Sort-Object CPU -Descending |
    Select-Object Name, CPU, Memory |
    Export-Csv report.csv
```

## Estrutura do Curso

### Fundamentos
1. [Introdução](ps-introduction.md)
   - História e evolução
   - Instalação e configuração
   - Conceitos básicos

2. [Cmdlets Básicos](ps-cmdlets.md)
   - Sintaxe e estrutura
   - Comandos essenciais
   - Ajuda e documentação

3. [Pipeline & Objetos](ps-pipeline.md)
   - Conceito de pipeline
   - Manipulação de objetos
   - Filtragem e transformação

### Intermediário
1. [Scripting](ps-scripting.md)
   - Variáveis e tipos
   - Funções e módulos
   - Controle de fluxo

2. [Módulos & Extensões](ps-modules.md)
   - Criação de módulos
   - Importação e exportação
   - Módulos populares

3. [Tratamento de Erros](ps-error-handling.md)
   - Try/Catch
   - Logging
   - Debug e troubleshooting

### Avançado
1. [Remoting](ps-remote.md)
   - Sessões remotas
   - Execução distribuída
   - Segurança

2. [Segurança](ps-security.md)
   - Políticas de execução
   - Assinatura de scripts
   - Boas práticas

3. [DevOps & Cloud](ps-devops-integration.md)
   - Azure PowerShell
   - AWS Tools
   - CI/CD Integration

## Ambiente de Desenvolvimento

### Configuração Recomendada
```powershell
# Profile básico
Set-ExecutionPolicy RemoteSigned
Install-Module PSReadLine
Install-Module Terminal-Icons
Install-Module PSFzf
```

### Ferramentas Essenciais
- Visual Studio Code
- Windows Terminal
- PowerShell ISE
- Pester (testes)

## Quick Reference

### Comandos Essenciais
```powershell
# Sistema
Get-Process    # Lista processos
Get-Service    # Lista serviços
Get-EventLog   # Logs do sistema

# Arquivos
Get-ChildItem  # Lista arquivos
Copy-Item      # Copia
Move-Item      # Move
Remove-Item    # Remove

# Rede
Test-Connection # Ping avançado
Get-NetAdapter  # Adaptadores
Invoke-WebRequest # Requisições web
```

## Melhores Práticas

### Desenvolvimento
- Use verbos aprovados
- Siga convenções de nomes
- Documente com comentários
- Implemente tratamento de erros
- Escreva testes unitários

### Performance
- Evite loops desnecessários
- Use pipeline adequadamente
- Otimize consultas
- Cache resultados quando possível

## Integração com Ferramentas

### DevOps
- Git
- Docker
- Kubernetes
- Azure DevOps
- Jenkins

### Desenvolvimento
- Visual Studio
- VS Code
- Git
- Node.js
- Python

## Próximos Passos

1. [Introdução ao PowerShell](ps-introduction.md)
2. [Cmdlets Fundamentais](ps-cmdlets.md)
3. [Scripting Básico](ps-scripting.md)

## Recursos Adicionais

- [Documentação Oficial](https://docs.microsoft.com/powershell)
- [PowerShell Gallery](https://www.powershellgallery.com)
- [GitHub PowerShell](https://github.com/PowerShell/PowerShell)
- [PowerShell.org](https://powershell.org)

---
_"Com grande poder vem grande capacidade de automação."_