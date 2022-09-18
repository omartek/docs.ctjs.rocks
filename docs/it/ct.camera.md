# ct.camera

::: tip Ehi,

questa pagina descrive i metodi ei parametri dell'oggetti `ct.camera` sotto forma di riferimenti. Puoi capire meglio le tecniche e l'utilizzo in modo pratico nella [pagina "Lavorare con le viste."](/viewport-management.html) 
:::

## Geometria della telecamera

### `ct.camera.x`, `ct.camera.y`

Le coordinate reali x e y della telecamera. Non ha l'effetto di vibrazione dello schermo, così come potrebbe succedere con `targetX` e `targetY` se la telecamera è in transizione.

### `ct.camera.targetX` and `ct.camera.targetY`

Le coordinate x e y della posizione di destinazione. Spostandolo invece di usare semplicemente il parametro `x`/ `y` attiverà l'effetto deriva.

### `ct.camera.computedX`, `ct.camera.computedY`

La posizione risultante della telecamera nelle coordinate di gioco. Queste hanno il tremolio dello schermo e `ct.camera.shiftX`, `ct.camera.shiftY` applicato.

### `ct.camera.width`, `ct.camera.height`

La larghezza e l'altezza della telecamera della regione mostrata non ridimensionata. Utilizzare `ct.camera.scale` per ottenere una versione ridimensionata. Per modificare questi valori, confronta le proprietà `ct.width`e `ct.height`.

### `ct.camera.rotation`

Un valore in gradi che ruota la telecamera.

### `ct.camera.scale.x`, `ct.camera.scale.y`

Un valore scalare che ridimensiona l'inquadratura. Se confrontato con gli strumenti di visualizzazione delle immagini, `1` e  `1` significa nessun ridimensionamento, `0.5` ingrandirà fino al 200%, `3` rimpicciolirà producendo un rimpicciolimento del 33%.

### `ct.camera.left`, `ct.camera.top`, `ct.camera.right` and `ct.camera.bottom`

Questi rappresentano la posizione risultante di un particolare lato della telecamera nelle unità di gioco. Non possono essere modificati manualmente.

### `ct.camera.moveTo(x, y)` and `ct.camera.teleportTo(x, y)`

Entrambi spostano la telecamera in una nuova posizione. `ct.camera.moveTo` è utile per filmati e transizioni fluide tra oggetti, poiché funziona con `ct.camera.drift`. `ct.camera.teleportTo` non provoca transizioni e resetta gli effetti di vibrazione dello schermo. È utile per modifiche istantanee precise, ad esempio quando si sposta una fotocamera in una posizione lontana.

### `ct.camera.uiToGameCoord(x, y)` and `ct.camera.gameToUiCoord(x, y)`

Converti un punto da uno spazio di coordinate a un altro. Restituisce un oggetto (`PIXI.Point`) con due proprietà: le componenti `x` e `y`.

Ci sono anche `ct.u.uiToGameCoord` e `ct.u.gameToUiCoord`, che chiamano questi metodi dell'oggetto corrente `ct.camera`.

### `ct.camera.getTopLeftCorner()`, `ct.camera.getTopRightCorner()`, `ct.camera.getBottomLeftCorner()`, `ct.camera.getBottomRightCorner()`

Restituisce un oggetto (`PIXI.Point`) con due proprietà: le componenti `x` e `y`. Questi sono espressi nelle coordinate di gioco e tengono conto della rotazione e del ridimensionamento.

### `ct.camera.getBoundingBox()`

Restituisce un riquadro di delimitazione della telecamera, nelle coordinate di gioco. Confronta [PIXI.Rectangle](https://pixijs.download/release/docs/PIXI.Rectangle.html) per le sue proprietà.

## Seguire una copia

### `ct.camera.follow`

Se impostata, la fotocamera seguirà la copia dato (l'oggetto grafico o sprite giocatore).

### `ct.camera.followX`, `ct.camera.followY`

Funziona se `follow` è impostato su una copia. L'impostazione di uno di questi su `false` disabiliterà la telecamera automatica in una determinata direzione.

### `ct.camera.borderX`, `ct.camera.borderY`

Funziona se `follow` è impostato su una copia. Imposta la cornice all'interno della quale verrà mantenuta la copia, nel sistema di coordinate dell'interfaccia utente. Può essere impostato su `null` così che la copia sia posizionata al centro dello schermo.

## Oscillazione e scuotimento dello schermo

### `ct.camera.shake`

Il valore dell'effetto di vibrazione dello schermo, relativa al lato massimo dello schermo (100 è il 100% della vibrazione dello schermo). Se impostato su 0 o meno, disabilita l'effetto.

### `ct.camera.shakePhase`

La fase corrente del tremolio dello schermo.

### `ct.camera.shakeDecay`

La quantità di `shake` sottratte in un secondo. Il valore predefinito è 5.

### `ct.camera.shakeFrequency`

La frequenza di base dell'effetto di vibrazione dello schermo. Il valore predefinito è 50.

### `ct.camera.shakeX`, `ct.camera.shakeY`

Un moltiplicatore applicato all'effetto di vibrazione dello schermo. Il valore predefinito è 1.

### `ct.camera.shakeMax`

Il valore massimo possibile per la proprietà `shake` per proteggere i giocatori dalla perdita del monitor, in unità `shake`. Il valore predefinito è 10.

### `ct.camera.minX`, `ct.camera.maxX`, `ct.camera.minY`, and `ct.camera.maxY`

Questi fanno muovere la telecamera solo all'interno di un rettangolo specifico. Per impostazione predefinita, la telecamera può muoversi illimitatamente, a meno che non sia impostato diversamente all'interno dell'editor stanza.

È possibile impostare tutte queste proprietà o solo alcune di esse.

Per annullare l'impostazione di uno di questi valori, utilizza, ad esempio `delete ct.camera.minX;`, o scrivi `ct.camera.minX = undefined;`.

## Altro

### `ct.camera.drift`

Se impostato su un valore compreso tra 0 e 1, renderà più fluido il movimento della telecamera.

### `ct.camera.shiftX`, `ct.camera.shiftY`

Questi spostano la fotocamera nelle unità dell'interfaccia utente, ma non modificano `ct.camera.x` o `ct.camera.y`.

### `ct.camera.realign(room)`

Riallinea tutte le copie in una stanza in base alle nuove dimensioni di una telecamera. È utile per il posizionamento rapido degli elementi dell'interfaccia utente su schermate diverse. La nuova posizione è il risultato dell'interpolazione basata sui parametri `xstart` e `ystart` delle copie, quindi non funzionerà con elementi in movimento. Puoi ignorare il riallineamento per alcune copie se imposti il loro parametro `skipRealign` su `true`.

Il metodo è generalmente applicabile solo alle modalità "Expand" e "Scaling without letterboxing" di fittoscreen.

No `ct.camera.realign` | With `ct.camera.realign`
-|-
![Gli elementi dell'interfaccia utente vengono ridimensionati, ma vengono spostati se le proporzioni dello schermo cambiano](ctCameraAlign_notIncluded.gif) | ![Gli elementi dell'interfaccia utente sono ridimensionati e distribuiti uniformemente sullo schermo](ctCameraAlign_included.gif) 

#### Esempio: riallineare gli elementi dell'interfaccia utente in una stanza

Codice "Frame end" della tua interfaccia utente:

```js
ct.camera.realign(this);
```

Sì, è tutto!

### `ct.camera.manageStage()`

Questo allineerà tutti i livelli non dell'interfaccia utente nel gioco in base alle trasformazioni della telecamera. Questo viene automaticamente chiamato internamente e non lo utilizzerai quasi mai.

## Generazione di una nuova istanza di una telecamera

Puoi creare un nuovo oggetto telecamera per le tue esigenze chiamando un costruttore dell'oggetto `Camera`:

```js
const oldCamera = ct.camera;
ct.camera = new Camera(0, 0, 1024, 768);
```
