# ct.emitters

Il modulo `ct.emitters` ti permette di creare effetti particellari, allegarli a copie o farli seguire.

Internamente, si basa sul  [modulo `pixi-particles` di CloudKid](https://github.com/pixijs/pixi-particles).

::: warning Nota:
Se non hai impiegato emettitori nel tuo progetto, `ct.emitters` non sarà disponibile. Viene fornito in bundle solo se nel progetto sono presenti sistemi di particelle per ridurre le build del browser.
:::

## Creazione di effetti

Esistono tre metodi con logiche diverse, ciascuno adatto a situazioni particolari:

* `ct.emitters.fire('NameOfTheTandem', x, y)` genera un effetto in una posizione specificata, e questo è tutto. È utile per creare effetti che non dovrebbero seguire nulla o muoversi,  ad esempio per esplosioni, esplosioni di scintillio o impatti.
* `ct.emitters.follow(parentCopy, 'NameOfTheTandem')` vanno bene per effetti lunghi che dovrebbero essere collegati a una copia. Lasciano inalterate le particelle già emesse se spostati. È adatto per un effetto fumo, una scia, bolle e così via.
* `ct.emitters.append(parentCopy, 'NameOfTheTandem')` is similar to `follow`, but old particles are moved with an emitter. It is useful while making magic shield bubbles, or for particles that should stay *inside* a copy (think of a movable flask with boiling liquid and bubbles in it). è simile a `follow`, ma le vecchie particelle vengono spostate indieme all'emettitore. È utile durante la creazione di uno scudo magico di bolle o per particelle che dovrebbero rimanere *all'interno* di una copia (pensa a una fiaschetta mobile con un liquido bollente e bolle al suo interno).

Vediamoli tutti in azione (nota come la scia reagisce al movimento del robot):

`ct.emitters.fire` | `ct.emitters.follow` | `ct.emitters.append`
-|-|-
![](../images/emittersFire.gif) | ![](../images/emittersFollow.gif) | ![](../images/emittersAppend.gif) 

```js
// L'esempio "fire"
ct.emitters.fire('HeartTrail', this.x, this.y - 70);
```

```js
// L'esempio "follow"
ct.emitters.follow(this, 'HeartTrail', {
    position: {
        x: 0,
        y: -70
    }
});
```

```js
// L'esempio "append"
ct.emitters.append(this, 'HeartTrail', {
    position: {
        x: 0,
        y: -70
    }
});
```

## Opzioni aggiuntive

Avrai notato che questi tre metodi accettano un argomento aggiuntivo (ad es `ct.emitters.fire('NameOfAnEffect', x, y, options);`. ). Essendo un oggetto ha proprietà per modificare l'aspetto e il comportamento di un effetto:

* `scale` — ridimensionare l'oggetto con valori `x` e `y`.
* `position` — impostarlo per spostare ulteriormente la sequenza dell'emettitore rispetto alla copia a cui è collegato o rispetto alla copia che segue. Non funziona con `ct.emitter.fire`.
* `prewarmDelay` — se inferiore a 0, anticipa la sequenza dell'emettitore, il che significa  che si avvierà un determinato numero di secondi prima di mostrare le particelle nel mondo. Se maggiore di 0, posticiperà l'effetto per il numero di secondi specificato.
* `tint` — un colore applicato all'intero effetto, ad esempio 0xff0000 lo rende rosso.
* `alpha` — opacità impostata all'intero effetto, da 0 (invisibile) a 1 (completamente opaco, come in ct.IDE).
* `rotation` — rotazione in gradi.
* `isUi` — se impostato su `true`, utilizzerà la scala temporale dei livelli dell'interfaccia utente. Ciò influisce sul modo in cui un effetto viene simulato durante gli effetti al rallentatore e le pause di gioco.
* `depth` — la profondità (parametro `Depth`) della sequenza. Il valore predefinito è `Infinity` (sarà sopra tutto).
* `room` — la stanza a cui applicare l'effetto. Il valore predefinito è il livello principale corrente (ct.room). Non ha effetto con `ct.emitters.attach`, poiché specifichi già il genitore di un effetto nel primo argomento.

Ogni proprietà è facoltativa. Un esempio: se volessimo creare un effetto rossastro più piccolo sopra una copia che rimane alla stessa profondità della copia, scriveremo:

```js
ct.emitters.follow(this, 'Debuff', {
    scale: {
        x: 0.75,
        y: 0.75
    },
    position: {
        x: 0,
        y: -80
    },
    tint: 0xff9999,
    depth: this.depth
});
```

## Manipolare degli emettitori

Da soli, gli effetti creati funzionano in modo autonomo: si fermeranno automaticamente quando il loro tempo sarà scaduto, o quando il loro proprietario verrà distrutto, lasciando una bella scia di particelle. Ma a volte abbiamo bisogno di ripulire completamente l'effetto, o metterlo in pausa e riprenderlo più tardi, o interromperlo completamente prima del  solito.

Ciascuno dei `ct.emitters.fire`, `ct.emitters.append` e `ct.emitters.follow` restituisce un riferimento all'effetto creato che possiamo utilizzare:

```js
// Creaiamo uno scudo di bolle!
this.shied = ct.emitters.append(this, 'BubbleEffect');

// Più tardi, quando non abbiamo più bisogno dello scudo:
this.shield.stop();
this.shield = null; // Eliminare l'effetto per liberare memoria
```

Ci sono un certo numero di proprietà che possiamo usare in questo modo:

* `emitter.stop();` impedisce la generazione di nuove particelle. Quando le particelle precedenti scompaiono, il tandem emettitore si autodistruggerà.
* `emitter.clear();` cancella istantaneamente tutte le particelle.
* `emitter.kill` è una proprietà simile alla proprietà delle copie `kill`: impostandola su `true` distruggerà istantaneamente l'effetto con tutte le sue particelle.
* `emitter.frozen` interrompe l'aggiornamento dell'effetto se impostato su `true`.
* `emitter.pause()` smette di generare nuove particelle, ma le particelle rimanenti sono ancora animate. Puoi riprendere la spawning con `emitter.resume();`.

