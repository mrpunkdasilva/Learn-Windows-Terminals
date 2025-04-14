```
                _____                  _             _     
               |_   _|__ _ __ _ __ ___ (_)_ __   __ _| |___ 
                 | |/ _ \ '__| '_ ` _ \| | '_ \ / _` | / __|
                 | |  __/ |  | | | | | | | | | | (_| | \__ \
                 |_|\___|_|  |_| |_| |_|_|_| |_|\__,_|_|___/
```

# Terminal Windows: Guia do Desenvolvedor

> "Em um mundo onde código é lei, o terminal é sua arma." 

## O que você vai encontrar aqui

Este guia foi feito por devs, para devs. Sem rodeios, direto ao ponto. Vamos dominar as duas principais ferramentas de linha de comando do Windows:

- **CMD**: O clássico prompt de comando
- **PowerShell**: A ferramenta moderna de automação

## Navegação Rápida

### CMD - O Básico do Underground
```bash
C:\> help
# Seu primeiro passo no submundo dos comandos
```
- [Comandos Essenciais](cmd-basic-commands.md)
- [Manipulação de Arquivos](cmd-file-operations.md)
- [Scripts Batch](cmd-batch-scripting.md)

### PowerShell - O Arsenal Avançado
```powershell
PS C:\> Get-Command
# Poder é conhecimento
```
- [Cmdlets Fundamentais](ps-cmdlets.md)
- [Pipeline & Objetos](ps-pipeline.md)
- [Automação & Scripts](ps-scripting.md)

## Por que este guia?

- ✓ Exemplos práticos do mundo real
- ✓ Foco em desenvolvimento e automação
- ✓ Sem enrolação, direto ao código
- ✓ Truques e hacks que você vai usar todo dia

## Comece Aqui

### Para Iniciantes
```bash
# Primeiro, aprenda a navegar
cd /d C:\seu\projeto
dir /b
```

### Para Devs Experientes
```powershell
# Automatize seu workflow
Get-ChildItem -Recurse -Include *.js | 
    Where-Object { $_.Length -gt 1MB }
```

## Projetos Práticos

1. **Build Automático**: Scripts para compilar e deployar
2. **Git Hooks**: Automatização de commits e pushes
3. **Monitoramento**: Scripts para logs e performance

## Contribua

Encontrou um bug? Tem uma dica ninja? 
[Abra uma issue](https://github.com/mrpunkdasilva/Learn-Windows-Terminals/issues) ou envie um PR.

## Dica do Dia
```
> Pressione Tab para autocompletar. Seu tempo é valioso demais 
> para digitar caminhos completos.
```

---
_"O terminal é mais do que uma ferramenta. É uma extensão do seu código."_


