# Getters y Setters

Uno de los features seguros de JS son estos, que al final son objeto virtuales, no existen directamente pero a travéz de ellos podemos correr funciones y obtener nuevos valores.

En el caso de el archivo ``` MediaPlayer.js ```, tenemos una function anonima dentro de ```MediPlayer.prototype.__initPlugins``` que gestiona el uso de los plugin pero no posee un control exacto de las mismas ni de los parametros que se le envian. Para esto haremos una modificación:
Dentro de ```MediPlayer.prototype.__initPlugins``` crearemos una constante:
```
const player = {
    play: () => this.play(),
    pause: () => this.pause(),
}
```
Y enviaremos esta constante a la ejecución de las funciones:
```
this.plugins.forEach(plugin => {
    plugin.run(player);
});
```
Ahora para obtener el valor que requerimos usaremos la ```get``` para ejecutar la funcion ```muted()``` dentro de la constante player:
```
get muted() {
    return this.media.muted;
}
```
Hasta este punto, lo que falta es que el elemento ```this``` no será invocado por el objeto ```player```, para solucionar esto tenemos que crear una constante que invoque a ```media``` desde ```player```:
```
media: this.media,
get muted() {
    return this.media.muted;
}
```
Ahora bien, en autoplay invocaremos el pluggin ```player.muted``` con una condicional para verificar que muted esté activado:
```
AutoPlay.prototype.run = function(player) {
    if (!player.muted) {
        player.mute = true;
    }
    player.play();
};
```
Dentro de ```MediaPlayer.js``` debemos crear un ```setter``` que devolvera un valor:
```
set muted(value) {
    this.media.muted = value;
}
```