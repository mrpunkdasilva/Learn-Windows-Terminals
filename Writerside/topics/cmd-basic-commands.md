```
           _____                              _           
          / ____|                            | |          
         | |     ___  _ __ ___   __ _ _ __  | |__   ___  
         | |    / _ \| '_ ` _ \ / _` | '_ \ | '_ \ / _ \ 
         | |___| (_) | | | | | | (_| | | | || |_) | (_) |
          \_____\___/|_| |_| |_|\__,_|_| |_||_.__/ \___/ 
```

# Comandos Básicos: Seu Arsenal Inicial

> "Conhecimento é poder. Poder é controle. Controle é tudo."

## Navegação no Sistema

```batch
:: Navegação básica
cd          :: Mostra diretório atual
cd ..       :: Volta um nível
cd /d D:    :: Muda drive e diretório
pushd       :: Salva diretório atual na pilha
popd        :: Retorna ao último diretório salvo
```

## Listagem e Busca

```batch
:: Listagem avançada
dir /a      :: Mostra arquivos ocultos
dir /s      :: Lista recursivamente
dir /b /s   :: Formato limpo e recursivo

:: Busca ninja
where python        :: Localiza executável
findstr /s /i TODO  :: Busca TODO em todos arquivos
```

## Manipulação de Arquivos

```batch
:: Operações básicas
copy src.txt dst.txt   :: Copia arquivo
move old.* new\        :: Move arquivos
del *.tmp              :: Deleta temporários
ren old.js new.js      :: Renomeia arquivo

:: Operações avançadas
robocopy src dst /MIR  :: Sincronização espelho
xcopy /s /e /h        :: Copia preservando atributos
```

## Processos e Rede

```batch
:: Gerenciamento de processos
tasklist | findstr "node"   :: Lista processos node
taskkill /IM node.exe /F    :: Mata processo
start "" "http://localhost" :: Abre navegador

:: Comandos de rede
netstat -ano              :: Portas em uso
ipconfig /all            :: Config de rede
ping -t localhost       :: Ping contínuo
```

## Informações do Sistema

```batch
:: Debug e diagnóstico
systeminfo | findstr "Memory"  :: Info de memória
wmic cpu get name             :: Info do processador
ver                          :: Versão do Windows
```

## Pipes e Redirecionamento

```batch
:: Manipulação de output
dir > lista.txt           :: Salva em arquivo
type file.txt | sort     :: Ordena conteúdo
findstr "erro" log.txt   :: Filtra conteúdo
```

## Scripts Rápidos para Dev

```batch
:: Limpeza de builds
@echo off
for /d /r . %%d in (node_modules) do @if exist "%%d" rmdir /s /q "%%d"
for /d /r . %%d in (dist) do @if exist "%%d" rmdir /s /q "%%d"

:: Monitor de porta
:check
netstat -ano | findstr ":3000"
timeout /t 5
goto check
```

## Atalhos de Produtividade

| Comando | Descrição | Uso Dev |
|---------|-----------|---------|
| `doskey h=history` | Alias para histórico | Reutilizar comandos |
| `cls` | Limpa tela | Manter terminal limpo |
| `echo %PATH%` | Mostra path | Debug de ambiente |

## Dicas Avançadas

```batch
:: Execução silenciosa
start /b programa.exe > nul 2>&1

:: Captura de erros
comando.exe 2> erro.log

:: Execução condicional
comando1.exe && comando2.exe || comando3.exe
```

## Próximos Passos

1. [Operações com Arquivos](cmd-file-operations.md)
2. [Scripts Batch](cmd-batch-scripting.md)
3. [Networking Avançado](cmd-networking.md)

---
_"Mestre dos comandos, mestre do sistema."_