# Exercícios CMD: Hora de Praticar

> "A prática leva à perfeição. Ou pelo menos a um histórico de comandos mais interessante."

## 🎯 Exercícios Básicos

### Navegação (Nível: Iniciante)
1. **Explorador de Diretórios**
   ```batch
   :: Crie esta estrutura e navegue entre as pastas
   /projeto
     /src
       /components
       /utils
     /docs
     /tests
   ```
   - [ ] Crie a estrutura usando apenas `md` e `cd`
   - [ ] Navegue até `/components` usando caminho relativo
   - [ ] Volte para raiz usando `cd` com caminho absoluto
   - [ ] Liste todos arquivos recursivamente

2. **Gerenciador de Backup**
   - [ ] Crie pasta `backup_YYYY_MM_DD`
   - [ ] Copie todos `.txt` para backup
   - [ ] Liste arquivos copiados
   - [ ] Compare tamanhos originais e cópias

## 🚀 Exercícios Intermediários

### Manipulação de Arquivos (Nível: Médio)
1. **Organizador de Downloads**
   ```batch
   :: Crie script que:
   :: 1. Entre na pasta Downloads
   :: 2. Crie pastas por extensão
   :: 3. Mova arquivos automaticamente
   ```
   - [ ] Identifique extensões únicas
   - [ ] Crie pastas necessárias
   - [ ] Mova arquivos preservando nomes
   - [ ] Gere log de operações

2. **Monitor de Alterações**
   - [ ] Monitore pasta específica
   - [ ] Registre mudanças em log
   - [ ] Notifique arquivos novos
   - [ ] Mantenha histórico de 7 dias

## 💪 Exercícios Avançados

### Automação (Nível: Expert)
1. **Deploy Automatizado**
   ```batch
   :: Crie sistema que:
   :: 1. Faça backup do atual
   :: 2. Copie novos arquivos
   :: 3. Valide a cópia
   :: 4. Permita rollback
   ```
   - [ ] Implemente verificação MD5
   - [ ] Adicione log detalhado
   - [ ] Crie sistema de rollback
   - [ ] Notifique resultado

2. **Monitor de Sistema**
   - [ ] Monitore CPU e memória
   - [ ] Alerte uso alto (>80%)
   - [ ] Registre processos ativos
   - [ ] Gere relatório diário

## 🔧 Projetos Práticos

### Projeto 1: Sistema de Backup
```batch
@echo off
:: Implemente backup incremental
:: - Compare datas de modificação
:: - Copie apenas alterados
:: - Mantenha estrutura
:: - Gere relatório
```

### Projeto 2: Ambiente Dev
```batch
@echo off
:: Configure ambiente desenvolvimento
:: - Clone repositório
:: - Instale dependências
:: - Configure variáveis
:: - Inicie serviços
```

## 🎮 Desafios

### Desafio 1: File Finder
- Crie buscador de arquivos avançado
- Suporte wildcards e regex
- Permita filtros (data, tamanho)
- Gere relatório formatado

### Desafio 2: Log Analyzer
- Processe arquivos de log
- Extraia estatísticas
- Identifique padrões
- Gere alertas

## ✅ Checklist de Habilidades

### Básico
- [ ] Navegação fluente
- [ ] Manipulação de arquivos
- [ ] Comandos essenciais
- [ ] Redirecionamento básico

### Intermediário
- [ ] Scripts simples
- [ ] Variáveis e condições
- [ ] Loops básicos
- [ ] Processamento de texto

### Avançado
- [ ] Funções complexas
- [ ] Tratamento de erros
- [ ] Networking
- [ ] Automação completa

## 📝 Avaliação

### Projeto Final
Crie uma ferramenta que combine:
1. Monitoramento de sistema
2. Backup automático
3. Log processing
4. Relatórios
5. Interface amigável

### Critérios
- Funcionalidade
- Tratamento de erros
- Documentação
- Performance
- Manutenibilidade

## 🎓 Certificação

Complete todos exercícios para:
1. Domínio básico do CMD
2. Automação eficiente
3. Troubleshooting avançado
4. Integração com ferramentas

## 📚 Recursos Adicionais

- [Documentação Microsoft](https://docs.microsoft.com)
- [Batch Script Tutorial](https://www.tutorialspoint.com/batch_script/)
- [Fórum de Suporte](https://stackoverflow.com)

---
_"A melhor forma de aprender é quebrando coisas... e depois consertando."_