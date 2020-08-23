# Progetto Clips
Questo progetto è stato realizzato per l'esame di Intelligenza Artificiale e Laboratorio del corso di Laurea Magistrale all'Università di Torino.

## Setup
- Installare [Clips](http://clipsrules.sourceforge.net/).
- Installare [VSCode](https://code.visualstudio.com/download) e l'apposita estensione [Clips Language support](https://marketplace.visualstudio.com/items?itemName=nerg.clips-lang).
- Clonare il progetto

## Il problema
L’obiettivo del progetto è quello di sviluppare un sistema esperto che giochi ad una versione
semplificata della famigerata _Battaglia Navale_.
Il gioco è nella versione “in-solitario”, per cui c’è un solo giocatore (il vostro sistema esperto) che
deve indovinare la posizione di una flotta di navi distribuite su una griglia 10x10.
Come di consueto le navi da individuare sono le seguenti:

- 1 corazzata da 4 caselle
- 2 incrociatori da 3 caselle ciascuno
- 3 cacciatorpedinieri da 2 caselle ciascuno
- 4 sottomarini da 1 casella ciascuno

Le navi saranno, ovviamente, posizionate in verticale o in orizzantale e deve esserci almeno una
cella libera (cioè con dell’acqua) tra due navi.
Per rendere più semplice il problema, il contenuto di alcune celle sarà noto fin dall’inizio. Inoltre, in
corrispondenza di ciascuna riga e colonna sarà indicato il numero di celle che contengono navi.
Ad esempio, la seguente situazione rappresenta un possibile stato iniziale del problema.

<p align="center">
  <img src="https://github.com/lorenzofavaro/IA-Clips/blob/master/utils/campo.png"/>
</p>

Sapete quindi che in posizione [7, 1] c’è dell’acqua, in posizione [6, 5] c’è un sottomarino, in
posizione [5, 10] c’è un pezzo di nave che probabilmente continuerà nelle celle subito sopra e
subito sotto. Sapete inoltre, che nella prima riga due celle sono occupate da una nave, mentre nella
prima colonna sono 5, ecc.

Il vostro sistema esperto avrà quattro possibili azioni:

- _fire_ x y
- _guess_ x y
- _unguess_ x y
- _solve_

L’azione _fire_ è l’equivalente di un’azione percettiva e vi permette di vedere il contenuto della cella
[x, y].
L’azione _guess_ serve per indicare che il vostro sistema esperto ritiene ci sia una nave in posizione
[x, y]. Questa azione è da considerarsi un’ipotesi, per cui è ritrattabile in un momento successivo,
cioè il vostro sistema esperto potrebbe tornare sui suoi passi con il comando _unguess_.


Il comando _solve_ è da usarsi solo quando il vostro sistema esperto ritiene di aver risolto il gioco,
l’azione termina il gioco attivando il calcolo del punteggio secondo la seguente formula:

```
( 10 ∗ fok + 10 ∗ gok + 15 ∗ sink )−( 25 ∗ fko + 15 ∗ gko + 10 ∗ safe )
```
dove:

- fok è il numero di azioni fire che sono andate a segno
- gok è il numero di celle guessed corrette
- sink è il numero di navi totalemente affondate
- fko è il numero di azioni fire andate in acqua
- gko è il numero di celle guessed errate
- safe è il numero di celle che contengono una porzione di nave e che sono rimaste inviolate (né
guessed né fired)

Per rendere le cose un po’ più interessanti, avete solamente 5 _fire_ a disposizione. Inoltre, in un dato
momento non possono esserci più di venti caselle marcate “guessed”.
Lo scopo del gioco è quindi marcare tutte le caselle che contengono una nave come guessed, o
eventualmente averle colpite con _fire_.


