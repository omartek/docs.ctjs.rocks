# ct

`ct` represents the game engine itself, extended with modules and core libraries. The core lib contains of:

`ct `rappresenta il motore stesso di gioco, esteso con moduli e librerie di base. La core lib contiene:

* [ct.backgrounds](ct.backgrounds.html) per la gestione degli sfondi;
* [ct.camera](ct.camera.html) per la gestione del viewport (ripresa fotografica);
* [ct.emitters](ct.emitters.html) per sistemi di particelle;
* [ct.inputs](ct.inputs.html) e [ct.actions](ct.actions.html) fper la gestione dell'input dell'utente;
* [ct.res](ct.res.html) per il caricamento delle risorse;
* [ct.rooms](ct.rooms.html) per spostarsi tra i livelli e impilarli (per l'interfaccia utente, l'illuminazione e il gameplay, ad esempio);
* [ct.sound](ct.sound.html) per riprodurre e modificare gli effetti sonori;
* [ct.styles](ct.styles.html) per riprodurre e modificare gli effetti sonori;
* [ct.tilemaps](ct.tilemaps.html) per la generazione dinamica di livelli realizzati con le tile;
* [ct.timer](ct.timer.html) per eventi asincroni;
* [ct.templates](ct.templates.html) per creare, trovare e gestire template e copie;
* [ct.u](ct.u.html) per le funzioni vettoriali e altre utilità.

Di solito utilizzerai le API di cui sopra, oltre a quelle fornite tramite moduli ct.js.

Di per sé, ct.js è basato su [Pixi.js](https://www.pixijs.com/) , una libreria grafica HTML5. Puoi usare le sue API se ritieni che quella di ct.js non sia sufficiente.

## Metodi e proprietà

### `ct.pixiApp`

L' [applicazione Pixi.js](https://pixijs.download/release/docs/PIXI.Application.html) del gioco.

### `ct.stage`

Lo [stage](https://pixijs.download/release/docs/PIXI.Application.html#stage) principale del gioco.

### `ct.meta`

Restituisce i metadati forniti all'interno dell'editor ct.js , come `author`,`site`, `version` e `name`.

### `ct.delta`

A multiplier that shows how much a current frame differs from the target FPS. It will change depending on game's performance. For example, it will be `2` at 30 FPS, as a target one is 60 FPS, and it will be `1` at completely smooth target framerate.

Un moltiplicatore che monitora quanto un fotogramma corrente differisce dall'FPS target. Cambierà a seconda delle prestazioni del gioco. Ad esempio, varrà `2 ` a 30 FPS, poiché l'obiettivo è 60 FPS e varrà `1` a framerate target fluido.

You can use this delta while designing movement, so things move uniformly at any framerate, e.g.:

Puoi utilizzare questo delta durante la progettazione del movimento, in modo che gli oggetti si muovano uniformemente a qualsiasi framerate, ad esempio:

```js
this.x += 10 * ct.delta;
```

But this delta is mostly useful while designing complex or logic-driven movement, as [the default movement system](ct.templates.html#moving-copies-around) already takes `ct.delta` into account.

Comuque questo valore delta è per lo più utile durante la progettazione di movimenti complessi o basati sulla logica, poiché [il sistema di movimento predefinito](ct.templates.html#moving-copies-around) tiene già conto di `ct.delta`.

### `ct.deltaUi`

`ct.deltaUi` is similar to `ct.delta`, but it ignores time scaling factors that can happen during slow-mo effects or game pause (see ["Pausing a game"](game-pause.html) for examples).

`ct.deltaUi` è simile a `ct.delta`, ma ignora i fattori di ridimensionamento del tempo che possono verificarsi durante effetti al rallentatore o durante una pausa del gioco (consulta ["Sospendere un gioco"](game-pause.html) per gli esempi).
