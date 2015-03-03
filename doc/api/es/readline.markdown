# Readline

    Estabilidad: 2 - Inestable

Para usar este módulo haz `require('readline')`. Readline permite leer un stream
stream (como `process.stdin`) línea por línea.

Ten en cuenta que una utilices este módulo, tu programa node no acabará hasta hayas cerrado la interfaz. He aquí cómo hacer que tu programa acabe elegantemente:

    var readline = require('readline');

    var rl = readline.createInterface({
      input: process.stdin,
      output: process.stdout
    });

    rl.question("¿Qué piensas de node.js? ", function(answer) {
      // TODO: Escribir la respuesta en una base de datos
      console.log("Gracias por sus valiosos comentarios:", answer);
      rl.close();
    });

## readline.createInterface(options)

Crea una instancia `Iterface` readline. Acepta un objecto "opciones" que toma los siguientes valores:

 - `input` - el stream leíble al que escuchar (Required).

 - `output` - el stream escribible al que escribir datos readline (Opcional).

 - `completer` - una función opcional que es usada para autocompletar comandos al usar el Tabulador. Ver abajo un ejemplo de cómo usar esto.

 - `terminal` - pasa `true` si los streams `input` y `output` deben de ser tratados como TTY, y han de escribirsele códigos de escape ANSI/VT100.
   Por defecto se comprueba `isTTY` en el stream `output` una vez se está haciendo la instancia.

A la función `completer` se le proporciona la línea que el usuario ha introducido, y se supone que ha de devolver un Array con dos elementos:

 1. Un Array con el autocompletado.

 2. The substring que se usó para obtener el Array anterior.

Lo que termina siendo algo como:
`[[substr1, substr2, ...], substringoriginal]`.

Ejemplo:

    function completer(line) {
      var completions = '.help .error .exit .quit .q'.split(' ')
      var hits = completions.filter(function(c) { return c.indexOf(line) == 0 })
      // mostrar todas las posibilidades si no se encuentra ninguna
      return [hits.length ? hits : completions, line]
    }

Además `completer` puede ser llamado en modo asíncrona si acepta dos argumentos:

    function completer(linePartial, callback) {
      callback(null, [['123'], linePartial]);
    }

`createInterface` se usa comúnmente con `process.stdin` y
`process.stdout` para aceptar entrada de usuario:

    var readline = require('readline');
    var rl = readline.createInterface({
      input: process.stdin,
      output: process.stdout
    });

Una vez que tienes una instancia readline, lo más común es escuchar al evento `"line"`.

Si `terminal` es `true` para esta instancia, el stream `output` obtendrá la mayor compatibilidad si define una propiedad `output.columns` y emite un evento `"resize"` en el `output` si/cuando la columnas cambian en algún momento
(`process.stdout` hace esto automáticamente cuando es un TTY).

## Clase: Interface

La clase que representa una interfaz readline con unos streams entrada y salida (input y output).

### rl.setPrompt(prompt)

Establece el prompt. Por ejemplo, cuando ejecutas `node` en la línea de comandos, verás `> `, que es el prompt de node.

### rl.prompt([preserveCursor])

Prepara readline a tener entrada (input) del usuario, dejando las opciones de `setPrompt` en ese momento en una nueva línea, dando al usuario un nuevo lugar donde escribir. Usa `preserveCursor` con el valor `true` para prevenir que la posición del cursor sea reestablecida a `0`.

Esto también resume el stream `input` usado con `createInterface` si este estaba pausado.

Si `output` tiene el valor `null` o `undefined` al llamar `createInterface`, el
prompt no se escribe.

### rl.question(query, callback)

Antepone el prompt con `query` e invoca `callback` con la respuesta dada por el usuario. Muestra la pregunta al usuario, y luego invoca `callback`
con la respuesta del usuario después de que haya sido escrita.

Esto también resume el stream `input` usado con `createInterface` si había sido pausado.

Si `output` tiene el valor `null` o `undefined` al llamar `createInterface`,
no se mostrará nada.

Ejemplo de uso:

    interface.question('¿Cual es tu comida favorita?', function(answer) {
      console.log('Ah, así que tu comida favoria es' + answer);
    });

### rl.pause()

Pausa el stream input `input`, permitiendo que este sea resumido más tarde si es necesario.

Nota que esto no pausa inmediatamente el flujo de eventos. Varios eventos pueden ser emitidos después de llamar `pause`, `line` inclusive.

### rl.resume()

Resume el stream `input` de readline.

### rl.close()

Cierra la instancia `Interface`, renunciando al control de los streams `input` y
`output`. También se emitirá un evento "close".

### rl.write(data[, key])

Escribe `data` al stream `output`, a no ser que `output` tenga el valor `null` o
`undefined` cuando se llamó `createInterface`. `key` es un objeto literal representado una secucia de tecla; disponible si el terminal es un TTY.

Esto también resume el stream `input` si estaba pausado.

Example:

    rl.write('Delete me!');
    // Simular ctrl+u para eliminar la línea escrita anteriormente
    rl.write(null, {ctrl: true, name: 'u'});

## Eventos

### Evento: 'line'

`function (line) {}`

Emitido siempre que el stream `input` recibe un `\n`, usualmente recivido cuando el usuario presiona enter, o return. Es un buen lugar donde escuchar a la entrada del usuario.

Ejemplo de escucha para `line`:

    rl.on('line', function (cmd) {
      console.log('Acabas de escribir: '+cmd);
    });

### Evento: 'pause'

`function () {}`

Emitido siempre que el stream `input`.

También se emite cuando el stream `input` no está pausado y recibe el evento
`SIGCONT`. (Ver eventos `SIGTSTP` y `SIGCONT`)

Ejemplo de esucha para `pause`:

    rl.on('pause', function() {
      console.log('Readline ha sido pausado.');
    });

### Evento: 'resume'

`function () {}`

Emitido siempre que el stream `input` se ha resumido.

Ejemplo de escucha para `resume`:

    rl.on('resume', function() {
      console.log('Readline ha sido resumido.');
    });

### Evento: 'close'

`function () {}`

Emitido cuando se llama `close()`.

También se emite cuando el stream `input` recibe su evento "end". La instancia `Interface` debe de considerarse "terminada" una vez este evento es emitido. Por ejemplo, cuando el stream `input` recibe `^D`, conocido respectivamente como `EOT`.

Este evento también se llama si no hay un listener para el evento `SIGINT` cuando el stream `input` recibe un `^C`, conocido respectivamente como `SIGINT`.

### Evento: 'SIGINT'

`function () {}`

Se emite siempre que el stream `input` recibe `^C`, conocido respectivamente como `SIGINT`. Si no hay listener para el evento `SIGINT`, cuando el stream `input` recibe un `SIGINT`, `pause` será activado.

Ejemplo de escucha para `SIGINT`:

    rl.on('SIGINT', function() {
      rl.question('¿Estas seguro de que quieres salir?'
        if (answer.match(/^y(es)?$/i)) rl.pause();
      });
    });

### Evento: 'SIGTSTP'

`function () {}`

**Esto no funciona en Windows.**

Emitido cuando el stream `input` recive un `^Z`, respectivamente conocido como `SIGTSTP`. Si no hay listener para el evento `SIGTSTP` cuando el `input` recibe `SIGTSTP`, el programa se pondrá en el background.

Cuando el programa se resume con `fg`, los eventos `pause` y `SIGCONT` serán emitidos. Puedes usar cualquiera de los dos para resumir el stream.

Los eventos `pause` y `SIGCONT` no se activarán si los streams estaban pausados antes de que el programa fuera puesto en el background.

Ejemplo de escucha para `SIGTSTP`:

    rl.on('SIGTSTP', function() {
      // Esto sobreescribira SIGTSTP y evitará que el programa sea puesto en el
      // background.
      console.log('Se atrapó SIGTSTP.');
    });

### Evento: 'SIGCONT'

`function () {}`

  **Esto no funciona en Windows.**

Emitido cuando el stream `input` ha sido puesto en el background con `^Z`,
respectivamente conocido como `SIGTSTP`, y luego continuado con `fg(1)`. Este evento se emite si el stream no fue pausado antes de poner el programa en el background.

Ejemplo de escucha para `SIGCONT`:

    rl.on('SIGCONT', function() {
      // `prompt` resumirá automáticamente el stream
      rl.prompt();
    });


## Example: pequeña CLI

He aquí un ejemplo de como se puede usar todo en conjunto para constuir una pequeña interfaz de línea de comandos:

    var readline = require('readline'),
        rl = readline.createInterface(process.stdin, process.stdout);

    rl.setPrompt('OHAI> ');
    rl.prompt();

    rl.on('line', function(line) {
      switch(line.trim()) {
        case 'hola':
          console.log('¡mundo!');
          break;
        default:
          console.log('¿Cómo? Puede que haya escuchado `' + line.trim() + '`');
          break;
      }
      rl.prompt();
    }).on('close', function() {
      console.log('¡Que tengas un buen día!');
      process.exit(0);
    });

## readline.cursorTo(stream, x, y)

Mover el curor a la posición especificada en un stream TTY dado.

## readline.moveCursor(stream, dx, dy)

Mover el cursor relativo a su posición dada en un stream TTY dado.

## readline.clearLine(stream, dir)

Limpia la línea actual del stream TTY dado en una dirección específica.
`dir` debe de tener uno de los valores siguientes:

* `-1` - hacia la izquierda del cursor
* `1` - hacia la derecha del cursor
* `0` - linea completa

## readline.clearScreenDown(stream)

Limpia la pantalla desde la posción actual del cursor hacia abajo.
