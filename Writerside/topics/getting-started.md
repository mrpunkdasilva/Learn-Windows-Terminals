```
     ____ _____ _____ _____ ___ _   _  ____ 
    / ___| ____|_   _|_   _|_ _| \ | |/ ___|
   | |  _|  _|   | |   | |  | ||  \| | |  _ 
   | |_| | |___  | |   | |  | || |\  | |_| |
    \____|_____| |_|   |_| |___|_| \_|\____|
    ____ _____  _    ____ _____ _____ ____  
   / ___|_   _|/ \  |  _ \_   _| ____|  _ \ 
   \___ \ | | / _ \ | |_) || | |  _| | | | |
    ___) || |/ ___ \|  _ < | | | |___| |_| |
   |____/ |_/_/   \_\_| \_\|_| |_____|____/ 
```

# Começando com Terminais Windows

> "O começo de toda jornada começa com um simples comando."

## Pré-requisitos

- Windows 10/11
- Permissões de administrador (opcional)
- Vontade de aprender
- ☕ Café (muito café)

## Escolhendo seu Terminal

### CMD
```batch
:: Para iniciantes
cmd.exe
```
- Interface clássica
- Presente em todo Windows
- Ótimo para começar

### PowerShell
```powershell
# Para usuários avançados
pwsh.exe
```
- Mais poderoso
- Orientado a objetos
- Melhor para automação

## Primeiros Passos

### 1. Abrindo o Terminal

Métodos rápidos:
- `Windows + R`, digite `cmd` ou `powershell`
- `Windows + X`, selecione Terminal
- Busque "Terminal" no menu iniciar

### 2. Configuração Básica

```batch
:: CMD - Configuração inicial
chcp 65001          :: UTF-8
color 0A            :: Verde sobre preto
mode con: cols=120  :: Largura confortável
```

```powershell
# PowerShell - Configuração inicial
Set-ExecutionPolicy RemoteSigned
Install-Module PSReadLine
```

### 3. Comandos Essenciais

#### Navegação
```batch
:: Comandos universais
cd              # Mostra diretório atual
dir             # Lista arquivos
cls             # Limpa tela
help            # Mostra ajuda
```

#### Sistema
```batch
:: Informações básicas
systeminfo      # Info do sistema
tasklist       # Processos ativos
ipconfig       # Config de rede
```

## Estrutura do Curso

### Fase 1: Fundamentos
1. [CMD Básico](cmd-introduction.md)
2. [PowerShell Básico](ps-introduction.md)
3. [Navegação](cmd-navigation.md)

### Fase 2: Prática
1. [Exercícios CMD](cmd-exercises.md)
2. [Exercícios PowerShell](ps-exercises.md)
3. [Projetos Práticos](terminal-projects.md)

## Dicas para Iniciantes

### Boas Práticas
- Comece com CMD
- Pratique diariamente
- Mantenha anotações
- Use o `help` frequentemente
- Não tenha medo de errar

### Evite
- Copiar sem entender
- Pular conceitos básicos
- Executar scripts desconhecidos
- Ignorar mensagens de erro

## Ferramentas Recomendadas

### Essenciais
- Windows Terminal
- Visual Studio Code
- Git Bash
- Notepad++

### Opcionais
- ConEmu
- Cmder
- Terminal-Icons
- Oh My Posh

## Próximos Passos

1. [Introdução ao CMD](cmd-introduction.md)
   - Comandos básicos
   - Navegação
   - Scripts simples

2. [Introdução ao PowerShell](ps-introduction.md)
   - Cmdlets básicos
   - Pipeline
   - Objetos

3. [Comparação de Terminais](terminal-comparison.md)
   - Quando usar cada um
   - Vantagens e desvantagens
   - Casos de uso

## Recursos Adicionais

- [Documentação Microsoft](https://docs.microsoft.com)
- [Learn PowerShell](https://learn.microsoft.com/powershell)
- [CMD Reference](https://ss64.com/nt)
- [PowerShell Gallery](https://www.powershellgallery.com)

## Checklist do Iniciante

- [ ] Instalar Windows Terminal
- [ ] Configurar ambiente básico
- [ ] Aprender comandos essenciais
- [ ] Completar exercícios iniciais
- [ ] Criar primeiro script

---
_"Todo expert já foi iniciante. O segredo é começar."_