<!-- # 3 june Old

## Esercizio 1

I numeri di Pincopallo, P(m,n), sono definiti per ogni m, n >= 0 da:
- P(0,0) = 1
- P(m, 0) = 0 per ogni m >= 1
- P(0, n) = 2 per ogni n >= 1
- P(m,n) = nP(m – 1, n) + P(m – 1, n – 1), altrimenti.

1. Scrivere lo pseudocodice di un algoritmo di programmazione dinamica per il calcolo di P(m,n).
2. Analizzarne la complessità di tempo e di spazio, giustificando la risposta.

```
PincoPallo(m, n)
{
    PP[m][n];

    PP[0][0] = 1
    for i=1 to m {
        PP[i][0] = 1;
    }

    for i=1 to n {
        PP[0][i] = 2;
    }

    for i=1 to m {
        for j=1 to n {
            PP[i][j] = n * PP[i-1][j] + PP[i-1][j-1];
        }
    }

    return PP[m][n];
}
```

La complessità di tempo del seguente algoritmo, è chiaramente dominata dal tempo necessario per riuscire a riempire la matrice PP. La complessità è quindi O(n*m).<br>
In questo caso la soluzione impiega tempo omega di n*m. Dato che la matrice dovrà sempre essere riempita per terminare l'esecuzione dell'algoritmo.

## Esercizio 2

Il tuo caro zio ti ha chiesto di accompagnarlo a comprare il battiscopa da fissare lungo tutto il corridoio di
casa per una lunghezza di N cm. Il tipo di battiscopa scelto è disponibile in mattonelle di legno di lunghezza
1 cm, 7 cm o 10 cm, e in negozio sono disponibili tutte le mattonelle che vi servono. Poiché le mattonelle
hanno tutte lo stesso prezzo, indipendentemente dalla lunghezza, volete capire qual è il minimo numero di
mattonelle necessarie per coprire tutta la lunghezza del corridoio. Ti ricordi che questo genere di problemi
possono essere risolti efficientemente usando la programmazione dinamica.

Definisci allora OPT(x) come il minimo numero di mattonelle necessarie per coprire una lunghezza di x cm.
Esempio: Se N = 14, potreste usare 1 mattonella da 10 cm e 4 da 1 cm, oppure 2 da 7 cm, oppure
fare altre scelte; in questo caso OPT(14) = 2.

Scrivere una relazione di ricorrenza per OPT(x) che permetta di calcolare OPT(N), dove N è la lunghezza
in cm del vostro corridoio. Giustificare la risposta.

$$OPT(x) = 
\begin{cases}
x & se \ x < 7 \\
min{OPT(x - 1), OPT(x - 7)} + 1 & se \ x < 10 \\
min(OPT(x - 1), OPT(x - 7), OPT(x - 10)) & se \ altrimenti
\end{cases}
$$

Perchè questa relazione di ricorrenza è corretta?<br>
Analizziamo i vari casi in maniera differente.<br>
Il primo caso, è molto banale. Se x è minore di 7, allora l'unico genere di mattonella che si può utilizzare, è quella unitaria. In questo caso quindi non possiamo fare altro che prendere x mattonelle per coprire questo spazio residuo di x.<br>
Analizziamo ora il secondo caso. Dato che lo spazio residuo è minore strettamente di 10, possiamo scegliere fra due tipologie di mattonelle. Quelle unitarie e quelle di lunghezza 7. Scegliamo di prendere il minimo tra le soluzioni ottime con capacità residua x - 1 e x - 7, aggiungendo poi 1. Praticamente, stiamo prendendo una sola mattonella e sfruttando le soluzioni ottime ai sottoproblemi. Dimostriamo ora la corretteza di questo approccio. <br>
Si suppone ragionando per assurdo che OPT(x) non sia la soluzione ottima, mentre invece O rappresenta la soluzione ottima a questo problema. Siamo sicuri che nella soluzione O, sia presente almeno una mattonella di tipologia unitaria o di dimensione 7. Dato che non sappiamo quale delle due, diremo semplicemente che tale mattonella ha una taglia t.<br>
Sappiamo dalla nostra relazione di ricorrenza che vale che:<br>
$$OPT(x) <= OPT(x - t) + 1$$
Si può quindi affermare che:
$$O < OPT(x) \le OPT(x - t) + 1$$
Questo implica che si può scrivere:
$$O - 1 < OPT(x - t)$$
Questo significa che abbiamo trovato una soluzione migliore al sottoproblema per capacità x - t. Questo contraddice l'ipotesi. C.V.D.<br>
Per l'ultimo caso, si può ripetere un ragionamento speculare.

## Esercizio 3 

Descrivere un algoritmo che, dato un grafo non orientato connesso G = (V, E) e un vertice u in V,
determini un orientamento degli archi di G in modo che nel grafo orientato risultante ogni vertice di V
sia raggiungibile da u.
Più precisamente, l’algoritmo, preso un grafo non orientato G = (V, E) e un vertice u in V, deve
restituire un grafo orientato G→ = (V, E→), con |E| =| E →|; in cui ogni arco (u, v) di E è sostituito con
l’arco diretto (u, v) oppure con l’arco diretto (v, u), in modo che G→ soddisfi la richiesta.
La descrizione può essere effettuata tramite pseudo-codice oppure verbalmente, ma in maniera precisa. Giustificare la correttezza dell’algoritmo descritto.

```
build(G = (V, E), v)
{
    E2 = {}
    explored[|V|] = {False}
    explored_edges[|E|] = {False}

    queue = {v}
    explored[v] = True

    while ! queue.empty()
    {
        for e=(u, w) in E[u]
        {
            if ! explored[w]
            {
                queue.insert(w)
                explored[w] = True
            }

            if ! explored_edges[(u, w)] and ! explored_edges[(w, u)]
            {
                explored_edges[(u, w)] = True
                E2[u].push((u, w)) // arco direzionato da u a w
            }
        }
    }

    return (u, E2)
}

```

La complessità temporale di questo algoritmo, è ovviamente data dalla complessità dell'esplorazione, quindi l'algoritmo è O(|V| + |E|). Per quanto riguarda la complessità spaziale, in questo caso, sarà O(|V| + |E|).

Il seguente algoritmo è corretto perchè, è noto che la BFS visita il grafo costruendo una sorta di albero radicato nel primo nodo esplorato, che nel nostro caso è v. Dall'algoritmo, è visibile che costruiamo sempre un arco dal nodo padre al nodo figlio all'interno dell'albero. Questo ci porta a concludere che nel grafo orientato costruito G', sarà presente un sottografo che è un albero radicato in v, dove sono presenti tutti quanti i nodi. Dato che è presente questo sottografo, siamo sicuri che ogni nodo che era raggiungibile a partire da v nel grafo originale lo sarà anche qui.

## Esercizio 4



## Esercizio 5

Dato un grafo G=(V,E), si individui un algoritmo che è in grado in tempo O(|V| + |E|) di verificare se è presente o meno un ciclo all'interno del grafo.

Dato un grafo G=(V, E), si individui un algoritmo che è in grado in tempo O(|V|) di verificare se è presente un ciclo all'interno del grafo. -->

# Soluzioni di alcuni esercizi

*I seguenti appunti sono da intendere come delle note personali. Non è consigliato quindi copiarli prendendoli per buoni e il sottoscritto non si assume nessuna responsabilità riguardo la correttezza di quanto scritto.*

# 3 June

## Esercizio 1 (Problema dello zaino con elementi adiacenti)

Si descriva ed analizzi un algoritmo per la seguente variazione del problema dello zaino: Dati n oggetti di peso $w_1,...,w_n$ e con rispettivi valori $v_1,...,v_n$ e una capacità massima $W$. Si vuole individuare un sottoinsieme $S = \{j|1 \le j \le n\}$ tale che $\sum_{w \in S}w \le W$ di valore totale massimo e dove l'insieme di oggetti è tale che se è presente l'elemento di indice i non è presente l'elemento di indice i+1.

Il seguente problema può essere risolto attraverso la seguente relazione di ricorrenza.

```math
\text{Opt}(x, i) = \begin{cases} 
0 & \text{se } i = 0 \\ 
\text{Opt}(x, i - 1) & \text{se } x < w_i \\ 
\max\{\text{Opt}(x, i-1), \text{Opt}(x - w_i, i - 2) + v_i\} & \text{altrimenti} 
\end{cases}
```

Dimostriamo che la seguente relazione di ricorrenza è coretta.

Nel caso del passo base, è banale che stiamo individuando la soluzione ottima. Non è possibile scegliere una soluzione di valore > 0 se non abbiamo elementi rimasti.<br>
Il primo passo ricorsivo, è anch'esso banalmente ottimo. Se la nostra capacità è x e l'elemento i-esimo ha un peso che eccede questa capacità, non possiamo che cercare semplicemente di realizzare uno zaino con gli elementi rimasti.<br>
Rimane quindi da dimostrare la corretteza dell'ultimo passo. Ragionando per assurdo, ipotizziamo di avere una certa i > 0 e una $x \ge w_i$ e che $Opt(x, i)$ non è la soluzione ottima. Si suppone quindi che la soluzione ottima per questo sottoproblema sia $O$.<br>
La soluzione ottima, o contiene l'elemento i-esimo oppure non lo contiene.<br>
Supponiamo che lo contenga.<br>
Vale quindi la seguente relazione: $O > Opt(x, i) \ge max\{Opt(x - w_i, i - 2) + v_i, Opt(x, i - 1)\}$. Inoltre, dato che $O$ è la soluzione ottima e contiene l'elemento i-esimo, si può immaginare di rimuovere da $O$ l'elemento i-esimo. In questo modo otteniamo una soluzione ammissibile per il sottoproblema con capacità residua $x - w_i$ e dove vengono solamente utilizzati $i - 2$. Questa soluzione avrà un valore $O - v_i$.<br>
Tramite le relazioni precedenti è possibile ricavare che $O > Opt(x - w_i, i - 2) + v_i$ dal quale è possibile determinare che $O - v_i > Opt(x - w_i, i - 2)$, ma questo significherebbe che si è trovato una soluzione ammissibile al sottoproblema di capacità residua $x - w_i$ con i-2 elementi rimasti, che è assurdo.<br>
Ci rimane quindi l'altra opzione, quella in cui in $O$ non è presente l'elemento i-esimo. In questo caso, $O$ è una soluzione ammissibile per il sottoproblema con capacità residua $x - w_i$ e con i-1 elementi restanti. Ma sappiamo anche dalla relazioni precedenti che è possibile scrivere $O > Opt(x, i - 1)$. Ma questo è assurdo, perchè avremmo trovato una soluzione ammissibile per quel sottoproblema con un valore migliore di quello dell'ottimo.<br>
C.V.D.

Data la seguente relazione di ricorrenza, scrivere una soluzione al problema diventa abbastanza banale. Ci basta infatti implementare un algoritmo che va a riempire una matrice che viene utilizzata per mantenere in memoria le soluzioni a tutti quanti i sottoproblemi.

La complessità spaziale e temporale è $O(nW)$. Quindi sia la memoria necessaria per la matrice, che il tempo necessario a risolvere il problema.

# 5 June

## Esercizio 1 (Ponti)

<!---T: Da riportare almeno la relazione di ricorrenza-->

## Esercizio 2 (Perturbazione MST)

Dato un grafo G e la funzione $c$ che assegna un peso a un arco di G. $T$ è un MST pLa complessità temporale dell'algoritmo è dominata, nel caso peggiore, dal tempo necessario per la visita.er G. Si consideri una funzione $c'$ che rappresenta una perturbazione di G, dove $c'(e) = c(e) + 2$ con e arco di G. Si dimostri che $T$ è un MST anche nel grafo con i costi perturbati.

Ragionando per assurdo si supponga che $T$ non è l'MST nel grafo perturbato G. Questo significa che esiste $T'$ che è un MST per G perturbato. Quindi si può concludere che $c'(T') < c'(T)$. Si può però scrivere:

```math
c'(T') = \sum_{e \in T'} (c(e) + 2) = \sum_{e \in T'} c(e) + 2*|E| \\
c'(T) = \sum_{e \in T} (c(e) + 2) = \sum_{e \in T} c(e) + 2*|E| \\
```

Ma quindi si può determinare che:
```math
\sum_{e \in T'} c(e) + 2|E| < \sum_{e \in T} c(e) + 2|E|
```
Possiamo rimuovere la quantità 2|E| da entrambi i membri mantendendo invariata la disequazione.<br>
In questo modo, si arriva a:
```math
\sum_{e \in T'} c(e) < \sum_{e \in T} c(e)
```
Che può essere riscritto come:
```math
c(T') < c(T)
```
T' è sicuramente un albero ricoprente a prescindere dal costo assegnato agli archi, ma inoltre, avrebbe un costo minore del MST T per G. Questo è assurdo, dato che abbiamo negato l'ipotesi.<br>
C.V.D.

## Esercizio 3 (Trovare un ciclo all'interno del grafo)

Sappiamo bene come trovare un ciclo all'interno di un grafo non orientato.<br>
Per trovare un ciclo, in questo grafo, ci basta una visita del grafo.<br>
Questo è un esempio di algoritmo che ci consente di individuare un ciclo all'interno del grafo.<br>
```
FindCycle(G=(V, E))
{
    for v in V
    {
        explored[v] = False;
    }
    explored[V[0]] = True;
    return FindCycleRec(G, V[0], null, explored);
}

FindCycleRec(G=(V, E), v, w, explored)
{
    find = False;

    for (v, u) in E[v]
    {
        if ! explored[u]
        {
            explored[u] = True;
            find = find || FindCycleRec(G, u, v, explored);
        }
        else if w != u // Se è stato esplorato e tale nodo non è il "padre" di v, allora è un ciclo di sicuro.
        {
            return True;
        }
    }

    return find;
}
```
E' da notare che per un grafo non orientato non connesso, sarà necessario chiamare la procedura di FindCycleRec molteplici volte.
La seguente procedura, quanto tempo impiega? $O(|E| + |V|)$, il costo dell'esplorazione.

E' possibile fare di meglio?

In tal senso, è bene ricordare una proprietà dei grafi.<br>
Se in un grafo sono presenti un numero di archi $|E| > |V| - 1$ allora è presente almeno un ciclo nel grafo.

Come possiamo giustificare questa proprietà?

Proviamo a costruire un grafo con un numero di archi $|E| > |V| - 1$ ma senza neanche un ciclo nel grafo.

La soluzione migliore per cercare di piazzare un numero di archi pari a $|E| = |V|$, è quella di piazzare i primi $|V| - 1$ archi quasi a creare una catena. Rimane comunque un arco da piazzare e non riusciremo a farlo senza creare un ciclo.

Con un numero di archi ancora maggiore, di sicuro non possiamo fare altro che aumentare il numero di cicli.

Ma quindi se in un grafo ci sono un numero di archi $|E| <= |V| - 1$ allora non è presente nemmeno un ciclo all'interno del grafo?<br>
Falso, ed è anche facile mostrare un controesempio a questa asserzione.

Vediamo quindi come utilizzare questa proprietà a nostro vantaggio.

Il problema, non caratterizzava il grafo al punto di obbligarci a scegliere di utilizzare una matrice di adiacenza piuttosto che le liste di adiacenza. Nel nostro caso, decideremo di utilizzare le liste di adiacenza.

Dato che, se il numero di archi che sono presenti all'interno del grafo, sono strettamente maggiori di |V| - 1, allora siamo sicuri che esiste un ciclo all'interno del grafo, non dobbiamo fare altro.<br>
Se il numero di archi che sono presenti nel grafo non sono pari a questo numero, allora non sappiamo se è presente o meno il ciclo. Procediamo quindi ad utilizzare l'algoritmo che abbiamo visto precedentemente per metterci alla ricerca del ciclo nel grafo. Quando lanciamo tale algoritmo, siamo sicuri che vale che $|E| < |V|$. Quindi, dato che la complessità dell'algoritmo di visita del grafo è $O(|V| + |E|)$ e vale che $|E| < |V|$, allora vale che $O(|V| + |E|) = O(|V| + |E|) = O(2|V|) = O(|V|)$.<br>
In poche parole, abbiamo dimostrato che nel caso peggiore, eseguiamo l'algoritmo di visita, ma quest'ultimo, impighierà tempo lineare nel numero di nodi. Dato che la complessità di tutto l'algoritmo è dominato dalla complessità della visita, diremo che l'intero algoritmo è O(|V|).

In questo caso, abbiamo assunto di possedere a priori come informazione il numero di archi che sono presenti all'interno del grafo.<br>
Ci potrebbe capitare, di non possedere a priori tale informazione. Tuttavia, utilizzando la lista di adiacenze che abbiamo deciso di utilizzare per rappresentare il grafo, possiamo tentare di contare gli archi finchè non si arriva a $|V|$ archi.<br>
A quel punto, se ci sono almeno $|V|$ archi, sappiamo che possiamo restituire True.<br>
Se ci sono un numero minori di questi archi, ce la siamo comunque sbrigata in tempo $O(|V|)$ e sappiamo che è necessario eseguire la visita nel grafo come abbiamo detto prima.

## Esercizio 4 (Verificare che è presente un pozzo all'interno del grafo)

Dato un certo grafo $G=(V,E)$ che è un grafo non orientato, allora si vuole determinare se è presente un pozzo all'interno del seguente grafo.<br> Si definisce pozzo, un nodo $v \in V$ tale che il numero di archi uscenti da $v$ è pari a zero mentre il numero di archi entranti in $v$ è pari a $|V| - 1$.<br>
Il grafo in questione è rappresentato mediante la sua matrice di adiacenza.

Stiamo cercando di risolvere il seguente problema. Il primo obbiettivo, è sempre quello di avere un algoritmo che funziona, anche se non è il migliore di tutti.

Il seguente problema, può essere risolto banalmente contando il numero di archi uscenti e il numero di archi entranti in ciascuno dei nodi e prendendo nota di queste informazioni.<br>

E' da notare, che questo problema risulta essere molto più semplice, nel caso in cui ci fosse data la scelta di quale metodo utilizzare per rappresentare la lista di archi che sono presenti all'interno del grafo.<br>
Se si potesse utilizzare una lista di adiacenza, in questo caso, il problema sarebbe pressochè risolto.

Dato che abbiamo la matrice di adiacenza, ci tocca effettuare per lo meno questo operazione di contare sia gli archi uscenti che gli archi entranti per ciascun nodo. Procediamo a scrivere un algoritmo che effettua l'operazione di cui stiamo parlando.
```
Pozzi(G=(V,E))
{
    edges_in[|V|];
    edges_out[|V|];

    for v=0 to |V| - 1
    {
        edges_in[v] = 0;
        edges_out[v] = 0;
    }

    for i=0 to |V| - 1
    {
        for j=0 to |V| - 1
        {
            if E[i, j] == 1 // Significa che c'è un arco dal nodo i al nodo j
            {
                edges_in[j] += 1;
                edges_out[i] += 1;
            }
        }
    }

    for i=0 to |V| - 1
    {
        // Verifichiamo che il numero di archi entranti 
        // uscenti rispettino la definizione
        if edges_in[i] == |V| - 1 && edges_out[i] == 0
        {
            return True;
        }
    }

    // Significa che non abbiamo trovato neanche un pozzo
    return False;
}
```

Il seguente algoritmo, è intuitivamente corretto. Stiamo letteralmente contando e poi verificando per ciascuno dei nodi la condizione.<br>
La complessità del seguente algoritmo, è $O(|V|^2)$. Infatti, la complessità temporale è dominata dall'operazione di visita della matrice delle adiacenze.

Si può fare di meglio. In particolare, la matrice delle adiacenze ci permette di verificare per una coppia di nodi $i$ e $j$ se è presente un arco uscente da $i$ a $j$ in tempo $O(1)$.<br>
Un singolo controllo, su una coppia di nodi, ci permette in realtà di scartare sempre un nodo nella ricerca del pozzo. Infatti, assumendo di analizzare una coppia di nodi $i$ e $j$, avremo due casi possibili:
1. Esiste un arco diretto da $i$ a $j$, questo implica che $i$ non è un pozzo, dato che è presente almeno un arco uscente.
2. Non esiste un arco diretto da $i$ a $j$, questo implica che $j$ non è un pozzo, dato che non raggiungerà mai il numero di archi entranti pari a $|V| - 1$.

La seguente proprietà, ci può essere molto utile per sviluppare un algoritmo efficiente.<br>
Immaginiamo di lavorare con un insieme di nodi che rappresentano i nodi che sono "candidati" a poter essere dei pozzi. Ad ogni iterazione, si selezionano due nodi qualunque da tale insieme. In tempo $O(1)$ con la matrice di adiacenza si verifica se è presente un arco dal nodo $i$ al nodo $j$. Ad ogni iterazione, uno dei due nodi sappiamo che può essere scartato e quindi lo rimuoviamo da tale insieme.

Dato che a ogni iterazione la taglia dell'insieme viene ridotta di 1 e che questa operazione ha praticamente un costo costante grazie alla matrice delle adiacenze, in $O(|V|)$ ci ritroveremo con un insieme con un solo elemento al suo interno.

Non siamo sicuri che l'elemento che è rimasto all'interno dell'insieme, sia un pozzo, nè tanto meno che non lo sia. Siamo *solamente* sicuri che tutti quanti gli altri elementi non sono dei pozzi.<br>
Si può quindi procedere, in tempo $O(|V|)$ a verificare se questo ultimo è un pozzo o meno.

Saremo quindi riusciti, in tempo $O(|V|)$ a determinare se è presente un pozzo o meno all'interno di un grafo.