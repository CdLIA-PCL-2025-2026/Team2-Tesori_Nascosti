# Tesori Nascosti

Tesori Nascosti è un gioco sviluppato in Python con interfaccia grafica realizzata tramite breezypythongui.  
Il gioco è per due giocatori e si svolge su una griglia 6×6 di carte coperte. Ogni giocatore deve costruire una mano di 5 carte gestendo i propri punti azione e sfruttando le carte speciali per ottenere vantaggi strategici.

Autori del progetto: Riccardo Sciabbarrasi, Andrea Barilà, Marco Oliveri

## Contenuto del repository

Struttura principale del progetto

- main.py  
  Punto di ingresso del progetto. Mostra il menu principale, avvia una nuova partita e consente l’accesso alla classifica

- tesori_nascosti_gui.py  
  Interfaccia e logica del gioco. Gestisce round, turni, griglia 6×6, azioni, carte speciali, punteggi e fine partita

- Carta.py  
  Modello della carta, sia numerica sia speciale. Gestisce stato coperta e rivelata permanente e associazione al giocatore

- Mazzo.py  
  Creazione del mazzo, mescolamento ed estrazione delle 36 carte per la griglia

- Giocatore.py  
  Modello del giocatore. Gestisce mano, punti azione, punteggi e stato di conclusione del round

- validator.py  
  Valutazione delle combinazioni della mano e calcolo del punteggio del round. Gestisce anche la Gemma come jolly per gruppi

- GestoreClassifica.py  
  Salvataggio delle partite su file di testo e ricostruzione della classifica con statistiche aggregate

Cartelle e file di supporto

- Immagini_mazzo  
  Contiene le immagini delle carte e degli sfondi usati dalla GUI. Il gioco carica le immagini da questa cartella per retro, carte numeriche e carte speciali

- Audio  
  Contiene il file mp3 della musica di sottofondo riprodotta in loop nel menu principale

- classifica.txt  
  Viene creato automaticamente se non esiste. Contiene lo storico delle partite in formato testuale

## Requisiti

Versione consigliata

- Python 3.10 o superiore

Librerie utilizzate

- breezypythongui
- tkinter
- pillow
- pygame

Nota su pillow e pygame  
Pillow è usata per caricare e ridimensionare le immagini. Pygame viene usata per la riproduzione della musica nel menu principale

## Installazione

Clona il repository e posizionati nella cartella del progetto, poi installa le dipendenze

```bash
pip install pillow pygame
```

## Dipendenze

breezypythongui è incluso nel progetto oppure deve essere reso disponibile nel Python Path in base alla configurazione del repository.

## Avvio

Per avviare il gioco eseguire:

```bash
python main.py
```

All’avvio viene mostrato il menu principale, dal quale è possibile iniziare una nuova partita o accedere alla classifica.

## Regole del gioco

### Panoramica

- Il gioco si svolge su una griglia 6×6 composta da 36 carte coperte  
- Le carte vengono estratte da un mazzo e disposte coperte all’inizio di ogni round  
- Ogni round assegna 15 punti azione a ciascun giocatore  
- I giocatori si alternano nel rivelare una carta e scegliere un’azione tra Accetta, Rifiuta, Cambia  
- Ogni giocatore deve costruire una mano di 5 carte  
- I punteggi dei round si sommano e la partita termina quando un giocatore raggiunge almeno 110 punti  

### Carte numeriche

- **Semi**: Picche, Cuori, Quadri, Fiori  
- **Valori numerici**: da 1 a 10  

Nel modello sono presenti anche valori oltre il 10 per la rappresentazione testuale, ma la logica di punteggio si concentra sulle carte numeriche utili alle combinazioni.

### Carte speciali

#### Gemma

Funziona come jolly solo per combinazioni basate su valori uguali, quindi coppia, doppia coppia, tris, full e poker.  
Non può completare scala, colore o scala reale.  
La logica è gestita in `validator`.

#### Moneta

Quando viene accettata, assegna immediatamente un punto azione extra al giocatore.  
La gestione è implementata in `tesori_nascosti_gui`.

#### Pergamena

Quando viene accettata, permette di rivelare permanentemente una carta coperta della griglia.  
La carta resta visibile per il resto del round e non viene assegnata automaticamente.  
La gestione è implementata in `tesori_nascosti_gui`.

## Azioni del turno

Durante il proprio turno un giocatore seleziona una carta coperta nella griglia per rivelarla e poi sceglie una delle azioni disponibili.

### Accetta

- **Costo**: 1 punto azione  
- **Effetto**: aggiunge la carta alla mano se la mano non è già completa  
- Implementazione in `tesori_nascosti_gui`

### Rifiuta

- **Costo**: 1 punto azione  
- **Effetto**: la carta torna coperta e non viene assegnata a nessuno  
- Implementazione in `tesori_nascosti_gui`

### Cambia

- **Costo**: 2 punti azione  
- **Effetto**: scambia la carta rivelata con una carta già presente nella mano, scegliendo quale carta sostituire  
- Implementazione in `tesori_nascosti_gui`

### Concludi

- Disponibile solo quando la mano è completa  
- Permette al giocatore di terminare la propria partecipazione al round conservando i punti azione residui  
- Implementazione in `tesori_nascosti_gui`

## Fine del round

Il round termina quando entrambi i giocatori hanno concluso oppure quando entrambi hanno terminato i punti azione.  
A fine round vengono valutate le mani e al punteggio della combinazione si sommano i punti azione residui del giocatore.

## Combinazioni e punteggi

Le combinazioni assegnano un punteggio fisso e il punteggio finale del round è:

`punteggio round = punteggio combinazione + punti azione residui`

### Valori delle combinazioni

- **Scala reale**: 100  
- **Poker**: 50  
- **Full**: 30  
- **Colore**: 25  
- **Scala**: 20  
- **Tris**: 15  
- **Doppia coppia**: 10  
- **Coppia**: 5  
- **Nessuna combinazione**: 0  

La logica di valutazione è implementata in `validator`.

## Fine della partita

La partita termina quando almeno un giocatore raggiunge 110 punti o più.  
Se entrambi raggiungono la soglia nello stesso round, vince chi ha il punteggio maggiore.  
In caso di ulteriore parità vince chi ha più punti azione residui.  
Se permane la parità la partita termina in pareggio.  

La gestione è implementata in `tesori_nascosti_gui`.

## Interfaccia di gioco

### Griglia 6×6

- Ogni carta in griglia è rappresentata da un pulsante con immagine  
- Una carta assegnata a un giocatore viene evidenziata con colore diverso e diventa non selezionabile dagli altri  

Implementazione in `tesori_nascosti_gui`.

### Pannello informazioni

Mostra in tempo reale:

- giocatore di turno  
- punti azione rimanenti  
- punteggio attuale  
- numero carte in mano  
- round corrente  
- timer  

Implementazione in `tesori_nascosti_gui`.

### Timer

Il timer mostra i secondi trascorsi e viene aggiornato periodicamente durante la partita.  
Implementazione in `tesori_nascosti_gui`.

### Menu

Il gioco include un menu in alto con opzioni per:

- nuova partita  
- apertura classifica  

Implementazione in `tesori_nascosti_gui`.

## Classifica

### Persistenza su file

Le partite vengono salvate su file di testo in append.  
Ogni riga contiene i campi separati da un carattere separatore e include data, durata, round, nomi, punteggi, punti azione finali e vincitore.

### Visualizzazione

La schermata classifica legge il file, calcola statistiche aggregate e mostra:

- classifica ordinata  
- partite totali  
- ultime partite  

La logica è implementata in `GestoreClassifica`.

## Dettagli implementativi rilevanti

### Gestione dello stato delle carte

Ogni `Carta` mantiene lo stato coperta o scoperta e un flag di rivelazione permanente usato dalla Pergamena.

### Associazione delle carte ai giocatori

Le carte accettate vengono aggiunte alla mano del giocatore e associate al giocatore chiamando un metodo dedicato nella carta.

### Valutazione della mano e complessità

- La mano è al massimo di 5 carte, quindi i controlli di combinazione hanno costo costante rispetto alla dimensione della mano  
- La gestione delle frequenze dei valori usa `Counter` ed esegue operazioni su insiemi molto piccoli  

Implementazione in `validator`.

### Salvataggio della classifica

- Se il file non esiste viene creato automaticamente  
- La lettura ricostruisce oggetti `Partita` e aggrega le statistiche per nome  

Implementazione in `GestoreClassifica`.

## Risorse grafiche e audio

### Immagini

Il gioco utilizza immagini per:

- retro carta  
- carte numeriche nominate con convenzione valore e seme  
- carte speciali con nome in minuscolo  

La GUI carica e ridimensiona le immagini a runtime.  
Implementazione in `tesori_nascosti_gui`.

### Audio

Nel menu principale viene avviata musica di sottofondo in loop con volume ridotto.  
Implementazione in `main.py`.

## Credits

### Autori

- Riccardo Sciabbarrasi  
- Andrea Barilà  
- Marco Oliveri  
