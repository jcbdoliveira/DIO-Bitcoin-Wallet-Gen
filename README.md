# DIO Desafio Criando e Utilizando a Sua Carteira de Criptomoedas

Nesse desafio vamos criar um gerador de carteiras bitcoin, utilizando pares de chave pública e privada. Você irá desenvolver um gerador para criar endereços de depósito e de envio de bitcoin para a carteira gerada. Além disso, vamos importar essa carteira dentro do Electrum, um aplicativo desktop pra enviar e receber transações em bitcoin, e por fim enviar criptomoedas.

```javascript
//importando as dependencias
const bip32 = require('bip32')
const bip39 = require('bip39')
const bitcoin = require('bitcoinjs-lib')

//definir a rede
//bitcoin - rede principal - mainnet
//testnet - rede de teste - tesnet
const network = bitcoin.networks.testnet

//derivação de carteiras HD
const path = `m/49'/1'/0'/0` 

//criando o mnemonic para a seed (palavras de senha)
let mnemonic = bip39.generateMnemonic()
const seed = bip39.mnemonicToSeedSync(mnemonic)

//criando a raiz da cartiera HD
let root = bip32.fromSeed(seed, network)

//criando uma conta - par pvt-pub keys
let account = root.derivePath(path)
let node = account.derive(0).derive(0)

let btcAddress = bitcoin.payments.p2pkh({
    pubkey: node.publicKey,
    network: network,
}).address

console.log("Carteira gerada")
console.log("Endereço: ", btcAddress)
console.log("Chave privada:", node.toWIF())
console.log("Seed:", mnemonic)
```