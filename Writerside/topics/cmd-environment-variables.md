# Variáveis de Ambiente: O Coração do Sistema

> "Variáveis de ambiente são a alma do sistema operacional, definindo seu comportamento e capacidades."

## Fundamentos

### Variáveis do Sistema
```batch
:: Variáveis comuns do sistema
echo %SYSTEMROOT%        :: Pasta do Windows
echo %USERPROFILE%      :: Pasta do usuário
echo %APPDATA%          :: Dados de aplicativos
echo %PATH%             :: Caminhos executáveis
echo %COMPUTERNAME%     :: Nome do computador
```

### Variáveis de Usuário
```batch
:: Variáveis personalizadas
setx MY_VAR "valor"     :: Permanente
set TEMP_VAR=valor      :: Temporária
set "COMPLEX_VAR=valor com espaços"
```

## Manipulação de Variáveis

### Operações Básicas
```batch
:: Criação e modificação
set PATH=%PATH%;C:\NovoPath    :: Append ao PATH
set VAR=                       :: Remove variável
set VAR_NAME                   :: Lista variável
set                           :: Lista todas
```

### Expansão Delayed
```batch
:: Uso de !variável!
setlocal EnableDelayedExpansion
set VAR=Inicial
for %%i in (1 2 3) do (
    set VAR=Novo%%i
    echo !VAR!
)
endlocal
```

## Variáveis Especiais

### Variáveis de Data e Hora
```batch
:: Data e hora
echo %DATE%             :: Data atual
echo %TIME%             :: Hora atual
set timestamp=%date:~-4%%date:~3,2%%date:~0,2%_%time:~0,2%%time:~3,2%
```

### Variáveis de Argumentos
```batch
:: Argumentos de script
echo %0                 :: Nome do script
echo %1                 :: Primeiro argumento
echo %*                 :: Todos argumentos
shift                   :: Desloca argumentos
```

## Escopo e Persistência

### Escopo Local vs Global
```batch
:: Controle de escopo
setlocal
set LOCAL_VAR=valor
endlocal

:: Preservando valores
setlocal
set RESULT=valor
endlocal & set RESULT=%RESULT%
```

### Persistência
```batch
:: Variáveis permanentes
setx PATH "%PATH%;C:\NovoPath" /M  :: Sistema
setx USER_VAR "valor"              :: Usuário
reg add "HKCU\Environment" /v MyVar /t REG_SZ /d "valor"
```

## Variáveis para Desenvolvimento

### Configuração de Ambiente Dev
```batch
:: Setup desenvolvimento
set JAVA_HOME=C:\Program Files\Java\jdk-17
set MAVEN_HOME=C:\apache-maven
set NODE_HOME=C:\Program Files\nodejs
set PATH=%PATH%;%JAVA_HOME%\bin;%MAVEN_HOME%\bin;%NODE_HOME%
```

### Variáveis para Build
```batch
:: Variáveis de build
set BUILD_VERSION=1.0.0
set BUILD_ENV=development
set DEBUG=true
set LOG_LEVEL=verbose
```

## Técnicas Avançadas

### Manipulação de Strings
```batch
:: Operações com strings
set STR=Hello World
echo %STR:~0,5%         :: Substring (Hello)
echo %STR:Hello=Hi%     :: Substituição
echo %STR:o=0%          :: Replace global
```

### Variáveis Dinâmicas
```batch
:: Nomes dinâmicos
set PREFIX=TEST
set %PREFIX%_1=Valor1
set %PREFIX%_2=Valor2
call set DYNAMIC=%%%PREFIX%_1%%
```

## Debug e Troubleshooting

### Diagnóstico
```batch
:: Ferramentas de debug
set                     :: Lista todas variáveis
echo %ErrorLevel%       :: Último código de erro
set VAR 2>nul          :: Verifica existência
if defined VAR (echo Existe)
```

### Logging
```batch
:: Log de variáveis
echo [%date% %time%] VAR=%VAR% >> vars.log
set > snapshot.txt     :: Dump de variáveis
comp snapshot1.txt snapshot2.txt :: Compara estados
```

## Melhores Práticas

### Segurança
- Evite armazenar senhas em variáveis
- Use `setx` com cautela
- Valide valores antes de usar
- Limpe variáveis sensíveis após uso

### Organização
- Use prefixos consistentes
- Documente variáveis importantes
- Mantenha backup das configurações
- Agrupe variáveis relacionadas

## Próximos Passos

1. [Batch Scripting](cmd-batch-scripting.md)
2. [Automação](cmd-automation.md)
3. [Integração DevOps](cmd-integration.md)

---
_"As variáveis de ambiente são o DNA do seu sistema - cuide bem delas."_