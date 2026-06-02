# 3 june

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

Dato un grafo G=(V, E), si individui un algoritmo che è in grado in tempo O(|V|) di verificare se è presente un ciclo all'interno del grafo.