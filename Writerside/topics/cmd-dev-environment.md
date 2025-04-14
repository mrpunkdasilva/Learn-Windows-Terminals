# Ambiente de Desenvolvimento Automatizado

## Visão Geral
Script para configuração automática de ambientes de desenvolvimento com suporte a múltiplas stacks (Node.js, Python, Java, etc.).

## Pré-requisitos
- Windows 10 ou superior
- Conexão com internet
- Permissões de administrador
- Git instalado

## Estrutura do Projeto
```batch
dev-environment/
│   setup.bat
│   config.bat
│   packages.json
│   requirements.txt
│   
├───scripts/
│       node-setup.bat
│       python-setup.bat
│       java-setup.bat
│       git-setup.bat
│
└───templates/
        .gitignore
        .editorconfig
        .eslintrc
        pytest.ini
```

## Implementação Detalhada

### 1. Configuração Base
Crie o arquivo `config.bat`:

```batch
@echo off
:: Configurações Globais
set "DEV_ROOT=%USERPROFILE%\dev"
set "TOOLS_DIR=%DEV_ROOT%\tools"
set "WORKSPACE_DIR=%DEV_ROOT%\workspace"
set "TEMP_DIR=%DEV_ROOT%\temp"
set "LOG_FILE=%DEV_ROOT%\setup.log"

:: Versões das ferramentas
set "NODE_VERSION=18.16.0"
set "PYTHON_VERSION=3.11.0"
set "JAVA_VERSION=17.0.2"

:: URLs de download
set "NODE_URL=https://nodejs.org/dist/v%NODE_VERSION%/node-v%NODE_VERSION%-win-x64.zip"
set "PYTHON_URL=https://www.python.org/ftp/python/%PYTHON_VERSION%/python-%PYTHON_VERSION%-amd64.exe"
set "JAVA_URL=https://download.oracle.com/java/%JAVA_VERSION%/latest/jdk-%JAVA_VERSION%_windows-x64_bin.zip"
```

### 2. Script Principal
Implemente o `setup.bat`:

```batch
@echo off
setlocal enabledelayedexpansion

:: Carrega configurações
call config.bat

:: Menu de instalação
:menu
cls
echo === Configuração do Ambiente de Desenvolvimento ===
echo 1. Ambiente Node.js
echo 2. Ambiente Python
echo 3. Ambiente Java
echo 4. Ambiente Completo
echo 5. Sair
echo.

set /p CHOICE="Escolha uma opção: "

if "%CHOICE%"=="1" call :setupNode
if "%CHOICE%"=="2" call :setupPython
if "%CHOICE%"=="3" call :setupJava
if "%CHOICE%"=="4" call :setupComplete
if "%CHOICE%"=="5" exit /b 0

goto menu

:setupComplete
    call :initializeEnvironment
    call :setupNode
    call :setupPython
    call :setupJava
    call :setupGit
    call :configureVSCode
    call :finalizeSetup
    exit /b 0

:initializeEnvironment
    echo Inicializando ambiente...
    if not exist "%DEV_ROOT%" mkdir "%DEV_ROOT%"
    if not exist "%TOOLS_DIR%" mkdir "%TOOLS_DIR%"
    if not exist "%WORKSPACE_DIR%" mkdir "%WORKSPACE_DIR%"
    if not exist "%TEMP_DIR%" mkdir "%TEMP_DIR%"
    exit /b 0
```

### 3. Configuração Node.js
Crie `scripts/node-setup.bat`:

```batch
@echo off
echo Configurando ambiente Node.js...

:: Download e instalação do Node.js
curl -o "%TEMP_DIR%\node.zip" "%NODE_URL%"
powershell -command "Expand-Archive '%TEMP_DIR%\node.zip' '%TOOLS_DIR%'"

:: Configuração do ambiente
setx PATH "%PATH%;%TOOLS_DIR%\node" /M

:: Instalação de pacotes globais
call npm install -g yarn typescript eslint prettier

:: Configuração do projeto base
pushd "%WORKSPACE_DIR%"
mkdir node-project
cd node-project
call npm init -y
call npm install --save-dev jest typescript @types/node
copy "%~dp0\..\templates\.eslintrc" .
popd
```

### 4. Configuração Python
Implemente `scripts/python-setup.bat`:

```batch
@echo off
echo Configurando ambiente Python...

:: Download e instalação do Python
curl -o "%TEMP_DIR%\python-installer.exe" "%PYTHON_URL%"
"%TEMP_DIR%\python-installer.exe" /quiet InstallAllUsers=1 PrependPath=1

:: Instalação de pacotes essenciais
python -m pip install --upgrade pip
pip install -r "%~dp0\..\requirements.txt"

:: Configuração do ambiente virtual
pushd "%WORKSPACE_DIR%"
mkdir python-project
cd python-project
python -m venv venv
call venv\Scripts\activate
copy "%~dp0\..\templates\pytest.ini" .
popd
```

## Uso do Sistema

### Instalação Básica
1. Clone o repositório
2. Execute como administrador:
```batch
setup.bat
```
3. Selecione o ambiente desejado

### Personalização
1. Edite `config.bat` para versões específicas
2. Modifique `packages.json` para pacotes NPM
3. Atualize `requirements.txt` para pacotes Python

## Verificação do Ambiente

### Checklist de Instalação
- [ ] Variáveis de ambiente configuradas
- [ ] Ferramentas base instaladas
- [ ] Pacotes globais disponíveis
- [ ] Projetos template criados

### Comandos de Verificação
```batch
:: Verificar Node.js
node --version
npm --version

:: Verificar Python
python --version
pip --version

:: Verificar Java
java -version
javac -version
```

## Resolução de Problemas

### Problemas Comuns
1. **Falha no Download**
   - Verifique conexão
   - Atualize URLs em `config.bat`

2. **Erro de Permissão**
   - Execute como administrador
   - Verifique políticas de segurança

3. **Conflito de Versões**
   - Limpe instalações anteriores
   - Atualize PATH manualmente

## Melhorias Sugeridas

### Funcionalidades Adicionais
1. Suporte a Docker
2. Integração com WSL
3. Backup de configurações
4. Interface gráfica simples

### Otimizações
1. Downloads paralelos
2. Cache de instaladores
3. Atualização automática
4. Perfis personalizados

## Próximos Passos
1. Explore integrações adicionais
2. Personalize para seu workflow
3. Contribua com melhorias
4. Continue com [Automação de Deploy](cmd-deployment-automation.md)