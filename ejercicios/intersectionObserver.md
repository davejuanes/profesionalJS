# Intersection Observer
Este observa un elemento, que sele envia un objeto de configuracion.Lo que haremos es que en nuestro proyecto haremos que cuando el 25% de la pantalla quede restando Intersection Observer nos avisara.

```
class AutoPause {
    run(player) {
        const observer = new IntersectionObserver(this.handleIntersection, {
            threshold: 0.25
        })
        observer.observe(player.media)
    }
    handleIntersection(entries) {}
}
```
En el segmento de codigo es una clase donde le pasaron run con el argumento player, debemos crear un nuevo objeto IntersectionObserver donde este recibe dos parametros un handle (una trampa) y el threshold que nos dira el pocentaje del elemento para avisarnos.

Luego se tiene que poner a observar el objeto en este caso ```observer.observer``` estara observando al elemento ```player.media``` y nuestro contener será toda la pantalla.

Ahora para definifir los entries que en este caso son solamente uno, lo definimos con ```handleIntersection(entriees)``` y dentro definimos una constante pasandole la posicion 0 como un array