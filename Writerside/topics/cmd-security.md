```
    ____                      _ _         
   / ___|  ___  ___ _   _ __(_) |_ _   _ 
   \___ \ / _ \/ __| | | | '__| | __| | | |
    ___) |  __/ (__| |_| | |  | | |_| |_| |
   |____/ \___|\___|\__,_|_|  |_|\__|\__, |
                                     |___/ 
```

# Segurança no CMD: Porque Paranoia é Apenas Bom Senso

> "Em um mundo onde até sua torradeira pode ser hackeada, seu terminal é a última linha de defesa."

## A Ilusão da Segurança

Ah, você achava que estava seguro só porque usa Windows? Que fofo. Vamos mergulhar no submundo da segurança do CMD, onde cada comando é uma roleta russa digital.

## Permissões: Seu Primeiro Pesadelo

```batch
:: Verificando permissões (ou a falta delas)
icacls "C:\Secrets"
:: Spoiler: Você provavelmente não tem acesso ao que precisa
```

### Elevação de Privilégios (Ou: Como Se Tornar um Deus Digital)

```batch
:: O famoso "Execute como Administrador"
runas /user:Administrator cmd.exe
:: Parabéns, agora você tem poder. Use com sabedoria... ou não.
```

## Criptografia para Paranoicos

```batch
:: Encriptando arquivos (porque você nunca sabe)
cipher /e /s:C:\ProjetosSecretos

:: Descriptografando (quando a paranoia passar)
cipher /d /s:C:\ProjetosSecretos
```

### Sanitização de Dados (Ou: Como Dormir à Noite)

```batch
:: Limpeza segura (modo paranoia)
@echo off
set "file=segredos.txt"
for /l %%i in (1,1,7) do (
    copy /y NUL %file%
    del /f /q %file%
)
:: Se você ainda está preocupado, queime o HD
```

## Firewall: Seu Amigo Neurótico

```batch
:: Configurando o firewall (porque sim)
netsh advfirewall firewall add rule name="Block_Evil" dir=in action=block protocol=TCP localport=666
:: Agora você está "seguro" (use aspas mentais aqui)
```

## Monitoramento de Atividades Suspeitas

```batch
:: Quem está te observando?
netstat -anb | findstr "ESTABLISHED"
:: Spoiler: Todo mundo está te observando
```

### Log de Eventos (Ou: Seu Diário Digital Paranóico)

```batch
:: Verificando logs (prepare-se para pesadelos)
wevtutil qe Security /c:100 /f:text
:: Se você entendeu algo, parabéns, você é um unicórnio
```

## Dicas de Sobrevivência Digital

1. **Nunca confie em scripts baixados da internet**
   (Mas você vai baixar mesmo assim, não é?)

2. **Sempre verifique hashes**
   ```batch
   certutil -hashfile suspicious.exe SHA256
   :: Se os hashes não baterem... bem, você já estava infectado mesmo
   ```

3. **Backup é para os fracos**
   (Mentira, faça backup de tudo, três vezes)

## Checklist do Paranoico Digital

- [ ] Firewall ativado
- [ ] Antivírus atualizado
- [ ] Permissões verificadas
- [ ] Chapéu de papel alumínio colocado
- [ ] Câmera coberta com fita
- [ ] Terminal rodando em modo seguro
- [ ] Paranoia em níveis saudáveis

## Conclusão

> "Se você não está paranóico com segurança, é porque não está prestando atenção."

## Próximos Passos

1. [PowerShell Security](ps-security.md) (Para quando o CMD não for paranoico o suficiente)
2. [Advanced Security](cmd-advanced.md) (Para os verdadeiramente perturbados)
3. [Troubleshooting](troubleshooting.md) (Para quando tudo der errado, e vai dar)

---
_"Em um mundo digital, paranoia não é um bug, é uma feature."_