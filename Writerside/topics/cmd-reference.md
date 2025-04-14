# Referência CMD

> "Todo comando tem seu propósito, todo parâmetro sua razão."

## Comandos Essenciais

### Navegação
```batch
cd [path]        :: Muda diretório
dir [options]    :: Lista arquivos
path [options]   :: Mostra/define PATH
```

### Arquivos
```batch
copy [src] [dst] :: Copia arquivos
move [src] [dst] :: Move arquivos
del [file]       :: Deleta arquivos
ren [old] [new]  :: Renomeia
```

### Sistema
```batch
tasklist        :: Lista processos
taskkill        :: Termina processo
systeminfo      :: Info sistema
sfc             :: System File Checker
```

## Parâmetros Comuns

### Diretórios
| Parâmetro | Descrição |
|-----------|-----------|
| `/s` | Recursivo |
| `/a` | Atributos |
| `/b` | Básico |
| `/p` | Pausa |

### Arquivos
| Parâmetro | Uso |
|-----------|-----|
| `/y` | Confirma |
| `/v` | Verbose |
| `/q` | Quiet |
| `/f` | Força |

## Operadores

### Redirecionamento
```batch
>   :: Saída
>>  :: Append
<   :: Entrada
|   :: Pipe
```

### Condicionais
```batch
&&  :: AND
||  :: OR
^   :: Escape
```

## Variáveis Ambiente

### Sistema
```batch
%PATH%          :: Caminhos
%SYSTEMROOT%    :: Windows
%TEMP%          :: Temporário
%USERPROFILE%   :: Usuário
```

### Customizadas
```batch
set VAR=valor   :: Define
echo %VAR%      :: Usa
setx VAR valor  :: Permanente
```

## Códigos de Retorno

| Código | Significado |
|--------|-------------|
| 0 | Sucesso |
| 1 | Erro geral |
| 2 | Arquivo não encontrado |
| 3 | Path não encontrado |

## Exemplos Práticos

### Backup
```batch
:: Backup com data
for /f "tokens=1-3 delims=/ " %%a in ('%date%') do (
    set BACKUP=backup_%%c%%a%%b
)
xcopy /s /i "source" "%BACKUP%"
```

### Limpeza
```batch
:: Limpa temporários
del /s /q %temp%\*.*
for /d %%x in (%temp%\*) do rd /s /q "%%x"
```

## Troubleshooting

### Erros Comuns
1. 'X' não é reconhecido
   - Verificar PATH
   - Confirmar extensão

2. Acesso negado
   - Executar como admin
   - Verificar permissões

## Dicas Avançadas

### Performance
```batch
:: Desativa echo
@echo off
:: Processa rápido
setlocal enabledelayedexpansion
```

### Debugging
```batch
:: Debug mode
echo on
set
pause
```

---
_"Um comando bem documentado vale por mil tentativas e erros."_