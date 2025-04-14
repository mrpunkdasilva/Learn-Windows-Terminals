```
    _   _    __     _____ ____    _  _____ _____ 
   | \ | |   \ \   / /_ _/ ___|  / \|_   _| ____|
   |  \| |    \ \ / / | | |  _  / _ \ | | |  _|  
   | |\  |     \ V /  | | |_| |/ ___ \| | | |___ 
   |_| \_|      \_/  |___\____/_/   \_\_| |_____|
```

# Navegação no CMD: Perdido no Terminal

> "Se você nunca se perdeu no sistema de arquivos, você não está explorando o suficiente."

## Comandos Básicos de Navegação

### Localização Atual
```batch
:: Onde estou?
cd                  :: Mostra diretório atual
echo %CD%           :: Alternativa para ver caminho atual
chdir               :: Outro comando para mostrar diretório
```

### Movimentação Básica
```batch
:: Navegação essencial
cd ..               :: Volta um nível
cd \                :: Vai para raiz do drive atual
cd /d D:           :: Muda drive e diretório
cd "Pasta Com Espacos"  :: Navega para pasta com espaços
```

## Técnicas Avançadas

### Stack de Diretórios
```batch
:: Salvando e recuperando localizações
pushd C:\Projetos   :: Salva local atual e vai para C:\Projetos
popd                :: Retorna ao último local salvo
pushd \\servidor\share  :: Mapeia rede e navega
```

### Navegação Rápida
```batch
:: Atalhos úteis
cd -                :: Alterna entre últimos diretórios
cd ..\..\..         :: Volta múltiplos níveis
cd %USERPROFILE%    :: Vai para pasta do usuário
cd %TEMP%           :: Acessa pasta temporária
```

## Listagem e Busca

### Listagem de Diretórios
```batch
:: Visualização de conteúdo
dir                 :: Lista arquivos e pastas
dir /a              :: Mostra itens ocultos
dir /b              :: Formato simples
dir /s              :: Lista recursiva
dir /p              :: Pausa por página
```

### Busca de Arquivos
```batch
:: Localização de itens
where python.exe    :: Encontra executável no PATH
dir /s /b *.txt     :: Busca recursiva de .txt
forfiles /p C:\ /m *.doc /s  :: Busca avançada
```

## Truques e Dicas

### Navegação com Wildcards
```batch
:: Usando padrões
cd *proj*           :: Entra na primeira pasta que contém "proj"
dir *2023*          :: Lista itens com "2023" no nome
cd /d D:\*\src      :: Navega para pasta src em qualquer subdiretório
```

### Aliases e Atalhos
```batch
:: Criando atalhos
doskey home=cd %USERPROFILE%
doskey back=cd ..
doskey ls=dir /b
doskey up=cd ..
```

## Mapeamento de Rede

### Conexões de Rede
```batch
:: Mapeamento de unidades
net use Z: \\servidor\share    :: Mapeia compartilhamento
subst X: C:\Projetos\Atual    :: Cria drive virtual
net use * /delete             :: Remove todos mapeamentos
```

## Troubleshooting

### Problemas Comuns
```batch
:: Resolvendo erros
cd.                 :: Corrige "Invalid directory"
cd /d .             :: Força atualização do prompt
chdir /d %path%     :: Reseta contexto do diretório
```

## Dicas de Produtividade

### Autocompletar
- `Tab`: Completa nomes de arquivos/pastas
- `F7`: Mostra histórico de comandos
- `↑/↓`: Navega pelo histórico
- `Ctrl+C`: Cancela comando atual

### Atalhos do Sistema
```batch
:: Atalhos para locais especiais
cd %APPDATA%        :: Pasta de dados de aplicativos
cd %ProgramFiles%   :: Pasta Program Files
cd %SystemRoot%     :: Pasta do Windows
cd %TEMP%           :: Pasta temporária
```

## Checklist de Navegação

- [ ] Conhecer localização atual
- [ ] Usar comandos básicos com confiança
- [ ] Dominar navegação com wildcards
- [ ] Configurar aliases úteis
- [ ] Mapear drives necessários

## Próximos Passos

1. [Operações com Arquivos](cmd-file-operations.md)
2. [Comandos do Sistema](cmd-system-commands.md)
3. [Automação](cmd-automation.md)

---
_"No CMD, não existem caminhos errados, apenas desvios não planejados."_