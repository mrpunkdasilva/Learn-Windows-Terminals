```
     ____ _ _     _____ _   _ _____ _____ ____ ____      _  _____ _____ 
    / ___(_) |_  |_   _| \ | |_   _|  ___/ ___|  _ \    / \|_   _| ____|
   | |  _| | __|   | | |  \| | | | | |_ | |  _| |_) |  / _ \ | | |  _|  
   | |_| | | |_    | | | |\  | | | |  _|| |_| |  _ <  / ___ \| | | |___ 
    \____|_|\__|   |_| |_| \_| |_| |_|   \____|_| \_\/_/   \_\_| |_____|
```

# Git Integration com CMD: Controlando o Caos

> "Git é como um checkpoint em um jogo difícil: você nunca sabe se salvou no momento certo."

## Configuração Inicial (Ou: Preparando-se Para o Desastre)

### Setup Básico
```batch
:: Configurando sua identidade (para saber quem culpar depois)
git config --global user.name "Seu Nome"
git config --global user.email "seu@email.com"

:: Configurando cores (porque preto e branco é muito 1980)
git config --global color.ui auto
```

## Scripts de Automação Git (Ou: Automatizando Seus Erros)

### Commit Rápido
```batch
@echo off
:: Script para commit rápido (e arrependimento posterior)
:quickCommit
set /p COMMIT_MSG="Digite sua mensagem de commit (ou pressione Enter para 'oops'): "
if "%COMMIT_MSG%"=="" set COMMIT_MSG=oops: provavelmente quebrei algo

git add .
git commit -m "%COMMIT_MSG%"
git push

if errorlevel 1 (
    echo Parabéns! Você conseguiu quebrar o Git!
    goto :fixItLater
)
```

### Branch Management
```batch
:: Criando e mudando branches (ou: como se perder em seu próprio código)
:branchManager
set /p BRANCH_NAME="Nome da nova branch (pressione Enter para 'feature/outro-bug'): "
if "%BRANCH_NAME%"=="" set BRANCH_NAME=feature/outro-bug

git checkout -b %BRANCH_NAME%
if errorlevel 1 (
    echo Branch já existe ou você digitou algo muito criativo
    echo Tentando plano B...
    git checkout %BRANCH_NAME%
)
```

## Workflows Comuns (Ou: Rituais Diários de Desespero)

### Pull Before Push
```batch
:: Script para evitar conflitos (boa sorte com isso)
:safePush
echo Verificando se tem alguém mais quebrando o código...
git fetch

git pull
if errorlevel 1 (
    echo CONFLITO DETECTADO!
    echo Iniciando protocolo de pânico...
    goto :mergeConflictPanic
)

git push
```

### Merge Conflict Resolution
```batch
:: Quando tudo dá errado (e vai dar)
:mergeConflictPanic
echo ==========================================
echo PROTOCOLO DE CONFLITO DE MERGE ATIVADO
echo ==========================================
echo 1. Não entre em pânico
echo 2. Tarde demais para o item 1
echo 3. Tentando resolver automaticamente...

git config merge.tool vimdiff
git mergetool

if errorlevel 1 (
    echo Resolução automática falhou
    echo Recomendação: Finja que não foi você
)
```

## Scripts de Manutenção (Ou: Mantendo a Sanidade)

### Limpeza de Branches
```batch
:: Removendo branches mortas (RIP)
:cleanBranches
echo Iniciando limpeza de branches...
echo (Ou: deletando evidências)

git branch --merged | findstr /v "* master main develop" > temp.txt
for /f "tokens=*" %%a in (temp.txt) do (
    git branch -d %%a
)
del temp.txt
```

### Backup de Emergência
```batch
:: Quando tudo mais falhar
:panicBackup
set BACKUP_DIR=backup_%date:~-4%%date:~3,2%%date:~0,2%
mkdir %BACKUP_DIR%
xcopy /s /e .git %BACKUP_DIR%\.git\
echo Backup criado em %BACKUP_DIR%
echo (Mas você provavelmente nunca vai usar isso)
```

## Protocolos de Emergência

### Reset de Emergência
```batch
:: Botão de pânico nuclear
:nuclearOption
echo ATENÇÃO: INICIANDO PROTOCOLO DE RESET
echo Este é o botão vermelho. Tem certeza?
pause

git reset --hard HEAD~1
echo O que está feito, está feito
echo (Espero que você tenha feito backup)
```

### Stash Rápido
```batch
:: Escondendo as evidências
:quickStash
git stash push -m "Mudanças que eu deveria ter commitado ontem"
echo Suas alterações foram escondidas
echo (Boa sorte lembrando onde)
```

## Checklist de Sobrevivência Git

- [ ] Repositório inicializado (provavelmente do jeito errado)
- [ ] Remote configurado (apontando para algum lugar)
- [ ] Branches criadas (e perdidas)
- [ ] Commits feitos (e arrependidos)
- [ ] Conflitos resolvidos (ou ignorados)
- [ ] Stack Overflow aberto em várias tabs

## Próximos Passos (Se Você Sobreviveu)

1. [Docker Integration](cmd-docker-integration.md) (Para mais aventuras em containers)
2. [Cloud Integration](cmd-cloud-integration.md) (Leve seus problemas para a nuvem)
3. [Terapia](cmd-exercises.md) (Altamente recomendado neste ponto)

---
_"Git é como um relacionamento complicado: ninguém entende realmente como funciona, mas todo mundo finge que sim."_