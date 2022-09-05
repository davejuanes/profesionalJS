# Generators
Los generadores son funciones especiales, estos pueden comenzar su ejecución y detenerlos a la mitad y el programa seguirá su funcionamiento, volvemos al generador y continuar su ejecución donde la dejamos, lo interesante es que los generadores recuerdan sus variables y su scope.

Para definir un generador debemos utilizar ```*``` luego de la ```keyWord function``` de esta manera ```function*``` luego ya definimos la funcion normal:
```
function* simpleGenerator () {
    console.log('GENERATOR START');
    console.log('GENERATOR END');
}

const gen = simpleGenerator()
```
Ya dentro de la consola del browser, si ejecutamos ```gen.next()``` ejecutará nuestro generador, con los mensajes por consola:
```
GENERATOR START
GENERATOR END
```
Dentro del mensaje por consola, podemos ver dos propiedades
```
value: undefined, done: true
```
Value nos lo muestra como indefinido porque no tenemos algún valor dentro y done significa que el constructor termino la ejecución.

En que caso podemos tener valor definido, para esto utilizamos la ```keyWord yield```
 (ceder), para ver el ejemplo:
 ```
 function* simpleGenerator() {
    console.log('GENERATOR START');
    yield 1;
    yield 2;
    yield 3;
    console.log('GENERATOR END');
}
 ```
 La primera ejecución llegará al primer ```yield``` y retornada el valor 1 y continuará hasta el mensaje GENERATOR END donde ```done``` sera true.

 ## Generadores infinitos
 Ahora haremos un generador de ```ids```:
 ```
 function* idMaker() {
    let id = 1;
    while(true) {
        yield id;
        id = id + 1;
    }
 }
 ```
 Esta función lo que hará es ingrementar el id cada vez que ejecutemos nuestro generador.

 Ahora, podemos enviar valores a nuestro generador para poder restablecer el valor del id:
 ```
 function* idMakerWithReset() {
    let id = 1;
    let reset
    while(true) {
        reset = yield id;
        if (reset) {
            id = 1
        } else {
            id = id + 1;
        }
    }
 }
 ```
 La función ```idMakerWithReset()``` lo que hace es definir una variable local reset que cuando le pasemos el valor true a la funcion con yield restablecerá el valor del id a 1.
 
 ## La secuencia Fibonacci
 La secuencia fibonacci es 0, 1, 1, 2, 3, 5, 8 para poder generar la secuencia con generadores definimos la siguiente función:
 ```
 function* fibonacci() {
    let a = 1;
    let b = 1;
    while(true) {
        const nextNumber = a + b
        a = b
        b = nextNumber
        yield nextNumber
    }
 }
 ```