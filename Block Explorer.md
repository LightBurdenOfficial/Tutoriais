# Block Explorer

## Plataforma Utilizada: Ubuntu 16

### Instalação das dependências
```sh
$ sudo apt-get update
$ sudo apt install nodejs-legacy
$ sudo apt-get install npm
$ sudo apt-get install

$ sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv EA312927
$ echo "deb http://repo.mongodb.org/apt/ubuntu xenial/mongodb-org/3.2 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.2.list

$ sudo apt-get update
$ sudo apt-get install -y mongodb-org
$ sudo systemctl start mongod
```
### Configuração do cliente Mongodb:
Para iniciar o cliente digite: `mongo`
Para criar e selecionar uma nova db digite: `use explorerdb` - onde explorerdb é o nome da database
Para criar um usuário e definir as suas credenciais digite: `db.createUser( { user: "iquidus", pwd: "3xp!0reR", roles: [ "readWrite" ] } )` - onde "iquidus" é o usuário e "3xp!0reR" é a senha
Digite `exit` para sair

### Clonando o repositório do Block Explorer:
```sh
git clone https://github.com/iquidus/explorer explorer
```
### Acesse o diretório e realize a instalação:
```sh
cd explorer && npm install --production
```

### Faça uma cópia do arquivo de configuração ./settings.json.template e o renomeie para ./settings.json:
```sh
cp ./settings.json.template ./settings.json
```

### Para a utilização do FOREVER para manter o serviço rodando em segundo plano digite dentro do diretório de instalação do Block Explorer:
```sh
forever start -c "npm start" ./
```
### Adicione as seguintes tarefas no crontab:
```sh
*/1 * * * * cd explorer && /usr/bin/nodejs scripts/sync.js index update > /dev/null 2>&1
*/5 * * * * cd explorer && /usr/bin/nodejs scripts/peers.js > /dev/null 2>&1
```

Fonte: https://pastebin.com/SEYVXH2C
