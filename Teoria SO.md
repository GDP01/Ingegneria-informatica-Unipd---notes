## Indice

- [[#1. Introduzione]]
	- [[#1.1 Cos'è un Sistema Operativo?]]
	- [[#1.2 Struttura del sistema di un computer]]
	- [[#1.3 Cosa fa un SO?]]
	- [[#1.4 Ruoli del SO]]
	- [[#1.5 Definizione di SO]]
	- [[#1.6 Storia dei SO]]
	- [[#1.7 Organizzazione del sistema del computer]]
	- [[#1.8 Operazioni di sistema del computer]]
	- [[#1.9 Funzionalità comuni degli interrupt]]
	- [[#1.10 Sistema Multiprocessore]]
	- [[#1.11 Architettura SMP]]
	- [[#1.12 Multi-core design]]
	- [[#1.13 Non-uniform memory access system (**NUMA**)]]
	- [[#1.14 Sistemi Embedded]]
	- [[#1.15 Operazioni del SO]]
	- [[#1.16 Operazioni Dual-mode (Importanti!)]]
	- [[#1.17 Transizione dalla modalità utente a quella kernel]]
	- [[#1.18 Multiprogramming (batch system)]]
	- [[#1.19 Multitasking (time sharing)]]
	- [[#1.20 Gestione dei Processi]]
	- [[#1.21 Attività di gestione dei processi]]
	- [[#1.22 Gestione della memoria]]
	- [[#1.23 Attività di gestione della memoria]]
	- [[#1.24 Struttura di stoccaggio]]
	- [[#1.25 Gerarchia e tipi di stoccaggio]]
	- [[#1.26 Gestione dell'archiviazione di massa]]
	- [[#1.27 Gestione del File-system]]
	- [[#1.28 Sottosistema I/O]]
	- [[#1.29 Protezione e sicurezza]]
- [[#]]
## 1. Introduzione

### 1.1 Cos'è un Sistema Operativo?

- Un sistema operativo è un programma che fa da intermediario tra l'utente e l'hardware di un computer
- Obiettivi: 
	- Eseguire i programmi dell'utente e risolvere problemi utente
	- Rende il sistema conveniente da usare
	- Usa l'hardware in modo efficiente
- Facilita l'uso e lo sviluppo di programmi per computer in un ambiente comune, senza preoccuparsi dell'hardware

### 1.2 Struttura del sistema di un computer

1. **Utenti**: persone, macchine, altri computer
2. **Programmi Applicativi**: definiscono come vengono utilizzate le risorse per risolvere i problemi informatici degli utenti
3. **Sistema Operativo**: controlla e coordina l'uso dell'hardware tra varie applicazioni e utenti
4. **Hardware**: fornisce risorse di base come CPU, memoria, dispositivi I/O, ...

![[1. Modello a cipolla SO.png|250]]![[2. Computer system structure.png|360]]

### 1.3 Cosa fa un SO?

- Gli utenti spesso **condividono risorse** (es. dai **server**)
- **Dispositivi mobili** hanno Il controller informa la CPU di aver terminato la sua operazione causando un'interruzione, ottimizzate per l'utilizzo e batteria
- **computers embedded** possono non avere un interfaccia utente

- Ma l'utente vuole **convenienza, facilità d'uso** e buone **performance**
	- non gli interessa dell'utilizzo delle risorse
- Inoltre il sistema operativo deve soddisfare tutti gli utenti
$=>$ **OS è un allocatore di risorse** che utilizza in modo efficiente l'HW e gestisce l'esecuzione dei programmi utente

![[3. what operating system do.png|100]]

### 1.4 Ruoli del SO

- Nato quando l'**uso fisso** dei computer (per applicazioni militari) divenne più **general purpose** e necessitava di gestione delle risorse e controllo dei programmi

### 1.5 Definizione di SO

- Non c'è una definizione accettata universalmente
- *"L'unico programma in esecuzione in ogni momento sul computer"* è il **kernel**, parte del SO
- Anche tutto il resto lo è
	- Un **system program** (viene fornito con il SO, ma non fa parte del kernel), o
	- Un **application program**, tutti i programmi non associati al SO (es. applicazioni aziendali, ambiente di sviluppo, giochi, ...)
- Gli attuali SO per l’informatica generale e mobile includono anche il **middleware**, un insieme di software frameworks che forniscono servizi aggiuntivi agli sviluppatori di applicazioni come database, multimedia, grafica

![[4. OS definition.png|300]]

### 1.6 Storia dei SO

- (1945-55) Plugboards, vacuum tubes
	- programmi caricati manualmente tramite switch

- (1955-65) Transistors e sistemi batch
	- Programmi e dati scritti nelle carte perforate (punched cards)
	- Serviva troppo tempo per operazioni di I/O

- (1965-80) ICs e Multiprogrammazione
	- **Multiprogramming**: due o più programmi eseguiti allo stesso tempo per massimizzare l'utilizzo del processore

- (1980-oggi) Personal Computers
	- dominanza di DOS/Windows systems
	- numero significativo di utenti Linux e MAC

![[5. Microsoft Windows.png|500]]

- Android
	- Sviluppato da Andy Rubin e soci
	- Progettato per alimentare il telefono Sidekick (cellulari con tastiera)
	- Sfide iniziali
		- Limite potenza batteria
		- CPU piccole
		- Spazio di memoria limitato
	- Funzionalità Android
		- Progettato per essere eseguito su un dispositivo mobile: nello specifico in smartphone e tablet
		- Basato sul kernel di Linux
		- Open source (non tutto)

>[!Example] Esempi
>
>![[Pasted image 20260306183255.png|200]]

### 1.7 Organizzazione del sistema del computer

- SO strettamente correlato al sistema informatico
	- Una o più CPU, i controller dei dispositivi si collegano tramite un bus comune che fornisce accesso alla memoria condivisa
	- Esecuzione simultanea di **CPU e dispositivi in competizione per i cicli di memoria**

![[6. Computer-system organization.png|400]]

### 1.8 Operazioni di sistema del computer

- Dispositivi I/O e la CPU possono essere eseguiti contemporaneamente
- Ogni **device controller** è responsabile di un particolare tipo di dispositivo
- Ogni tipo di device controller ha un **device driver** del SO per gestirlo
- Il controller informa la CPU di aver terminato la sua operazione causando un'**interrupt**

### 1.9 Funzionalità comuni degli interrupt

- Interrupt = segnali che fermano la CPU per soddisfare richieste con alta priorità
- L'interruzione trasferisce il controllo alla **interrupt service routine**, generalmente tramite un **interrupt vector**, che contiene gli indirizzi di tutte le routine di servizio
- L'architettura di interrupt deve salvare l'indirizzo dell'istruzione interrotta
- Una **trap** (o **exception**) è un'**interruzione generata dal software** causata da un errore o da una richiesta dell'utente
- SO sono **interrupt driven**

![[7. Interrupt timeline.png|600]]

### 1.10 Sistema Multiprocessore

- In origine, la maggior parte dei sistemi utilizzava un singolo processore generico
- I sistemi **multiprocessore** sono cresciuti di utilizzo e importanza
	- Conosciuti anche come "**parallel systems**", o sistemi strettamente accopiati
	- I vantaggi includono:
		- Aumento della **produttività**
		- **Economia di scala** (- costi + efficienza)
		- Aumento dell'**affidabilità**: degradazione graduale o tolleranza agli errori
	- 2 tipi:
		- **Asymmetric Multiprocessing** - ad ogni processore è assegnato un *task specifico*
		- **Symmetric Multiprocessing** (**SMP**) - ogni processore può eseguire *qualsiasi task*

### 1.11 Architettura SMP

- Chip differenti
- Memoria condivisa

![[8. SMP architecture.png|300]]

### 1.12 Multi-core design

- Multi-chip e **multi-core**
- Ogni core con la propria cache (L1)
- Condivisione di una cache (L2) più grande ma più lenta

![[9. Multi-core design.png|300]]

### 1.13 Non-uniform memory access system (**NUMA**)

- Le CPU hanno un accesso rapido alle aree di memoria dedicato
- Sono interconnesse e condividono uno spazio di indirizzamento fisico
- Vantaggi: 
	- Accesso rapido alla memoria locale
	- Nessuna controversia sull'interconnessione del sistema
	- Può essere scalato in modo più efficace aggiungendo più processori
- Potenziali svantaggi:
	- Aumento della latenza quando la CPU accede alla memoria remota (ad esempio $CPU_0$ → $memory_3$): il SO può ridurre al minimo questo problema tramite un'attenta pianificazione della CPU (scheduling) e una gestione della memoria (memory management)
- NUMA è popolare su server e sistemi di elaborazione ad alte prestazioni

![[10. NUMA.png|200]]

### 1.14 Sistemi Embedded

- I sistemi embedded sono la forma più diffusa di computer
	- SO degli embedded variano considerevolmente: scopi speciali o limitati, real-time
	- Alcune task senza SO
- **SO real-time** ha vincoli di tempo fissi ben definiti
	- L'elaborazione deve essere effettuata entro i limiti
	- Funzionamento corretto solo se rispettati i vincoli

![[11. Embedded systems.png|350]]

### 1.15 Operazioni del SO

1. Bootstrap loader program: codice che inizializza il sistema e carica il kernel
2. Caricamento del kernel
3. Avvio dei **system daemons** (servizi forniti al di fuori del kernel)
4. Kernel **interrupt driven** (Hardware e Software)
	- **Interruzione Hardware** da parte di uno dei dispositivi
	- **Interruzione Software** (**exception** o **trap**):
		- Errore Software (es. dividere per 0)
		- Richiesta di servizio del SO - **system call**
		- Altre cause, es.: cicli infiniti, processi che tentano di modificare altri processi o il SO, ecc.

### 1.16 Operazioni Dual-mode (Importanti!)

- Il funzionamento a doppia modalità consente al SO di proteggere sé stesso e gli altri componenti del sistema
	- **modalità utente** e **modalità kernel**
	- **modalità bit** fornita dall'HW
		- consente di distinguere quando il sistema sta eseguendo codice utente o codice kernel
		- quando un utente è in esecuzione, il mode bit è "utente"
		- quando il codice del kernel è in esecuzione, il bit di modalità è "kernel"

### 1.17 Transizione dalla modalità utente a quella kernel

- Come possiamo garantire che l'utente non imposti esplicitamente il mode bit su "kernel"?
	- La chiamata di sistema cambia modalità in kernel (tramite interrupt software)
	- Il ritorno dalla chiamata lo reimposta sull'utente
- Alcune istruzioni sono designate come privilegiate, eseguibili solo in modalità kernel

![[12. Transition from user to kernel mode.png|550]]

### 1.18 Multiprogramming (batch system)

Un sistema operativo batch è un vecchio tipo di sistema informatico che esegue gruppi di jobs (tasks) insieme
- Un singolo utente non sempre tiene occupati i dispositivi CPU e I/O
- La multiprogrammazione organizza i **lavori** (codice e dati) in modo che la CPU ne abbia sempre uno da eseguire
- Un sottoinsieme dei lavori totali nel sistema viene mantenuto in memoria
- Un lavoro viene selezionato ed eseguito tramite **job scheduling**
- Quando un lavoro deve attendere (ad esempio per I/O), il SO passa a un altro lavoro
- In genere **non c'è interazione** con l'utente durante l'esecuzione del lavoro

### 1.19 Multitasking (time sharing)

- Un'estensione dei sistemi Batch: la CPU può cambiare lavoro così frequentemente che gli utenti possono interagire con ogni lavoro durante l'esecuzione, creando un'**elaborazione interattiva**
	- Il **tempo di risposta** dovrebbe essere < 1 secondo
	- Ogni utente ha almeno un programma in esecuzione in memoria, chiamato **processo**
	- Se più processi sono pronti per essere eseguiti contemporaneamente, c'è bisogno della **CPU scheduling**
	- Se i processi non si adattano alla memoria, lo swapping li sposta dentro e fuori per eseguirli
		- La memoria virtuale consente l'esecuzione di processi caricati solo parzialmente nella memoria

![[13. Multitasking (time sharing).png|100]] ![[14. Panoramica SO.png|318]]

### 1.20 Gestione dei Processi

- **Un processo è un programma in esecuzione**
	- Programma = entità passiva, Processo = entità attiva
- Il processo ha bisogno di risorse (CPU, memoria, I/O, file) + dati di inizializzazione per svolgere il suo compito
- Il SO recupera tutte le risorse riutilizzabili rimaste dopo la terminazione del processo
- Il processo a thread singolo (Single-threaded process) ha *un PC* che specifica la posizione della prossima istruzione da eseguire
	- Le istruzioni del processo vengono **eseguite in sequenza**, una alla volta, fino al completamento
- Un processo Multi-threaded ha *un PC per thread*
- I processi del SO e dell’utente vengono eseguiti contemporaneamente su una o più CPU

### 1.21 Attività di gestione dei processi

- Il SO è responsabile delle seguenti attività di gestione dei processi:
	- **Creazione** e **eliminazione** di processi utente e di sistema
	- **Sospensione** e **ripresa** dei processi
	- Fornire meccanismi per la **sincronizzazione**, **comunicazione**, **gestione dello stallo** (deadlock handling) dei processi

### 1.22 Gestione della memoria

- Per eseguire un programma, tutte (o in parte) le **istruzioni** *devono essere in memoria*
- Tutti (o in parte) i **dati** richiesti dal programma *devono essere in memoria*
- La gestione della memoria determina **cos'è** in memoria e **quando**
	- Ottimizzazione dell'utilizzo della CPU e della risposta del computer agli utenti

### 1.23 Attività di gestione della memoria

- Il SO è responsabile delle seguenti attività di gestione della memoria:
	- Tiene traccia di quali parti della memoria sono attualmente utilizzate e da chi
	- Decide quali processi (o parti di essi) e dati spostare dentro e fuori dalla memoria
	- **Allocazione** e **deallocazione** dello spazio di memoria secondo necessità

### 1.24 Struttura di stoccaggio

- **Memorie principali** - l'unico grande supporto di memorizzazione a cui la *CPU può accedere direttamente*
	- **Random access**
	- Tipicamente **volatile**
	- es. DRAM
- **Memorie secondarie** - estensione della memoria principale che fornisce una grande capacità di archiviazione **non volatile** (ad esempio dischi rigidi)
- **Memorie terziarie** - come quelle secondarie, ma più grandi e lenti (ad esempio i nastri magnetici)

### 1.25 Gerarchia e tipi di stoccaggio

Sistemi di archiviazione organizzati in gerarchia
- Velocità
- Costi
- Volatilità

![[16. Gerarchia di stoccaggio.png|400]]
![[Pasted image 20260307231238.png|500]]

### 1.26 Gestione dell'archiviazione di massa

- Di solito i dischi vengono utilizzati per memorizzare dati che non entrano nella memoria principale o dati che devono essere conservati per un periodo di tempo "lungo"
- L'intera velocità di funzionamento del computer dipende dal sottosistema del disco e dai suoi algoritmi
- Attività SO
	- **Montaggio** e **smontaggio**
	- Gestione del **free-space**
	- **Allocazione** dello spazio di archiviazione
	- **Scheduling** del disco
	- **Partizionamento**
	- **Protezione**

### 1.27 Gestione del File-system

- Il SO fornisce una visione logica e uniforme dell'archiviazione delle informazioni
	- Astrae le proprietà fisiche in unità di archiviazione logica - **file**
	- Ogni supporto è controllato da un dispositivo dedicato (ad esempio, unità disco, unità nastro)
		- Le proprietà includono velocità di accesso, capacità, velocità di trasferimento dati, metodo di accesso (sequenziale o casuale)
- Gestione del File-System
	- File solitamente organizzati in **directories**
	- Controllo degli accessi sulla maggior parte dei sistemi per determinare **chi può accedere a cosa**
	- Le attività del sistema operativo includono
		- **Creazione** ed **eliminazione** di file e directory
		- primitive per **manipolare** file e directory
		- **Mappatura** dei file su storage secondario
		- eseguire il **backup** dei file su supporti di memorizzazione stabili (non volatili)

### 1.28 Sottosistema I/O

- Uno degli scopi del sistema operativo è quello di nascondere all'utente le peculiarità dei dispositivi hardware.
- Sottosistema I/O responsabile di
	- Gestione della memoria di I/O, incluso il **buffering** (memorizzazione temporanea dei dati durante il trasferimento), la **memorizzazione nella cache (caching)** (memorizzazione di parti di dati in un archivio più veloce per migliorare le prestazioni), lo **spooling** (sovrapposizione dell'output di un lavoro con l'input di altri lavori)
	- **Drivers** per dispositivi hardware specifici
	- **Interfaccia** generale device-driver

### 1.29 Protezione e sicurezza

- **Protezione** - qualsiasi meccanismo per controllare l'accesso dei processi o degli utenti alle risorse definite dal sistema operativo
- **Sicurezza** – difesa del sistema contro attacchi interni ed esterni
	- Ampia gamma, tra cui denial-of-service, worm, virus, furto di identità, furto di servizio
- I sistemi generalmente distinguono prima tra gli utenti, per determinare chi può fare cosa
	- Le identità utente (**user IDs**, security IDs) includono nome + numero associato, uno per utente
		- User ID quindi associato a tutti i file, processi di quell'utente per determinare il controllo di accesso
	- L'identificatore di gruppo (**group ID**) consente di definire un insieme di utenti e di gestire i controlli, quindi di associarli a ciascun processo, file

