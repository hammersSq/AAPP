What is relevant?
- no proof required
for each algorithm/topic:
- how it works
- complexity of any of the algorithm that we have seen(also if not explained)
- assumptions and example 
# Question
**Describe how skip-list delete works.**
la skip list è una struttura dati che consente ricerca con un running time Theta (logn).
Per spiegare come funziona la delete dobbiamo spiegare come funziona la search:
si parte dalla lista piu in alto, e si cerca l' elemento come si farebbe in una normale linked list, se non lo si trova allora si va alla lista sottostante usando il collegamento dal noto che contiene un elemento x< di quello che stiamo cercando e il cui successivo contiene un elemento x > di quello che stiamo cercando.
questo procedimento continua fino ad arrivare all' ultima lista, la quale contiene tutti gli elementi. 
La delete è un caso particolare della search: mentre cerchiamo l' elemento ogni volta che lo troviamo in una lista lo eliminiamo da questa ed andiamo nella lista sottostante, eliminandolo anche da qui fino alla lista di livello più basso.
Anche la delete come la search ha un running time Theta (log(n))

**Describe how radix and counting sort algorithms interact.**
Il radix sort è un algoritmo di sorting che ordina dei numeri "cifra per cifra".
in particolare parte dalla cifra meno significativa, ed ordina tutti gli elementi usando uno stable sort(ovvero che tra elementi uguali mantiene ordine di input), finito con la cifra meno significativa passa man mano alle altre fino ad arrivare alla cifra più significativa. così facendo riesce ad ordinare tutte gli elementi.
Il counting sort in questo contesto prende ruolo dello stable sort algorithm, infatti esso gode di questa proprietà
la complessità del counting sort è Theta (n+k), dove k è l' elemento massimo salvato nella lista.
se teniamo conto di radix sort che usa il counting sort su word a 4 byte abbiamo una complessita
Thet(4+2<sup>16</sup>)
in generale su b bit divisi per r (ottenendo b/r digits) abbiamo una complessità di 
Theta(b/r x 2<sup>r</sup>)


**Describe what the memoization technique is and for what is relevant. List and discuss some examples using this technique.**
la meoization technique è una tecnica utilizzata in dynamic programming. In dynamic programming abbiami:
- optimal sub structure
- overlapping sub problem
la memoiztion va a "gestire" gli overlapping sub problem, infatti abbiamo che un problema viene risolto combinando le soluzioni di diversi sotto problemi, solo che quando si parla di dynamic programming accade che la soluzione di alcuni sotto problemi viene utilizzata più volte. con la memoization technique questi risultati vengono salvati in delle strutture dati e calcolati solo la prima volta che vengono incontrati. le volte successive, se già calcolati si prende solo il risultato.
Un esempio di problema che sfrutta la memoization e LCS .
infatti calcolando la lunghezza dei c [i]  [j] capita che più di una volta si vada a calcolare una cella specifica. quindi prima di calcolarla ricorsivamente si controlla se è già stata calcolata e si evita il lavoto.
Un altro esempio sono le ROBDD (Reducted Ordered BDD). Queste sono delle strutture dati che vanno a rappresentare efficaciemente delle funzioni booleane. qui la dynamic programming cade a pennello, ogni funzione può essere rappresentata come delle sotto funzioni, e nel rappresentarle graficamente ci possono essere delle ripetizioni. Questa struttura dati viene contratta facendo in modo che non ci siano ripetizioni, e quando un risultato di qualche sotto espressione viene calcolato per la prima volta allora lo si salva in una tabella (unique table) così che quando serietà di nuovo non dovrà essere ricalcolato


**Describe the move to front heuristic.**
La move to front heuristic è un euristica utilizzata nelle self organizing list. l' obbiettivo è quello di andare a migliorare le prestazioni di ricerca in una lista, spostando nelle parti iniziali della lista gli elementi ricercati più di frequente.
La MTF funziona cosi:
- ogni volta che un elemento viene richiesto, lo si sposta in testa alla lista, tramite un operazione di traslazione (lo si inverte con l' elemento precedente finchè non arrivi in testa)
è dimostrato che questo tipo di euristica è molto efficiente, in particolare si dice che è 4-competitive se lo spostamento in lista richiede Theta(n) e 2 competitive se invece lo spostamento in testa è " free"

a-competitive significa che il costo CA(S) (quanto ci mette il nostro algoritmo su una generica sequnza di ricerche ) è <= a 
a x Copt(S) +k
dove Copt è il costo del miglior algoritmo online(che conosce a priori la sequenza) che esiste.

**Describe the randomized select algorithm (Rand-Select).**
il randomized select algorithm è un algoritmo che sfrutta rand-partition per cercare, in una lista di elementi quello con il rank(k), ovvero quello che è maggiore di k elementi 
Rand-partition è una funzione che viene utilizzata nel quicksort e che, scegliendo un pivot casuale mette a sinistra del' elemento pivot tutti gli elementi che sono minori del pivot e a destra tutti quelli che sono maggiori. 
il rand select sfrutta questa cosa, per cercare man mano l' elemento di rank(k):
RandSelect(A,i,p,q)
r=RandPartition(A,p,q)
k=r-p+1
if i=k -> return A[i]
if i<k randSelect(A,i,p,r-1,i) ## chiamo algoritmo sulla parte sinistra della partizione
else randSelect(A,r+1,q, i-k)
Nel caso peggiore questo richiede un tempo Theta(n<sup>2</sup>)
ma è dimostrato che nel il valore atteso del running time di questo algoritmo è Theta(n)

**Describe for what the amortized analysis is meant and specifically discuss the accounting method.**
la amortized analysis  è qualsiasi tipo di analisi di un algoritmo che vuole studiare la complessità di quest' ultimo non guardando casi specifici, ma anlizzando una ripetizione di operazioni col fine di dimostrare che, anche se il costo di una operazione può essere alto, nel complesso questo viene " ammortizzato " dal basso costo delle altre, avendo una complessità generale molto minore di quella che si può vedere semplicemente facendo n volte il worst case.
l' accounting method è un metodo per fare questo tipo di analisi. 
il metodo si fonda sul concetto di "saldo", e di "costo per operazione".
funziona così:
- teniamo conto che un operazione costa 1
- ogni volta che dobbiamo fare questa operazione che costa 1 mettiamo c-dollari nel saldo
- così facendo, se riusciamo a mantenere il saldo sempre positivo possiamo dire che il costo totale di tutte le operazioni è limitato da c
per capire meglio quest' esempio possiamo usare una dynamic list:
- partiamo con un saldo=2
- aggiungimo un elemento nella lista di solo 1, paghiamo 1 (saldo=1)
- per aggiungere un'altro elemento dobbiamo creare una nuova lista e copiare con size doppia. 
- copiamo il nuovo elemento pagando 1
- ora per aggiungere il nuovo elemento paghiamo 1, ma salviamo 2 nel conto (3-1)
così facendo possiamo dire che su n inserimenti paghiamo 3n, quindi Theta(n)... Theta(1) per ogni inserimento


**Describe the main characteristics of a PRAM model.**
Il PRAM model è un modello di macchina che serve a studiare e analizzare come si comportano gli algoritmi che vanno a sfruttare la parallelizzazione di processi. 
IL PRAM model si basa su un altro modello, il RAM (RANDOM ACCESS MACHINE).
La RAM descrive una macchina con un numero illimitato di registri, ogni registro può contenere interi formati da un numero illimitato di bit, tutte le operazioni prendono 1 unit of time e consideriamo il running time di un algoritmo come il numero di operazioni effettuate.
Il PRAM model è <m,X,Y,A>
dove m è un insieme illimitato di RAM
X è un insieme illimitato di celle di input
Y è un insieme illimitato di celle di output
A è un insieme illimitato di celle di memoria condivise
Dipende da come gestiamo l' accesso alle celle condivise possiamo classificare una PRAM:
- EW ogni cella di memoria è scritta esclusivamente ad un processore
- ER ogni cella di memoria è scritta esclutivamente da un processore
- CW scrittura concorrente delle celle
- CR lettura concorrente delle celle
Naturalmente le cose si possono combinare EWCW, EWER...
la gestione delle CW è importante, possiamo avere:
- il valore ralmente scritto è scelto a random
- il valore realmente scritto viene scelto in base a una priorità
- il valore può essere scritto se tutti i processi che scrivono su quella cella stanno scrivendo la stessa cosa.
