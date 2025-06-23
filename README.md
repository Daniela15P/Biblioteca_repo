# Biblioteca_repo


# SET UP DELL'AMBIENTE

E' possibile installare MongoDB Locale.
Si consiglia di eseguire l'installazione locale di MongoDB se si vuole lavorare offline, se si vuole avere il controllo totale sul server e se si vuole testare direttamente ciò che si scrive in locale.


## INSTALLAZIONE DI MongoDB LOCALE
### STEP 1

Installa MongoDB, visita il sito `https://www.mongodb.com/try/download/community` e scarica la versione che desideri di Community MongoDB. Seleziona quindi il tuo sistema operativo e prosegui con l'installazione. 

### STEP 2

Verifica se l'installazione è andata a buon fine eseguendo il seguente comando:
````
mongosh
````
**mongosh** è la shell ufficiale di MongoDB, in particolar modo è un client interattivo a riga di comando.
Lanciando questo comando e non ottenendo errori di nessun tipo significherà che l'installazione sarà andata a buon fine.

### STEP 3

Successivamente avvia il **server** eseguendo il seguente comando:
````
mongod
````
**mongod** è il MongoDB Daemon, è quindi il programma server di MongoDB. Si occupa di avviare il server MongoDB, gestire i database e accettare connessioni dai client. Perciò MongoDB senza mongod in esecuzione non funzionerebbe perchè il client non potrebbe connettersi.

### STEP 4
Poi è necessario connettersi al server tramite la shell di MongoDB con il seguente comando:

````
mongosh
````
Connettendomi dovrei ottenere questo prompt o prompt simili: 

````bash
test>
````


# DOCUMENTO SULLA BASE DATI

Un gestionale per la biblioteca dovrebbe avere diverse collezioni, come:
- AUTORE
- LIBRO
- UTENTE


Per la realizzazione di questo gestionale ho utilizzato sia l'incorporamento che il riferimento.
L'incorporamento, detto anche embedding, consiste nell'inserimento all'interno del documento principale dei dati. Creando così una struttura nidificata.
Nel riferimento invece, detto anche referencing, i dati correlati vengono archiviati in dpcumenti separati e collegati tramite un identificatore, come l'id.

All'interno di libri ho utilizzato il riferimento per fare in modo che vengano riportati gli id degli autori, che risultano essere chiave primaria nella tabella autori.
Invece all'interno della tabella utente ho utilizzato l'inserimento, perciò sarà possibile visualizzare la data di inizio, la data di fine e la data di restituzione di ogni prestito all'interno dell'utente.

# JSON DI ESEMPIO STRUTTURA

### Libro
````json
[
  {
    "id_libro": "L1",
    "titolo": "1984",
    "id_autore": "A1",
    "anno_pubblicazione": 1949,
    "genere": "Distopia",
    "copie_totali": 5,
    "copie_disponibili": 3
  }
]


````

### Autore
````json
[
  {
    "id_autore": "A1",
    "nome": "George",
    "cognome": "Orwell"
  }
]

````

### Utente
````json

[
  {
    "id_utente": "U1",
    "nome": "Mario",
    "cognome": "Rossi",
    "data_nascita": "1980-05-15",
    "prestiti": [
      {
        "id_prestito": "P1",
        "id_libro": "L1",
        "data_inizio_prestito": "2025-06-01",
        "data_fine_prestito": "2025-06-21",
        "data_restituzione": null
      }
    ]
  }
]

````

# SCRIPT DI CARICAMENTO DATASET

Nella mongosh usa il comando:
````
use nome_database
````
Questo comando permette di creare un database e di nominarlo come si vuole. Se adesso ricarichiamo i database, quello appena creato non sarà visibile in quanto ancora vuoto. Successivamente procediamo ad aggiungere libri, autori e utenti.

### **Caricamento dei libri**

Per inserire la collezione libri nel database e i suoi dati metti questo comando nella mongosh:

```js
db.libri.insertMany([
  {
    "id_libro": "L1",
    "titolo": "1984",
    "id_autore": "A1",
    "anno_pubblicazione": 1949,
    "genere": "Distopia",
    "copie_totali": 5,
    "copie_disponibili": 3
  },
  {
    "id_libro": "L2",
    "titolo": "Il nome della rosa",
    "id_autore": "A2",
    "anno_pubblicazione": 1980,
    "genere": "Romanzo storico",
    "copie_totali": 4,
    "copie_disponibili": 2
  },
  {
    "id_libro": "L3",
    "titolo": "Fahrenheit 451",
    "id_autore": "A3",
    "anno_pubblicazione": 1953,
    "genere": "Fantascienza",
    "copie_totali": 6,
    "copie_disponibili": 6
  }
])
````

### **Caricamento degli autori** 
Per inserire la collezione autori nel database e i suoi dati metti questo comando nella mongosh:
```js
db.autori.insertMany([
  {
    "id_autore": "A1",
    "nome": "George",
    "cognome": "Orwell"
  },
  {
    "id_autore": "A2",
    "nome": "Umberto",
    "cognome": "Eco"
  },
  {
    "id_autore": "A3",
    "nome": "Ray",
    "cognome": "Bradbury"
  }
])
```

### **Caricamento degli utenti** 
Per inserire la collezione utenti nel database e i suoi dati metti questo comando nella mongosh:
```js
db.utenti.insertMany(
  [
  {
    "id_utente": "U1",
    "nome": "Mario",
    "cognome": "Rossi",
    "data_nascita": "1980-05-15",
    "prestiti": [
      {
        "id_prestito": "P1",
        "id_libro": "L1",
        "data_inizio_prestito": "2025-06-01",
        "data_fine_prestito": "2025-06-21",
        "data_restituzione": null
      }
    ]
  },
  {
    "id_utente": "U2",
    "nome": "Lucia",
    "cognome": "Verdi",
    "data_nascita": "1990-08-20",
    "prestiti": [
      {
        "id_prestito": "P2",
        "id_libro": "L2",
        "data_inizio_prestito": "2025-05-20",
        "data_fine_prestito": "2025-06-10",
        "data_restituzione": "2025-06-05"
      },
      {
        "id_prestito": "P3",
        "id_libro": "L1",
        "data_inizio_prestito": "2025-06-10",
        "data_fine_prestito": "2025-07-01",
        "data_restituzione": null
      }
    ]
  },
  {
    "id_utente": "U3",
    "nome": "Gianni",
    "cognome": "Neri",
    "data_nascita": "1975-12-30",
    "prestiti": []
  }
])
```
