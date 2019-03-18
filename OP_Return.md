# OP_RETURN
## PREPARANDO AS INFORMAÇÕES INICIAIS
1 - Você deverá localizar o TXID e o VOUT que será utilizado na sua transação, vamos abrir primeiro a Janela de Depuração.

2 - Localize o endereço que será utilizado para enviar as SperoCoins.

3 - Copie o endereço.

4 - Com o endereço em mãos, retornaremos a Janela de Depuração. Lá iremos enviar o seguinte comando:
```sh
listunspent 0 999999 '["SjJnmjRhqzPBbfN2aWTT9MJRMyTUF4N9rX"]'
```
O retorno será:
```sh
[
{
"txid" : "97e4a0b3eec93229034477dd7bb3eb54466195148d22b321ac6a5ca030d015e9",
"vout" : 1,
"address" : "SjJnmjRhqzPBbfN2aWTT9MJRMyTUF4N9rX",
"account" : "OP_RETURN_ADDRESS",
"scriptPubKey" : "76a914f178f2da8d2efcf00ff4650d073024e648b0879488a",
"amount" : 20.00000000,
"confirmations" : 33
}
]
```
Já temos agora em mãos o TXID e o VOUT:

> TXID = 97e4a0b3eec93229034477dd7bb3eb54466195148d22b321ac6a5ca030d015e9<br>
> ENDEREÇO = SjJnmjRhqzPBbfN2aWTT9MJRMyTUF4N9rX<br>
> VOUT = 1<br>

## MONTANDO A TRANSAÇÃO

1 - Envie o seguinte comando na Janela de Depuração, substituindo as informações do VOUT e da TXID conforme o seu retorno no passo anterior:

```sh
createrawtransaction '[{"txid":"97e4a0b3eec93229034477dd7bb3eb54466195148d22b321ac6a5ca030d015e9","vout":1}]' '{"data":"4573736120736572612061206d656e736167656d20717565206972656d6f7320656e7669617220646520666f726d6120656e6372697074616461","SeNu516XDPtSjJ9B6Kz8YqXpzF2gkpxmDJ":1,"SjJnmjRhqzPBbfN2aWTT9MJRMyTUF4N9rX":18.9999}'
```

O retorno será como: 

> 01000000ab00905c01e915d030a05c6aac21b3228d1495614654ebb37bdd7744032932c9eeb3a0e4970100000000ffffffff0300000000000000003c6a3a4573736120736572612061206d656e736167656d20717565206972656d6f7320656e7669617220646520666f726d6120656e637269707461646100e1f505000000001976a914bb673d9d56b0de8199f6bbd6147a0aa906da407b88acf08b3f71000000001976a914f178f2da8d2efcf00ff4650d073024e648b0879488ac00000000


O que está dentro das aspas em "data" é a mensagem convertida em HEXADECIMAL, para converter uma string ou decodificar foi utilizado o site: http://string-functions.com/string-hex.aspx

> "data":"4573736120736572612061206d656e736167656d20717565206972656d6f7320656e7669617220646520666f726d6120656e6372697074616461"

Aqui a transação já foi criada com a mensagem dentro da mesma, precisamos agora realizar a assinatura da transação.

Observação importante: A transação possui um saldo de :
> "amount" : 20.00000000,

Então colocamos o endereço de destino e o valor a ser enviado:
> SeNu516XDPtSjJ9B6Kz8YqXpzF2gkpxmDJ:1

Endereço de Troco já descontando a taxa da rede:
> SjJnmjRhqzPBbfN2aWTT9MJRMyTUF4N9rX:18.9999

Ou seja, estamos enviando 1 SPERO para o endereço SeNu516XDPtSjJ9B6Kz8YqXpzF2gkpxmDJ e estamos recebendo de troco 18.9999 SPERO no endereço Sg9Xi9Q2W91j1e6r2YmU5Yq6coLf1Pbozx.

## ASSINANDO A TRANSAÇÃO

1 - Para assinar a transação utilize o comando:
```sh
signrawtransaction 01000000ab00905c01e915d030a05c6aac21b3228d1495614654ebb37bdd7744032932c9eeb3a0e4970100000000ffffffff0300000000000000003c6a3a4573736120736572612061206d656e736167656d20717565206972656d6f7320656e7669617220646520666f726d6120656e637269707461646100e1f505000000001976a914bb673d9d56b0de8199f6bbd6147a0aa906da407b88acf08b3f71000000001976a914f178f2da8d2efcf00ff4650d073024e648b0879488ac00000000
```

Segue o retorno:
```sh
{
"hex" : "01000000ab00905c01e915d030a05c6aac21b3228d1495614654ebb37bdd7744032932c9eeb3a0e497010000006a47304402204e7e3dd2bf5c4291517301993a3ef5c3a671d768fd35c78d89ef1279f35f69330220706f79098bee1772ea12dd62af4dbdadc3a30aa8a8f287cf12e67fdd741aed9c01210214e5eedd2e8ca38dc26345b2d294665a354cc9ff5b1ec12f9469df95701d19f5ffffffff0300000000000000003c6a3a4573736120736572612061206d656e736167656d20717565206972656d6f7320656e7669617220646520666f726d6120656e637269707461646100e1f505000000001976a914bb673d9d56b0de8199f6bbd6147a0aa906da407b88acf08b3f71000000001976a914f178f2da8d2efcf00ff4650d073024e648b0879488ac00000000",
"complete" : true
}
```

Se retornar TRUE, quer dizer que a transacao foi assinada corretamente , se retornar false, a transação tem algo de errado que pode ser alguma das situações:

Erro de parametro
Erro de taxa de fee
Endereço errado
Vout/Txid errado
Txid de outra privkey que nao seja de quem esta assinando
dentre outras situações

## ENVIANDO A TRANSAÇÃO

1 - Para enviar a transação utilize o comando:

```sh
sendrawtransaction 01000000ab00905c01e915d030a05c6aac21b3228d1495614654ebb37bdd7744032932c9eeb3a0e497010000006a47304402204e7e3dd2bf5c4291517301993a3ef5c3a671d768fd35c78d89ef1279f35f69330220706f79098bee1772ea12dd62af4dbdadc3a30aa8a8f287cf12e67fdd741aed9c01210214e5eedd2e8ca38dc26345b2d294665a354cc9ff5b1ec12f9469df95701d19f5ffffffff0300000000000000003c6a3a4573736120736572612061206d656e736167656d20717565206972656d6f7320656e7669617220646520666f726d6120656e637269707461646100e1f505000000001976a914bb673d9d56b0de8199f6bbd6147a0aa906da407b88acf08b3f71000000001976a914f178f2da8d2efcf00ff4650d073024e648b0879488ac00000000
```
Você então receberá o retorno com sua TXID da transação criada e enviada:

> ab12d7709b5c412308bf897361c6a8437f5db958462f0c3ea7f0e81e20db82c5

A transação pode ser conferida em 

>http://sperocoin.ddns.net:3001/tx/ab12d7709b5c412308bf897361c6a8437f5db958462f0c3ea7f0e81e20db82c5

E de forma mais detalhada mostrando o OP_RETURN em:

> http://sperocoin.ddns.net:3001/api/getrawtransaction?txid=ab12d7709b5c412308bf897361c6a8437f5db958462f0c3ea7f0e81e20db82c5&decrypt=1
