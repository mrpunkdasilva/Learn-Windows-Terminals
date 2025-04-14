```
     ____ __  __ ____  
    / ___|  \/  |  _ \ 
   | |   | |\/| | | | |
   | |___| |  | | |_| |
    \____|_|  |_|____/ 
```

# Command Prompt (CMD): O Terminal Clássico do Windows

> "Às vezes o mais antigo é o mais confiável."

## Visão Geral

O Command Prompt (CMD) é a interface de linha de comando tradicional do Windows. Apesar de sua idade, continua sendo uma ferramenta essencial para:

- Automação de tarefas
- Diagnóstico de problemas
- Gerenciamento de sistema
- Desenvolvimento de software
- Scripts batch

## Por que Usar CMD em 2025?

### Vantagens
- Baixo consumo de recursos
- Presente em todo Windows
- Compatibilidade histórica
- Execução rápida
- Scripts batch simples

### Limitações
- Sintaxe antiga
- Menos recursos que PowerShell
- Comandos limitados
- Case-insensitive

## Estrutura do Curso

### Fundamentos
1. [Introdução](cmd-introduction.md)
2. [Comandos Básicos](cmd-basic-commands.md)
3. [Navegação](cmd-navigation.md)

### Intermediário
1. [Operações com Arquivos](cmd-file-operations.md)
2. [Comandos do Sistema](cmd-system-commands.md)
3. [Batch Scripting](cmd-batch-scripting.md)

### Avançado
1. [Automação](cmd-automation.md)
2. [Networking](cmd-networking.md)
3. [Debugging](cmd-debugging.md)

## Ambiente de Desenvolvimento

### Configuração Recomendada
```batch
:: Configurações básicas
chcp 65001          :: UTF-8
prompt $P$G         :: Prompt personalizado
color 0A            :: Cores (verde sobre preto)
mode con: cols=120 lines=3000  :: Tamanho da janela
```

### Ferramentas Complementares
- Windows Terminal
- ConEmu
- Cmder
- Git Bash

## Quick Reference

### Comandos Essenciais
```batch
:: Navegação
cd          :: Mostra/muda diretório
dir         :: Lista arquivos
path        :: Mostra/define PATH

:: Sistema
tasklist    :: Lista processos
systeminfo  :: Info do sistema
ipconfig    :: Config de rede

:: Arquivos
copy        :: Copia arquivos
move        :: Move arquivos
del         :: Deleta arquivos
```

### Atalhos do Teclado
| Atalho | Função |
|--------|---------|
| `↑/↓` | Histórico |
| `Tab` | Autocompletar |
| `F7` | Histórico visual |
| `Ctrl+C` | Cancelar |

## Integração com Ferramentas

### DevOps
- Git
- Docker
- Cloud CLIs

### Build Tools
- MSBuild
- Maven
- npm

## Melhores Práticas

### Organização
- Scripts em pasta dedicada
- Nomenclatura consistente
- Documentação inline
- Logs estruturados

### Segurança
- Validação de inputs
- Escape de caracteres
- Privilégios mínimos
- Tratamento de erros

## Próximos Passos

1. [Introdução ao CMD](cmd-introduction.md)
2. [Comandos Básicos](cmd-basic-commands.md)
3. [PowerShell](powershell.md) (Quando precisar mais poder)

## Recursos Adicionais

- [Documentação Microsoft](https://docs.microsoft.com/windows-server/administration/windows-commands/windows-commands)
- [SS64 CMD Reference](https://ss64.com/nt/)
- [Batch Script Tutorial](https://www.tutorialspoint.com/batch_script/index.htm)

---
_"CMD: Porque às vezes o básico é tudo que você precisa."_