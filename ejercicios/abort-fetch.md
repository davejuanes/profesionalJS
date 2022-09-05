# Abort fetch
Fetch se invento para poder optimizar las peticiones XML, un inconveniete de fetch es que una vez enviada la petición no había manera de detenerla.
A travéz de esto llega abort controller, AbortController es un simple objeto que genera un evento abort sobre su propiedad signal cuando el método abort() es llamado (y también establece signal. aborted en true ). fetch está integrado con él: pasamos la propiedad signal como opción, y entonces fetch la escucha, así se vuelve posible abortar fetch.

En nuestro browser en los devtools -> Network o Red hay un apartado con el simbolo de WiFi donde podemos definir la velocidad que podría tener el usuario y ver la calidad de sistema que estamos desarrollando en el uso de ```AbortController```.

En el documento ```HTML```, tenemos los elementos para un petición de una imagen, esta imagen tiene un peso considerable pero en caso de tener mala conexión de internet el usuario no podrá cargar la imagen de manera eficiente.

Para esto utilizaremos AbortController, asi nosotros podrémos anular la petición atravéz de una función ejectutada en el boton Stop Fetching.

