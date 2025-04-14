```
    ____       _                       _             
   |  _ \  ___| |__  _   _  __ _  ___(_)_ __   __ _ 
   | | | |/ _ \ '_ \| | | |/ _` |/ __| | '_ \ / _` |
   | |_| |  __/ |_) | |_| | (_| | (__| | | | | (_| |
   |____/ \___|_.__/ \__,_|\__, |\___|_|_| |_|\__, |
                           |___/               |___/ 
```

# Debugging no CMD: Onde os Erros Vão Para Festa

> "Se você não está tendo problemas com seu código, provavelmente não está rodando ele."

## A Arte da Depuração (Ou: Como Perder Sua Sanidade com Estilo)

### Debug Mode: Seu Novo Melhor Amigo
```batch
:: Ative o modo debug e observe o caos
@echo on
set DEBUG=true
:: Agora você pode ver tudo queimando em tempo real
```

## Técnicas de Debugging Para Masoquistas Digitais

### 1. Echo Debug (Para os Tradicionalistas)
```batch
@echo off
echo [DEBUG] Iniciando script...
echo [DEBUG] Variável X = %X%
echo [DEBUG] Se você está lendo isso, algo deu errado
```

### 2. Logging Avançado (Porque Você Vai Precisar de Provas)
```batch
:: Crie um arquivo de log que ninguém vai ler
set "LOG_FILE=%TEMP%\debug_%DATE:~-4%%DATE:~3,2%%DATE:~0,2%_%TIME:~0,2%%TIME:~3,2%.log"
echo %DATE% %TIME% - Script iniciado >> %LOG_FILE%
echo %DATE% %TIME% - Algo deu errado >> %LOG_FILE%
echo %DATE% %TIME% - Como sempre >> %LOG_FILE%
```

## Ferramentas do Debugger (Seu Kit de Sobrevivência Digital)

### Error Levels: Seu Guia Para o Desespero
```batch
:: Verificando error levels
call seu_script.bat
if errorlevel 1 (
    echo Houston, temos um problema
    goto :PANIC
)

:PANIC
echo Iniciando protocolo de pânico...
echo Considere mudar de profissão
```

### Pause Estratégico (Ou: A Arte de Congelar o Tempo)
```batch
:: Pausa dramática para debug
set /p "DEBUG=Pressione ENTER para continuar o desastre..."
```

## Técnicas Avançadas de Masoquismo

### 1. Step-by-Step Debugging
```batch
:: Debug passo a passo (para os muito pacientes)
@echo on
set "comando1=executando..."
echo %comando1%
pause
set "comando2=ainda executando..."
echo %comando2%
pause
:: Repita até perder a vontade de viver
```

### 2. Variável Watching (Stalking Digital)
```batch
:: Monitorando variáveis como um stalker
set | findstr "IMPORTANTE"
set | findstr "CRÍTICO"
:: Observe suas variáveis desaparecerem misteriosamente
```

## Armadilhas Comuns (Ou: Como Não Se Enforcar com o Próprio Código)

```batch
:: Armadilha #1: Espaços em paths
if exist "C:\Program Files\Sua App" (
    echo Parabéns, você caiu na armadilha mais básica
)

:: Armadilha #2: Variáveis vazias
if "%VAR%"=="" (
    echo Surpresa! Sua variável evaporou
)
```

## Checklist do Debugger Desesperado

- [ ] Echo on ativado
- [ ] Logs criados
- [ ] Sanidade mental checada
- [ ] Café reabastecido
- [ ] Stack Overflow aberto em 15 tabs
- [ ] Plano B de carreira atualizado

## Soluções de Emergência

1. **O Famoso Ctrl+C**
   > Quando tudo falhar, sempre existe o botão de pânico

2. **Taskkill**
   ```batch
   :: Quando a sutileza não é uma opção
   taskkill /F /IM cmd.exe
   :: Adeus, cruel mundo dos processos
   ```

3. **O Reboot Mágico**
   > Porque às vezes, o melhor debug é um reboot

## Próximos Passos (Se Você Ainda Tiver Coragem)

1. [Advanced Debugging](cmd-advanced.md) (Para os verdadeiros masoquistas)
2. [PowerShell Debugging](ps-debugging.md) (Quando o CMD não for complexo o suficiente)
3. [Terapia](cmd-exercises.md) (Altamente recomendado após este módulo)

---
_"Debug é como investigação criminal: o código é sempre culpado até que se prove o contrário."_