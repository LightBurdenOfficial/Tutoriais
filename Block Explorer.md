# Block Explorer

## Plataforma Utilizada: Ubuntu 15.10 e Ubuntu 16.04

### Definir senha para usuário root / Atualização de Sistema
Definir senha:
```sh
$ sudo passwd //change root password
```

Atualização de pacotes do servidor:
```sh
$ sudo apt-get update
$ sudo apt-get upgrade -y
```

Instalação do pacote `git` e `nano`:
```sh
$ sudo apt-get install nano git -y
```

### Instalação e Configuração do Node / Wallet SperoCoin
Criação de arquivo Swap - Utilize em servidores com pouca memória RAM
```sh
$ sudo fallocate -l 4G /swapfile
$ sudo chmod 600 /swapfile
$ sudo mkswap /swapfile
$ sudo swapon /swapfile
$ sudo nano /etc/fstab
```

Adicione as linhas abaixo no arquivo fstab:
```sh
/swapfile none swap sw 0 0
```

Instale as dependências para a utilização da wallet SperoCoin:
```sh
$ sudo apt-get install build-essential libboost-all-dev libcurl4-openssl-dev libdb5.3-dev libdb5.3++-dev qt-sdk libminiupnpc-dev qrencode libqrencode-dev git libtool automake autotools-dev autoconf pkg-config libssl-dev libgmp3-dev libevent-dev bsdmainutils
```

Clone o repositório da SperoCoin:
```sh
$ git clone https://github.com/DigitalCoin1/SperoCoin
```

Compile o daemon no diretório SperoCoin/src:
```sh
$ cd SperoCoin/src
$ make -f makefile.unix USE_UPNP=- USE_IPV6=1
$ strip SperoCoind
```

Inicie o daemon para a criar o arquivo de configuração dentro do diretório de dados da SperoCoin:
```sh
$ ./SperoCoind -daemon
```
Editando o arquivo SperoCoin.conf dentro do diretório .SperoCoin/SperoCoin.conf:
```sh
$ cd
$ sudo nano .SperoCoin/SperoCoin.conf
```

Adicione ao arquivo as seguintes linhas:
```sh
rpcuser=DEFINA_UM_USUARIO
rpcpassword=DEFINA_UMA_SENHA_SEGURA
listen=1
maxconnections=500
```

Certifique que o arquivo possui permissão de apenas leitura:
```sh
$ sudo chmod 400 .SperoCoin/SperoCoin.conf
$ cd SperoCoin/src
$ sudo ./SperoCoind -daemon -txindex
```

verifique se o daemon foi iniciado:
```sh
$ sudo ./SperoCoind getinfo
```

Pare o daemon:
```sh
$ sudo ./SperoCoind stop
```

### Instalação das dependências do Bloco Explorer
```sh
$ sudo apt-get update
$ sudo apt install nodejs nodejs-legacy -y
$ sudo apt-get install npm -y
```

### Instalação das dependências do Mongodb
Opção 01
```sh
$ sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv EA312927
$ echo "deb http://repo.mongodb.org/apt/ubuntu xenial/mongodb-org/3.2 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.2.list

$ sudo apt-get update
$ sudo apt-get install -y mongodb-org
$ sudo systemctl start mongod
```

Opção 02 (https://docs.mongodb.org/manual/tutorial/install-mongodb-on-ubuntu/)
```sh
$ sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv EA312927
$ echo "deb http://repo.mongodb.org/apt/ubuntu trusty/mongodb-org/3.2 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.2.list
$ sudo apt-get update
$ sudo apt-get install -y mongodb-org
$ sudo service mongod start
```

Comandos Mongodb:
```sh
$ sudo service mongod start
$ sudo service mongod stop
$ sudo service mongod restart
```
### Configuração do cliente Mongodb:
Para iniciar o cliente digite:
```sh
$ mongo
```

Para criar e selecionar uma nova db digite, onde `explorerdb` é o nome da database:
```sh
> use explorerdb
```

Para criar um usuário e definir as suas credenciais digite, onde `iquidus` é o usuário e `3xp!0reR` é a senha:
```sh
> db.createUser( { user: "iquidus", pwd: "3xp!0reR", roles: [ "readWrite" ] } )
> exit
```
### Clonando o repositório do Block Explorer:
```sh
git clone https://github.com/DigitalCoin1/BlockExplorer.git explorer
```
### Acesse o diretório e realize a instalação:
```sh
cd explorer && npm install --production
```

### Faça uma cópia do arquivo de configuração ./settings.json.template e o renomeie para ./settings.json:
```sh
cp ./settings.json.template ./settings.json
```
### Edite o arquivo de configuração do Block Explorer:
```sh
sudo nano settings.json
```

### Teste o Block Explorer:
```sh
npm start
```

### Atualize a database:
```sh
sudo node scripts/sync.js index update
```

### Para a utilização do FOREVER para manter o serviço rodando em segundo plano digite dentro do diretório de instalação do Block Explorer:
```sh
$ sudo npm install forever -g
$ sudo npm install forever-monitor
$ forever start bin/cluster
```
### Adicione as seguintes tarefas no crontab:
Instalar o gnome-schedule:
```sh
$ sudo apt-get install gnome-schedule -y
```

Editar o crontab:
```sh
$ sudo crontab -e
```

Adicione as linhas abaixo no crontab:
```sh
*/1 * * * * cd /home/SEU_USUARIO/explorer && /usr/bin/nodejs scripts/sync.js index update > /dev/null 2>&1
*/5 * * * * cd /home/SEU_USUARIO/explorer && /usr/bin/nodejs scripts/peers.js > /dev/null 2>&1
```

Fonte: https://pastebin.com/SEYVXH2C

Fonte: https://gist.github.com/zeronug/5c66207c426a1d4d5c73cc872255c572
