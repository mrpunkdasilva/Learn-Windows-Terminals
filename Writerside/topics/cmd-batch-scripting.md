```
    ____        _       _     
   | __ )  __ _| |_ ___| |__  
   |  _ \ / _` | __/ __| '_ \ 
   | |_) | (_| | || (__| | | |
   |____/ \__,_|\__\___|_| |_|
```

# Scripts Batch: Automatize ou Morra Tentando

> "Automatização não é futuro, é sobrevivência."

Scripts batch são a espinha dorsal da automação no Windows. Eles permitem automatizar tarefas repetitivas, gerenciar processos e criar fluxos de trabalho complexos usando apenas o prompt de comando.

## Estrutura Básica

Todo script batch começa com algumas configurações essenciais:

```batch
@echo off
setlocal enabledelayedexpansion

:: Configurações
set PROJECT_ROOT=C:\projetos
set LOG_FILE=script.log

:: Seu código aqui
echo Iniciando script... >> %LOG_FILE%
```

- `@echo off`: Evita que os comandos apareçam durante a execução
- `setlocal enabledelayedexpansion`: Permite uso de variáveis em tempo de execução
- Variáveis de ambiente são definidas com `set`
- Use `>>` para append em logs, `>` para sobrescrever

## Variáveis e Ambiente

As variáveis são o coração de qualquer script. Existem dois tipos principais:

```batch
:: Variáveis do sistema (sempre disponíveis)
echo Sistema: %OS%                  :: Windows_NT
echo Usuário: %USERNAME%           :: JohnDoe
echo Temp: %TEMP%                 :: C:\Users\JohnDoe\AppData\Local\Temp

:: Variáveis customizadas (definidas por você)
set VERSION=1.0.0                              :: Simples
set /p USER_INPUT=Digite seu nome:            :: Input do usuário
set "COMPLEX_VAR=Valor com espaços"          :: Com espaços
set /a RESULT=(%NUM1% + %NUM2%) * 2         :: Operações matemáticas
```

### Dicas de Variáveis
- Use `set /a` para operações matemáticas
- Envolva valores com espaços em aspas
- `%~dp0` representa o diretório do script atual

## Estruturas de Controle

O controle de fluxo permite decisões e repetições:

```batch
:: Condicionais avançados
if exist "package.json" (
    echo Projeto Node.js detectado
    call npm install
) else if exist "pom.xml" (
    echo Projeto Maven detectado
    call mvn install
) else (
    echo Tipo de projeto desconhecido
    exit /b 1
)

:: Loops poderosos
:: Loop em arquivos
for %%i in (*.js) do (
    echo Processando: %%i
    call node %%i
)

:: Loop em diretórios
for /d %%d in (*) do (
    echo Verificando diretório: %%d
    if exist "%%d\package.json" call :processNodeProject "%%d"
)

:: Loop com números
for /l %%n in (1,1,5) do (
    echo Tentativa %%n de 5
    call :attemptConnection || (
        if %%n equ 5 exit /b 1
    )
)
```

## Funções e Módulos

Organize seu código em funções reutilizáveis:

```batch
:: Função com parâmetros
:processProject
set "PROJECT_DIR=%~1"
set "BUILD_TYPE=%~2"

echo Processando %PROJECT_DIR% como %BUILD_TYPE%
pushd %PROJECT_DIR%

if "%BUILD_TYPE%"=="node" (
    call npm install
    call npm run build
) else if "%BUILD_TYPE%"=="java" (
    call mvn clean install
)

popd
exit /b 0

:: Chamada da função
call :processProject "C:\projetos\meu-app" "node"
if errorlevel 1 echo Falha no processamento

:: Função com retorno de valor
:calculateSum
set /a "RESULT=%~1 + %~2"
exit /b %RESULT%

:: Capturando retorno
call :calculateSum 5 3
echo Resultado: %errorlevel%
```

## Scripts para Dev

Ferramentas práticas para desenvolvimento:

```batch
:: Setup completo de projeto
:initProject
set "PROJECT_NAME=%~1"
set "PROJECT_TYPE=%~2"

mkdir "%PROJECT_NAME%"
cd "%PROJECT_NAME%"

:: Estrutura base
mkdir src tests docs config
echo node_modules> .gitignore
echo dist>> .gitignore
echo .env>> .gitignore

:: Setup específico por tipo
if "%PROJECT_TYPE%"=="node" (
    call npm init -y
    call npm install --save-dev jest eslint prettier
    copy nul src\index.js
    copy nul tests\index.test.js
) else if "%PROJECT_TYPE%"=="react" (
    call npx create-react-app .
    call npm install --save-dev @testing-library/react
)

:: Git init
git init
git add .
git commit -m "Initial commit"

echo Projeto %PROJECT_NAME% criado com sucesso!
exit /b 0

:: Watch com hot reload
:devWatch
echo Iniciando modo desenvolvimento...
:watchLoop
    cls
    echo [%time%] Compilando...
    call npm run build
    
    if errorlevel 1 (
        echo [ERRO] Falha na compilação
        timeout /t 5 /nobreak > nul
    ) else (
        echo [OK] Compilação bem sucedida
    )
    
    timeout /t 2 /nobreak > nul
    goto watchLoop
```

## Manipulação de Erros

Sistema robusto de tratamento de erros:

```batch
:: Sistema completo de erro
setlocal enabledelayedexpansion
set "ERROR_LOG=%~dp0\errors.log"

:: Try-Catch simulado
:try
    set "ERROR_OCCURRED="
    
    :: Seu código aqui
    call npm install 2>nul || set ERROR_OCCURRED=1
    
    if defined ERROR_OCCURRED (
        call :catch
        exit /b 1
    )
    goto :finally

:catch
    echo [%date% %time%] Erro durante npm install >> %ERROR_LOG%
    echo Falha na instalação. Tentando recuperar...
    call npm cache clean --force
    call :notifyTeam "Falha no build"
    exit /b 1

:finally
    echo Limpando recursos...
    if exist "temp" rmdir /s /q temp
    exit /b 0

:: Sistema de notificação
:notifyTeam
set "MESSAGE=%~1"
curl -X POST "https://api.slack.com/webhook" -d "{'text':'%MESSAGE%'}"
exit /b
```

## Integração com DevOps

Automatização de processos de desenvolvimento:

```batch
:: Pipeline completo
:buildAndDeploy
set "ENV=%~1"
set "VERSION=%~2"

:: Validações
if "%ENV%"=="" set "ENV=dev"
if "%VERSION%"=="" set "VERSION=latest"

:: Backup
set "BACKUP_DIR=backup_%date:~-4%%date:~3,2%%date:~0,2%"
if not exist "%BACKUP_DIR%" mkdir "%BACKUP_DIR%"
xcopy /s /e dist\ "%BACKUP_DIR%\"

:: Build
echo [%time%] Iniciando build %VERSION%...
call npm run build
if errorlevel 1 goto :handleError

:: Testes
echo [%time%] Executando testes...
call npm test
if errorlevel 1 goto :handleError

:: Deploy
echo [%time%] Realizando deploy para %ENV%...
if "%ENV%"=="prod" (
    call :validateProd || goto :handleError
    robocopy dist\ \\prod-server\deploy\ /MIR /MT:8
) else (
    robocopy dist\ \\dev-server\deploy\ /MIR /MT:8
)

echo Deploy concluído com sucesso!
exit /b 0

:handleError
echo [ERRO] Falha no pipeline
call :notifyTeam "Pipeline falhou em %ENV%"
exit /b 1

:validateProd
echo Validando ambiente de produção...
call :checkDiskSpace || exit /b 1
call :checkPermissions || exit /b 1
call :backupDB || exit /b 1
exit /b 0
```

## Scripts Práticos

### Monitor de Recursos
```batch
:monitorResources
cls
echo === Monitor de Recursos ===
tasklist | findstr "node"
wmic cpu get loadpercentage
timeout /t 5 > nul
goto monitorResources
```

### Limpeza de Ambiente
```batch
:: Limpa ambiente de desenvolvimento
:cleanDev
rmdir /s /q node_modules
rmdir /s /q dist
del package-lock.json
del yarn.lock
npm cache clean --force
```

## Dicas Avançadas

```batch
:: Debug mode
set DEBUG=1
if defined DEBUG (
    echo [DEBUG] Variável=%VARIAVEL%
)

:: Captura de output
for /f "tokens=* delims=" %%a in ('git status') do (
    set "GIT_STATUS=%%a"
)
```

## Troubleshooting

Soluções para problemas comuns:

| Problema | Solução | Exemplo Detalhado |
|----------|---------|-------------------|
| Variável não expande | Use delayed expansion | `set "var=123" & echo !var!` |
| Caracteres especiais | Use escape ^ | `echo ^|^&^<^>` |
| Path com espaço | Use aspas | `cd "C:\Program Files\App"` |
| Permissão negada | Execute como admin | `runas /user:admin "script.bat"` |
| Codificação UTF-8 | Use chcp 65001 | `chcp 65001 > nul` |

## Próximos Passos

1. [Automação Avançada](cmd-automation.md) - Aprenda técnicas avançadas de automação
2. [PowerShell Migration](ps-introduction.md) - Evolua seus scripts para PowerShell
3. [CI/CD Integration](cmd-cicd.md) - Integre com pipelines modernos

## Recursos Adicionais

- [Documentação Oficial do Windows](https://docs.microsoft.com/windows-server/administration/windows-commands/windows-commands)
- [Batch Script Tutorial](https://www.tutorialspoint.com/batch_script/index.htm)
- [Fórum da Comunidade](https://stackoverflow.com/questions/tagged/batch-file)

---
_"Automatize hoje o trabalho de amanhã."_