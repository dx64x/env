### Passo 1: Instalar o tmux
1. Abra o terminal no Ubuntu (Ctrl + Alt + T).
2. Atualize a lista de pacotes e instale o **tmux**:
   ```bash
   sudo apt update && sudo apt upgrade && sudo apt autoremove && sudo apt autoclean
   sudo apt install tmux xclip xsel
   ```

### Passo 2: Testar o tmux
Para testar se o **tmux** foi instalado corretamente, execute o comando:
```bash
tmux
```
Isso deve iniciar uma sessão do tmux. Você verá a interface do tmux no terminal.

### Passo 3: Configurar o tmux (opcional)
O tmux tem várias configurações que podem ser ajustadas. Para personalizar a configuração, edite o arquivo `~/.tmux.conf`. Se o arquivo não existir, você pode criá-lo:
1. Abra o arquivo `~/.tmux.conf` em um editor de texto:
   ```bash
   nano ~/.tmux.conf
   ```
2. Adicione algumas configurações básicas.
3. Salve o arquivo (Ctrl + O) e saia do editor (Ctrl + X)
   ```bash
   tmux source ~/.tmux.conf
   ```

### Passo 4: Tornar o tmux padrão no gnome-terminal
Agora, vamos configurar o **gnome-terminal** para abrir automaticamente o tmux sempre que você abrir uma nova janela de terminal.

1. Abra o **gnome-terminal**.
2. Vá em **Preferências** (clique no menu no canto superior direito da janela do terminal e escolha "Preferências").
3. Selecione o **perfil** que você usa (geralmente "Sem nome" ou "Padrão").
4. No menu da esquerda, clique em **Comando**.
5. Marque a opção **Executar um comando personalizado ao iniciar**.
6. No campo de texto, digite:
   ```bash
   tmux
   ```
7. Feche a janela de configurações.

Agora, sempre que você abrir o **gnome-terminal**, o tmux será iniciado automaticamente.

# Tmux Plugin Manager

Installs and loads `tmux` plugins.

Tested and working on Linux, OSX, and Cygwin.

See list of plugins [here](https://github.com/tmux-plugins/list).

### Installation

Requirements: `tmux` version 1.9 (or higher), `git`, `bash`.

Clone TPM:

```bash
git clone https://github.com/tmux-plugins/tpm ~/.tmux/plugins/tpm
```
