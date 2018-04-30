# Controle de Acesso

**Toda documentação do projeto encontra-se em fase de desenvolvimento!**

![img](https://raw.githubusercontent.com/douglaszuqueto/controle-de-acesso/master/.github/images/print-dashboard.png)

## Índice

- [Introdução](#introdução)
- [Objetivo](#objetivo)
- [Materiais](#materiais)
- [Tecnologias](#tecnologias)
- [Organização](#organização)
- [Fluxo](#fluxo)
- [Rodando o projeto](#rodando-o-projeto)
- [Resultados](#resultados)
- [Referências](#referências)


## Introdução

Depois de completar quase 1 ano que lancei o primeiro 'projeto' de envolvendo controle de acesso, eis que foi desenvolvido uma versão mais completa.

Para quem ainda não conhece, o projeto [Controle de acesso utilizando NodeMCU, RFID, MQTT e Banco de Dados MySQL](https://github.com/douglaszuqueto/esp8266-rfid-banco-de-dados) foi desenvolvido em Julho de 2017, onde tinha como objetivo desenvolver um projeto simples e direto ao ponto para sanar muitas dúvidas que membros da comunidade tinham, principalmente no que tange integração, arquitetura e comunicação com banco de dados. Portanto, para quem ainda não viu, eu recomendo dar uma olhada no projeto antigo para já ter um embasamento do que se trata - [link](https://github.com/douglaszuqueto/esp8266-rfid-banco-de-dados).

Como o objetivo era desenvolver um projeto mais completo, e como também, eu sempre estou em busca de testar e validar novas tecnologias, neste projeto não foi diferente - Utilizei **Go(golang)** como linguagem de programação para o *Back-end* e **VueJS** como biblioteca para construção do *Front-end*. No embarcado fiquei com as mesmas tecnologias pois é a que mais domino no momento.

## Objetivo

Desenvolver tal projeto que seja de fácil implementação - tanto no que tange **Embarcado** como também **Arquitetura** e **Software** e também oferecer uma boa experiência através de networking e troca de ideias - sendo os 2 últimos itens mencionados, os fatores que mais levo em consideração no desenvolvimento de algum projeto :)

## Materiais

Abaixo segue uma seção resumida dos materiais principais que foram utilizadas no decorrer do desenvolvimento do projeto.

* Embarcado
    * NodeMCU
    * Rfid Mfrc522 Mifare
* Servidor
    * Raspberry Pi 3
    * Raspberry Pi Zero W
    * Servidor em Nuvem

**Observação:** No que tange servidor, apenas um item citado acima basta! Detalharei com mais calma em outro tópico.

## Tecnologias

* Firmware
    * [PubSubClient](https://github.com/knolleary/pubsubclient)
    * [MFRC522](https://github.com/miguelbalboa/rfid/)
* Back-end - Go
    * [pq - A pure Go postgres driver for Go's database/sql package](https://github.com/lib/pq)
    * [gorilla/handlers](https://github.com/gorilla/handlers)
    * [gorilla/mux](https://github.com/gorilla/mux)
    * [Eclipse Paho MQTT Go client](https://github.com/eclipse/paho.mqtt.golang)
    * [jwt-go](https://github.com/dgrijalva/jwt-go)
    * [statik](https://github.com/rakyll/statik)
* Front-end - VueJS
    * [VueJS](https://github.com/vuejs/vue)
    * [Vue Router](https://github.com/vuejs/vue-router)
    * [Bootstrap 4](https://github.com/twbs/bootstrap)
    * [Axios](https://github.com/axios/axios)
    * [MQTT.js](https://github.com/mqttjs/MQTT.js)
    * [Font-Awesome](https://github.com/FortAwesome/Font-Awesome)
    * [Sweet Alert 2](https://github.com/sweetalert2/sweetalert2)

## Organização

**Level 1**
```
~/projetos/controle-de-acesso on  master ⌚ 14:43:36
$ tree -L 1
.
├── app
├── bin
├── cli
├── database
├── firmware
├── front
├── README.md
└── systemd

7 directories, 1 file
```

Num primeiro momento, o projeto é formado por **7 pastas** - podendo crescer futuramente. 

**Observação:** Detalhes mais 'aprofundados' de cada pasta será abordado em um **README** dentro do sua respectiva pasta.

### 1 - app

Conterá todo back-end da aplicação, ou seja, tudo que é necessário para realizar o build da aplicação e 'colocar em produção'. Possui 3 scripts(em bash mesmo) para colocar todo o projeto rodar em 3 ambientes: **Docker**, **Local** e na **Raspberry Pi**.

### 2 - bin

Pasta responsável por armazenar os binários(aplicação compilada) para arquitetura **Linux 64** e **ARM(raspberry)**

### 3 - cli

Binário que será desenvolvido para manipular todos *endpoints* da aplicação através da linha de comando.

**Ex:**

* 1º Criar uma tag
* 2º Associar a um usuário
* 3º Associar a um device

**Observação: Em desenvolvimento**

### 4 - database

O banco de dados utilizado foi o **PostgreSQL** - um banco que fazia algum tempo que queria validar em 1 projeto :)

Confira abaixo a estrutura do projeto CDA.

![img](https://raw.githubusercontent.com/douglaszuqueto/controle-de-acesso/master/database/estrutura.png)

Em resumo, o projeto se dá pela seguinte forma:

* administradores para acessar o painel
* usuários que por sua vez podem possuir diversas tags
* devices - podem ser catracas, portas de acesso dentre outros cenários
* tais tags são associadas a devices, assim liberando ou proibindo o acesso daquela tag específica e que pertence a um usuario X

Demais informações serão descritas na respectiva pasta *database*

### 5 - firmware

A pasta firmware contem 2 subpastas, são elas: **cda** e **reader**.

O firmware **cda** é o principal. Ele que rodará no seu hardware principal - catracas, portas de acesso e etc.

Já o firmware **reader** é como se fosse um *Plus*, você poderá ter um hardware adicional no balcão - por exemplo, para realizar o cadastro de tags. Então este firmware em conjunto com um nodemcu e um leitor rfid irá preencher automaticamente o **uuid** quando a tag for lida na tela de cadastro da Dashboard.

### 6 - front

### 7 - systemd

**Level 2**
```
~/projetos/controle-de-acesso on  master! ⌚ 14:44:44
$ tree -L 2
.
├── app
│   ├── app
│   ├── bin
│   ├── config
│   ├── deploy-docker.sh
│   ├── deploy-local.sh
│   ├── deploy-rpi.sh
│   ├── Dockerfile
│   ├── handlers
│   ├── main.go
│   ├── models
│   ├── mqtt
│   ├── pkg
│   ├── public
│   ├── README.md
│   ├── routes.go
│   ├── services
│   ├── src
│   ├── statik
│   └── utils
├── bin
│   ├── app
│   └── README.md
├── cli
│   └── README.md
├── database
│   ├── controle-de-acesso.mwb
│   ├── controle-de-acesso.mwb.bak
│   ├── estrutura.png
│   ├── README.md
│   └── sistema.sql
├── firmware
│   ├── cda
│   └── reader
├── front
│   ├── build
│   ├── config
│   ├── dist
│   ├── Dockerfile
│   ├── index.html
│   ├── node_modules
│   ├── package.json
│   ├── README.md
│   ├── src
│   ├── static
│   └── yarn.lock
├── README.md
└── systemd
    ├── cda
    ├── cda.service
    └── README.md

26 directories, 25 files
```


## Fluxo

## Rodando o projeto

## Resultados

## Referências

- [Controle de acesso utilizando NodeMCU, RFID, MQTT e Banco de Dados MySQL](https://github.com/douglaszuqueto/esp8266-rfid-banco-de-dados)

# Gostou do projeto?

Se você curtiu o projeto e quer trocar uma ideia, foi criado um grupo no **Telegram** para debate! - [clique aqui para participar](https://t.me/joinchat/BPOe2hAc3mNcw_y7f0qipg).