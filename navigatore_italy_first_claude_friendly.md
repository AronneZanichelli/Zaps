# Navigator Italy-First - Handoff Prodotto, UX e Architettura per Claude

Documento Claude-friendly per sviluppo MVP di un navigatore con traffico, costo viaggio e suggerimento rifornimento ottimale.

Versione: 1.0  
Data: 19 marzo 2026  
Target: Claude / team di sviluppo  
Ambito: MVP consumer, Italy-first, Android-first

---


## Come leggere questo file

Questo documento è pensato per essere inoltrato direttamente a Claude come briefing di progetto.

Obiettivo del documento:
- trasferire a Claude il contesto della conversazione già avuta;
- fissare le decisioni già prese;
- definire un MVP chiaro, realistico e sviluppabile;
- evitare che Claude reinventi il progetto da zero;
- fornire una base tecnica, UX e logica abbastanza dettagliata da poter iniziare l’implementazione.

Questo file è volutamente scritto in modo:
- semplice;
- molto strutturato;
- leggibile a blocchi;
- con poche ambiguità;
- facile da spezzare in task di sviluppo.

---

# 1. Contesto conversazione già avuta (da considerare come decision log)

Questa sezione va letta come se fosse l’inoltro della nostra conversazione precedente.

## 1.1 Idea iniziale

L’idea di prodotto è creare un navigatore che unisca:
- l’esperienza di navigazione di Google Maps;
- traffico e tempi di percorrenza dinamici;
- costo del viaggio stile ViaMichelin;
- scelta del proprio veicolo prima di partire;
- stima di carburante e pedaggi;
- suggerimento di dove fermarsi a fare rifornimento al prezzo migliore lungo il percorso.

Il concetto chiave non è “una mappa qualsiasi”, ma un navigatore che sappia rispondere a domande come:
- quanto mi costa davvero questo viaggio;
- con il mio veicolo quanto spenderò;
- devo fermarmi a fare carburante oppure no;
- se devo fermarmi, dove mi conviene farlo;
- quanto mi conviene deviare per risparmiare;
- qual è il costo totale reale, non solo il tempo di arrivo.

## 1.2 Decisione strategica: Italy-first

È stato deciso di partire con una versione Italy-first.

Questo significa:
- focus iniziale sul mercato italiano;
- dati carburante prima di tutto italiani;
- UX e copy in italiano;
- ottimizzazione pensata per rete stradale italiana, autostrade italiane e distributori italiani;
- niente ambizione europea immediata nell’MVP.

## 1.3 Decisione strategica: non sviluppare mappe/routing da zero

È stato chiarito che non conviene sviluppare tutto da zero.

La parte differenziante del prodotto non è:
- ricostruire una mappa del mondo;
- costruire un motore traffico da zero;
- ricreare un navigatore turn-by-turn da zero.

La parte differenziante è invece:
- il modello veicolo;
- il costo viaggio personalizzato;
- la logica carburante;
- l’algoritmo che decide se conviene fermarsi e dove;
- la UX che rende tutto semplice e affidabile.

## 1.4 Decisione tecnologia: preferenza TomTom rispetto a Google

Dopo il confronto iniziale è stato deciso di impostare l’MVP su TomTom, non su Google.

Motivi principali della scelta:
- TomTom ha un piano free giornaliero chiaro e utilizzabile anche commercialmente [S1];
- TomTom fornisce stack completo per mappe, routing, traffico, search e navigation su Android [S2][S5];
- TomTom ha una funzione ufficiale di Along Route Search, utile per cercare distributori lungo il percorso con deviazione massima [S4];
- con Google i costi salgono facilmente quando si usano feature avanzate come traffico, toll e routing più evoluto [S9];
- Google vieta l’uso dei contenuti di Routes/Navigation con mappe non-Google [S10], quindi lascia meno libertà architetturale.

Nota importante:
- TomTom iOS è disponibile solo su richiesta [S3].
- Per questo motivo, il progetto MVP viene impostato come Android-first.

## 1.5 Decisione dati carburante: partire con dati ufficiali MIMIT

Il dubbio principale emerso è questo:
- i dati open ufficiali del MIMIT sono pubblicati con fotografia delle informazioni in vigore alle ore 8 del giorno precedente [S7];
- quindi potrebbero non coincidere sempre con il prezzo esatto trovato dall’utente alla pompa nel momento dell’arrivo.

Conclusione condivisa:
- per l’MVP questi dati sono comunque sufficienti;
- il prodotto non deve promettere “prezzo garantito in tempo reale”;
- il prodotto deve mostrare in modo trasparente timestamp dell’ultimo aggiornamento;
- il prodotto deve parlare di ultimo prezzo ufficiale disponibile e non di prezzo certo all’arrivo.

## 1.6 Possibile fase futura: feed premium prezzi carburante

È emerso che TomTom dispone anche di una Fuel Prices API con refresh più frequente (ogni 10 minuti), ma:
- è indicata come Automotive only;
- non è disponibile nel piano free/PAYG;
- richiede accesso dedicato / contatto commerciale [S6].

Questa API quindi non va usata nel MVP.
Va però prevista nell’architettura la possibilità di sostituire il provider prezzi in futuro.

## 1.7 Visione prodotto

Il prodotto non deve essere pensato come:
- “app che mostra distributori vicini”.

Il prodotto deve essere pensato come:
- “navigatore intelligente che ottimizza il costo reale del viaggio in base al veicolo e al carburante”.

Questa è la vera differenziazione.

---

# 2. Sintesi esecutiva del progetto

## 2.1 One-liner

Creare un navigatore mobile Italy-first che usa TomTom per mappa, routing, traffico e navigazione, e aggiunge un livello proprietario di:
- profilo veicolo;
- costo viaggio;
- costo pedaggi;
- stima carburante;
- suggerimento ottimizzato del distributore lungo il percorso.

## 2.2 Valore percepito dall’utente

L’utente deve percepire che l’app sa rispondere a quattro domande meglio dei navigatori classici:
1. Quanto mi costa il viaggio?
2. Con il mio veicolo, quanto carburante consumerò?
3. Devo fermarmi oppure arrivo senza rifornire?
4. Se devo fermarmi, dove mi conviene fermarmi davvero?

## 2.3 Principio guida di prodotto

Non ottimizzare solo:
- ETA;
- prezzo al litro.

Ottimizzare invece il costo totale di viaggio:
- carburante consumato;
- eventuale deviazione;
- tempo perso;
- eventuali pedaggi;
- autonomia residua e rischio.

---

# 3. Obiettivi prodotto

## 3.1 Obiettivo MVP

Rilasciare una prima versione Android in Italia che consenta a un utente di:
- creare e salvare uno o più veicoli;
- pianificare un percorso;
- vedere durata, distanza, traffico, costo pedaggi stimato e costo carburante stimato;
- ricevere un suggerimento se conviene fermarsi a fare rifornimento;
- vedere il distributore consigliato lungo il percorso;
- avviare la navigazione turn-by-turn;
- ricevere durante la navigazione avvisi semplici sul punto carburante consigliato.

## 3.2 Obiettivi secondari MVP

- rendere affidabile la percezione del calcolo costi;
- rendere trasparente la freschezza del dato carburante;
- evitare promesse che il sistema non può mantenere;
- progettare fin da subito una struttura scalabile a fonti dati premium.

## 3.3 Non-obiettivi MVP

Non fanno parte del MVP:
- copertura Europa completa;
- iOS nativo subito;
- EV routing avanzato;
- community price reporting aperta e non moderata;
- pagamenti in-app;
- comparazione assicurazioni/manutenzione;
- route planning multi-stop complesso da logistica;
- funzioni social.

---

# 4. Target utenti

## 4.1 Utente principale

Conducente privato italiano che usa l’auto o la moto per:
- tragitti lunghi;
- viaggi di lavoro;
- weekend fuori porta;
- vacanze;
- commuting extraurbano;
- trasferte in cui il costo del carburante conta davvero.

## 4.2 Profili utenti principali

### Profilo A - guidatore attento ai costi
Vuole sapere quanto spenderà prima di partire.

### Profilo B - guidatore che macina chilometri
Vuole ottimizzare soste e rifornimenti.

### Profilo C - motociclista
Vuole una navigazione pulita e un algoritmo che tenga conto di autonomia spesso inferiore.

### Profilo D - utente pragmatico
Non vuole smanettare. Vuole che l’app dica in modo chiaro:
- parti;
- non fermarti;
- fermati qui;
- risparmi circa X.

---

# 5. Posizionamento del prodotto

## 5.1 Cosa non deve sembrare

Non deve sembrare:
- un clone low-cost di Google Maps;
- una semplice app di prezzi carburante;
- un elenco di distributori;
- un comparatore statico.

## 5.2 Cosa deve sembrare

Deve sembrare:
- un navigatore intelligente focalizzato sul costo reale del viaggio;
- un assistente di guida che conosce il veicolo dell’utente;
- un prodotto affidabile, pratico e immediato.

## 5.3 Messaggio di valore

“Non solo ti porto a destinazione. Ti dico anche quanto spenderai e dove ti conviene fermarti.”

---

# 6. Requisiti funzionali MVP

## 6.1 Gestione veicoli

L’utente deve poter:
- creare un veicolo;
- salvare più veicoli;
- impostare un veicolo predefinito;
- duplicare un veicolo e modificarlo;
- modificare i parametri nel tempo.

### Campi minimi veicolo
- nome veicolo;
- categoria veicolo: auto o moto;
- carburante: benzina, diesel, GPL, metano;
- consumo medio reale;
- unità consumo;
- capacità serbatoio;
- carburante attualmente presente nel serbatoio;
- riserva di sicurezza desiderata;
- preferenze pedaggio.

### Campi consigliati ma opzionali
- marca;
- modello;
- anno;
- motore;
- note.

Nota: per l’MVP il consumo deve essere impostabile manualmente. Non bisogna dipendere da un database veicoli perfetto al day one.

## 6.2 Pianificazione percorso

L’utente deve poter:
- inserire partenza e destinazione;
- usare posizione attuale come origine;
- scegliere veicolo prima del calcolo;
- vedere percorsi alternativi se disponibili;
- vedere per ogni percorso:
  - ETA;
  - distanza;
  - traffico/ritardi;
  - pedaggi stimati;
  - consumo stimato;
  - costo stimato totale;
  - eventuale necessità di rifornimento.

## 6.3 Suggerimento carburante

Per ogni percorso l’app deve poter stabilire se:
- non serve fermarsi;
- conviene fermarsi;
- è obbligatorio fermarsi per autonomia.

Se serve/conviene fermarsi, l’app deve proporre almeno:
- distributore consigliato;
- carburante disponibile;
- prezzo considerato;
- deviazione stimata;
- tempo extra;
- costo totale viaggio con quella sosta;
- risparmio stimato rispetto a non ottimizzare.

## 6.4 Navigazione

L’app deve poter:
- avviare la navigazione turn-by-turn;
- mostrare stato del viaggio;
- mostrare autonomia residua stimata;
- mostrare distanza al rifornimento consigliato;
- consentire “ignora questa sosta”;
- ricalcolare se l’utente salta la sosta suggerita.

## 6.5 Trasparenza dato carburante

L’app deve mostrare chiaramente:
- fonte del prezzo;
- data/ora del dato usato;
- eventuale badge tipo:
  - Ufficiale;
  - Ultimo dato disponibile;
  - Aggiornamento del giorno precedente.

Non usare wording che faccia pensare a garanzia assoluta del prezzo all’arrivo.

---

# 7. Requisiti di UX

## 7.1 Principi UX

- pochissimi input obbligatori;
- chiarezza prima della perfezione;
- costo totale sempre ben visibile;
- niente gergo tecnico inutile;
- l’algoritmo deve essere “spiegabile” in una frase comprensibile.

## 7.2 Regola UX fondamentale

L’utente non deve chiedersi:
- perché mi stai facendo fermare qui?

L’app deve sempre rispondere visivamente con una motivazione semplice tipo:
- “Con il tuo livello carburante attuale arrivi in riserva. Questa sosta riduce il rischio e mantiene il costo del viaggio competitivo.”
oppure
- “Questa sosta aggiunge 4 minuti ma ti fa risparmiare circa 6,20 euro rispetto ai distributori lungo il tratto successivo.”

## 7.3 Struttura schermate MVP

### Schermata 1 - Onboarding iniziale
Obiettivo:
- spiegare il valore dell’app in 3 schermate;
- far creare subito il primo veicolo.

### Schermata 2 - Creazione veicolo
Campi minimi semplificati:
- nome veicolo;
- tipo veicolo;
- carburante;
- consumo medio;
- serbatoio;
- carburante attuale.

### Schermata 3 - Home / Cerca destinazione
Elementi:
- barra ricerca destinazione;
- pulsante usa posizione attuale;
- selettore veicolo attivo;
- pulsante “pianifica viaggio”.

### Schermata 4 - Risultati percorso
Cards alternative con:
- nome percorso opzionale (più veloce / più economico / meno soste);
- durata;
- distanza;
- pedaggi;
- costo carburante;
- costo totale;
- badge “serve rifornimento” o “arrivi senza fermarti”.

### Schermata 5 - Dettaglio percorso
Elementi:
- mappa;
- sommario viaggio;
- punto rifornimento consigliato;
- motivazione della scelta;
- timestamp del prezzo carburante;
- CTA “Avvia navigazione”.

### Schermata 6 - Navigazione attiva
Elementi:
- mappa turn-by-turn;
- barra superiore con ETA;
- barra inferiore con:
  - km residui;
  - costo totale stimato aggiornato;
  - autonomia residua;
  - distanza alla sosta consigliata;
- pulsante “salta sosta”.

### Schermata 7 - Fine viaggio
Elementi:
- riepilogo percorso;
- costo stimato finale;
- pedaggi stimati;
- eventuale feedback:
  - il prezzo al distributore era corretto?
  - il suggerimento di sosta è stato utile?

---

# 8. Logica di business centrale

## 8.1 Formula base costo viaggio

Costo totale stimato =
- costo carburante stimato sul percorso
+ costo pedaggi stimato
+ eventuale costo extra deviazione per rifornimento

Per l’MVP, il costo deviazione può essere incorporato indirettamente nel carburante consumato sulla deviazione e nel tempo extra mostrato all’utente, senza monetizzare il tempo perso.

## 8.2 Formula consumo percorso

Per benzina/diesel/GPL:
- litri consumati = (km percorso / 100) * consumo_medio_l_100km

Per metano:
- normalizzare internamente in kg/100km oppure convertire da km/kg.
- raccomandazione: usare internamente kg/100km per coerenza di calcolo.

## 8.3 Autonomia teorica

autonomia = carburante_disponibile / consumo_per_km

## 8.4 Riserva di sicurezza

Ogni veicolo deve avere un parametro di riserva minima.

Esempio:
- auto: default 10% del serbatoio;
- moto: default 15%;
- modificabile dall’utente.

La logica non deve usare il serbatoio a zero come soglia. Deve sempre esistere una fuel safety margin.

## 8.5 Tipi di decisione carburante

L’algoritmo deve produrre uno di questi stati:
- Stato A - Nessuna sosta necessaria
- Stato B - Sosta consigliata ma non obbligatoria
- Stato C - Sosta obbligatoria

---

# 9. Algoritmo di suggerimento rifornimento

Questa è la parte più importante del prodotto.

## 9.1 Obiettivo algoritmo

Non trovare “il distributore più economico in assoluto”.

Trovare invece il distributore che massimizza il valore reale della sosta considerando:
- prezzo carburante;
- distanza lungo il percorso;
- deviazione;
- tempo extra;
- autonomia residua;
- necessità reale di fermarsi;
- rischio di arrivare in riserva.

## 9.2 Input algoritmo

Input minimi:
- percorso calcolato;
- distanza totale;
- ETA;
- route geometry/polyline;
- traffico/ritardi;
- eventuali pedaggi;
- veicolo attivo;
- carburante disponibile al momento della partenza;
- prezzo carburante per i distributori candidati;
- risultati along-route search per distributori lungo percorso [S4].

## 9.3 Selezione candidati

Passo 1:
- ottenere il percorso da TomTom routing.

Passo 2:
- interrogare la ricerca along-route per categoria distributori/carburanti con una deviazione massima controllata [S4].

Passo 3:
- filtrare solo stazioni compatibili con il carburante del veicolo.

Passo 4:
- escludere stazioni raggiungibili solo violando la riserva minima.

Passo 5:
- escludere stazioni senza prezzo disponibile o con prezzo troppo vecchio oltre una soglia definita.

## 9.4 Finestra utile di sosta

Non tutte le stazioni raggiungibili sono utili.

Definire una finestra di analisi:
- inizio finestra = punto in cui, continuando senza sosta, il carburante residuo scende sotto una soglia di comfort;
- fine finestra = ultimo punto in cui è ancora possibile fermarsi senza violare la riserva minima.

## 9.5 Ranking candidati

Ogni stazione candidata riceve uno score.

Esempio di score semplificato:
- peso prezzo
- peso deviazione
- peso tempo extra
- peso sicurezza
- peso freschezza dato

## 9.6 Logica pratica da usare nel MVP

Per il MVP usare regole semplici e robuste, non machine learning.

### Regola 1
Se il veicolo arriva a destinazione mantenendo una riserva confortevole, non proporre sosta obbligatoria.

### Regola 2
Se esiste una sosta che aggiunge deviazione minima e porta un risparmio materiale, mostrarla come “facoltativa conveniente”.

### Regola 3
Se il veicolo non arriva mantenendo la riserva minima, proporre la migliore sosta obbligatoria.

### Regola 4
A parità quasi di prezzo, preferire:
- meno deviazione;
- meno tempo extra;
- maggiore affidabilità dato;
- maggiore margine residuo.

## 9.7 Quantità rifornimento nel MVP

Per semplificare, nel MVP il sistema può assumere due strategie configurabili:
- rifornimento minimo sufficiente a completare il viaggio con riserva;
- pieno virtuale per confronto costi.

Raccomandazione MVP:
- usare rifornimento minimo sufficiente per la logica di necessità;
- mostrare anche un valore secondario “con pieno stimato”.

## 9.8 Pseudocodice consigliato

```text
1. Calcola percorso base.
2. Calcola consumo stimato del percorso.
3. Calcola autonomia residua reale del veicolo.
4. Se autonomia sufficiente con margine >= riserva + comfort:
       stato = NO_STOP_REQUIRED
       cerca opzionalmente soste vantaggiose
   Altrimenti:
       stato = STOP_REQUIRED
5. Esegui along-route search per distributori entro deviazione massima.
6. Filtra per carburante compatibile.
7. Filtra per raggiungibilità con riserva minima.
8. Recupera/aggancia prezzo ufficiale disponibile per ogni stazione.
9. Calcola score per ogni candidata.
10. Ordina candidate.
11. Restituisci:
       - best mandatory stop oppure
       - best optional stop oppure
       - nessuna sosta.
12. Spiega la ragione della scelta in una frase leggibile.
```

---

# 10. Dati carburante: strategia MVP

## 10.1 Fonte primaria MVP

Usare come fonte primaria il dataset open del MIMIT sui prezzi praticati e anagrafica impianti [S7].

## 10.2 Limite da accettare

Il dataset pubblica quotidianamente le informazioni in vigore alle 8 del giorno precedente [S7].

Quindi:
- non è un vero live feed di tipo sub-hour;
- può differire dal prezzo reale trovato alla pompa;
- va comunicato all’utente con onestà.

## 10.3 Strategia UX raccomandata per gestire il limite

Mostrare sempre:
- prezzo usato;
- timestamp;
- etichetta fonte;
- disclaimer breve e non difensivo.

Esempi di copy:
- “Ultimo prezzo ufficiale disponibile”
- “Dato ufficiale aggiornato il …”
- “Il prezzo alla pompa può variare rispetto all’ultimo aggiornamento disponibile”

## 10.4 Fonte futura premium

Prevedere interfaccia astratta FuelPriceProvider.

Implementazioni previste:
- MimitDailyProvider (MVP)
- TomTomFuelPricesProvider (futuro, se accessibile) [S6]
- eventuale CommercialPartnerProvider (futuro)
- eventuale CrowdVerifiedOverlayProvider (futuro, con moderazione)

## 10.5 Dato Osservaprezzi

Il sito Osservaprezzi è presentato come consultazione in tempo reale [S8].

Per il MVP, però, va evitato qualsiasi approccio fragile/non contrattualizzato. Se non esiste una API ufficiale riusabile chiaramente documentata per il progetto, non basare l’architettura su scraping fragile.

---

# 11. Perché TomTom è la scelta giusta per il MVP

## 11.1 Vantaggi pratici

TomTom offre già:
- map display;
- routing;
- traffico [S5];
- navigation SDK Android [S2];
- search;
- along-route search [S4];
- gestione route sections con informazioni utili su traffico, traghetti, motorway, toll sections [S11].

## 11.2 Vantaggio economico iniziale

TomTom pricing attuale:
- 50.000 tile request/giorno;
- 2.500 non-tile request/giorno;
- uso commerciale consentito [S1].

## 11.3 Perché non Google per il MVP

Google resta fortissimo, ma per questo progetto non è la scelta migliore iniziale perché:
- il billing cambia in base alle feature richieste [S9];
- feature come traffico avanzato/toll fanno salire di SKU [S9];
- c’è meno libertà di combinazione con mappe non-Google [S10].

---

# 12. Assunzioni architetturali consigliate

## 12.1 Piattaforma

Raccomandazione forte per MVP:
- Android-first.

Motivo:
- SDK Android TomTom disponibile e documentato pubblicamente [S2];
- iOS disponibile solo su richiesta [S3];
- riduce complessità iniziale.

## 12.2 Frontend mobile

Scelta consigliata MVP:
- Kotlin nativo + Jetpack Compose.

## 12.3 Backend

Scelta consigliata MVP:
- Python FastAPI.

## 12.4 Database

Scelta consigliata MVP:
- PostgreSQL
- opzionale PostGIS se serve più avanti per query geospaziali avanzate.

## 12.5 Cache

Facoltativa MVP:
- Redis per caching di route plan, stazioni candidate, prezzi normalizzati.

---

# 13. Architettura applicativa proposta

## 13.1 Componenti principali

### Mobile App Android
Responsabilità:
- autenticazione leggera o guest+sync futuro;
- gestione veicoli;
- ricerca destinazione;
- visualizzazione percorsi;
- avvio navigazione;
- interazione utente durante viaggio.

### Backend API
Responsabilità:
- normalizzazione richieste client;
- chiamate a TomTom routing/search;
- calcolo costo viaggio;
- algoritmo suggerimento rifornimento;
- gestione dati veicoli e preferenze;
- esposizione endpoint interni.

### Fuel Data Ingestion Service
Responsabilità:
- scaricare dataset MIMIT;
- validarlo;
- normalizzarlo;
- importarlo in DB;
- mantenere storico minimo.

### Pricing/Optimization Engine
Responsabilità:
- calcolo consumo;
- calcolo autonomia;
- calcolo costo viaggio;
- ranking distributori;
- motivazione leggibile suggerimento.

### Database
Responsabilità:
- utenti;
- veicoli;
- preferenze;
- distributori normalizzati;
- prezzi carburante;
- storico calcoli opzionale.

---

# 14. Flusso tecnico end-to-end

## 14.1 Pianificazione viaggio

1. L’utente apre app.
2. Seleziona veicolo.
3. Inserisce destinazione.
4. App invia richiesta a backend.
5. Backend chiama TomTom routing.
6. Backend riceve route alternatives, traffic e metadati percorso.
7. Backend calcola consumo stimato del veicolo per ogni route.
8. Backend decide se cercare soste carburante.
9. Se serve, backend chiama along-route search per distributori [S4].
10. Backend associa i distributori ai prezzi MIMIT più recenti disponibili [S7].
11. Backend ranka le soste.
12. Backend restituisce al client:
    - routes
    - costo viaggio
    - stato rifornimento
    - sosta consigliata
    - spiegazione leggibile.
13. App mostra risultati.
14. Utente avvia navigazione.

## 14.2 Navigazione attiva

1. App avvia navigation SDK.
2. Durante il viaggio monitora:
   - progresso route;
   - ETA;
   - distanza residua;
   - posizione rispetto alla sosta consigliata.
3. Se utente devia o salta la sosta:
   - il backend ricalcola;
   - aggiorna consiglio.

---

# 15. Modello dati consigliato

## 15.1 Tabella users
Campi minimi:
- id
- created_at
- locale
- home_country
- default_vehicle_id

## 15.2 Tabella vehicles
Campi minimi:
- id
- user_id
- name
- vehicle_category (car, motorcycle)
- fuel_type (petrol, diesel, lpg, cng)
- make
- model
- year
- consumption_value
- consumption_unit
- tank_capacity
- reserve_threshold_value
- reserve_threshold_unit
- default_refuel_strategy
- is_default
- created_at
- updated_at

## 15.3 Tabella stations
Campi minimi:
- id interno
- external_station_id
- provider_source
- name
- brand
- latitude
- longitude
- address
- city
- province
- is_highway
- supports_petrol
- supports_diesel
- supports_lpg
- supports_cng
- services_json

## 15.4 Tabella station_prices
Campi minimi:
- id
- station_id
- fuel_type
- price_value
- service_mode (self, served)
- currency
- source_provider
- source_timestamp
- ingested_at
- raw_payload_hash

## 15.5 Tabella route_plans
Campi minimi:
- id
- user_id
- vehicle_id
- origin_text
- destination_text
- route_provider
- selected_route_id
- distance_meters
- duration_seconds
- delay_seconds
- toll_estimated_value
- fuel_cost_estimated_value
- total_estimated_value
- stop_required_flag
- recommended_station_id
- created_at

## 15.6 Tabella route_stop_candidates
Campi minimi:
- id
- route_plan_id
- station_id
- detour_seconds
- detour_meters
- fuel_price_value
- price_timestamp
- reachable_flag
- mandatory_flag
- score_value
- rank_position
- explanation_text

---

# 16. API interne consigliate

## 16.1 Veicoli
- POST /vehicles
- GET /vehicles
- GET /vehicles/{id}
- PATCH /vehicles/{id}
- DELETE /vehicles/{id}

## 16.2 Pianificazione percorso
- POST /trip/plan
  Input: origin, destination, vehicle_id, current_fuel_level, route_preferences
  Output: routes, cost summary, fuel decision, recommended stop

## 16.3 Navigazione
- POST /trip/recalculate
- POST /trip/feedback

## 16.4 Dati carburante
- GET /fuel/stations/along-route-preview
- GET /fuel/station/{id}

---

# 17. Schema risposta consigliato per /trip/plan

```json
{
  "tripSummary": {
    "distanceMeters": 312400,
    "durationSeconds": 12840,
    "trafficDelaySeconds": 960,
    "tollEstimated": 22.40,
    "fuelEstimated": 28.10,
    "totalEstimated": 50.50,
    "currency": "EUR"
  },
  "fuelDecision": {
    "status": "STOP_REQUIRED",
    "reasonCode": "INSUFFICIENT_RANGE_WITH_RESERVE",
    "humanExplanation": "Con il livello carburante attuale non arrivi mantenendo la riserva di sicurezza. Questa sosta aggiunge 3 minuti di deviazione."
  },
  "recommendedStop": {
    "stationId": "st_123",
    "name": "Nome Stazione",
    "brand": "Brand",
    "fuelType": "diesel",
    "price": 1.689,
    "priceTimestamp": "2026-03-18T08:00:00+01:00",
    "detourSeconds": 180,
    "detourMeters": 1600,
    "arrivalFuelPercent": 12,
    "postRefuelFuelPercent": 47,
    "estimatedSavingsVsBaseline": 4.30
  },
  "alternatives": []
}
```

---

# 18. Regole decisionali dettagliate

## 18.1 Quando NON proporre una sosta

Non proporre una sosta se tutte queste condizioni sono vere:
- arrivo a destinazione con riserva > soglia minima;
- nessuna sosta vicina produce un risparmio materiale;
- la deviazione delle stazioni economiche è poco sensata.

## 18.2 Quando proporre una sosta opzionale

Proporre sosta opzionale se:
- l’utente arriva comunque;
- esiste una stazione lungo il percorso o con micro-deviazione;
- il risparmio supera una soglia minima configurabile.

Soglia iniziale consigliata MVP:
- risparmio almeno 2,50 euro oppure almeno 3% del costo carburante di riferimento.

## 18.3 Quando proporre una sosta obbligatoria

Proporre sosta obbligatoria se:
- senza fermarsi l’utente non arriva mantenendo la riserva minima;
- oppure arriverebbe con margine considerato pericoloso.

## 18.4 Parametri iniziali consigliati

- deviazione massima ricerca stazioni: 5-7 minuti
- soglia comfort oltre riserva minima: +5% serbatoio
- risparmio minimo per sosta facoltativa: 2,50 euro
- massimo numero candidate da rankare visivamente: 3

---

# 19. Gestione pedaggi

## 19.1 Cosa fare nel MVP

Usare i dati restituiti dal provider routing ove disponibili.

In UX mostrare:
- pedaggi stimati;
- nota che la stima può variare.

## 19.2 Perché serve comunque

Il prodotto promette costo viaggio, quindi non può ignorare i pedaggi.

---

# 20. Edge case da prevedere

- Nessuna stazione compatibile trovata
- Prezzi mancanti
- Prezzo troppo vecchio
- Traffico cambia durante il viaggio
- L’utente non fa rifornimento anche se consigliato
- Dati veicolo errati
- Metano e GPL con rete più limitata

---

# 21. Sicurezza, privacy e compliance

- raccogliere solo ciò che serve;
- separare cronologia da profilo quando possibile;
- retention breve per location logs;
- chiara privacy policy;
- non salvare coordinate raw inutilmente per sempre.

---

# 22. Metriche prodotto da tracciare

Metriche core:
- numero route plan/giorno;
- % route con veicolo selezionato;
- % route con sosta consigliata;
- % sosta ignorata;
- % utenti che modificano consumo veicolo;
- tempo medio per ottenere il primo piano viaggio;
- crash rate durante navigazione.

Metriche di valore:
- % utenti che dichiarano utile il consiglio rifornimento;
- delta medio stimato di risparmio;
- % utenti che tornano a usare lo stesso veicolo;
- retention 7/30 giorni.

Metriche qualità dati:
- % stazioni without price;
- % mismatch price feedback utente vs prezzo stimato;
- anzianità media del dato prezzo usato.

---

# 23. Migliorie e consigli pratici

- Non cercare il database veicoli perfetto al day one.
- Mostrare sempre la motivazione del suggerimento.
- Android-first è una scelta sana.
- Introdurre una modalità di compilazione ultra-rapida del veicolo.
- Preparare fin da subito un layer provider-agnostic per i prezzi.
- Non monetizzare il tempo perso nel MVP.
- Tenere separate tre nozioni: prezzo più basso, sosta più vicina, sosta migliore.

---

# 24. Piano di sviluppo consigliato

## Fase 0 - Prototipo tecnico
- verificare end-to-end stack TomTom + backend + dataset MIMIT.

## Fase 1 - MVP Alpha
- veicolo singolo;
- pianificazione viaggio;
- costo viaggio;
- sosta obbligatoria/opzionale;
- navigazione base.

## Fase 2 - MVP Beta
- multi-veicolo;
- route alternatives migliori;
- UX pulita;
- feedback utente;
- analytics.

## Fase 3 - Post-MVP
- iOS;
- feed prezzi premium;
- storico prezzi;
- suggerimento quantità rifornimento più sofisticato;
- supporto Europa.

---

# 25. Backlog suggerito (ordinato per priorità)

## Must have
- onboarding base
- CRUD veicoli
- route planning
- costo carburante
- costo pedaggi
- fuel decision engine
- along-route search distributori
- recommended stop
- navigation start
- recalc after skip stop
- timestamp prezzo

## Should have
- route alternatives etichettate
- motivazione leggibile suggerimento
- fine viaggio + feedback
- caching
- analytics base

## Could have
- storico prezzi
- full trip history
- suggerimenti personalizzati per stile guida
- prezzo servito vs self comparabile
- filtri brand distributore

---

# 26. Istruzioni esplicite per Claude

Questa sezione è scritta direttamente per Claude.

## 26.1 Cosa devi fare

Prendi questo documento come specifica iniziale di prodotto + architettura.

Il tuo compito non è riscrivere l’idea da zero, ma:
- trasformarla in struttura implementabile;
- definire i moduli software;
- proporre il codice base del progetto;
- costruire un MVP reale e progressivo.

## 26.2 Vincoli già decisi

Non rimettere in discussione senza motivo questi punti:
- scope Italy-first;
- provider principale TomTom;
- Android-first;
- dati carburante MVP da MIMIT;
- niente scraping fragile del sito prezzi carburante;
- niente dipendenza iniziale da feed premium;
- focus sul costo reale del viaggio.

## 26.3 Cosa voglio da te come output iniziale

1. Una proposta di repository structure.
2. Una proposta di stack definitivo per app Android + backend.
3. Un piano sprint-by-sprint per costruire il MVP.
4. Uno schema API dettagliato.
5. Uno schema DB dettagliato.
6. I componenti principali del fuel decision engine.
7. Le prime implementazioni scaffold:
   - app shell Android
   - backend FastAPI
   - ingestion job MIMIT
   - route plan endpoint
   - stop recommendation service.

## 26.4 Come ragionare

Quando devi scegliere tra:
- soluzione elegante ma lenta;
- soluzione semplice ma robusta;

per il MVP scegli la seconda.

Quando devi scegliere tra:
- UX sofisticata ma ambigua;
- UX semplice ma chiarissima;

scegli la seconda.

Quando devi scegliere tra:
- massimo numero di feature;
- affidabilità del caso base;

scegli la seconda.

## 26.5 Cosa non fare

- non progettare un sistema enterprise inutile al day one;
- non introdurre microservizi non necessari;
- non bloccare il MVP in attesa di un catalogo veicoli perfetto;
- non promettere prezzo pompa garantito;
- non dipendere da provider non contrattualizzati.

---

# 27. Open questions da lasciare aperte ma non bloccanti

- Conviene supportare da subito anche il “served” oltre al “self” nei prezzi carburante?
- Per metano è meglio mostrare kg/100 km o km/kg in UI?
- Nel ranking conviene già introdurre una minima monetizzazione del tempo perso?
- Quanto aggressiva deve essere la proposta di sosta facoltativa?
- Conviene introdurre subito un account utente o partire con profilo locale?

Raccomandazione:
- prendi una decisione pragmatica per il MVP;
- documentala;
- vai avanti.

---

# 28. Raccomandazione finale sintetica

La via migliore è questa:
- Android-first;
- TomTom come provider mappe/routing/navigation/search;
- MIMIT come provider prezzi carburante MVP;
- backend FastAPI;
- veicoli con consumo manuale;
- algoritmo semplice e spiegabile;
- UX orientata al costo reale del viaggio.

Questo permette di costruire un MVP credibile, utile e differenziante senza cercare di rifare da zero un intero Google Maps.


---

# 29. Appendice fonti ufficiali

- **S1** — TomTom pricing: 50.000 tile request/giorno e 2.500 non-tile request/giorno nel piano free; uso commerciale consentito. Fonte: TomTom Developer Portal - Pricing. https://developer.tomtom.com/pricing
- **S2** — TomTom Maps and Navigation SDK for Android: supporta map visualization, location, search, routing e driver guidance. Fonte: TomTom Developer Portal - Maps and Navigation SDK for Android. https://developer.tomtom.com/navigation/android/introduction/introduction
- **S3** — TomTom Navigation SDK per iOS disponibile solo su richiesta. Fonte: TomTom Developer Portal - Navigation SDK for iOS introduction. https://developer.tomtom.com/navigation/ios/introduction/introduction
- **S4** — TomTom Along Route Search: ricerca POI lungo un percorso con limite di deviazione. Fonte: TomTom Search API - Along Route Search. https://developer.tomtom.com/search-api/documentation/search-service/along-route-search
- **S5** — TomTom Routing usa IQ Routes e TomTom Traffic. Fonte: TomTom Routing API introduction. https://developer.tomtom.com/routing-api/documentation/tomtom-maps/product-information/introduction
- **S6** — TomTom Fuel Prices API: dati aggiornati ogni 10 minuti, ma Automotive only e non disponibile nel piano free/PAYG. Fonte: TomTom Fuel Prices API introduction e Fuel Prices endpoint docs. https://developer.tomtom.com/fuel-prices-api/documentation/product-information/introduction ; https://developer.tomtom.com/fuel-prices-api/documentation/fuel-prices-api/fuel-price
- **S7** — MIMIT Open Data carburanti: pubblicazione quotidiana con informazioni in vigore alle ore 8 del giorno precedente. Fonte: MIMIT - Carburanti, prezzi praticati e anagrafica degli impianti. https://www.mimit.gov.it/it/open-data/elenco-dataset/carburanti-prezzi-praticati-e-anagrafica-degli-impianti
- **S8** — Osservaprezzi carburanti: consultazione in tempo reale dei prezzi comunicati dai gestori. Fonte: MIMIT - Osservaprezzi carburanti. https://www.mimit.gov.it/it/mercato-e-consumatori/prezzi/mercati-dei-carburanti/osservatorio-carburanti
- **S9** — Google Routes API: le richieste vengono fatturate in base alle feature usate; una richiesta con feature di categoria superiore viene fatturata al tier più alto richiesto. Fonte: Google Routes API Usage and Billing e SKU details. https://developers.google.com/maps/documentation/routes/usage-and-billing ; https://developers.google.com/maps/billing-and-pricing/sku-details
- **S10** — Google vieta l’uso dei contenuti Routes API e Navigation SDK insieme a mappe non-Google. Fonte: Google Maps Platform service specific terms. https://cloud.google.com/maps-platform/terms/maps-service-terms
- **S11** — TomTom route sections evidenziano caratteristiche del percorso, incluse sezioni con incidenti/traffico, traghetti, autostrade e sezioni a pedaggio. Fonte: TomTom Navigation SDK Android - Route sections. https://developer.tomtom.com/navigation/android/guides/routing/route-sections
- **S12** — TomTom Routing supporta vehicle types / engine types. Fonte: TomTom product routing page e riferimenti vehicle SDK. https://www.tomtom.com/products/routing-apis/ ; https://developer.tomtom.com/assets/downloads/tomtom-sdks/android/api-reference/2.1.2/vehicle/vehicle-common/com.tomtom.sdk.vehicle/-vehicle/-motorcycle/index.html

---

# 30. Mini prompt da usare con Claude

```text
Ti inoltro il briefing completo del progetto. Voglio che tu lo tratti come una specifica di prodotto e architettura già ragionata, non come un’idea grezza da reinventare.

Obiettivo: costruire un MVP Android-first, Italy-first, di un navigatore basato su TomTom con costo viaggio, profilo veicolo e suggerimento intelligente del rifornimento lungo il percorso.

Leggi il documento, estrai assunzioni, moduli, entità, API e piano di sviluppo. Poi restituiscimi:
1. architettura proposta,
2. struttura repository,
3. roadmap sprint,
4. schema DB,
5. schema API,
6. task di implementazione in ordine di priorità,
7. primi scaffold di codice.

Non rimettere in discussione i vincoli principali già decisi, a meno che ci sia un problema tecnico serio.
```