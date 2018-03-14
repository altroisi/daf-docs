**********************************
Utenti, Workgroup e ruoli 
**********************************

============================
Utenti
============================
Un utente può appartenere a più di una organizzazione e a diversi workgroup. Questa flessibilità è concessa in virtù del fatto che quando il DAF verrà integrato con SPID un utente può avere una sola identità (quindi utenza), non sarebbe quindi possibile creare più utenze per un utente. 
La visibilità di un dataset può essere cross-organizzativa perché i permessi in lettura/scrittura di un dataset possono essere concessi ad un workgroup oppure ad una organizzazione qualsiasi.
Ogni volta che viene creato un utente questo viene associato ad un'organizzazione di default open_data che raccoglie tutti gli open data raccolti tramite harvesting. Quando saranno disponibili i workgroup, verrà anche associato ad un workgroup di default open_workgroup nel quale confluiranno tutti i dataset delle organizzazioni che vogliono essere condivisi con tutti gli utenti del DAF. 

============================
Workgroup
============================
Saranno utilizzati per definire un cono di visibilità sui dataset più atomico rispetto a quello previsto dalla multitenancy implementata con le organizzazioni. Quando un dataset viene creato solo l’utenza di chi lo ha creato può accedervi (sia sul filesystem HDFS che su query engine come Impala). I workgroup permetteranno di definire un raggruppamento di utenze, più ristretto rispetto all’intera organizzazione, nell’ambito del quale l’utente che ha creato il dataset potrà concedere i permessi di lettura e/o scrittura.

Un workgroup apparterrà ad una ed una sola una organizzazione. 


============================
Ruoli
============================
Ad ogni utente è associato un ruolo (realizzati mediante gruppi LDAP). A differenza di organizzazioni e i workgroup, questi non verranno utilizzati per definire i permessi di accesso ai dati ma per profilare l’utilizzo delle funzionalità del DAF. 

Nell’ambito di una organizzazione si definiscono tre ruoli:

* **Admin** - Potrà creare workgroup nell'ambito dell’organizzazione. Potrà associare utenti nella propria organizzazione e associare utenti della propria organizzazione ai propri workgroup. Gestirà la configurazione dell'interfaccia pubblica della propria organizzazione. Gestirà la condivisione delle data stories nella propria organizzazione. 

* **Editor** - Potrà fare l'ingestion di file, modificare dataset e relative tabelle. Potrà creare dashboards e data stories. Ne gestirà la pubblicazione. 

* **Viewer** - Potrà solo utilizzare solo le funzioni di visualizzazione. 


**********************************
Gestione dei dataset 
**********************************

La metadatazione e il caricamento di un dataset sul DAF sono operazioni concesse solamente a specifiche utenze dell'organizzazione che assumono il ruolo di *Editor*. Tali operazioni possono essere eseguite sfruttando le funzionalità presenti nel menu *Toolkit > Dataset > Crea scheda e carica*.

La ricerca e l'accesso ai dataset sono funzionalità accessibili a tutti gli utenti, i dataset rilasciati in formato open data sono direttamente ricercabili dall'`interfaccia pubblica del portale <https://dataportal.daf.teamdigitale.it/>`_, i dataset caricati e appartenenti alla propria organizzazione sono accedibili tramite l' `interfaccia privata del portale <https://dataportal-private.daf.teamdigitale.it>`_ secondo i permesssi selezionati.


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

Per inserire un nuovo dataset all'interno della piattaforma DAF è necessario eseguire due procedure distinte:

* la metadatazione del dataset per deinire come verranno interpretati i dati al suo interno
* il caricamento vero e proprio del dataset contenente i dati (al momento esclusivamente via SFTP)

  
Creazione e metadatazione del dataset 
=====================================
Per iniziare la procedura di caricamento di un nuovo dataset è necessario cliccare nel menu *Toolkit > Dataset > Crea scheda e carica*.

Chi crea il dataset è responsabile della sua visibilità e potrà lui solo scegliere i workgroup o le organizzazioni da inserire. Quando viene creato il dataset questo viene salvato nell’infrastruttura Big Data con i permessi legati al solo utente che l'ha creato. Il DAF mette a disposizione funzioni con le quali l’utente può associare al suo dataset uno o più workgroup e/o organizzazioni con i diritti che preferisce.

La funzione *Carica* consente di definire un flusso di acquisizione per un nuovo dataset associato all'organizzazione. I dati dovranno poi essere caricati attraverso il canale che verrà istaurato. 


**Nota:** la prima volta che un utente editor iniziare il processo di metadatazione è necessario eseguire questa procedura affinché struttura delle cartelle poi indicata alla fine della procedura venga correttamente caricata:

**Autenticazione sull’host SFTP**

Effettuare l’accesso all’host daf.teamdigitale.it (porta 22) utilizzando il nome utente e la propria password. Il nome utente è disponibile accedendo dal portale ai dettagli del proprio profilo.


Il cruscotto di registrazione di compone di 3 passi.

**Passo 1: Carica file e descrivi le colonne**

Nel primo è possibile caricare un file campione rappresentativo del dataset oggetto del flusso. Il tipo supportato al momento è il CSV, a breve sarà possibile caricare anche file JSON. Questo permette al sistema di inferire la struttura del dataset (campi e tipi dei campi) e di proporla all'utente. 

La modalità di caricamento ad oggi funzionante è quella denominata "Drag and Drop".


E' necessario che il file campione rispetti alcune regole:

* Il numero di record deve esser limitato (max 50 righe)
* I nomi dei campi non devono avere spazi al loro interno
* I nomi dei campi devono essere tutti diversi
* I nomi dei campi non devono contenere i caratteri ‘@’ e ‘-’
* Dimensione del file sample massima:
* Devono rappresentare il file nella sua interezza, questo vuol dire che l’utente che carica il file si deve preoccupare di inserire nelle righe del sample, records coerenti con il tipo di dato che ci si aspetta sulla tabella finale.
* I file con separatore devono contenere l’header sia nel sample che in tutti i file che vengono caricati successivamente
* Encoding UTF-8

Dopo aver caricato il sample verrà visualizzata una schermata con l'elenco dei campi inferiti dal file di sample e per ognuno dei campione dei valori contenuti.

Inizialmente è necessario indicare se il dataset è privato; in caso contrario, il dataset sarà automaticamente aggiunto tra quelli fruibili liberamente al di fuori dell'organizzazione e come open data. 

Successivamente, per ogni campo del dataset è possibile indicare:
Per ogni campo è necessario specifiare le seguenti informazioni:

* tipo del campo: scegliere il tipo di dato rappresentato (int, string, timestamp...)
* concetto semantico: individuare nelle ontologie installate il concetto espresso dal campo; il sistema fornisce suggerimenti man mano che viene valorizzato
* descrizione: inserire una descrizione per il campo
* tags
* obbligatorio: indicare se il campo è obbligatorio o meno
* tipo della colonna

**Passo 2 - Metadati:** 

Indicare 

* il titolo
* una descrizione del dataset
* categoria
* licenza
* organizzazione di appartenenza

**Passo 3 - Modalitá di invio**

Definire:

* Dominio di appartenenza del dataset

al momento trascurare i seguenti campi:

* se il dataset definisce uno standard
* se il dataset segue uno standard
* il tipo di lettura del dataset
* il tipo di dataset

Al termine viene fornito un riepilogo, con l'indicazione del path SFTP sul quale è possibile caricare i dati. Il nuovo dataset è visibile nell'elenco mostrato cliccando sulla casella di ricerca presente in alto. Si ricordi però che non è presente ancora alcun dato.



Caricamento dei dataset via SFTP
================================
Nel caso di caricamento di dataset in modalità batch mediante canale SFTP (unica disponibile al momento), prima di procedere è necessario effettuare un accesso preliminare, come indicato di seguito.

Step 1: Autenticazione sull’host SFTP

Effettuare l’accesso all’host daf.teamdigitale.it (porta di default 22) utilizzando il nome utente e la propria password. Il nome utente è disponibile accedendo dal portale ai dettagli del proprio profilo.

Step 1: Caricamento del dataset
Caricare il/i file relativi al dataset definito in precedenza al path che sarà stato creato dal sistema. La struttura segue la convenzione ``/home/utente/dominio/sottodominio/dataset/``.

Tutti i file che verranno man mano caricati in tale cartella incrementeranno il contenuto del dataset. Un processo in ascolto si occuperà di acquisire i file al massimo entro qualche minuto, in base alle dimensioni del file.


Caricamento con interrogazione di web service
=============================================
Funzionalità disponibile a breve





