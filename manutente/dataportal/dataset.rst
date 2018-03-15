**********************************
Gestione dei dataset 
**********************************

La metadatazione e il caricamento di un dataset sul DAF sono operazioni concesse solamente a specifiche utenze dell'organizzazione che assumono il ruolo di *Editor*. Tali operazioni possono essere eseguite sfruttando le funzionalità presenti nel menu *Toolkit > Dataset > Crea scheda e carica*.

La ricerca e l'accesso ai dataset sono funzionalità accessibili a tutti gli utenti, i dataset rilasciati in formato open data sono direttamente ricercabili dall'`interfaccia pubblica del portale <https://dataportal.daf.teamdigitale.it/>`_, i dataset caricati e appartenenti alla propria organizzazione sono accedibili tramite l'`interfaccia privata del portale <https://dataportal-private.daf.teamdigitale.it>`_ secondo i permesssi selezionati.


============================
Elenco e ricerca dei dataset
============================

La funzionalità di elenco e ricerca è attivabile dalla voce di menu *Dataset* o attraverso il filtro di ricerca presente in alto. In questo secondo caso i dataset mostrati sono quelli rispondenti alla ricerca effettuata. In entrambi i casi, sono mostrati solo i dataset sui quali l’utente ha diritti di accesso. L’elenco può essere ordinato o filtrato; è inizialmente composto dai primi 10 dataset, continuando a scorrere la lista vengono man mano caricati i successivi.

Cliccando su un dataset viene mostrata una pagina contenente:

* informazioni di dettaglio relative al dataset
* la possibilità di eseguire il download del dataset
* un’anteprima del contenuto del dataset
* l’API endpoint per effettuare chiamate REST API
* informazioni di accesso agli strumenti di analisi: Superset e Jupyter
* eventuali slice realizzare con il dataset


=================================
Caricamento di un nuovo dataset
=================================

Per inserire un nuovo dataset all'interno della piattaforma DAF è necessario prima di tutto eseguire una procedura di metadatazione, così da definire le caratteristiche del flusso di ingestion e quelle del dataset stesso.

Nel caso di un flusso batch, inoltre, i file dovranno essere caricati su un canale SFTP.

**Nota:** la prima volta che si definisce un flusso batch è necessario effettuare un accesso preliminare all'host daf.teamdigitale.it (porta 22) utilizzando il proprio nome utente e la propria password. Il nome utente è disponibile accedendo dal portale ai dettagli del proprio profilo.

Creazione e metadatazione del dataset 
=====================================
Per iniziare la procedura di caricamento di un nuovo dataset è necessario cliccare nel menu *Toolkit > Dataset > Crea scheda e carica*.

Il cruscotto di registrazione si compone di 3 passi.

Passo 1: Carica file e descrivi le colonne
******************************************

Il primo passo permette di definire il tipo di file utilizzato per la trasmissione dei dati e la modalità di caricamento. Al momento sono supportati i formati CSV e JSON (quest'ultimo in alpha test) che possono essere conferiti in modalità batch o indicando un servizio web da interrogare periodicamente (quest'ultimo in alpha test).

In entrambi i casi, il processo di metadatazione è supportato da una procedura di inferenza su un file campione rappresentativo del dataset reale. Questo permette al sistema di inferire la struttura del dataset (campi e tipi dei campi) e di proporla all'utente. Per i flussi batch caricare il file campione mediante il *Drag and Drop*

E' necessario che il file campione rispetti alcune regole:

* Il numero di record deve esser limitato (max 50 righe)
* I nomi dei campi non devono avere spazi al loro interno
* I nomi dei campi devono essere tutti diversi
* I nomi dei campi non devono contenere i caratteri ‘@’ e ‘-’
* Dimensione del file sample massima:
* Devono rappresentare il file nella sua interezza, questo vuol dire che l’utente che carica il file si deve preoccupare di inserire nelle righe del sample, records coerenti con il tipo di dato che ci si aspetta sulla tabella finale.
* I file con separatore devono contenere l’header sia nel sample che in tutti i file che vengono caricati successivamente
* Encoding UTF-8

Dopo aver caricato il file verrà visualizzata una schermata con l'elenco dei campi inferiti e per ognuno un campione dei valori contenuti.

Inizialmente è necessario indicare se il dataset è privato; in caso contrario, il dataset sarà automaticamente aggiunto tra quelli fruibili liberamente al di fuori dell'organizzazione e come open data. 

Successivamente, per ogni campo del dataset è possibile indicare:

* tipo del campo: scegliere il tipo di dato rappresentato (int, string, timestamp...)
* concetto semantico: individuare nelle ontologie installate il concetto espresso dal campo; il sistema fornisce suggerimenti man mano che viene valorizzato
* descrizione: inserire una descrizione per il campo
* tags
* obbligatorio: indicare se il campo è obbligatorio o meno
* tipo della colonna

Passo 2 - Metadati: 
*******************

Indicare:

* il titolo
* una descrizione del dataset
* categoria
* licenza
* organizzazione di appartenenza (nel caso di dataset pubblico selezionare default_org)

Passo 3 - Modalitá di invio
***************************

Definire:

* Dominio e sottodominio di appartenenza del dataset

al momento trascurare i seguenti campi:

* se il dataset definisce uno standard
* se il dataset segue uno standard
* il tipo di lettura del dataset
* il tipo di dataset

Al termine si viene indirizzati alla pagina di dettaglio del dataset. Si ricordi però che non è presente ancora alcun dato, quindi il download e l'anteprima non produrranno alcun risultato.


Caricamento dei dataset via SFTP
================================

Nel caso di caricamento di dataset in modalità batch mediante canale SFTP effettuare l'accesso all'host daf.teamdigitale.it (porta 22) utilizzando il nome utente e la propria password (il nome utente è disponibile accedendo dal portale ai dettagli del proprio profilo).

Caricare il/i file relativi al dataset definito in precedenza al path che sarà stato creato dal sistema. La struttura segue la convenzione ``/home/utente/dominio/sottodominio/dataset/``.

Tutti i file che verranno man mano caricati in tale cartella incrementeranno il contenuto del dataset. Un processo in ascolto si occuperà di acquisire i file al massimo entro qualche minuto, in base alle dimensioni del file.


Caricamento con interrogazione di web service
=============================================
Funzionalità disponibile a breve
