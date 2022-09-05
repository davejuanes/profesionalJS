# Proxy JavaScript
El proxy es de las caracteristicas mas nuevas de JS, con este podemos interceptar muchas cosas que con ```getters and setters```.
Dentro de la documentación de MDN, podemos buscar Method of the handler object, metodos del objeto que va a trabajar con los interceptores.
El funcionamiento de ```proxy``` es por ejemplo realizar una recomendación cuando ejecutamos comandos por terminal que no son exactos:
```
-> git addd
La terminal, nos enviará un mensaje donde diga:
git addd no existe
Quisiste decir:
-> git add?
```
## Ejercicio práctico
Para poder comprender mejor este concepto, utilizaremos la librería ```levenshtein.js```, esta libreria funciona buscando coincidencias en cuanto a caracteres:
Dentro de console del browser:
```
window.Levenshtein.get("JavaScript","Java")
```
Output:
```
6
```
Esto quiere decirnos que entre JavaScript y Java hay 6 caracteres que los diferencian, en otro ejemplo:
```
window.Levenshtein.get("Script","Scripd")
```
Output:
```
1
```
La diferencia solo es solo 1 caracter.

En un ejemplo mucho mas práctico definiremos una constante para invocar a ```Proxy```:
```
const p = new Proxy(target, handler)
```
Aquí, tarjet es el objeto que queremos intercepatar. Ahora definiremos nuestro objeto de colores:
```
const target = {
    red: 'Rojo',
    green: 'Verde',
    blue: 'Azul',
}
```
Si nos equivocamos buscando estas propiedades queremos que se nos corrija, para eso creamos nuestro handler (trampas) en este caso ```get```:
```
const handler = {
    get(obj, prop){
        if (prop in obj) {
            return obj[prop]
        }
    }
}
```
El handler requiere el objeto como tal y la propiedad a buscar, para esto definiremos una constante sugerencia que primero obtendra las llaves del objeto y buscará la llave con 3 caracteres parecidos:
```
const suggestion = Object.keys(obj).find(key => {
    return Levenshtein.get(key, prop) <= 3
})
```
Para mostrar la sugerencia verificamos que exista (suggestion == true) y lo enviamos por consola:
```
if (suggestion) {
    console.log(`${prop}, no se encontró. Quisiste decir ${suggestion}?`)
}
```
Ahora hay que regresar el objeto
```
return obj[prop]
```