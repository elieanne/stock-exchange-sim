# SIMULATION BOURSIERE
# français
Objectifs
Ce projet consiste à exécuter un programme qui optimisera les performances d'une chaîne de processus, en maximisant un résultat et/ou en réduisant le délai autant que possible.

Vous devez donc exécuter un programme qui, en entrée, lit un fichier décrivant les processus, analyse l'ensemble du fichier et propose une solution d'intérêt valide.

Vous devrez également créer un deuxième petit programme de vérification.

Vous devez créer au moins deux fichiers de processus qui vous sont propres, qui ne sont pas un copier/coller de ceux fournis.

Pour ce projet, vous pouvez utiliser n'importe quel langage compilé (comme C, Rust, Go ou autre).

Introduction
Vous avez peut-être déjà vu un projet dans lequel toutes les tâches sont liées, selon leurs dépendances et restrictions respectives. L'objectif de ce programme est d'organiser une chaîne de tâches afin d'optimiser une tâche définie de la manière la plus rapide et la plus efficace possible.

Exemple:

Imaginez un projet comportant différentes tâches visant à atteindre un objectif final. Chacune de ces tâches peut avoir ses propres dépendances et restrictions. Imaginons que vous construisiez une nouvelle armoire avec sept planches.

Tâche	Nécessaire	Résultat	Faire du vélo
faire_des_poignées_de_porte	planche : 1	poignées de porte : 1	15
faire_arrière-plan	planche : 2	contexte : 1	20
faire_étagère	planche : 1	étagère : 1	10
do_cabinet	poignées de porte : 2 ; arrière-plan : 1 ; étagère : 3	armoire : 1	30
Le but de cet exemple est d'optimiser le temps et l'armoire, en gros l'armoire est le dernier produit à réaliser dans le projet et le temps doit être priorisé afin que la construction de votre armoire se fasse le plus rapidement possible.

Les tâches doivent être effectuées dans cet ordre pour obtenir une optimisation maximale en termes de temps et d'armoire :

0:do_shelf
0:do_shelf
0:do_shelf
0:do_doorknobs
0:do_doorknobs
0:do_background
20:do_cabinet
No more process doable at cycle 51
Le numéro précédant la tâche représente le cycle de démarrage de la tâche. Les tâches peuvent être exécutées séparément ou simultanément, selon les stocks utilisés.

Dans l'exemple ci-dessus, les 6 premières tâches commencent au cycle 0 et s'exécutent simultanément. Cela est dû au fait que nous disposons de 7 cartes et qu'elles peuvent toutes être utilisées dans les 6 premières tâches sans aucune dépendance. Une fois les 6 tâches terminées, 20 cycles ont déjà été écoulés ; la tâche suivante (do_cabinet) débutera donc au cycle 20 et se terminera au cycle 50. Cette tâche n'a pas démarré plus tôt car elle dépend des résultats des autres.

Eh bien, optimiser un horaire régulier peut vous aider à prioriser vos tâches et à faire en sorte que votre programme fonctionne dans ce but.

La planification de projet basée sur les priorités est une technique de planification heuristique rapide et simple qui utilise deux composants pour construire un calendrier de projet réalisable en termes de ressources, une règle de priorité et un schéma de génération de calendrier .

Voici quelques façons de planifier un programme :

Schéma de génération de planning série : sélectionne les activités une par une dans la liste et les planifie dès que possible dans le planning.

Schéma de génération de planning parallèle : sélectionne à chaque période de temps prédéfinie les activités disponibles pour être planifiées et les planifie dans la liste tant que suffisamment de ressources sont disponibles.

Format de fichier
Nous avons d’abord besoin d’un fichier de configuration qui contient les processus et doit obéir aux instructions suivantes :

Un #qui définit les commentaires

Une description des stocks disponibles au départ, avec le format suivant

<stock_name>:<quantity>

Une description des processus :

<name>:(<need>:<quantity>;<need>:<quantity>;...):(<result>:<quantity>;<result>:<quantity>;...):<nb_cycle>

Une seule ligne pour indiquer les éléments à optimiser, contenant éventuellement le timemot clé :

optimize:(<stock_name>|time)

Comme indiqué précédemment, vous devez créer au moins deux fichiers de configuration. L'un se termine lorsque les ressources sont consommées, et l'autre peut être utilisé en rotation indéfinie (voir la section suivante).

Programme de bourse
Nous pouvons ensuite exécuter le programme de bourse pour générer un calendrier.

Il faut 2 paramètres :

./stock_exchange <file> <waiting_time>

Le premier paramètre concerne configuration filetous les stocks et processus. Le deuxième paramètre indique le temps d'attente que le programme ne doit pas dépasser, en secondes.

Dans la situation où les ressources seront consommées et les processus s'arrêteront, en raison du manque de ressources, l'affichage se déroulera jusqu'au bout sans erreur.

Dans le cas où le système s'auto-alimente et tourne indéfiniment, vous choisirez une condition d'arrêt raisonnable et vos stocks devraient montrer que l'ensemble du processus s'est bien déroulé à plusieurs reprises.

Il n'existe aucune obligation d'invariance entre deux exécutions. Cela signifie que, pour une même configuration, la première solution recommandée peut être différente de la seconde.

C'est à vous de créer et d'organiser l'affichage, celui-ci doit néanmoins permettre la compréhension des principales actions réalisées par le programme.

Main Processes:
 0:do_shelf
 0:do_shelf
 0:do_shelf
 0:do_doorknobs
 0:do_doorknobs
 0:do_background
 20:do_cabinet
No more process doable at cycle 51
Stock:
 board => 0
 doorknobs => 0
 background => 0
 shelf => 0
 cabinet => 1
L'affichage doit également produire une sortie utilisable par le programme de vérification, au format suivant :

<cycle>:<process_name>

Le calendrier généré est ensuite enregistré sous forme de journal portant le même nom.

Donc si vous exécutez :

go run . examples/simple 10
Vous obtenez ce planning généré :

$ cat examples/simple.log
0:buy_materiel
10:build_product
40:delivery
No more process doable at cycle 61
$
Vérificateur
Nous pouvons ensuite vérifier si le planning est réalisable avec le programme vérificateur :

Il faut 2 paramètres :

.\checker <file> <log_file>

Le premier paramètre est le fichier de configuration, le second est un fichier journal contenant la trace du programme boursier qui doit être vérifié.
L'affichage doit indiquer si la séquence est correcte ou indiquer le cycle et le procédé à l'origine du problème. Dans tous les cas, à la fin du programme, les stocks sont affichés, ainsi que le dernier cycle.

Exemple
Fichier de configuration:
#
# simple example
#
# stock
name:quantity
euro:10
#
# process name:(need1:quantity1;need2:quantity2;...):(result1:quantity1;result2:quantity2;...):delay
#
buy_materiel:(euro:8):(material:1):10
build_product:(material:1):(product:1):30
delivery:(product:1):(client_content:1):20
#
# optimize time for no process possible (eating stock, produce all possible),
# or maximize some products over a long delay
# optimize:(time|stock1;time|stock2;...)
#
optimize:(time;client_content)
#
Usage
Exécution du programme de bourse :

$ go run . examples/simple 10
Main Processes:
 0:buy_materiel
 10:build_product
 40:delivery
No more process doable at cycle 61
Stock:
 euro => 2
 materiel => 0
 product => 0
 client_content => 1
$
Exécution du programme de vérification :

$ go run ./checker examples/simple examples/simple.log
Evaluating: 0:buy_materiel
Evaluating: 10:build_product
Evaluating: 40:delivery
Trace completed, no error detected.
$
Exécution du programme de bourse avec erreur, vous pouvez voir le fichier correct ici

$ cat examples/simple
euro:10
:(euro:8):(material:1):10
build_product:(material:1):(product:1):30
delivery:(product:1):(client_content:1):20
optimize:(time;client_content)
$ go run . examples/simple 10
Error while parsing `:(euro:8):(material:1):10`
$
En exécutant le programme de vérification avec une erreur, vous pouvez voir le fichier correct ici

$ cat examples/simple.log
0:buy_materiel
10:build_product
10:build_product
40:delivery
# No more process doable at cycle 61
$ go run ./checker examples/simple examples/simple.log
Evaluating: 0:buy_materiel
Evaluating: 10:build_product
Evaluating: 10:build_product
Evaluating: 40:delivery
Error detected
at 10:build_product stock insufficient
$

# stock-exchange-sim
# anglais
Objectives
This project consists of executing a program that will optimize the performance of a process chain, maximizing a result and/or reducing the delay as much as possible.

You must therefore run a program that, as input, reads a file that describes the processes, analyze the entire file and propose a valid solution of interest.

You will also have to create a second small checker program.

You have to create at least two process file of your own, that is not a copy/paste of the ones provided.

For this project you may use any compiled language (like C, Rust, Go or other).

Introduction
You may have already seen a project in which all tasks are linked, according to their respective dependencies and restrictions. The goal of this program is to organize a certain chain of project tasks in order to optimize a defined task the faster and most efficient way.

Example:

Imagine a project with different tasks in which we have to achieve a final goal. Each of the tasks can have their dependencies and restrictions, lets say you are putting together a new cabinet and you have 7 boards.

Task	Needed	Result	Cycle
do_doorknobs	board: 1	doorknobs: 1	15
do_background	board: 2	background: 1	20
do_shelf	board: 1	shelf: 1	10
do_cabinet	doorknobs: 2; background: 1; shelf: 3	cabinet: 1	30
The purpose of this example is to optimize time and cabinet, basically the cabinet is the last product to achieve in the project and time is to be prioritized so that the building of your cabinet is done the fastest way possible.

The tasks should be done in this order to achieve a maximum optimization regarding time and cabinet:

0:do_shelf
0:do_shelf
0:do_shelf
0:do_doorknobs
0:do_doorknobs
0:do_background
20:do_cabinet
No more process doable at cycle 51
So the number before the task represents the cycle when the task starts. The tasks can run separately or at the same time depending on the stocks that are used.

In the example above we have the first 6 tasks beginning at cycle 0 and running at the same time, this happens because we have 7 boards and all of them can be used in the first 6 tasks without any dependencies. After the 6 tasks are finished it has already passed 20 cycles, so the next task (do_cabinet) will start at cycle 20 and end at cycle 50. This task did not start earlier because it is dependant of the results of the others.

Well, optimizing a regular schedule may help you to prioritize your tasks and make your program work with that purpose.

Priority based project scheduling is a quick and easy heuristic scheduling technique that makes use of two components to construct a resource feasible project schedule, a priority rule and a schedule generation scheme.

Here are some ways to schedule a scheme:

Serial schedule generation scheme: selects the activities one by one from the list and schedules them as-soon-as-possible in the schedule.

Parallel schedule generation scheme: selects at each predefined time period the activities available to be scheduled and schedules them in the list as long as enough resources are available.

File format
First we need a configuration file that contains the processes and must obey the following instructions:

A # that defines comments

A description of the stocks available at the start, with the following format

<stock_name>:<quantity>

A description of the processes:

<name>:(<need>:<quantity>;<need>:<quantity>;...):(<result>:<quantity>;<result>:<quantity>;...):<nb_cycle>

A single line to indicate the elements to optimize, possibly containing the time keyword :

optimize:(<stock_name>|time)

So, as we said above, you must create at least two configuration file of your own. One file that ends when the resources are consumed, and the other which can rotate indefinitely.(will be explained in the next topic)

Stock exchange program
Then we can run the The stock exchange program to generate a schedule.

It takes 2 parameters:

./stock_exchange <file> <waiting_time>

The first is the configuration file with all stocks and processes. The second parameter is a waiting time that the program should not exceed in seconds.

In the situation where the resources will be consumed and the processes will stop, due to the lack of resources, the display will go all the way without errors.

In the situation where the system self-powers and rotates indefinitely, you will choose a reasonable shutdown condition, and your stocks should show that the whole process went well several times.

There is no obligation of invariance between two executions. This means that, for the same configuration, the first recommended solution, may be different from the second one.

It is up to you to create and organize the display, it must nevertheless allow the understanding of the main actions carried out by the program.

Main Processes:
 0:do_shelf
 0:do_shelf
 0:do_shelf
 0:do_doorknobs
 0:do_doorknobs
 0:do_background
 20:do_cabinet
No more process doable at cycle 51
Stock:
 board => 0
 doorknobs => 0
 background => 0
 shelf => 0
 cabinet => 1
The display must also produce an output usable by the checker program, in the following format:

<cycle>:<process_name>

The generated schedule is then saved as a log with the same name.

So if you run:

go run . examples/simple 10
You get this generated schedule:

$ cat examples/simple.log
0:buy_materiel
10:build_product
40:delivery
No more process doable at cycle 61
$
Checker
We can then verify if the schedule is doable with the checker program:

It takes 2 parameters:

.\checker <file> <log_file>

The first parameter is the configuration file, the second is a log file containing the trace of stock exchange program which must be checked.
The display must indicate whether the sequence is correct, or indicate the cycle and the process which are causing the problem. In all cases, at the end of the program, stocks are displayed, as well as the last cycle.

Example
Configuration file:
#
# simple example
#
# stock
name:quantity
euro:10
#
# process name:(need1:quantity1;need2:quantity2;...):(result1:quantity1;result2:quantity2;...):delay
#
buy_materiel:(euro:8):(material:1):10
build_product:(material:1):(product:1):30
delivery:(product:1):(client_content:1):20
#
# optimize time for no process possible (eating stock, produce all possible),
# or maximize some products over a long delay
# optimize:(time|stock1;time|stock2;...)
#
optimize:(time;client_content)
#
Usage
Running the stock exchange program:

$ go run . examples/simple 10
Main Processes:
 0:buy_materiel
 10:build_product
 40:delivery
No more process doable at cycle 61
Stock:
 euro => 2
 materiel => 0
 product => 0
 client_content => 1
$
Running the checker program:

$ go run ./checker examples/simple examples/simple.log
Evaluating: 0:buy_materiel
Evaluating: 10:build_product
Evaluating: 40:delivery
Trace completed, no error detected.
$
Running the stock exchange program with error, you can see the correct file here

$ cat examples/simple
euro:10
:(euro:8):(material:1):10
build_product:(material:1):(product:1):30
delivery:(product:1):(client_content:1):20
optimize:(time;client_content)
$ go run . examples/simple 10
Error while parsing `:(euro:8):(material:1):10`
$
Running the checker program with error, you can see the correct file here

$ cat examples/simple.log
0:buy_materiel
10:build_product
10:build_product
40:delivery
# No more process doable at cycle 61
$ go run ./checker examples/simple examples/simple.log
Evaluating: 0:buy_materiel
Evaluating: 10:build_product
Evaluating: 10:build_product
Evaluating: 40:delivery
Error detected
at 10:build_product stock insufficient
$


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
