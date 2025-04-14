# Sistema de Backup em CMD

## Visão Geral
Este projeto implementa um sistema de backup robusto usando CMD, com recursos de backup incremental, compressão e rotação automática de arquivos.

## Pré-requisitos
- Windows 7 ou superior
- 7-Zip instalado (para compressão)
- Permissões de administrador
- Conhecimento básico de CMD

## Estrutura do Projeto
```batch
backup-system/
│   backup.bat
│   config.bat
│   utils.bat
│
├───logs/
│       backup.log
│       error.log
│
└───backups/
    ├───daily/
    ├───weekly/
    └───monthly/
```

## Implementação Passo a Passo

### 1. Configuração Inicial
Primeiro, crie o arquivo `config.bat`:

```batch
@echo off
:: Configurações do Sistema de Backup
set "BACKUP_ROOT=C:\backup-system"
set "SOURCE_DIR=C:\dados"
set "BACKUP_DIR=%BACKUP_ROOT%\backups"
set "LOG_DIR=%BACKUP_ROOT%\logs"
set "MAX_BACKUPS=5"
set "COMPRESSION_LEVEL=5"
```

### 2. Funções Utilitárias
Crie o arquivo `utils.bat`:

```batch
@echo off
:: Funções de Utilidade

:logMessage
echo [%date% %time%] %~1 >> "%LOG_DIR%\backup.log"
exit /b 0

:checkDiskSpace
for /f "tokens=3" %%a in ('dir %BACKUP_DIR% /-c /-o /w') do set "space=%%a"
if %space% LSS 1073741824 (
    call :logMessage "ERRO: Espaço insuficiente"
    exit /b 1
)
exit /b 0
```

### 3. Script Principal
Implemente o `backup.bat`:

```batch
@echo off
setlocal enabledelayedexpansion

:: Carrega configurações
call config.bat

:: Inicialização
call :initializeBackup
if errorlevel 1 exit /b 1

:: Backup incremental
call :performIncrementalBackup
if errorlevel 1 goto :errorHandler

:: Rotação de backups
call :rotateBackups

:: Finalização
call :cleanup
exit /b 0

:initializeBackup
    if not exist "%BACKUP_DIR%" mkdir "%BACKUP_DIR%"
    if not exist "%LOG_DIR%" mkdir "%LOG_DIR%"
    call :checkDiskSpace
    exit /b

:performIncrementalBackup
    set "TIMESTAMP=%date:~-4%%date:~3,2%%date:~0,2%_%time:~0,2%%time:~3,2%"
    set "BACKUP_NAME=backup_%TIMESTAMP%"
    
    :: Copia apenas arquivos modificados
    robocopy "%SOURCE_DIR%" "%BACKUP_DIR%\%BACKUP_NAME%" /MIR /Z /B /R:3 /W:10 /LOG+:"%LOG_DIR%\backup.log"
    
    :: Compressão
    7z a -t7z -mx=%COMPRESSION_LEVEL% "%BACKUP_DIR%\%BACKUP_NAME%.7z" "%BACKUP_DIR%\%BACKUP_NAME%\*"
    if errorlevel 1 (
        call :logMessage "ERRO: Falha na compressão"
        exit /b 1
    )
    
    rmdir /s /q "%BACKUP_DIR%\%BACKUP_NAME%"
    exit /b 0
```

## Uso do Sistema

### Execução Manual
1. Abra o prompt de comando como administrador
2. Navegue até a pasta do projeto
3. Execute:
```batch
backup.bat
```

### Agendamento Automático
1. Abra o Agendador de Tarefas do Windows
2. Crie uma nova tarefa
3. Configure o gatilho (diário/semanal)
4. Adicione a ação: `cmd.exe /c "C:\backup-system\backup.bat"`

## Monitoramento e Logs

### Estrutura de Logs
- `backup.log`: Operações normais
- `error.log`: Erros e avisos

### Exemplo de Log
```
[2024-01-20 15:30:45] Iniciando backup
[2024-01-20 15:30:46] Verificando espaço em disco
[2024-01-20 15:31:00] Backup incremental iniciado
[2024-01-20 15:35:22] Compressão concluída
[2024-01-20 15:35:23] Backup finalizado com sucesso
```

## Resolução de Problemas

### Problemas Comuns
1. **Erro de Permissão**
   - Execute como administrador
   - Verifique ACLs dos diretórios

2. **Espaço Insuficiente**
   - Execute limpeza de backups antigos
   - Ajuste `MAX_BACKUPS`

3. **Falha na Compressão**
   - Verifique instalação do 7-Zip
   - Ajuste `COMPRESSION_LEVEL`

## Melhorias Sugeridas

### Funcionalidades Adicionais
1. Backup diferencial
2. Verificação de integridade
3. Notificações por email
4. Interface web simples

### Otimizações
1. Compressão paralela
2. Exclusão inteligente
3. Backup em rede

## Próximos Passos
1. Implemente as melhorias sugeridas
2. Adapte para seus casos de uso
3. Integre com outros scripts
4. Explore o [Ambiente de Desenvolvimento](cmd-dev-environment.md)