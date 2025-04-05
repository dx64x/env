# CONFIGURAR AMBIENTE DEV – GITHUB COMMITS

## Actualização do Sistema
```bash
sudo apt update && sudo apt upgrade && sudo apt autoremove && sudo apt autoclean
```

## Configuração Básica do Git
```bash
git config --global user.name "Seu Nome"
git config --global user.email "seuemail@exemplo.com"
ssh-keygen -t rsa -b 4096 -C "seuemail@exemplo.com"
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_rsa
cat ~/.ssh/id_rsa.pub
```
*Copiar chave gerada e ir inserir no github*
  
```bash
ssh -T git@github.com
```

## Configurar Usuário Root
```bash
sudo -i
mkdir -p /root/.ssh
cp /home/seu_usuario/.ssh/id_rsa* /root/.ssh/
chmod 700 /root/.ssh
chmod 600 /root/.ssh/id_rsa
chmod 644 /root/.ssh/id_rsa.pub
eval "$(ssh-agent -s)"
ssh-add /root/.ssh/id_rsa
ssh -T git@github.com
```

## Configurar Commits Assinados
```bash
sudo apt update && sudo apt upgrade
sudo apt install gnupg
gpg --full-generate-key
```

O comando irá pedir algumas informações:
- Tipo de chave: Escolha o padrão, que é `RSA and RSA` (opção 1).
- Tamanho da chave: O padrão é 2048 bits, mas recomenda-se 4096 bits para maior segurança. Então, digite `4096`.
- Data de validade: Se você quiser que a chave expire, pode escolher uma data ou optar por nunca expirar (digite `0`).
- Nome e email: Insira seu nome e o mesmo email associado ao seu GitHub.
- Senha: Escolha uma senha segura para proteger a chave.

```bash
gpg --list-secret-keys --keyid-format LONG
gpg --armor --export <chave_long>
```

*Copiar chave gerada e ir inserir no github*

```bash
git config --global user.signingkey <chave_long>
git config --global commit.gpgSign true
```

*Testar o commit assinado*

## Configurar o root
```bash
sudo nano /root/.bashrc
```

Adicionar as seguintes linhas:
```bash
export GNUPGHOME=/home/usuario/.gnupg
git config --global user.signingkey <chave_long>
git config --global commit.gpgSign true
```
