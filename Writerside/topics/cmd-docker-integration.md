```
    ____   ___   ____ _  _______ ____  
   |  _ \ / _ \ / ___| |/ / ____|  _ \ 
   | | | | | | | |   | ' /|  _| | |_) |
   | |_| | |_| | |___| . \| |___|  _ < 
   |____/ \___/ \____|_|\_\_____|_| \_\
```

# Docker Integration com CMD: Containers de Caos

> "Docker é como uma caixa surpresa: você nunca sabe o que vai quebrar quando abrir."

## Configuração Inicial (Ou: Preparando o Ambiente para o Desastre)

### Verificação do Ambiente
```batch
@echo off
:: Checando se o Docker está instalado (e rezando)
docker --version
if errorlevel 1 (
    echo Docker não encontrado!
    echo Você esqueceu de instalar a fonte de todos os seus problemas futuros
    goto :installDocker
)
```

### Status Check
```batch
:: Verificando se o daemon está vivo
:checkDaemon
docker info > nul 2>&1
if errorlevel 1 (
    echo O daemon Docker está morto... de novo
    echo Iniciando ritual de ressurreição
    start "" "C:\Program Files\Docker\Docker\Docker Desktop.exe"
    timeout /t 30 /nobreak
    goto :checkDaemon
)
```

## Gerenciamento de Containers (Ou: Malabarismo com Caixas Virtuais)

### Container Lifecycle
```batch
:: Script para gerenciar o ciclo de vida dos containers
:containerManager
set "CONTAINER_NAME=container-do-caos"

:: Criando um container (o que poderia dar errado?)
docker run -d --name %CONTAINER_NAME% nginx
if errorlevel 1 (
    echo Container recusou-se a iniciar
    echo Tentando convencê-lo educadamente...
    docker rm -f %CONTAINER_NAME% 2>nul
    goto :containerManager
)
```

### Container Cleanup
```batch
:: Limpeza de containers zumbis
:zombieHunter
echo Iniciando caça aos containers mortos-vivos...
docker container prune -f
echo Se algo importante foi deletado, foi mal
```

## Imagens e Builds (Ou: Arte Abstrata em Camadas)

### Build Automation
```batch
:: Script de build com tentativas infinitas
:buildImage
set "IMAGE_NAME=app-que-funciona-local"
set "TAG=versao-otimista"

echo Tentativa de build #%ATTEMPT%
docker build -t %IMAGE_NAME%:%TAG% .
if errorlevel 1 (
    echo Build falhou... que surpresa
    set /a ATTEMPT+=1
    if %ATTEMPT% leq 3 (
        echo Tentando novamente, porque insistência é a chave
        goto :buildImage
    ) else (
        echo Desistindo... por enquanto
        goto :panicMode
    )
)
```

### Image Cleanup
```batch
:: Liberando espaço (e deletando evidências)
:imageJanitor
echo Procurando imagens para "otimizar"...
docker image prune -a --force
echo Seu HD agradece
echo (Mas seu time de desenvolvimento não)
```

## Networking (Ou: Labirinto de Conexões)

### Network Creation
```batch
:: Criando redes Docker (mais uma camada de complexidade)
:networkCreator
docker network create rede-do-caos
if errorlevel 1 (
    echo A rede se recusa a existir
    echo Tentando plano B...
    goto :useHostNetwork
)
```

### Container Connectivity
```batch
:: Testando conectividade (ou falta dela)
:connectivityCheck
docker run --rm busybox ping -c 1 google.com
if errorlevel 1 (
    echo Internet não encontrada
    echo Culpando o firewall...
)
```

## Volume Management (Ou: Persistência é Superestimada)

### Volume Creation
```batch
:: Criando volumes (porque perder dados é divertido)
:volumeManager
docker volume create dados-importantes
echo Volume criado com sucesso
echo (Boa sorte encontrando onde)
```

### Backup Volumes
```batch
:: Script de backup (para quando der tudo errado)
:backupVolumes
set "BACKUP_DIR=backup_volumes_%date:~-4%%date:~3,2%%date:~0,2%"
mkdir %BACKUP_DIR%

docker run --rm -v dados-importantes:/data -v %cd%/%BACKUP_DIR%:/backup alpine tar cvf /backup/backup.tar /data
echo Backup concluído
echo (Espero que você não precise dele)
```

## Troubleshooting (Ou: Guia de Sobrevivência)

### Log Analysis
```batch
:: Analisando logs (ou tentando)
:logDiver
docker logs --tail 100 %CONTAINER_NAME%
echo Se você entendeu esses logs, parabéns
echo Você é oficialmente um especialista Docker
```

### Container Debug
```batch
:: Modo debug desesperado
:debugMode
echo Entrando em modo de depuração...
docker exec -it %CONTAINER_NAME% /bin/sh
echo Se isso funcionou, foi sorte
```

## Checklist de Sobrevivência Docker

- [ ] Docker Desktop instalado (e reiniciado 3 vezes)
- [ ] Containers rodando (alguns deles)
- [ ] Volumes montados (em algum lugar)
- [ ] Redes configuradas (teoricamente)
- [ ] Logs verificados (e ignorados)
- [ ] Stack Overflow como página inicial

## Próximos Passos (Se Ainda Tiver Coragem)

1. [Cloud Integration](cmd-cloud-integration.md) (Para problemas em escala global)
2. [Kubernetes](cmd-exercises.md) (Porque Docker sozinho não é complexo o suficiente)
3. [Terapia](cmd-exercises.md) (Seriamente, você vai precisar)

---
_"Docker é como uma cebola: tem várias camadas e provavelmente vai te fazer chorar."_