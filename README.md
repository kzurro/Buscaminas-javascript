# Buscaminas javascript

Buscaminas en Javascript realizado para la asignatura de DWEC para el ciclo de DAW.

### [Enlace al juego](https://rafasojo.github.io/Buscaminas-javascript/buscaminas.html)

Las instrucciones que recibo para hacer el buscaminas son:

Juega al buscaminas y analiza en qué consiste el juego:

* Según el nivel que elijas, tu campo de minas tiene unas dimensiones con un número de minas concreto. 
* Al iniciar todo el campo está cubierto. Se pueden realizar las siguientes acciones:
    * Botón izquierdo del ratón: se levanta la casilla. 
        * Si es una mina se pierde el juego. 
        * Si no lo es te indica el número de minas que hay alrededor (1, 2... 8). En caso de no haber ninguna mina alrededor, el juego despeja las casillas colindantes de forma recurrente.
    * Botón derecho: Se marca/desmarca una mina con bandera.
        * Si la casilla no tiene bandera, entonces se marca.
        * Si la casilla sí tiene bandera, entonces de desmarca.
    * Botón izquierdo y derecho: Se intenta destapar aquellas casillas de alrededor a una ya destapada-
        * Si están marcadas las minas de alrededor de forma correcta, se despejan las casillas de alrededor.
        * Si falta alguna mina por marcar, se indican las casillas mediante un parpadeo.
* Al iniciarse el juego se pone en marcha el temporizador. En caso de superarse el récord, el juego te lo indica.
* Al iniciarse el juego aparece un contador con las minas del campo. Conforme se marca/desmarca una mina, el contador se actualiza.
Aprende a jugar para implementarlo mediante JavaScript. 


## Animaciones

### Click
Cuando se hace un click (o doble) y se despeja una casilla, lo que hace es un effecto de *puff* y en el callback cuando termina se hace un *fadeIn* y se escribe el valor correspondiente.
```javascript
$casilla.addClass('casillaDescubierta').effect('puff', {}, contadorAnimaciones, function () {
                $(this).fadeIn(300).html((casillaDatos.valorMostrar == '0') ? '' : casillaDatos.valorMostrar);
            });
```

### Click derecho (bandera)
Cuando se coloca o quita una bandera lo que se hace es añadir o quitar una clase y también un efecto de *subrayado* para que el contraste sea mayor. 

```javascript
$casilla.effect('highlight', 200);
```

### Click ambos botones (despejar)
Cuando se hace doble click y el número de banderas no coincide con el de minas hace que parpadeen las casillas que se pueden marcar.
Uso el efecto pulsate de Jquery UI y le añado la opción para que sólo parpadee 3 veces y durante medio segundo.

```javascript
function parpadeaCasillas(casillas) {
    casillas.forEach(element => {
        $casilla = $('#' + element.x + '-' + element.y).effect('pulsate', {'times':3} ,500);
    });
}
```

### Ganar
La estructura de los efectos es muy parecida a cuando se despejan casillas pero el efecto que se usa es el *shake* con 1 segundo de más de retardo igual que el fadeIn.
```javascript
$casilla.addClass('casillaDescubierta').effect('shake', {}, contadorAnimaciones + 1000, function () {
     $(this).fadeIn(1000).html(casillaDatos.valorMostrar).addClass('mina').addClass('minaGanada');
});
```

### Perder
Cuando se pierde se añade la clase *casillaDescubierta* y *mina*, ambas con un contador incremental para el tiempo.
Además, dentro de la mina hay un span el cual tiene un *animate* que le aumenta el tamaño y después de lo que tarda más 300 milisegundos desaparece.
```javascript
$casilla.addClass('casillaDescubierta', contadorAnimaciones)
    .addClass('mina', contadorAnimaciones);
$('span', $casilla)
    .animate({
            'font-size': '3em'
        }, contadorAnimaciones)
    .fadeOut(contadorAnimaciones + 300);
```

Además, la función de perder tiene el sync-await para retrasar la ejecución para que cuando salte el diálogo de acabar la partida los efectos estén prácticamente terminados.

```javascript
await sleep(contadorAnimaciones + 1500);
```
