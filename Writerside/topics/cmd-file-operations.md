```
    _____ _ _      ____            
   |  ___(_) | ___/ ___| _   _ ___ 
   | |_  | | |/ _ \___ \| | | / __|
   |  _| | | |  __/___) | |_| \__ \
   |_|   |_|_|\___|____/ \__, |___/
                         |___/      
```

# Operações com Arquivos: Dominando o Sistema

> "Arquivos são dados. Dados são poder. Poder é controle."

## Operações Essenciais

### Criação e Manipulação Básica
```batch
:: Criação de estrutura
md projeto                    :: Cria diretório
md "projeto/src" "projeto/tests"  :: Múltiplos diretórios
type nul > index.js          :: Cria arquivo vazio
copy con config.json         :: Cria e edita arquivo
echo console.log() >> index.js   :: Append em arquivo

:: Manipulação básica
copy arquivo.txt backup.txt   :: Copia arquivo
move src\*.js dist\          :: Move arquivos
del *.tmp                    :: Deleta arquivos
ren old.js new.js           :: Renomeia arquivo
```

### Operações Avançadas
```batch
:: Cópia com atributos
xcopy /s /e /h /i src dest   :: Copia preservando tudo
robocopy src dest /MIR       :: Sincronização espelhada
robocopy src dest /E /XO     :: Copia apenas mais novos

:: Compactação
compact /c /s *.log         :: Compacta logs
compact /u /s *.*           :: Descompacta tudo
```

## Operações em Massa

### Processamento em Lote
```batch
:: Manipulação em lote
for /r %%f in (*.js) do (
    echo Processando: %%f
    :: Substitui texto
    powershell -Command "(Get-Content '%%f') -replace 'const', 'let' | Set-Content '%%f'"
)

:: Renomeação em lote
for %%f in (*.jsx) do (
    set "filename=%%~nf"
    ren "%%f" "!filename!.tsx"
)

:: Processamento condicional
for /r %%f in (*) do (
    if %%~zf gtr 1048576 (
        echo Arquivo grande: %%f
        echo %%~zf bytes
    )
)
```

## Backup e Sincronização

### Sistema de Backup Completo
```batch
:: Backup com timestamp
set "timestamp=%date:~-4%%date:~3,2%%date:~0,2%_%time:~0,2%%time:~3,2%"
set "backup_dir=backup_%timestamp%"

:: Cria estrutura
md "%backup_dir%"
md "%backup_dir%\code"
md "%backup_dir%\data"

:: Backup de código
xcopy /s /e /h src "%backup_dir%\code"
xcopy /s /e /h config "%backup_dir%\data"

:: Compactação
7z a "%backup_dir%.zip" "%backup_dir%\*"
rmdir /s /q "%backup_dir%"

:: Log
echo Backup realizado em %date% %time% >> backup_log.txt
```

### Sincronização de Projetos
```batch
:: Sincronização bidirecional
:syncProjects
set "source=%~1"
set "target=%~2"

:: Verifica parâmetros
if "%source%"=="" goto :usage
if "%target%"=="" goto :usage

:: Sincroniza
robocopy "%source%" "%target%" /MIR /XD node_modules /XF *.log
if errorlevel 8 goto :syncError

echo Sincronização concluída
exit /b 0

:syncError
echo Erro na sincronização
exit /b 1

:usage
echo Uso: syncProjects source_dir target_dir
exit /b 1
```

## Operações para Desenvolvimento

### Limpeza de Projeto
```batch
:: Limpeza completa
:cleanProject
echo Limpando projeto...

:: Remove builds
rmdir /s /q dist
rmdir /s /q build
rmdir /s /q .next
rmdir /s /q coverage

:: Remove dependências
rmdir /s /q node_modules
del package-lock.json
del yarn.lock

:: Remove caches
del /s /q *.cache
del /s /q .eslintcache
```

### Busca de Código
```batch
:: Busca avançada
:searchCode
set "pattern=%~1"
set "extensions=%~2"

if "%extensions%"=="" set "extensions=js,ts,jsx,tsx"

:: Busca em múltiplos tipos
for %%e in (%extensions::=,%) do (
    echo Buscando em *.%%e
    findstr /s /i /n /p "%pattern%" *.%%e
)

:: Conta ocorrências
for %%e in (%extensions::=,%) do (
    findstr /s /i /m "%pattern%" *.%%e > matches.tmp
    for /f %%a in ('type matches.tmp ^| find /c /v ""') do (
        echo %%a ocorrências em arquivos *.%%e
    )
)
del matches.tmp
```

## Manipulação de Permissões

### Gerenciamento de Acesso
```batch
:: Permissões de arquivo
icacls arquivo.txt /grant Users:F
icacls pasta /grant "Users:(OI)(CI)F"
takeown /f pasta /r /d y

:: Herança
icacls pasta /inheritance:d
icacls pasta /inheritance:e

:: Remoção de permissões
icacls arquivo.txt /remove Users
```

## Scripts Práticos

### Monitor de Alterações
```batch
:: Monitor em tempo real
:watchFiles
set "dir=%~1"
if "%dir%"=="" set "dir=."

echo Monitorando alterações em %dir%
echo Pressione Ctrl+C para sair

:watchLoop
    cls
    echo === %date% %time% ===
    
    :: Lista arquivos modificados hoje
    forfiles /p "%dir%" /m *.* /d 0 /c "cmd /c echo @file @fdate @ftime"
    
    :: Verifica tamanho total
    for /f "tokens=3" %%a in ('dir /-c /w "%dir%"') do set size=%%a
    echo Total: %size% bytes
    
    timeout /t 5 > nul
    goto watchLoop
```

### Backup Automático
```batch
:: Sistema de backup incremental
:incrementalBackup
set "source=%~1"
set "dest=%~2"
set "timestamp=%date:~-4%%date:~3,2%%date:~0,2%"

:: Verifica último backup
if exist "%dest%\last_backup.txt" (
    set /p last_backup=<"%dest%\last_backup.txt"
    
    :: Copia apenas arquivos novos/modificados
    robocopy "%source%" "%dest%\%timestamp%" /E /XO /XD node_modules
) else (
    :: Primeiro backup (completo)
    robocopy "%source%" "%dest%\%timestamp%" /E /XD node_modules
)

:: Atualiza registro
echo %timestamp% > "%dest%\last_backup.txt"
```

## Boas Práticas

1. **Sempre faça backup antes de operações destrutivas**
2. **Use caminhos absolutos em scripts importantes**
3. **Verifique permissões antes de operações críticas**
4. **Mantenha logs de operações importantes**
5. **Use variáveis para paths frequentemente usados**

## Troubleshooting

| Problema | Causa | Solução |
|----------|-------|---------|
| Acesso Negado | Permissões | `takeown` + `icacls` |
| Path Muito Longo | Limite Windows | Usar `subst` ou `\\?\` |
| Arquivo em Uso | Locked | `handle.exe` para identificar |
| Caracteres Especiais | Encoding | Usar `chcp 65001` |

## Próximos Passos

1. [Automação Avançada](cmd-automation.md)
2. [Scripting Batch](cmd-batch-scripting.md)
3. [PowerShell Migration](ps-introduction.md)

---
_"Quem controla os arquivos, controla o sistema."_