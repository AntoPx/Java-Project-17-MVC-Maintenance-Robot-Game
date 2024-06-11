# Progetto: Mondo Robot

## Componenti del gruppo
- Antonino Granata, 20034202
- Cecilia Tasca, 20040354

## Relazione del progetto: Proposta 2 - Mondo Robot

Il robot manutentore si muoverà all’interno della mappa di una casa ed avrà la possibilità di effettuare varie operazioni sulla cella in cui si trova oppure muoversi all’interno della mappa, spostandosi solo attraverso le celle adiacenti. Oltre al robot, è presente anche un animale domestico che si muove in maniera autonoma sulla mappa. Esistono diverse tipologie di celle, la più semplice è la cella vuota, la quale può essere percorsa dal robot o dall’animale domestico e può acquisire lo status di bagnata se è vicina ad una cella lavatrice o rubinetto. Sia la lavatrice sia il rubinetto sono due tipologie di celle non percorribili dal robot o dall’animale domestico, ma se il robot si posiziona in una cella adiacente, può modificarne lo status (ad esempio, se il rubinetto è aperto, il robot può chiuderlo. Analogamente avviene per la lavatrice). Infine, la mappa presenta la cella fornello, non percorribile dal robot o dall’animale domestico ed il suo stato è modificabile dal robot, spegnendolo o accendendolo. Il robot manutentore non solo ha la possibilità di muoversi tra le celle adiacenti, ma se riconosce che la cella in cui si trova è bagnata può asciugarla. Inoltre, può accendere o spegnere un fornello se si trova nella cella adiacente; mentre, se è nella cella adiacente ad un rubinetto può chiuderlo. Infine, può aggiustare una cella lavatrice, a patto che sia nella cella adiacente.

L’animale domestico sfrutta un Thread per muoversi indipendentemente dal robot manutentore, sulla mappa sarà identificato da un cane. Infine, è necessario specificare che il robot e l’animale domestico sono due oggetti distinti che si muovono sulle celle.

### Class Diagram

Per avere una maggiore comprensione del progetto abbiamo deciso di iniziare a descrivere le classi nel dettaglio, per poi arrivare a specificare le interazioni e tutto ciò che ne deriva, quindi ereditarietà, gestione delle eccezioni e uso di una classe annidata. Prima di definire gli oggetti, definiamo il thread dell’acqua: ha lo scopo di allagare le celle asciutte adiacenti alle celle contenenti gli oggetti lavatrice e rubinetto.

Il thread dell’acqua, definito dalla classe `AvviaThPerditaAcqua`, ha lo scopo di modificare lo status della cella, passando da cella asciutta a cella bagnata ed una condizione per fermare il Thread. All’interno di questa classe è presente una classe annidata di tipo Thread `ThPerditaAcqua`. Per definirlo abbiamo utilizzato il metodo `AllagaCella(int n, int r, int c)` che riceve un intero `n`, compreso tra 0 e 3, per scegliere in maniera randomica quale cella adiacente bagnare, e la posizione della cella (tramite `r` e `c`); inoltre, abbiamo creato un metodo `PerditaAdiacente(int r, int c)`, che riceve la posizione della cella, tramite `r` e `c`, e rende bagnata quella cella. Infine abbiamo definito il metodo `Run()` che richiama `AllagaCella()` fornendogli un numero randomico tra 0 e 3 (per il movimento) e la posizione della cella, tramite riga e colonna. Il thread ha un tempo di sleep pari a 10 secondi.

### Gli oggetti che costituiscono la mappa

- **Fornello**: estensione di `Cella`, definita da un costruttore che prende le coordinate, se è già passato da quella cella e lo stato del fornello. Metodi: `SetSpegni()`, `SetAccendi()`, `GetStato()`.
- **Lavatrice**: estensione di `Cella`, definita da un costruttore che prende le coordinate, se è già passato da quella cella e lo stato della lavatrice. Metodi: `SetSpegni()`, `SetAccendi()`, `GetStato()`.
- **Muro**: estensione di `Cella`, definito da un costruttore che prende le coordinate esterne della mappa, definendo celle che non possono essere attraversate.
- **Rubinetto**: estensione di `Cella`, definita da un costruttore che prende le coordinate, se è già passato da quella cella e lo stato del rubinetto e l’avvio del thread dell’acqua. Metodi: `SetAperto()`, `SetChiuso()`, `SetStato()`.
- **Vuota**: estensione di `Cella`, definita da un costruttore che prende le coordinate e se è stata visitata oppure no. Metodi: `SetAsciutta()`, `SetBagnata()`, `GetStato()`.

La classe `Cella` è un’astrazione delle classi precedenti. Il suo costruttore è definito dalle coordinate della cella, dove `r` rappresenta le righe e `c` le colonne, e se la visita alla stessa è avvenuta o se deve ancora avvenire. Inoltre, presenta i metodi per definire le coordinate, per capire se la cella è stata visitata oppure no, restituendo un booleano; infine, un ultimo metodo per ricevere le coordinate come valore di ritorno.

### Oggetti Animale Domestico e Robot

Gli oggetti Animale Domestico e Robot presentano alcune similitudini, infatti si muovono all’interno della mappa passando da una cella adiacente all’altra, potendo passare su celle bagnate. Inoltre, entrambi presentano i metodi `GetC()`, `GetR()`, `SetC()` e `SetR()`, utilizzati per le coordinate. Infine i metodi per definire il loro movimento sono in `Controller.java`. Vi sono però alcune differenze: il movimento del robot è gestito dall’utente, che può scegliere in quale cella adiacente spostarlo tramite un click del mouse; mentre l’animale domestico si muove grazie ad un thread. Più precisamente, quest’ultimo, ha il metodo `MuoviAnimaleDomestico(int x, int y)`, dove `x` e `y` sono le coordinate della posizione effettiva, ed il metodo `MuoviAnimaleDomestico(int n)` che riceve un intero `n`, compreso tra 0 e 3, per scegliere in maniera randomica quale cella adiacente muoversi. Il thread ha un tempo di sleep pari a 0.5 secondi.

### Controller classe annidata

A differenza delle classi annidate statiche, le classi interne non statiche hanno un legame stretto con l'istanza della classe esterna e possono accedere ai membri (variabili e metodi) della classe esterna, inclusi quelli privati.

Ora ci occupiamo di definire le classi dell’MVC, ovvero quelle che si occupano della grafica. Sono presenti una classe `Model`, due classi `View` ed una classe `Controller`.

### Controller.java

Il `Controller.java` presenta i thread dell’acqua e dell’animale domestico, già presentati in precedenza, e tutti i metodi per definire le posizioni iniziali degli oggetti, quindi: `InizializzaMuri()`, `InizializzaFornello()`, `InizializzaRubinetto()`, `InizializzaLavatrice()`, `InizializzaAnimaleDomestico()`, `InizializzaRobot()`. Presenta i metodi per far sapere al robot se in una delle sue celle adiacenti è presente un oggetto, essi sono: `TrovaFornello(int r, int c)`, `TrovaRubinetto(int r, int c)`, `TrovaLavatrice(int r, int c)`, `TrovaBagnata(int r, int c)`. Infine sono presenti i metodi per poter modificare lo status delle celle: `SetAsciutta(int r, int c)`, `SetBagnata(int r, int c)`, `SpegniFornello(int r, int c)`, `AccendiFornello(int r, int c)`, `ChiudiRubinetto(int r, int c)`, `ChiudiLavatrice(int r, int c)`. Ogni intero `r` e `c` serve per definire la posizione della cella.

Le frecce indicano che il modello può essere navigato solo nella direzione opposta della freccia. Il simbolo "+" indica che la classe aggregante, `Controller.java`, ha una relazione di aggregazione con la classe aggregata, `AvviaThreadPerdita`. Questo significa che la classe aggregante contiene o fa riferimento alla classe aggregata, ma la classe aggregata può esistere indipendentemente dalla classe aggregante.

### SceltaView.java

La `SceltaView.java` presenta una classe `SceltaView` che estende `JPanel` e ha lo scopo di definire i bottoni da poter far cliccare all’utente per selezionare un’azione da compiere. L’utente può scegliere tra: `Asciuga`, `Accendi Fornello`, `Spegni Fornello`, `Chiudi Rubinetto`, `Chiudi Lavatrice`.

### GameView.java

La `GameView.java` inizialmente crea un pop up per far scegliere all’utente la dimensione `N` della griglia di gioco, successivamente `InizializzaMuri(int N)` riceve la dimensione `N` e crea la griglia definendo il perimetro, usando `r` e `c` per le coordinate, di Muri. Successivamente crea due mappe con la dimensione `N` inserita, la prima presenta la griglia di gioco, dove l’utente può scegliere le mosse del robot e sceglie cosa fare; al contrario nella seconda griglia vi è una rappresentazione di dove vengono generati randomicamente tutti gli oggetti. Questa `GameView` presenta il metodo `CaricaImmagini()` con le immagini degli oggetti e dei cambiamenti di stato delle celle o degli oggetti. I metodi `SetIconFornello(int r, int c, boolean stato)`, `SetIconLavatrice(int r, int c, boolean stato)`, oltre alle coordinate della cella (rappresentate da `r` e `c`), presentano lo stato per definire il cambio di stato e, di conseguenza, il cambio di immagine, `SetIconBaganata(int r, int c)`, che aggiunge l’Icon con l’acqua se la cella viene bagnata, `SetIconAnimaleDomestico(int r, int c)` specifica in quale cella vi è l’immagine dell’animale domestico.

### GameModel.java

La `GameModel.java` comunica direttamente con la `GameView.java` ed è la parte di codice dove avviene la creazione degli oggetti. In un primo momento riceve la dimensione dall’utente e, tramite `CreaGriglia()`, crea la mappa di gioco. Inoltre, ci sono i metodi per definire ogni oggetto, ad esempio `InizializzaRubinetto(int r, int c, boolean passato, boolean stato)`, dopo aver controllato che la cella sia vuota, definisce un oggetto di tipo Rubinetto e lo posiziona sulla cella, questo avviene tramite il metodo `TrovaRubinetto(int r, int c)`. I metodi `InizializzaLavatrice(int r, int c, boolean passato, boolean stato)` e `InizializzaFornello(int r, int c, boolean passato, boolean stato)` hanno un funzionamento analogo. Il metodo `InizializzaRobot(int r, int c)`, dopo aver controllato che la cella sia vuota, richiama il metodo `SettaTrueTutteLeCelleAdiacentiARobot()` con lo scopo di definire le celle adiacenti come attraversabili. Il metodo `InizializzaAnimaleDomestico(int r, int c)` posiziona, dopo aver controllato che la cella sia vuota, l’animale sulla cella. I metodi `MuoviRobot(int x, int y)` e `MuoviAnimaleDomestico()` sono pressoché simili, infatti entrambi si muovono sulla cella e hanno un controllo per evitare che i due oggetti si posizionino sulla stessa cella. L’unica differenza è il metodo `SettaTrueTutteLeCelleAdiacentiARobot()` per permettere di settare le celle adiacenti al robot come attraversabili. Infine, sono presenti i metodi `SpegniFornello(int r, int c)`, `AccendiFornello(int r, int c)` che hanno lo scopo di spegnere o accendere un oggetto di tipo Fornello, analogamente avviene per `ChiudiRubinetto(int r, int c)` per un oggetto di tipo Rubinetto e per `ChiudiLavatrice(int r, int c)` per un oggetto di tipo Lavatrice.

## Funzionalità del progetto

Lo scopo del progetto è creare una mappa, di grandezza superiore a 10x10 decidibile dall’utente, con alcuni oggetti statici, come i fornelli, le lavatrici ed i rubinetti, ed oggetti dinamici, come il robot, l’animale domestico e l’acqua. Il personaggio principale è il robot, rappresenta le scelte dell’utente, esso può effettuare una singola mossa per turno. Le mosse sono: muoversi sulla mappa avanzando tramite le celle adiacenti oppure può decidere se compiere un’interazione con la cella in cui si trova; infatti, può asciugare una cella bagnata, spegnere il fornello, aggiustare il rubinetto o la lavatrice. Appena la mappa viene generata tutti gli oggetti vengono generati in maniera randomica, l’utente conosce solo la posizione del robot e le sue celle adiacenti. Il suo obiettivo è continuare a muoversi per trovare dove sono posizionati sulla mappa tutti gli altri oggetti. L’utente può scegliere cliccando direttamente la cella in cui vuole che il robot si sposti e, quando avrà finalmente trovato una cella in cui effettuare una operazione, l’utente, può scegliere tramite alcuni bottoni situati di fianco alla mappa. I bottoni sono: asciuga, spegni fornello, accendi fornello, chiudi rubinetto e chiudi lavatrice. L’animale domestico, rappresentato da un cane, si muoverà in maniera autonoma, apparendo sulla mappa solo se si trova nella zona già sbloccata dal robot. Anche il flusso dell’acqua che perde avviene in maniera autonoma, in base ad una certa quantità di tempo, anziché in base alle mosse del robot.