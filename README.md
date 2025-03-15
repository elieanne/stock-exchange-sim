# Stock Exchange Simulation

![stock_exchange](https://github.com/christian-spooner/stock-exchange-simulation/assets/93479191/44775907-284a-4b63-8099-0f90d92f30ee)

Ce projet met en œuvre une simulation de bourse à l'aide d'une architecture microservices. Pour construire et déployer la bourse localement, exécutez
```
docker compose build
docker compose up
```

## System Overview

### Core Services

- `passerelle d'entrée des commandes` 
    - Reçoit les demandes d'ordre
    - Convertit les demandes d'ordre valides en ordres séquencés
    - Envoi des ordres au « moteur d'appariement » (matching-engine)
    - Envoi des rapports d'ordres à `market-data-feed`

- `matching-engine`
    - Tient un carnet d'ordres distinct, en mémoire, pour chaque titre coté en bourse. 
    - Il reçoit les ordres et les compare au carnet d'ordres correspondant.
    - Envoie les demandes d'exécution des ordres appariés à la `clearinghouse`
    - envoie des rapports sur les transactions et des mises à jour de prix à `market-data-feed`.

- `clearinghouse`
    - reçoit les demandes d'exécution
    - Compense et règle les demandes d'exécution valides
    - Envoie des rapports de règlement à `market-data-feed`.

- `market-data-feed`
    - Reçoit et stocke les données reçues de tous les autres services.
    - Fournit une API pour l'accès aux données qui comprend des points d'extrémité REST et WebSocket.

### Composants supplémentaires

- Persistance

    Les services partagent un serveur Postgres avec deux bases de données : `clearing_data` et `market_data`.

    - `clearing_data` maintient un enregistrement des soldes de comptes pour les traders ainsi qu'un dépôt de titres pour suivre la détention d'actions
    - `market_data` stocke les rapports d'autres services, ainsi que les mises à jour de prix de `matching-engine`.

- Communication entre services

    Les services communiquent via un serveur RabbitMQ. Le serveur contient un échange *direct* nommé `stock_exchange` vers lequel tous les messages sont publiés. Cet échange maintient trois files d'attente *durables* vers lesquelles les messages sont acheminés et consommés : 
    - `order_entry`
    - `order_execution`
    - `data_feed`

- Participants au marché

   Pour simuler les acteurs du marché, le projet inclut le répertoire `trading-bots` qui contient des traders basés sur Python. Un trader soumet des ordres à la passerelle selon une stratégie particulière (par exemple, une marche aléatoire).

   `market-data-feed` offre des points d'extrémité pour récupérer les données relatives à chaque trader, telles que :

   - `/holdings/{securityId}/{clientId}` : actions actuellement détenues par un trader, pour un titre donné.
   - `/balance/{clientId}` : solde actuel du compte d'un trader.

   Chaque trader démarre avec un solde initial de 10 000 dollars, qui varie en fonction de son activité de trading. Les robots de trading peuvent donc être testés les uns par rapport aux autres et évalués en fonction de leurs avoirs finaux en actions et de leurs soldes de compte.