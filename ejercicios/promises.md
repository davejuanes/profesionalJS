# Promises

API para consumo de Peliculas, The Movie DB

Dentro de las promesas cada función debe tener la keyword `async` por delante de la keyword `function`, una vez que invocamos esta palabra nos permite utilizar la siguiente keyword `await`.

En la siguiente función `getTopMoviesIds`, le darémos un argumento de `n = 3`, esto limitará la cantidad de peliculas. Ahora definiremos una constante que contendrá los ids de las películas:

```
const popularMovies = await getPopularMovies();
```

Ahora, necesitamos cortar ese `array` para solamente obtener 3 películas:

```
const ids = popularMovies.slice(0, n).map(movie => movie.id);
```

En la constante `ids` con `slice(0, n)` cortamos el array de 0 a n y luego con `map(movie => movie.id)` tomamos cada `movie` y de cada elemento retornamos el id (movie.id) convertimos el `array` de objetos en un `array` de ids.

Y finalmente retornamos el nuevo `array`:

```
return ids;
```

Finalemente tenemos esta función asincrona:

```
async function getTopMoviesIds(n = 3) {
    const popularMovies = await getPopularMovies();
    const ids = popularMovies.slice(0, n).map(movie => movie.id);
    return ids;
}
```

### Función

De la misma manera utilizamos una función `getPopularMovies()` que nos sirve para consumir la API solo de las peliculas mas populares, entonces debemos definirla para un uso asincrono:

```
asyn function getPopularMovies() {}
```

Dentro de la función definimos una constante del endpoint:

```
const url = `https://api.themoviedb.org/3/discover/movie?sort_by=popularity.desc&api_key=${apiKey}`;
```

Ahora para esperar la respuesta se necesita una constante con el metodo fetch:

```
const response = await fetch(url);
```

La constante `response`, no devuelve los datos legibles por lo tanto debemos convertirlo a formato `JSON` y retornar su respuesta:

```
const data = await response.json();
return data.results;
```

Se devuelve `results` porque si vamos al Browser dentro de Network -> Preview en el objeto y desplegamos `results`, podemos ver todas las peliculas.

Para poder consumir todos los detalles de las peliculas, utilizaremos la función `getMovie` de la misma manera que `getPopularMovies`:

```
const url = `https://api.themoviedb.org/3/movie/${id}?api_key=${apiKey}`;
const response = await fetch(url);
const data = await response.json();
return data;
```

Lo que hicimos en esta función es lo mismo, definimos un endpoint para consumo de información de API, mediante una constante esperamos con `await` y el metodo `fetch` la variable `url`, con la variable `data` convertimos la respuesta en formato `JSON` y finalmente retornamos `data`.

## Ejecución de funciones en Secuencia, Paralelo o como un Carrera

Para esto tenemos codígo HTML con 3 botones:

```
<ul>
    <li><button id="sequence">Get Top Movies in Sequence</button></li>
    <li><button id="parallel">Get Top Movies in Parallel</button></li>
    <li><button id="fastest">Get Fastest Top Movie</button></li>
</ul>

<ul id="movies"></ul>
```

Entonces, para funcionar utilizaremos la función `getTopMoviesInSequence`:

```
async function getTopMoviesInSequence() {}
```

Y definimos una constante para esperar las 3 películas Top que nos responde `getTopMoviesIds()`

```
const ids = await getTopMoviesIds();
```

Ahora necesitamos crear un arreglo vacío donde almacenaremos las 3 peliculas:

```
const movies = []
```

Para llenar los datos en el `array` con `forOf` recorremos y obtenemos los datos de las peliculas con `getMovie(id)` en base al id, finalmente insertamos los datos de cada película con `movies.push(movie)`:

```
for (const id of ids) {
    const movie = await getMovie(id)
    movies.push(movie)
}
return movies;
```

Para renderizar el `front` modificamos el `DOM`:

```
document.getElementById('sequence').onclick = async function() {
    const movies = await getTopMoviesInSequence();
    renderMovies(movies);
};
```

Aquí, con el evento `onclick` ejecutamos una función anónima, que buscará en `getTopMoviesInSequence()` las 3 películas con más votos para finalmente ejecutar la funcíon `renderMovies(movies)`.

```
function renderMovies(movies) {
    const movieList = document.getElementById('movies');
    movieList.innerHTML = '';

    movies.forEach(movie => {
        const listItem = document.createElement('li');
        listItem.innerHTML = `
        <img src="https://image.tmdb.org/t/p/w342${movie.poster_path}" />
        <h5>${movie.title}</h5>
        <p>Released on <em>${movie.release_date}</em></p>
        `;

        movieList.appendChild(listItem);
    });
}
```
### Función en Paralelo
Mediante la función ```getTopMoviesInParallel()``` esperaremos la promesa de los ids de peliculas con ```getTopMoviesIds``` mediante una constante ```ìds```
```
async function getTopMoviesInParallel() {
    const ids = await getTopMoviesIds()
}
```
Luego, convertiremos este array ```ids``` en promesas:
```
const moviePromises = ids.map(id => getMovie(id))
```
Ahora necesitamos una constante que almacene todas las promesas de la lista de ```ids```, para esto usamos ```Promise.all``` que contendrá todos los datos de las peliculas:
```
const movies = await Promise.all(moviePromises)
return movies;
```
Para terminar esta acción renderizamos el resultado con el botón correspondiente:
```
document.getElementById('parallel').onclick = async function() {
    const movies = await getTopMoviesInParallel();
    renderMovies(movies);
};
```
### La petición mas rapida 'fastest'
En esta petición vamos a obtener los ids con de igual manera pero ahora solamente mostraremos la primera que se cumpla:
```
async function getFastestTopMovie() {
    const ids = await getTopMoviesIds();
    const moviePromises = ids.map(id => getMovie(id));

    const movie = await Promise.race(moviePromises);
    return movie;
}
```
Entonces, definimos la constante ids para las promesas de ```getTopMoviesIds()```:
```
const ids = await getTopMoviesIds();
```
Luego necesitamos la misma lista de la anterior petición:
```
const moviePromises = ids.map(id => getMovie(id));
```
Y finalmente necesitamos la primera que gane la petición:
```
const movie = await Promise.race(moviePromises);
return movie;
```