# ct.backgrounds

`ct.backgrounds` ha alcune API per operare con gli oggetti [`Background`](Background.md) objects.

## Metodi e proprietà

### `ct.backgrounds.list['TextureName']`

Contiene una matrice di tutti gli sfondi della texture corrente del livello. L'array per questo o quel nome di texture potrebbe non esistere se non ci sono ancora background, quindi potrebbe essere necessario verificare se l'array stesso esiste prima di utilizzare uno dei suoi elementi.

#### Esempio: ottenere il primo sfondo della texture `bg_sand` e renderlo più scuro

```js
if (ct.backgrounds.list['BG_Sand']) {
    const bg = ct.backgrounds.list['BG_Sand'][0];
    bg.tint = 0x999999;
}
```

### `ct.backgrounds.add(texName, frame, depth, container)`

Argument | Type | Description
-|-|-
`texName` | `string` | Il nome di una texture da usare come sfondo
`frame` | `number` | *(facoltativo)* L'indice del frame da utilizzare. Per default è `0`.
`depth` | `number` | *(facoltativo)* La profondità a cui posizionare lo sfondo. Per default è `0`.
`container` | `PIXI.Container` | *(facoltativo)* Dove mettere lo sfondo. Per default è `ct.room`, ma puoi indicare qualsiasi altro livello o contenitore pixi valido.

**Returna** l'istanza [`Background`](Background.html) creata.

::: tip
Visita la [Documentazione per la classe `Background`](Background.html) per imparare a modificare la posizione, l'aspetto e gestire il movimento degli sfondi.
:::

#### Esempio: Creare uno sfondo, impostare la sua opacità e spostarlo orizzontalmente

```js
const bg = ct.backgrounds.add('BG_SkyClouds', 0, -1000);
bg.alpha = 0.5;
bg.movementX = 1;
```