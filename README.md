# Refactoring

## Chapter 3

### Duplicated code (Codice duplicato)

Se vedi lo stesso codice / stessa struttura di codice in piu’ di un posto puoi essere certo che il tuo programma sara’ migliore se trovi un modo per unificarlo.
Il caso piu’ comune e’ quando nella stessa classe ci sono due metodi che fanno la stessa cosa. In questo caso e’ utile utilizzare “Extract Method”.
Se lo stesso codice e’ in due sotto classi occorre estrarre il metodo e dopo eseguire un azione di “pull up method” e “Pull Up Field” per i campi che vengono utilizzati nel metodo.
Utilizzare “Pull Up Constructor Body” se il codice duplicato e’ a livello del costruttore.
Se il codice e’ simile ma non esattamente identico utilizzare “Form Template Method”.
Se invece ci sono due algoritmi diversi che eseguono la stessa operazione utilizzare “Substitute Algorithm”.
Se il codice e’ in due classi scorrelate(diverse) si pu’ utilizzare “Extract Class”.
Se C’e’ un grande numero di condizioni sullo stesso codice utilizzare “Consolidate Conditional Expression”

### Long Method (Metodo lungo)
Gli oggetti che vivono meglio e piu’ a lungo sono gli oggetti con metodi corti.
I metodi esplicano cio’ che viene fatto, possono sostituire una o piu’ linee di codice, non e’ importante, e’ importante che abbia un nome che spieghi esattamente cosa sta facendo. 
Il 99% delle volte si risolve utilizzando “Extract Method.”
Se le variabili locali o i parametri interferiscono con l’estrazione del metodo si possono utilizzare  Replace Temp with Query, Introduce Parameter Object o Preserve Whole Object. Se nessuno dei metodi precedenti risolve il problema si puo’ utilizzare Replace Method with Method Object.
Per decomporre le condizioni utilizza  Decompose Conditional.

### Large Class (Classi grandi)
Le classi inizialmente nascono piccole, ma durante lo sviluppo crescono.
Per spezzare queste classi si possono utilizzare almeno 3 metodi:
Extract Class: aiuta se il comportamento di una classe puo’ essere spostato in un altra classe.
Extract Subclass: Aiuta se il comportamento di una classe puo’ essere implementato in diversi modi.
Extract Interface: Aiuta se se e’ neccessario avere una lista di operazioni che si possono utilizzare.


### Long Parameter List (Elenco dei parametri lungo)
Come programmatori c'è stato insegnato a passare come parametri tutte le informazioni necessarie per eseguire un metodo. Questo e’ comprensibile perche’ le alternative sono le variabili globali che solitamente sono molto dolorose da gestire.
Gli oggetti cambiano questa situazione perche’ se non hai tutto quello che ti serve puoi chiedere ad un altro oggetto di ottenerlo per te. Quindi con gli oggetti non passi tutto quello che il metodo richiede. Questo e’ buono perche’ una lunga lista di parametri sono difficili da comprendere.
Se un parametro e’ il risultato di una chiamata ad un metodo utilizzare  Replace Parameter with Method Call.
Invece di passare un gruppo di parametri ricevuti da un oggetto passa l’oggetto stesso utilizzando Preserve Whole Object.
Se Invece c’e’ una serie di parametri scorrelati puoi fare il merge utilizzando  Introduce Parameter Object.
Quando non applicare il refactor: quando si vuole evitare una dipendenza tra le classi.

### Divergent Change (Cambiamento divergente)
Quando ti trovi a cambiare il codice in due punti apparentemente scorrelati siamo di fronte ad uno smell di tipo divergent change.
Questo smell si puo’ affrontare utilizzando  Extract Class per suddividere il comportamento da quella classe in due classi distinte.
Se diverse classi hanno lo stesso comportamento utilizzare  (Extract Superclass e Extract Subclass)

### Shotgun Surgery (Chirurgia del fucile)
Quando per fare una modifica devi cambiare diversi classi e’ sintomo di questo smell. Questo puo’ capitare quando si ha applicato troppo il Divergent Change. 
Utilizzando il Move Method e Move Field si puo’ unificare il comportamento in una singola classe.

### Feature envy (Caratteristica invidia)
Questo Smell potrebbe venirti al naso dopo che hai fatto un replace parameter with object e potrebbe venirti l’idea di spostare anche il metodo dentro quella classe.
Se il metodo puo’ essere spostato facilmente utilizza Move Method.
Se si vuole spostare solo una parte del metodo utilizzare  Extract Method e spostarlo successivamente con Move Method. L’idea generale e’ di mettere insieme tutto quello che cambia (dati e metodi)
A volte si vuole tenere il comportamento separato dalla classe che contiene i dati, questo per dare la possibilita’ di cambiare dinamicamente il comportamento( vedi Strategy, Visitor e altri patterns)

### Data Clumps (Clump di dati)
Diverse parti del codice richiedono lo stesso gruppo di variabili. Questo “clumps” di varibili dovrebbe essere messo in una classe. Se alcune variabili non hanno senso senza le altre ti trovi davanti ad un Data Clumps (esempio non ha senso avere l’host di un database se non hai la porta o il driver da utilizzare per collegarti)
Per pulire questo smell si puo’ utilizzare l’ Extract Class per muovere i campi dentro una classe.
Se sono passati come parametro in un metodo utilizzare  Introduce Parameter Object.
Se invece sono passati solo alcuni dati in un altro metodo, inizia a pensare di utilizzare l’intero oggetto invece dei singoli campi, utilizzando  Preserve Whole Object.
Passare un intero oggetto come parametro di un metodo invece di passarci i campi primitivi puo’ portare ad una dipendenza indesiderata tra due classi

### Primitive Obsession (Ossessione primitiva)
E’ piu’ facile gestire i primitivi piuttosto che classi e spesso si tende a pensare: “aggiungo solo un altro campo da salvare”. Ma cosi’ facendo si creano delle classi sempre piu’ grandi se non enormi.
Le primitive sono spesso usate per "simulare" i tipi. Quindi, invece di un tipo di dati separato, hai un insieme di numeri o stringhe che formano l'elenco dei valori consentiti per alcune entità. Nomi di facile comprensione vengono quindi assegnati a questi numeri e stringhe specifici tramite costanti, motivo per cui sono diffusi in modo ampio e lontano.
Se hai un grande numero di campi primitivi puoi logicamente raggrupparli utilizzando Replace Data Value with Object.
Se il valore del campo primitivo e’ utilizzato come parametro di un metodo utilizza Introduce Parameter Object or Preserve Whole Object.
Quando i dati complicati sono codificati in variabili utilizzare  Replace Type Code with Class, Replace Type Code with Subclasses o Replace Type Code with State/Strategy.
Se ci sono degli array utilizzare  Replace Array with Object.

### Switch Statements
Il problema con gli switch e’ essenzialmente lo stesso che si ha con la duplicazione. Quando devi aggiungere una clausola allo switch devi cercare tutti i posti e modificarli. Fortunatamente grazie alla programmazione orientata agli oggetti e’ possibile trovare una soluzione elegante a questo problema .
Per isolare lo switch e metterlo nella giusta classe dovresti utilizzare  Extract Method e poi Move Method.
Se lo switch e’ basato su un codice usare  Replace Type Code with Subclasses o Replace Type Code with State/Strategy.
Dopo avere specificato la struttura di ereditarietà’ utilizzare  Replace Conditional with Polymorphism.
Ignorare quando viene utilizzato da una factory o fa una semplice operazione.

### Parallel Inheritance Hierarchies (Gerarchie di ereditarietà parallele)
Ogni volta che devi creare una subClasse in una classe, devi crearla anche in un altra classe.
La soluzione e’ di utilizzare il  Move Method e Move Field per eliminare l’accoppiamento delle due classi.

### Lazy Class (Classi Pigre)
Ogni classe costa tempo e denaro quindi quando una classe non ha abbastanza responsabilita’ dovrebbe essere cancellata. Questo puo’ capitare dopo un refactor che una classe rimane senza responsabilita’, oppure quando si pensa che si implementera’ una nuova features ma non e’ piu’ stata fatta. Si puo’ utilizzare l Inline Class , oppure in caso di sottoclassi si puo’ utilizzare Collapse Hierarchy.

### Speculative Generality (Generalità speculativa)
Il sintomo si ha quando delle classi sono state create per features future che non sono state piu’ implementate, il codice quindi risulta inutilizzato.
Per rimuovere le classi astratte inutilizzate usare Collapse Hierarchy.
Usa Inline Class per rimuovere una classe nel caso di delegazione non necessaria.
Metodo non utilizzati? Utilizza Inline Method  per sbarazzarti di loro.
Se c’e’ un metodo che ha dei parametri che non utilizza dovrebbero essere rimossi con Remove Parameter.
I campi non utilizzati dovrebbero semplicemente essere cancellati.


### Temporary Field (campi temporanei)
I campi temporanei si notano perché vengono valorizzati sono in determinate circostanze, negli altri casi sono vuoti.
In questo caso e’ opportuno estrarre i campi orfani in una classe attraverso  Extract Class. Successivamente utilizzare Introduce Null Object per rimpiazzare i controlli con il metodo della classe estratta.

### Message Chains (Catene di Messaggi)
La catena di messaggi si ha quando un metodo richiama un proprieta’ di una classe che a sua volta richiama la proprieta’ di un altra classe … ($a->b()->c()->d())
Per eliminare questo smell utilizzare Hide Delegate. Qualche volta puo’ aver senso utilizzare l’Extract Method  e muovere il nuovo metodo all’inizio della catena utlizzando Move Method.

### Middle Man (Uomo in mezzo)
Questo smell puo’ essere dovuto ad una eliminazione troppo zelante  delle catene di messaggi. Se molti metodi di una classe delegano ad un altra classe semplicemente utilizzare Remove Middle Man. Se c’e’ da aggiungere del comportamento utilizzare replace delegation with Inheritance 

### Inappropriate Intimacy (Intimità inappropriata)
Quando una classe utilizza i campi e i metodi di un altra classe siamo di fronte ad uno smell di tipo “Intimità inappropriata” 
La soluzione piu’ semplice e’ utilizzare  Move Method e Move Field per spostare le parti di una classe nell’altra ma questo funziona solo se la prima classe non necessita di parti dell’altra classe.
Un’altra soluzione puo’ essere utilizzare  Extract Class e Hide Delegate per rendere la relazione tra il codice ufficiale.
Se le classi sono reciprocamente dipendenti dovresti utilizzare Change Bidirectional Association to Unidirectional.
Se l’intimita’ e’ tra una super classe e un  sotto classe considera di utilizzare Replace Delegation with Inheritance.

### Alternative Classes with different interfaces (
Classi alternative con diverse interfacce)
Due classi fanno la stessa cosa ma hanno il nome dei metodi diverso. Utilizzare Rename Methods per rendere il nome dei metodi identici in tutte le classi coinvolte.Move Method, Add Parameter e Parameterize Method per rendere la firma e l’implementazione del metodo uguali.
Se questi metodi sono solo una parte delle funzioni di quella classe utilizzare Extract Superclass, in questo caso le classi esistenti diventeranno sottoclassi.
Se invece sono tutti i metodi dovresti essere in grado di cancellare una delle due classi.

### Incomplete Library class (Classe di libreria incompleta)
Il riuso e’ sopravvalutato. Se si vuole utilizzare una libreria per evitare di riscrivere il codice non c’e’ nulla di male ma se si vogliono aggiungere alcuni metodi alla classe della libreria potrebbe non essere immediato.
Utilizzare Introduce Foreign Method per aggiungere funzionalita’ ad una classe di una libreria, se si intendono invece fare grandi cambiamenti e’ opportuno utilizzare Introduce Local Extension.

### Data class (Classi di Dati)
Quando ci sono classi con soli campi e getter/setter si e’ di fronte ad uno smell di data class.  La grande forza della programmazione ad oggetti e’ che gli oggetti possono contenere anche del comportamento. Se una classe ha field pubblici utilizzare Encapsulate Field o  Encapsulate Collection per nasconderli e renderli accessibili solo tramite getter/setter. Riguarda il codice che usa i getter/setter e cerca di capire se ci sono dei comportamenti che starebbero meglio dentro la classe che stiamo riscrivendo. Se ne trovi puoi utilizzare  Move Method and Extract Method per spostarli.
Una volta fatto il refactor potrebbe essere utile rimuovere i vecchi metodi setter/getter per fare questo potrebbero esserti utili  Remove Setting Method e Hide Method 

### Refused Bequest (Lascito rifiutato)
Quando una sotto classe utilizza solo alcuni metodi e proprietà della classe madre. I metodi utilizzati vengono implementati vuoti o con delle eccezioni.
La ragione del problema e’ che qualcuno ha creato ereditarietà tra due classi solo per riutilizzare il codice nella superclasse. Ma la sottoclasse e la superclasse sono cose completamente diverse.
Se l'ereditarietà non ha senso bisogna eliminarla in favore di Replace Inheritance with Delegation.
Se l'ereditarietà è appropriata, eliminare i campi e i metodi non necessari nella sottoclasse. Estrai tutti i campi e metodi necessari alla sottoclasse dalla classe genitore, inseriscili in una nuova sottoclasse e imposta entrambe le classi in modo che ereditino da essa

### Comments (Commenti)
I commenti sono solitamente creati con le migliori intenzioni dall’autore quando realizza che il codice non e’ intuitivo. In questo caso i commenti fungono da deodorante quando c’e’ della puzza. Il miglior commento e’ un buon nome per un metodo o una classe.
Utilizzare  Extract Variable quando un'espressione complessa può essere divisa in diverse sotto espressioni. Se un commento spiega una sezione di codice questa sezione puo’ essere estratta in un metodo separato utilizzando Extract Method. Il nome del metodo dovrebbe riassumere cio’ che e’ scritto nel commento.
Se il metodo e’ gia stato estratto ma necessita comunque una spiegazione utilizzare Rename Method e dare un nome piu’ esplicativo. Se necessiti di fare un controllo affinche’ il sistema funzioni usa Introduce Assertion.
Quando ingorare:
Quando spiegano perche’ una cosa e’ stata implementata in una maniera particolare.
Quando spiegano un complesso algoritmo.
