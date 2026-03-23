# MASTER_CONTEXT — Navigatore costo viaggio Italy-first

Versione: 1.0  
Data: 19 marzo 2026  
Uso previsto: file master da allegare insieme agli altri due documenti a qualunque agente di sviluppo  
Lingua: italiano  
Stile: Claude-friendly, leggibile, poco ambiguo

---

## Scopo di questo file

Questo è il file di contesto master del progetto.

Serve a trasferire a un nuovo agente:
- il senso reale del progetto;
- le decisioni già prese;
- le motivazioni dietro le decisioni;
- i compromessi accettati;
- i dubbi ancora aperti;
- le cose che non devono essere reinterpretate da zero.

Obiettivo pratico:
se questo file viene allegato insieme ai due file precedenti, un nuovo agente deve partire con una conoscenza del progetto molto vicina a quella maturata in questa chat.

Questo file non sostituisce gli altri due.
Questo file li completa.

---

## Come usare questo file con più agenti

Usare sempre questo ordine di priorità:

1. `MASTER_CONTEXT`  
2. `navigatore_italy_first_claude_friendly.md`  
3. `navigatore_italy_first_handoff.docx`

Regola di interpretazione:
- se un agente trova un’ambiguità, deve considerare il file master come riferimento principale;
- se il master non copre quel punto, deve rifarsi al file Claude-friendly;
- il DOCX serve come versione strutturata e presentabile del progetto, non come fonte principale in caso di conflitto.

---

## Che cosa vuole davvero il committente

Il committente non vuole semplicemente:
- una mappa;
- un navigatore generico;
- un clone base di Google Maps.

Vuole un prodotto consumer che unisca:

- navigazione moderna con traffico e tempi dinamici;
- costo del viaggio in modo chiaro e utile;
- selezione del veicolo prima della navigazione;
- stima personalizzata di carburante e pedaggi;
- suggerimento intelligente di dove fermarsi a fare rifornimento;
- logica del rifornimento che tenga conto non solo del prezzo al litro, ma anche del percorso, della deviazione, del tempo perso e del costo totale finale.

Detto in modo semplice:

**il cuore del prodotto non è la mappa.**
**Il cuore del prodotto è il motore decisionale sul costo reale del viaggio.**

---

## Definizione sintetica del prodotto

Nome descrittivo temporaneo del progetto:
**navigatore costo viaggio con rifornimento intelligente**

Formula breve:
**“come un navigatore moderno, ma capace di dire quanto spenderai davvero e dove conviene fermarti”**

---

## Riassunto fedele della conversazione avuta

Questa sezione è scritta come se si stesse inoltrando la chat a un nuovo agente.

### Idea iniziale discussa

L’idea iniziale è stata descritta così:
- unire il navigatore di Google Maps;
- avere traffico e tempo di percorrenza che cambia;
- aggiungere una logica simile a ViaMichelin per il calcolo delle spese di viaggio;
- permettere la scelta del proprio modello di veicolo;
- integrare prezzi carburante;
- decidere se e dove fermarsi per rifornire al prezzo migliore lungo il tragitto;
- fare qualcosa che per i carburanti termici assomigli all’esperienza che oggi esiste per molte auto elettriche.

### Primo chiarimento importante emerso

È stato chiarito subito che non è sensato sviluppare da zero:
- mappe mondiali;
- motore traffico;
- navigazione turn-by-turn completa;
- motore routing raw.

È molto più sensato costruire il prodotto sopra uno stack esistente e investire le energie nel vero elemento differenziante.

### Elemento differenziante vero individuato

La parte davvero originale del prodotto è:
- profilo veicolo;
- costo viaggio personalizzato;
- algoritmo carburante;
- scelta sosta carburante lungo il percorso;
- UX molto chiara;
- affidabilità percepita.

### Scelta geografica del MVP

È stata presa la decisione di partire **Italy-first**.

Questo significa:
- niente espansione europea immediata nel MVP;
- focus sui dati carburante italiani;
- focus su autostrade italiane, distributori italiani, UX italiana;
- l’internazionalizzazione è una fase successiva.

### Discussione su Google vs TomTom

Inizialmente è stata valutata anche l’opzione Google, ma è emerso che:
- Google è molto forte come resa e navigazione;
- però i costi salgono facilmente quando si usano traffico, pedaggi e funzionalità avanzate;
- inoltre Google impone vincoli più stretti sull’uso combinato dei propri servizi con mappe non Google.

Da qui si è arrivati a una preferenza per **TomTom**.

### Decisione provider stack mappa/routing/navigation

Decisione presa:
**preferenza TomTom per il MVP**

Motivazioni percepite come più convincenti:
- piano free iniziale semplice da capire;
- uso commerciale previsto;
- supporto a routing, traffico, navigation e search;
- Along Route Search utile per il caso distributori lungo il percorso;
- maggiore coerenza con un MVP Italy-first.

### Discussione sui prezzi carburante

È emerso un dubbio forte:
- il dataset open ufficiale MIMIT è una fotografia quotidiana;
- quindi il prezzo visto dal sistema potrebbe essere diverso dal prezzo trovato alla pompa quando l’utente arriva.

La conclusione condivisa è stata:
- questo non blocca il MVP;
- basta non promettere un prezzo garantito in tempo reale;
- bisogna comunicare bene timestamp e natura del dato;
- per iniziare il dato ufficiale italiano va bene.

### Possibile evoluzione futura

È emerso che in futuro si potrebbe sostituire il provider prezzi carburante con un feed premium più fresco, ma non è una condizione per partire.

---

## Decisioni già prese e da non rimettere in discussione senza un motivo serio

Queste decisioni sono da considerare stabili.

### Decisione 1 — ambito iniziale

Il progetto è **Italy-first**.

### Decisione 2 — piattaforma di partenza

Il progetto parte **Android-first**.

Motivo:
TomTom lato iOS richiede un percorso più complesso / disponibile su richiesta, quindi per correre ha più senso impostare il MVP su Android.

### Decisione 3 — stack mappe/routing/navigation

La direzione preferita è **TomTom-first**.

### Decisione 4 — provider prezzi carburante per MVP

Il provider iniziale per il MVP è il **dataset ufficiale MIMIT / Osservaprezzi carburanti**, nella forma open e riusabile.

### Decisione 5 — claim di prodotto sui prezzi carburante

Il prodotto **non deve promettere prezzo garantito alla pompa all’arrivo**.

Deve invece comunicare:
- ultimo prezzo ufficiale disponibile;
- orario/data ultimo aggiornamento;
- natura stimata della convenienza.

### Decisione 6 — punto di valore principale

Il progetto non si vende come “mappa nuova”.
Si vende come:
- navigatore con costo reale del viaggio;
- veicolo personalizzato;
- rifornimento intelligente.

### Decisione 7 — filosofia tecnica iniziale

Per il MVP bisogna preferire:
- semplicità;
- spiegabilità;
- modularità;
- possibilità di sostituire il provider carburante in futuro.

---

## Cose che il progetto non è

Per evitare che un agente parta nella direzione sbagliata.

Questo progetto non è:
- un clone completo di Google Maps;
- un progetto GIS generico;
- un software per flotte aziendali;
- un navigatore professionale per camion già in versione 1;
- un comparatore puro di prezzi carburante;
- un sistema di telemetria veicolo;
- un social network per automobilisti.

Potrà evolvere in più direzioni, ma nel MVP non deve esplodere di scope.

---

## North star del progetto

La metrica concettuale più importante non è:
- quante mappe mostra;
- quante funzioni mette nel menu.

La north star è qualcosa del tipo:

**“aiutare l’utente a prendere decisioni di viaggio più economiche e più consapevoli, senza complicargli la vita”**

Quindi ogni scelta di UX o architettura dovrebbe essere giudicata così:
- rende più chiaro il costo reale?
- rende più semplice capire se fermarsi?
- migliora davvero la decisione sul rifornimento?
- evita di appesantire la navigazione?

Se la risposta è no, probabilmente non è core per il MVP.

---

## Vantaggio competitivo percepito

Il vantaggio competitivo percepito non sarà:
- “la mappa è più bella”;
- “ha un’altra skin del navigatore”.

Sarà invece:
- “questa app mi fa spendere meno e me lo fa capire subito”;
- “mi dice se conviene fermarmi”;
- “mi aiuta a scegliere il distributore giusto senza farmi impazzire”;
- “calcola davvero il mio veicolo, non una media astratta”;
- “mi mostra il costo del viaggio in un modo più utile di un navigatore classico”.

---

## Scelte prodotto implicite emerse dalla chat

Anche se non sempre dette in forma formale, queste preferenze sono emerse con chiarezza.

### 1. Il prodotto deve essere consumer-friendly

L’esperienza deve essere semplice.
Non deve sembrare un software da ingegneri del traffico.

### 2. Il valore deve essere comprensibile in pochi secondi

L’utente deve capire quasi subito:
- costo viaggio;
- carburante stimato;
- pedaggi;
- se serve una sosta;
- migliore sosta suggerita.

### 3. L’algoritmo deve essere spiegabile

Il sistema non deve sembrare magico o arbitrario.

Deve poter spiegare in modo semplice:
- perché suggerisce quella sosta;
- perché non suggerisce la stazione più economica in assoluto;
- quanto si devia;
- quanto si risparmia;
- quanto tempo si perde o si evita di perdere.

### 4. L’MVP deve essere credibile, non perfetto

Meglio un primo prodotto:
- limitato;
- onesto;
- robusto;
- chiaro;

che una piattaforma enorme ma confusa.

---

## Scelte tecniche implicite emerse dalla chat

### 1. Architettura a provider sostituibili

Almeno questi moduli devono essere sostituibili:
- map/routing/navigation provider;
- fuel price provider;
- toll cost logic se necessario;
- vehicle data logic.

### 2. Algoritmo carburante come dominio proprietario

L’algoritmo di scelta sosta carburante va trattato come cuore proprietario del prodotto.

Non deve essere pensato come un dettaglio secondario.

### 3. Il profilo veicolo non deve bloccare il MVP

Non serve partire da un mega database perfetto di marca/modello/allestimento.

È accettabile partire con:
- selezione carburante;
- consumo manuale;
- capacità serbatoio;
- riserva;
- eventualmente pochi preset.

### 4. I dati “abbastanza buoni” vanno bene per partire

Il progetto non deve fermarsi aspettando:
- dati prezzi perfetti in tempo reale;
- database veicoli perfetto;
- copertura europea;
- iOS completo.

---

## Perché TomTom è stato preferito in questa fase

Non perché sia “migliore in assoluto” in tutto.
Ma perché per questo progetto, in questa fase, sembra più coerente.

Motivi chiave:
- pricing iniziale più semplice da digerire;
- disponibilità di search along route;
- stack utile per Android-first;
- minore attrito concettuale rispetto ai tier Google che salgono quando si attivano traffico/pedaggi/moto;
- buon fit per MVP focalizzato e non per clone completo di Maps.

Nota importante per gli agenti:
questa è una **preferenza forte**, ma non una religione.
Se in fase di sviluppo emerge un blocco tecnico serio e documentato, allora si può riesaminare.
Non si deve però tornare a discutere Google vs TomTom a ogni passo.

---

## Perché i dati MIMIT sono stati accettati per il MVP

La scelta è nata da un compromesso ragionato.

Problema noto:
- il dato open ufficiale non è sempre il prezzo reale esatto in quell’istante alla pompa.

Perché è stato comunque accettato:
- è una fonte ufficiale italiana;
- è sufficiente per trip planning e suggerimento;
- consente di partire senza accordi commerciali premium;
- è adatta a un MVP Italy-first;
- il rischio si gestisce bene con UX e trasparenza.

Condizione obbligatoria:
l’app deve essere trasparente sull’aggiornamento del dato.

---

## Come va comunicato il prezzo carburante all’utente

Questo punto è molto importante.

Da evitare:
- “prezzo garantito”
- “miglior prezzo certo”
- “questa pompa costerà sicuramente X quando arrivi”

Da preferire:
- “ultimo prezzo ufficiale disponibile”
- “aggiornato al…”
- “sosta consigliata in base ai dati disponibili”
- “convenienza stimata”
- “prezzo indicativo basato sull’ultimo aggiornamento ufficiale”

---

## Comportamento desiderato del motore di suggerimento carburante

L’algoritmo non deve scegliere semplicemente:
**il prezzo più basso nel raggio**

Deve scegliere:
**la sosta migliore nel contesto del viaggio**

Quindi deve considerare insieme:
- autonomia residua;
- distanza dalla route;
- deviazione;
- tempo aggiuntivo;
- prezzo carburante;
- quantità plausibile di carburante da acquistare;
- costo totale del viaggio;
- margine di sicurezza.

Questo è uno dei punti più importanti di tutto il progetto.

---

## Ipotesi di logica MVP considerate coerenti

Per il MVP va benissimo una logica spiegabile.

Esempio semplice:
1. calcolo percorso;
2. stima consumo sul percorso;
3. verifico se il serbatoio attuale basta;
4. se non basta, cerco distributori compatibili entro una deviazione massima;
5. scarto quelli che richiedono deviazione eccessiva;
6. stimo costo totale considerando deviazione e prezzo;
7. propongo la sosta con miglior equilibrio tra costo e impatto;
8. mostro anche 1 o 2 alternative.

Questa logica è abbastanza semplice da spiegare e abbastanza forte da creare valore.

---

## Cosa un agente non deve fare

Per ridurre perdite di tempo e scope creep.

Un agente non deve:
- reinventare da zero l’intero prodotto;
- proporre subito una versione Europa completa;
- bloccare tutto perché i prezzi carburante non sono real-time al minuto;
- introdurre troppe modalità veicolo già nel MVP;
- aprire subito il supporto camion, flotte, EV avanzato, car sharing, B2B, telemetria;
- costruire un sistema troppo astratto e lento da sviluppare;
- complicare la UX con 20 impostazioni prima della prima rotta.

---

## Cose ancora aperte ma non bloccanti

Questi punti sono aperti, ma non devono fermare la progettazione del MVP.

- autenticazione utente subito o profilo locale iniziale;
- numero esatto di preset veicolo;
- regole precise per la deviazione massima di default;
- come presentare il confronto tra 1 sosta suggerita e alternative;
- modello di monetizzazione;
- momento giusto per introdurre cronologia viaggi;
- momento giusto per supportare iOS;
- momento giusto per introdurre feed carburante premium.

---

## Migliorie consigliate emerse implicitamente

Queste non sono obblighi assoluti, ma sono consigli forti.

### 1. Costruire subito un layer `FuelPriceProvider`

Anche se all’inizio userà solo MIMIT, deve essere un modulo sostituibile.

### 2. Costruire subito un layer `RouteProvider`

Anche se all’inizio userà TomTom, meglio evitare hard-coupling ovunque.

### 3. Salvare il motivo della scelta sosta

Quando il sistema suggerisce una sosta, sarebbe utile salvare o poter ricostruire il “reasoning” sintetico:
- prezzo migliore utile;
- deviazione contenuta;
- autonomia compatibile;
- risparmio stimato.

Questo aiuta UX, debug e fiducia.

### 4. Tenere l’algoritmo semplice nella v1

Meglio una logica leggibile e verificabile che una scoring function opaca e troppo sofisticata.

### 5. Android-first davvero

Non solo come slogan.
Significa:
- design tecnico che non aspetta iOS;
- scelte di libreria coerenti con Android;
- roadmap che prevede iOS solo più avanti.

---

## Struttura dei tre file e ruolo di ciascuno

### File 1 — Claude-friendly handoff
Ruolo:
- briefing principale per sviluppo;
- panoramica completa di prodotto, UX, architettura e roadmap.

### File 2 — DOCX handoff
Ruolo:
- versione presentabile e strutturata;
- utile per consultazione, condivisione o stampa.

### File 3 — questo MASTER_CONTEXT
Ruolo:
- trasferire il contesto conversazionale;
- fissare priorità, intenzioni e confini;
- evitare che ogni nuovo agente reinterpreti tutto da zero.

---

## Istruzioni operative da dare agli agenti

Puoi allegare questo testo insieme ai file:

```text
Tratta questi tre file come il briefing ufficiale del progetto.

Ordine di priorità:
1. MASTER_CONTEXT
2. file Claude-friendly
3. DOCX handoff

Non reinventare il progetto da zero.
Non allargare lo scope senza motivo.
Se trovi un problema tecnico serio, spiegalo chiaramente e proponi una variante concreta.
Se trovi dettagli mancanti, fai assunzioni ragionevoli e dichiarale.
Blocca e chiedi solo quando una scelta influenza in modo importante architettura, costi o UX principale.
```

---

## Prompt breve consigliato per nuovi agenti

```text
Ti allego 3 file che costituiscono il briefing ufficiale del progetto.

Leggili in questo ordine:
1. MASTER_CONTEXT
2. navigatore_italy_first_claude_friendly.md
3. navigatore_italy_first_handoff.docx

Voglio che tu parta da questi file come se rappresentassero già una conversazione di allineamento fatta in precedenza.

Obiettivo:
costruire un MVP Android-first, Italy-first, basato su TomTom, per un navigatore con costo viaggio, profilo veicolo e suggerimento intelligente della sosta carburante.

Non reinventare il progetto da zero.
Non riportarmi una lista generica di idee.
Restituiscimi output concreti e coerenti con i vincoli già decisi.
Se trovi contraddizioni o lacune importanti, evidenziale in modo sintetico e proponi una soluzione pragmatica.
```

---

## Livello di certezza delle informazioni contenute qui

Molto alto su:
- obiettivo del prodotto;
- scope Italy-first;
- scelta Android-first;
- preferenza TomTom;
- uso iniziale di dati MIMIT;
- filosofia del MVP;
- valore centrale del motore carburante.

Medio su:
- dettaglio finale UX di ogni schermata;
- struttura dati definitiva del veicolo;
- monetizzazione;
- roadmap esatta dopo MVP.

Questo significa:
un agente può tranquillamente iniziare progettazione e sviluppo del MVP senza dover rifare discovery da zero.

---

## Regola finale di buon senso

Se un agente legge questi file e propone qualcosa che:
- ignora il profilo veicolo;
- riduce il tema carburante a un semplice elenco di distributori;
- tratta il progetto come una mappa generica;
- reintroduce subito scope europeo o multipiattaforma completo;
- promette prezzi carburante real-time garantiti senza base dati adeguata;

allora quell’agente non ha capito il progetto.

---

## Checklist finale per capire se un agente ha davvero capito

Un agente ha capito il progetto se nelle sue proposte compare chiaramente:

- TomTom come base iniziale;
- Italy-first;
- Android-first;
- MIMIT come fonte prezzi MVP;
- profilo veicolo;
- costo viaggio personalizzato;
- algoritmo di sosta carburante lungo il percorso;
- spiegabilità della scelta;
- modularità dei provider;
- roadmap pragmatica.

Se questi elementi non compaiono, va riallineato.

---

## Chiusura

Questo file serve a evitare che il progetto perda memoria quando viene passato tra agenti diversi.

La sintesi più importante da ricordare è questa:

**non stiamo costruendo una nuova mappa.**
**stiamo costruendo un navigatore che ragiona sul costo reale del viaggio e sulla sosta carburante migliore per quel viaggio, partendo dall’Italia e da un MVP Android-first costruito su TomTom.**
