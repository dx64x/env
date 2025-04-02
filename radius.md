Olá! Entendi sua situação, você quer controlar quem pode acessar sua rede Wi-Fi, garantindo que apenas usuários autorizados possam se conectar. A configuração de um servidor RADIUS pode ser uma solução bem eficaz para isso, pois ele permite autenticação centralizada e gerenciar quem pode ou não acessar a internet.

### Como você pode configurar isso na sua rede:

#### Equipamentos necessários:
1. **Roteadores TP-Link (já possui 2 e pode adicionar mais um)**.
2. **Switch** para conectar todos os dispositivos na rede.
3. **Laptop extra** para rodar o servidor RADIUS.

### Passos principais para configurar sua rede com RADIUS:

#### 1. **Instalar o Servidor RADIUS (FreeRADIUS) no Laptop**:
O primeiro passo é configurar o servidor RADIUS. Você pode usar o **FreeRADIUS**, que é um software gratuito e bastante robusto para autenticação, autorização e contabilidade de rede.

Aqui está como você pode instalar e configurar o FreeRADIUS no seu laptop (se for um sistema Linux, como o Ubuntu):

##### Instalar FreeRADIUS no Linux:
```bash
sudo apt update
sudo apt install freeradius
```

##### Configurar FreeRADIUS:
O arquivo de configuração principal do FreeRADIUS geralmente fica em `/etc/freeradius/3.0/`. Você vai precisar editar o arquivo `clients.conf` para adicionar os roteadores TP-Link como clientes do RADIUS e permitir que eles se autentiquem contra o servidor.

```bash
sudo nano /etc/freeradius/3.0/clients.conf
```

No arquivo `clients.conf`, adicione o seguinte para configurar os roteadores TP-Link como "clientes" do FreeRADIUS. A ideia é que o roteador envie as credenciais do usuário para o servidor RADIUS para autenticação.

```bash
client nome_do_rotoador {
    ipaddr = 192.168.0.1  # Endereço IP do roteador
    secret = "senha_rota"  # Senha que o roteador vai usar para se conectar ao RADIUS
    shortname = "roteador1"
}
```

**Repita para cada roteador.** Altere o endereço IP e a senha conforme necessário.

#### 2. **Configurar os Roteadores TP-Link**:
Nos roteadores TP-Link, você precisará configurar a autenticação via RADIUS. Isso é feito acessando a interface de configuração do roteador.

1. **Acesse o painel de administração do roteador** (geralmente em `192.168.0.1` ou `192.168.1.1`).
2. **Vá para a seção de "Wireless" (Wi-Fi)** e localize as configurações de segurança, onde você pode definir um método de autenticação.
3. **Escolha RADIUS (802.1X)** como método de autenticação.
4. Defina o endereço IP do servidor RADIUS (o IP do seu laptop), a porta (geralmente 1812) e a **senha secreta** que você definiu no arquivo `clients.conf` do FreeRADIUS.

Exemplo de configuração no roteador TP-Link:
- **Servidor RADIUS**: `192.168.0.10` (endereço IP do laptop)
- **Porta RADIUS**: `1812`
- **Senha RADIUS**: `senha_rota` (a mesma definida no FreeRADIUS)

#### 3. **Configurar os Usuários no FreeRADIUS**:
Agora, você precisa definir quem são os usuários autorizados a acessar sua rede Wi-Fi. Para isso, edite o arquivo `users` do FreeRADIUS, onde você pode adicionar usuários com nome de usuário e senha.

```bash
sudo nano /etc/freeradius/3.0/mods-config/files/authorize
```

Exemplo de usuário:
```bash
usuario1 Cleartext-Password := "senha123"
usuario2 Cleartext-Password := "senha456"
```

Esses usuários e suas senhas serão os credenciais que os dispositivos precisam fornecer para se conectar à rede.

#### 4. **Testar a Conexão**:
Depois de configurar o FreeRADIUS, os roteadores TP-Link e os usuários, é hora de testar.

- **Teste o login via Wi-Fi**: Ao tentar se conectar à rede Wi-Fi, o dispositivo pedirá um nome de usuário e uma senha.
- O **servidor RADIUS** validará essas credenciais e permitirá o acesso à internet apenas para os usuários autenticados.

#### 5. **Monitore a Rede**:
Você pode monitorar a conexão dos usuários e dispositivos diretamente pelo painel do **FreeRADIUS**. Isso é útil para garantir que não há usuários não autorizados conectados à sua rede.

### Considerações Finais:

- **Segurança**: O RADIUS oferece uma camada de segurança adicional ao exigir autenticação. Além disso, você pode usar criptografia forte nas senhas e autenticação via certificado, se necessário.
- **Redundância**: Para garantir uma configuração de alta disponibilidade, considere adicionar mais roteadores e configurar o RADIUS para equilibrar as conexões. Mas para começar, o básico que você configurou já deve funcionar bem.

### Possíveis Melhorias:
- Se você tiver mais dispositivos na rede, como a área de lazer, pode adicionar mais roteadores com a mesma configuração de RADIUS.
- Se precisar de mais segurança, configure também a criptografia WPA2 ou WPA3 no seu Wi-Fi para que a comunicação seja mais segura.

Com isso, você terá um controle muito mais rígido sobre quem pode acessar sua internet e, consequentemente, vai evitar o problema de pessoas indesejadas conectando-se à sua rede.

Se precisar de mais detalhes sobre algum passo específico, só avisar!


Sim, existe uma interface gráfica muito útil para gerenciar servidores **FreeRADIUS** de forma mais simples, sem precisar editar arquivos de configuração manualmente. Uma das mais populares é o **daloRADIUS**, que é uma interface web para administração do FreeRADIUS.

### **daloRADIUS**:
O **daloRADIUS** é uma interface web para o **FreeRADIUS**, que facilita a gestão de usuários, autenticação e configurações do servidor. Ele fornece uma maneira mais intuitiva de adicionar e gerenciar usuários, visualizar logs, e configurar o servidor RADIUS de forma amigável.

- # NEW SECTION

### Como instalar e configurar o **daloRADIUS**:

#### 1. **Instalar o daloRADIUS**:
Aqui está como você pode instalar o **daloRADIUS** no mesmo servidor onde o FreeRADIUS está rodando. Vamos assumir que você está utilizando uma distribuição baseada no Debian/Ubuntu.

##### Passo 1: Instalar as dependências

Primeiro, instale as dependências necessárias (Apache, PHP, MySQL, etc.).

```bash
sudo apt update
sudo apt install apache2 mysql-server php php-mysqli php-gd php-mbstring php-xml php-curl freeradius freeradius-mysql
```

##### Passo 2: Baixar o daloRADIUS

Baixe a versão mais recente do **daloRADIUS** diretamente do repositório oficial no GitHub.

```bash
cd /var/www/html
sudo git clone https://github.com/lirantal/daloRADIUS.git
```

##### Passo 3: Configurar o banco de dados MySQL

Agora, você precisa criar um banco de dados no MySQL para armazenar as informações de autenticação.

1. **Acesse o MySQL**:

```bash
sudo mysql -u root -p
```

2. **Crie o banco de dados e o usuário do daloRADIUS**:

```sql
CREATE DATABASE radius;
GRANT ALL PRIVILEGES ON radius.* TO 'radius'@'localhost' IDENTIFIED BY 'senha';
FLUSH PRIVILEGES;
EXIT;
```

3. **Importe o esquema do daloRADIUS**:

```bash
cd /var/www/html/daloRADIUS
sudo mysql -u root -p radius < sql/daloradius-mysql-database.sql
```

##### Passo 4: Configurar o FreeRADIUS para usar MySQL

1. **Edite a configuração do FreeRADIUS para usar MySQL**:

```bash
sudo nano /etc/freeradius/3.0/mods-available/sql
```

- Defina o tipo de banco de dados como MySQL:
  ```bash
  sql {
      driver = "mysql"
      server = "localhost"
      login = "radius"
      password = "senha"
      radius_db = "radius"
      ...
  }
  ```

2. **Ative o módulo SQL**:

```bash
sudo ln -s /etc/freeradius/3.0/mods-available/sql /etc/freeradius/3.0/mods-enabled/
```

##### Passo 5: Configurar a interface do daloRADIUS

Agora, você deve configurar o **daloRADIUS** para se conectar ao FreeRADIUS:

1. **Configure o daloRADIUS** para usar o banco de dados MySQL:

Edite o arquivo de configuração do daloRADIUS:

```bash
sudo nano /var/www/html/daloRADIUS/library/daloradius.conf.php
```

- Defina as configurações do banco de dados MySQL:
  ```php
  $configValues['CONFIG_DB_HOST'] = 'localhost';
  $configValues['CONFIG_DB_USER'] = 'radius';
  $configValues['CONFIG_DB_PASS'] = 'senha';
  $configValues['CONFIG_DB_NAME'] = 'radius';
  ```

2. **Permissões de diretório**:
Garanta que o servidor web (Apache) tenha permissões adequadas:

```bash
sudo chown -R www-data:www-data /var/www/html/daloRADIUS
```

#### 2. **Acessando a interface do daloRADIUS**:

Agora que você configurou o daloRADIUS, pode acessar a interface web. Abra o navegador e digite:

```
http://<IP_do_servidor>/daloRADIUS
```

Por exemplo, se o servidor estiver localmente, você acessaria `http://localhost/daloRADIUS`.

As credenciais padrão para login são:

- **Usuário**: `radius`
- **Senha**: `radius`

#### 3. **Usando a Interface do daloRADIUS**:

Após o login, você poderá realizar as seguintes tarefas diretamente pela interface web:

- **Adicionar/Remover Usuários**: Você pode facilmente adicionar novos usuários ao banco de dados do FreeRADIUS (e.g., novos dispositivos com suas credenciais) através da interface.
- **Gerenciar Perfis de Usuários**: Defina diferentes perfis de usuário com diferentes permissões ou limites de uso, como o tempo de navegação, tráfego, etc.
- **Visualizar Logs**: O daloRADIUS permite que você veja os logs de autenticação e autorizações em tempo real, facilitando a depuração de problemas.
- **Visualizar Estatísticas**: A interface oferece uma visão geral do uso da rede, incluindo o número de sessões ativas, usuários conectados e mais.

### Benefícios do **daloRADIUS**:

- **Facilidade de Uso**: Ao invés de editar arquivos de configuração manualmente, o daloRADIUS fornece uma interface gráfica intuitiva para gerenciar o FreeRADIUS.
- **Gestão de Usuários**: É fácil adicionar, editar ou remover usuários sem precisar mexer diretamente no banco de dados ou arquivos de configuração.
- **Relatórios e Logs**: Você pode facilmente monitorar a atividade da rede, ver quem está conectado e gerar relatórios detalhados.
- **Perfis de Usuário**: Permite criar perfis de usuários com diferentes permissões, limitando banda ou acesso de acordo com suas necessidades.

### Considerações Finais:

Usar uma interface gráfica como o **daloRADIUS** facilita muito a gestão do seu servidor RADIUS, especialmente se você não quer editar arquivos manualmente. Ele vai permitir que você tenha um controle completo e organizado da sua rede sem a complexidade de configurar tudo via linha de comando.

Se precisar de ajuda com mais detalhes sobre a configuração do **daloRADIUS**, fique à vontade para perguntar!

- # NEW SECTION

Sim, dentro de um servidor **RADIUS** (e usando o **daloRADIUS** ou qualquer outro software de gerenciamento), existem várias maneiras de **limitar a quantidade de uso**, **monitorar o uso da rede** e até **banir usuários** de forma eficaz. Vamos explorar essas opções em mais detalhes:

### 1. **Limitar a Quantidade de Uso (Bandwidth, Tempo, etc.)**
Você pode limitar o uso da rede de diversas maneiras com **FreeRADIUS** e com o **daloRADIUS**. Aqui estão as opções mais comuns:

#### a) **Limitar a Banda (Bandwidth)**:
É possível limitar a quantidade de banda (velocidade) que cada usuário pode usar ao se conectar à rede. No FreeRADIUS, você pode fazer isso utilizando o atributo **`Framed-IP-Address`** e controlando os parâmetros de **`Mikrotik-Rate-Limit`**, **`Simultaneous-Use`**, ou até mesmo **`Max-Daily-Session`**.

**Como configurar a limitação de banda**:

No arquivo de configuração do **FreeRADIUS**, você pode definir limites de banda para diferentes usuários:

1. **Configurar limite de banda no FreeRADIUS**:

   A limitação de banda pode ser configurada com atributos como **`Max-Data-Rate`** ou **`Max-Bandwidth`**. Por exemplo, você pode definir um limite de 1 Mbps de download e upload para um usuário específico.

   Para isso, edite o arquivo **`users`** do FreeRADIUS:

   ```bash
   sudo nano /etc/freeradius/3.0/mods-config/files/authorize
   ```

   E adicione os seguintes parâmetros para o usuário:

   ```bash
   usuario1 Cleartext-Password := "senha123"
   Framed-IP-Address = 192.168.1.100
   Max-Data-Rate = 1000000    # 1 Mbps de download/upload
   ```

2. **Utilizar o daloRADIUS para definir limites**:

   Se você estiver usando o **daloRADIUS**, você pode configurar as opções de **banda** diretamente na interface web. No painel do daloRADIUS, ao adicionar um novo usuário, você terá campos para definir os limites de **taxa de download** e **upload**, além de tempo de conexão.

#### b) **Limitar o Tempo de Conexão (Timeout)**:
Você também pode configurar um limite de tempo para quanto tempo um usuário pode estar conectado à rede.

1. **Definir limite de tempo no FreeRADIUS**:

   O **FreeRADIUS** permite que você defina um tempo máximo de sessão através de parâmetros como **`Max-Daily-Session`** ou **`Idle-Timeout`**.

   Exemplo de configuração para limitar o tempo diário de um usuário:

   ```bash
   usuario1 Cleartext-Password := "senha123"
   Max-Daily-Session := 3600  # Limita a 1 hora de uso por dia
   ```

#### c) **Limitar a Quantidade de Dispositivos Conectados (Simultaneous-Use)**:
Se você quiser evitar que o mesmo usuário utilize múltiplos dispositivos simultaneamente, você pode usar o parâmetro **`Simultaneous-Use`**.

1. **Configuração de número de dispositivos**:

   Se você quiser permitir que um usuário se conecte apenas de um dispositivo por vez, você pode configurar isso assim:

   ```bash
   usuario1 Cleartext-Password := "senha123"
   Simultaneous-Use := 1  # Permite apenas 1 dispositivo por vez
   ```

### 2. **Monitorar o Uso da Rede**:

O **daloRADIUS** oferece recursos de monitoramento e relatórios de utilização da rede. Ele pode fornecer informações sobre:

- **Usuários ativos**: Quem está conectado no momento.
- **Duração das sessões**: Quanto tempo os usuários estão conectados.
- **Consumo de banda**: Quanto de largura de banda está sendo usada por cada usuário.

#### a) **Relatórios no daloRADIUS**:
O daloRADIUS fornece uma seção de **"Relatórios"** onde você pode visualizar:

- **Sessões ativas**: Mostra quem está conectado no momento.
- **Uso de banda por usuário**: Você pode ver quanto de banda foi consumido por cada usuário.
- **Estatísticas de autenticação**: Exibe informações sobre o número de tentativas de login bem-sucedidas e falhadas.

#### b) **Logs do FreeRADIUS**:
Você também pode monitorar as **tentativas de autenticação** e **desconexões** diretamente nos logs do **FreeRADIUS**:

- O FreeRADIUS mantém logs detalhados em arquivos como `/var/log/freeradius/radius.log` e você pode verificar as autenticações, falhas e desconexões em tempo real.
- Para ver as informações em tempo real, você pode usar o comando:
  
  ```bash
  tail -f /var/log/freeradius/radius.log
  ```

Isso vai mostrar os eventos mais recentes no log, permitindo que você monitore ativamente as conexões da sua rede.

### 3. **Banir ou Desconectar um Usuário**:

Se você precisar **banir** ou **desconectar** um usuário específico, há várias formas de fazer isso:

#### a) **Desconectar via daloRADIUS**:
O **daloRADIUS** possui uma funcionalidade para **desconectar um usuário** da rede. Para isso:

1. Acesse a interface do **daloRADIUS**.
2. Vá até a seção de **Usuários**.
3. Localize o usuário que deseja desconectar.
4. Na interface, você pode clicar em um botão que força a desconexão do usuário, impedindo que ele continue navegando.

#### b) **Desconectar via FreeRADIUS (usando o comando radutmp)**:
Você também pode usar o comando **`radutmp`** do FreeRADIUS para desconectar um usuário de forma manual.

1. Para desconectar um usuário, execute o comando `radutmp` com o nome de usuário e a estação de rádio (se necessário).

#### c) **Banir via Blacklist**:
Se você deseja banir um usuário permanentemente, você pode adicionar seu nome de usuário à **lista negra** no **daloRADIUS** ou no **FreeRADIUS**.

1. **No FreeRADIUS**, você pode criar um script ou editar a base de dados para impedir a autenticação de determinados usuários.
2. **No daloRADIUS**, você pode facilmente desabilitar um usuário, removendo suas credenciais ou desmarcando a opção de "Ativo" no perfil do usuário.

### 4. **Automatizar Banimento com Condições Específicas**:
Você pode configurar regras automáticas no FreeRADIUS para **banir usuários** após um número determinado de falhas de autenticação, ou até mesmo para **limitar** a quantidade de tempo de conexão de forma dinâmica.

Por exemplo, para banir um usuário após 5 tentativas de login falhadas, você pode adicionar uma regra no **`/etc/freeradius/3.0/mods-enabled/expire`**:

```bash
failures = 5
```

### Resumo:

- **Limitação de Banda**: Controle a velocidade de download/upload dos usuários.
- **Limitação de Tempo**: Controle o tempo de conexão diário ou a inatividade.
- **Monitoramento**: Use **daloRADIUS** ou **logs do FreeRADIUS** para acompanhar quem está conectado e quanto estão usando.
- **Banimento**: Desconecte ou bloqueie permanentemente usuários indesejados através do **daloRADIUS** ou **FreeRADIUS**.

Com essas opções, você tem bastante flexibilidade para controlar e monitorar a sua rede, e pode facilmente agir caso algum usuário ultrapasse os limites ou cause problemas. Se precisar de mais detalhes sobre algum desses pontos ou sobre como configurar algo específico, é só falar!
