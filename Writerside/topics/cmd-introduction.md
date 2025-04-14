```
    ____ __  __ ____  
   / ___|  \/  |  _ \ 
  | |   | |\/| | | | |
  | |___| |  | | |_| |
   \____|_|  |_|____/ 
```

# CMD: Seu Primeiro Terminal

> "Antes do PowerShell, antes das GUIs, havia o CMD. E ele ainda tem seu lugar no arsenal do desenvolvedor moderno."

## Por que aprender CMD em 2025?

O Command Prompt (CMD) continua sendo uma ferramenta essencial no ambiente Windows por várias razões:

- **Velocidade**: Execução de comandos muito mais rápida que interfaces gráficas
- **Scripts Batch**: Amplamente utilizados em sistemas legados e processos de build
- **Troubleshooting**: Fundamental para diagnóstico e resolução de problemas
- **Base Sólida**: Entender CMD facilita a transição para PowerShell
- **Compatibilidade**: Funciona em qualquer versão do Windows sem dependências
- **Recursos**: Consome menos memória que terminais modernos

## Acessando o CMD

Existem várias formas de acessar o CMD:

```batch
:: Método 1 - Run Dialog
Windows + R
cmd
Enter

:: Método 2 - Menu Iniciar
Windows + X -> Command Prompt (Admin)

:: Método 3 - File Explorer
Alt + D
cmd
Enter
```

## Configuração para Desenvolvimento

### Configurações Básicas
```batch
:: Habilitar recursos avançados
reg add "HKEY_CURRENT_USER\Console" /v QuickEdit /t REG_DWORD /d 1 /f
reg add "HKEY_CURRENT_USER\Console" /v InsertMode /t REG_DWORD /d 1 /f
reg add "HKEY_CURRENT_USER\Console" /v ScreenBufferSize /t REG_DWORD /d 0x23290096 /f
reg add "HKEY_CURRENT_USER\Console" /v FontSize /t REG_DWORD /d 0x00100008 /f
reg add "HKEY_CURRENT_USER\Console" /v FontFamily /t REG_DWORD /d 0x00000036 /f
reg add "HKEY_CURRENT_USER\Console" /v FontWeight /t REG_DWORD /d 0x00000190 /f
```

### Personalização Avançada
```batch
:: Cores e aparência
reg add "HKEY_CURRENT_USER\Console" /v ColorTable00 /t REG_DWORD /d 0x00000000 /f
reg add "HKEY_CURRENT_USER\Console" /v ColorTable07 /t REG_DWORD /d 0x00cccccc /f
reg add "HKEY_CURRENT_USER\Console" /v WindowSize /t REG_DWORD /d 0x00320078 /f
```

## Comandos Essenciais para Desenvolvimento

### Navegação e Gerenciamento de Projetos
```batch
:: Navegação rápida
cd /d D:\projetos              :: Muda drive e diretório
dir /b /s *.js                :: Lista recursivamente arquivos JS
tree /f                       :: Mostra estrutura de diretórios
pushd \\servidor\share        :: Salva localização atual e navega
popd                         :: Retorna à localização salva

:: Gerenciamento de arquivos
xcopy src dist /E /I /H      :: Copia preservando estrutura
robocopy source dest /MIR    :: Sincronização espelhada
attrib +h node_modules       :: Oculta pasta
```

### Processos e Rede
```batch
:: Gerenciamento de processos
tasklist | findstr "node"     :: Lista processos Node.js
taskkill /F /IM node.exe     :: Força fechamento do Node
wmic process where "name='node.exe'" get commandline  :: Detalhes

:: Rede e portas
netstat -ano | findstr "3000" :: Verifica porta específica
ipconfig /flushdns           :: Limpa cache DNS
nslookup api.exemplo.com     :: Resolve DNS
```

## Dicas Práticas

### Aliases e Paths Personalizados
```batch
:: Criar arquivo autoexec.bat
@echo off
:: Aliases úteis
doskey ls=dir /b $*
doskey clear=cls
doskey ..=cd ..
doskey ...=cd ../..
doskey gs=git status
doskey gl=git log --oneline
doskey gp=git pull
doskey npm-clean=rmdir /s /q node_modules && del package-lock.json

:: Paths de desenvolvimento
set PATH=%PATH%;C:\dev\tools
set JAVA_HOME=C:\Program Files\Java\jdk-17
set MAVEN_HOME=C:\dev\apache-maven
```

### Debugging Rápido
```batch
:: Verificação de portas
:checkPort
set PORT=%1
netstat -ano | findstr ":%PORT%"
if errorlevel 1 (
    echo Porta %PORT% livre
) else (
    echo Porta %PORT% em uso
    for /f "tokens=5" %%a in ('netstat -ano ^| findstr ":%PORT%"') do (
        echo PID: %%a
        tasklist | findstr "%%a"
    )
)

:: Matar processo
:killProcess
set PORT=%1
for /f "tokens=5" %%a in ('netstat -ano ^| findstr ":%PORT%"') do (
    taskkill /F /PID %%a
    if errorlevel 1 (
        echo Falha ao matar processo
    ) else (
        echo Processo finalizado
    )
)
```

## Atalhos do Teclado Essenciais

| Atalho | Função | Contexto |
|--------|---------|----------|
| `F7` | Histórico de comandos | Mostra lista navegável |
| `Alt + F7` | Limpa histórico | Útil para segurança |
| `F3` | Repete último comando | Digitação rápida |
| `Alt + Enter` | Modo tela cheia | Melhor visualização |
| `Ctrl + C` | Cancela comando | Interrompe execução |
| `Ctrl + Break` | Força interrupção | Quando Ctrl+C falha |

## Próximos Passos

1. [Comandos Básicos](cmd-basic-commands.md)
   - Aprenda os comandos fundamentais
   - Pratique navegação e manipulação

2. [Operações com Arquivos](cmd-file-operations.md)
   - Gerenciamento avançado de arquivos
   - Automação de backups

3. [Scripts Batch](cmd-batch-scripting.md)
   - Crie seus primeiros scripts
   - Automatize tarefas repetitivas

## Recursos Adicionais

- [Documentação Oficial Microsoft](https://docs.microsoft.com/windows-server/administration/windows-commands/windows-commands)
- [SS64 CMD Reference](https://ss64.com/nt/)
- [Rob van der Woude's Scripting Pages](https://www.robvanderwoude.com/batchstart.php)

## Troubleshooting Comum

| Problema | Solução | Comando |
|----------|---------|---------|
| Acesso Negado | Executar como Admin | `runas /user:Administrator cmd` |
| Codificação | Mudar para UTF-8 | `chcp 65001` |
| Path Longo | Usar subst | `subst X: C:\caminho\muito\longo` |
| Erro de Sintaxe | Verificar aspas | `echo "Uso correto de aspas"` |

---
_"O terminal é a interface mais poderosa que você pode dominar."_