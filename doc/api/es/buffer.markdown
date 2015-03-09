# Buffer

    Estabilidad: 3 - Estable

JavaScript puro es amigable con Unicode pero no es amable con datos binarios.  Cuando estamos trabajando con streams TCP o el sistema de archivos, es necesario poder manipular streams en  octetos. Node tiene varias estrategias para manipular, crear y consumir streams en octetos.

Los datos en bruto se guardan en instancias de la clase `Buffer`. Un `Buffer` es similar a un array de enteros aunque correspondiente a asignación de memoria en bruto fuera del heap de V8. Un `Buffer` no se puede redimensionar.

`Buffer` es una clase global, haciendo que sea bastante infrecuente que uno necesite utilizar `require('buffer')`.

La conversión entre Buffers y strings en Javascript requiere de un método de codificación explícito. He aquí las distintas codificaciones de string.

* `'ascii'` - sólo para datos ASCII de 7 bits. Este método de codificación es muy rápido, y descartará el bit superior si se establece.

* `'utf8'` - Caracteres Unicode codificados en múltiples bytes. Muchas páginas web y otros formatos de documentos usan UTF-8.

* `'utf16le'` - 2 o 4 bytes, Unicode condificado mediante little-endian.
  Se soportan los pares suplentes (U+10000 a U+10FFFF).

* `'ucs2'` - Alias de `'utf16le'`.

* `'base64'` - Codificación string Base64.

* `'binary'` - Una forma de codificar datos binarios en bruto en strings usando solo los 8 primeros bits de cada carácter. Este método de codificación es obsoleto y debe ser evitado en favor de objetos `Buffer` siempre que sea posible. Esta codificación se eliminará en futuras versiones de Node.

* `'hex'` - Codifica cada byte a dos caracteres hexadecimales.

Crear un typed array a partir de un `Buffer` funciona con las siguientes advertencias:

1. La memoria del buffer es copiada, no compartida.

2. La memoria del buffer se interpreta como un array, no un byte array. Es decir, `new Uint32Array(new Buffer([1,2,3,4]))` crea un `Uint32Array` de 4 elementos `[1,2,3,4]`, no un `Uint32Array` con un sólo elemento
   `[0x1020304]` o `[0x4030201]`.

NOTE: Node.js v0.8 simplemente mantenía una referencia a el buffer en  `array.buffer` en lugar de clonarlo.

Aunque más eficiente, introduce incompatibilidades sutiles con la especificación de typed arrays.  `ArrayBuffer#slice()` hace una copia del "slice" mientras que
`Buffer#slice()` crea una vista.

## Clase: Buffer

La clase Buffer es un typo global para tratar directamente con datos binaros.
Puede ser construida de distintas formas.

### new Buffer(tamaño)

* `tamaño` Numero

Asigna un nuevo buffer en octetos `tamaño`. Nota, `tamaño` no ha de tener un tamaño mayor a
[kMaxLength](smalloc.html#smalloc_smalloc_kmaxlength). En caso contrario, un `RangeError` se lanzará aquí.

### new Buffer(array)

* `array` Array

Asigna un nuevo buffer usando un `array` de octetos.

### new Buffer(buffer)

* `buffer` {Buffer}

Copia los datos del `buffer` pasados a una nueva istancia `Buffer`.

### new Buffer(str[, codificado])

* `str` String - string a codificar.
* `codificado` String - codifcado a use, Opcional.

Asigna un nuevo buffer conteniendo `str`. Por defecto `codificado` es `'utf8'`.

### Método de Clase: Buffer.isEncoding(codificado)

* `codificado` {String} string codificado a probar

Devuelve true si el `codificado` es un argumento de codificado válido, o false en caso contrario.

### Método de Clase: Buffer.isBuffer(obj)

* `obj` Objeto
* Devuelve: Boolean

Prueba si `obj` es un `Buffer`.

### Método de Clase: Buffer.byteLength(string[, codificado])

* `string` String
* `codificado` String, Opcional, por defecto: 'utf8'
* Devuelve: Number

Da la longtiud real en bytes para el argumento string. Por defecto, `codificado` es `'utf8'`. Este valor no es el mismo que `String.prototype.length` ya que este último devuelve el número de *caractéres* en una string.

Ejemplo:

    str = '\u00bd + \u00bc = \u00be';

    console.log(str + ": " + str.length + " caractéres, " +
      Buffer.byteLength(str, 'utf8') + " bytes");

    // ½ + ¼ = ¾: 9 caractéres, 12 bytes

### Método de Clase: Buffer.concat(lista[, longitudTotal])

* `lista` {Array} Lista de objetos Buffer a concatenar.
* `longitudTotal` {Number} Longtiud total de los buffers una vez concatenados.

Devuelve un buffer resultante al concatenar todos los buffers de la lista juntos.

Si la lista no tiene elementos, o si la longitudTotal es 0, entonces se devuleve un buffer de longitud cero.

Si la lista tiene exactamente un item, se devuelve el primero elemento de la lista.

Si la lista tiene más de un elemento, se crea un nuevo Buffer.

Si no se proporciona la longitudTotal, será leida de los buffers en la lista.
Sin embargo, esto añade un loop adicional a la función, así que será más rápido dar la longitud de forma explícita.

### Método de Clase: Buffer.compare(buf1, buf2)

* `buf1` {Buffer}
* `buf2` {Buffer}

Equivalente a [`buf1.compare(buf2)`](#buffer_buf_compare_otherbuffer). Útil para ordenar un Array de Buffers:

    var arr = [Buffer('1234'), Buffer('0123')];
    arr.sort(Buffer.compare);


### buf.length

* Number

El tamaño del buffer en bytes. Tengase en cuenta que no es necesariamente el tamaño del contenido. `length` hace referencia a la cantidad de memoria asignada para el objeto buffer. No cambia cuando el tamaño del contenido de los buffers cambia.

    buf = new Buffer(1234);

    console.log(buf.length);
    buf.write("string", 0, "ascii");
    console.log(buf.length);

    // 1234
    // 1234

### buf.write(string[, offset][, longitud][, codificado])

* `string` String - datos a ser escrito sen el buffer
* `offset` Number, Opcional, Por defecto: 0
* `longitud` Number, Opcional, Por defecto: `buffer.length - offset`
* `codificado` String, Opcional, Por defecto: 'utf8'

Escribe `string` en el buffer en el `offset` usando el codificado dado.
Por defecto `offset` es `0`, por defecto `codificado` es `'utf8'`. `longitud` es
el número de bytes a escribir. Devuelve el numero de octetos escritos. Si `buffer` no contiene el espacio suficiente para alojar a string por completo, se escribirá el tamaño parcial de string. Por defecto `longitud` es `buffer.length - offset`.
Este método no escribirá caractéres parciales.

    buf = new Buffer(256);
    len = buf.write('\u00bd + \u00bc = \u00be', 0);
    console.log(len + " bytes: " + buf.toString('utf8', 0, len));

### buf.writeUIntLE(valor, offset, byteLength[, noAssert])
### buf.writeUIntBE(valor, offset, byteLength[, noAssert])
### buf.writeIntLE(valor, offset, byteLength[, noAssert])
### buf.writeIntBE(valor, offset, byteLength[, noAssert])

* `valor` {Number} Bytes to be written to buffer
* `offset` {Number} `0 <= offset <= buf.length`
* `byteLength` {Number} `0 < byteLength <= 6`
* `noAssert` {Boolean} Por defecto: false
* Devuelve: {Number}

Escribe `valor` al buffer en el especificado `offset` y `byteLength`.
Soporta hasta 48 bits de precisión. Por ejemplo:

    var b = new Buffer(6);
    b.writeUIntBE(0x1234567890ab, 0, 6);
    // <Buffer 12 34 56 78 90 ab>

Si `noAssert` es `true` se pasará por alto la validación de `valor` y `offset`. Por defecto, este valor es `false`.

### buf.readUIntLE(offset, byteLength[, noAssert])
### buf.readUIntBE(offset, byteLength[, noAssert])
### buf.readIntLE(offset, byteLength[, noAssert])
### buf.readIntBE(offset, byteLength[, noAssert])

* `offset` {Number} `0 <= offset <= buf.length`
* `byteLength` {Number} `0 < byteLength <= 6`
* `noAssert` {Boolean} Por defecto: false
* Devuelve: {Number}

Una versión generalizada para todos los métodos de lectura numéricos. Soporta hasta 48 bits de precisión. Por ejemplo:

    var b = new Buffer(6);
    b.writeUint16LE(0x90ab, 0);
    b.writeUInt32LE(0x12345678, 2);
    b.readUIntLE(0, 6).toString(16);  // Especifica 6 bytes (48 bits)
    // output: '1234567890ab'

Si `noAssert` es `true` se pasará por alto la validación de `offset`. Esto significa que `offset` puede estar más haya del buffer. Por defecto es `false`.

### buf.toString([codificado][, principio][, fin])

* `codificado` String, Opcional, Por defecto: 'utf8'
* `principio` Number, Opcional, Por defecto: 0
* `fin` Number, Opcional, Por defecto: `buffer.length`

Decodifica y devuelve string desde datos buffer codificados con `codificado`
(por defecto `'utf8'`) empezando por `principio` (por defecto `0`) y acaba en
`fin` (defaults to `buffer.length`).

Véase el ejemplo anterior `buffer.write()`.

### buf.toJSON()

Devuelve una representación JSON de la instancia Buffer. `JSON.stringify`
implicitamente llama a esta función cuando stringifica a una instancia Buffer.

Ejemplo:

    var buf = new Buffer('test');
    var json = JSON.stringify(buf);

    console.log(json);
    // '{"type":"Buffer","data":[116,101,115,116]}'

    var copia = JSON.parse(json, function(key, valor) {
        return valor && valor.type === 'Buffer'
          ? new Buffer(valor.data)
          : valor;
      });

    console.log(copia);
    // <Buffer 74 65 73 74>

### buf[index]

<!--type=property-->
<!--name=[index]-->

Obtener o asignar el octeto Get and set the octet at `index`. Los valores referencia a bytes individuales, por lo que el valor legar está entre `0x00` y `0xFF` hexadecimal o `0` o `255`.

Ejemplo: copia ASCII string a buffer, byte a byte:

    str = "node.js";
    buf = new Buffer(str.length);

    for (var i = 0; i < str.length ; i++) {
      buf[i] = str.charCodeAt(i);
    }

    console.log(buf);

    // node.js

### buf.equals(otroBuffer)

* `otroBuffer` {Buffer}

Devuelve un boolean distinguiendo entre si `this` y `otroBuffer` tienen los mismos bytes.

### buf.compare(otroBuffer)

* `otroBuffer` {Buffer}

Devuelve un número indicando si `this` viene antes o después o es lo igual a `otroBuffer` en orden de calsificación.

### buf.copy(targetBuffer[, targetStart][, sourceStart][, sourceEnd])

* `targetBuffer` objeto Buffer - Buffer donde copiar
* `targetStart` Number, Opcional, Por defecto: 0
* `sourceStart` Number, Opcional, Por defecto: 0
* `sourceEnd` Number, Opcional, Por defecto: `buffer.length`

Hace una copia entre buffers. Las regiones origen y el destino pueden superponerse.
`targetStart` y `sourceStart` valen `0` por defecto.
Por defecto `sourceEnd` es igual a `buffer.length`.

Todos los valores pasados que son `undefined`/`NaN` o están fuera del límite son asignados sus valores por defecto respectivamente.

Ejemplo: construir dos Buffers, luego copiar `buf1` desde el byte 16 hasta el byte 19 en el `buf2`, empezando en el 8º byte en `buf2`.

    buf1 = new Buffer(26);
    buf2 = new Buffer(26);

    for (var i = 0 ; i < 26 ; i++) {
      buf1[i] = i + 97; // 97 es a en ASCII
      buf2[i] = 33; // ASCII !
    }

    buf1.copy(buf2, 8, 16, 20);
    console.log(buf2.toString('ascii', 0, 25));

    // !!!!!!!!qrst!!!!!!!!!!!!!


### buf.slice([comienzo][, final])

* `comienzo` Number, Opcional, Por defecto: 0
* `final` Number, Opcional, Por defecto: `buffer.length`

Devuelve un nuevo buffer el cual hace referencia a la misma memoria que el antiguio, pero siendo su offset recortado por los índices `comienzo` (por defecto `0`) y `final` (por defecto `buffer.length`). Índices negativos empiezan or el final del buffer.

**¡Modificar el nuevo recorte del buffer modificará la memoria del buffer original!**

Ejemplo: construye un Buffer con el alfabeto ASCII, tomando un recorte y luego modificando un byte del Buffer original.

    var buf1 = new Buffer(26);

    for (var i = 0 ; i < 26 ; i++) {
      buf1[i] = i + 97; // 97 is ASCII a
    }

    var buf2 = buf1.slice(0, 3);
    console.log(buf2.toString('ascii', 0, buf2.length));
    buf1[0] = 33;
    console.log(buf2.toString('ascii', 0, buf2.length));

    // abc
    // !bc

### buf.readUInt8(offset[, noAssert])

* `offset` Number
* `noAssert` Boolean, Opcional, Por defecto: false
* Devuelve: Number

Lee un entero de 8 bits sin signo de un buffer en el offset especificado.

Si `noAssert` es true la validación se pasará por alto desde `offset`. Esto significa que `offset` puede estar más allá del final del buffer. Es `false` por defecto.

Ejemplo:

    var buf = new Buffer(4);

    buf[0] = 0x3;
    buf[1] = 0x4;
    buf[2] = 0x23;
    buf[3] = 0x42;

    for (ii = 0; ii < buf.length; ii++) {
      console.log(buf.readUInt8(ii));
    }

    // 0x3
    // 0x4
    // 0x23
    // 0x42

### buf.readUInt16LE(offset[, noAssert])
### buf.readUInt16BE(offset[, noAssert])

* `offset` Number
* `noAssert` Boolean, Opcional, Por defecto: false
* Devuelve: Number

Lee un entero de 16 bits sin signo de un buffer en la posición especificada por el offset con el formato endiano especificado.

Si `noAssert` es true la validación se pasará por alto desde `offset`. Esto significa que `offset` puede estar más allá del final del buffer. Es `false` por defecto.

Ejemplo:

    var buf = new Buffer(4);

    buf[0] = 0x3;
    buf[1] = 0x4;
    buf[2] = 0x23;
    buf[3] = 0x42;

    console.log(buf.readUInt16BE(0));
    console.log(buf.readUInt16LE(0));
    console.log(buf.readUInt16BE(1));
    console.log(buf.readUInt16LE(1));
    console.log(buf.readUInt16BE(2));
    console.log(buf.readUInt16LE(2));

    // 0x0304
    // 0x0403
    // 0x0423
    // 0x2304
    // 0x2342
    // 0x4223

### buf.readUInt32LE(offset[, noAssert])
### buf.readUInt32BE(offset[, noAssert])

* `offset` Number
* `noAssert` Boolean, Opcional, Por defecto: false
* Devuelve: Number

Lee un entero de 32 bits sin signo de un buffer en la posición especificada por el offset con el formato endiano especificado.

Si `noAssert` es true la validación se pasará por alto desde `offset`. Esto significa que `offset` puede estar más allá del final del buffer. Es `false` por defecto.

Ejemplo:

    var buf = new Buffer(4);

    buf[0] = 0x3;
    buf[1] = 0x4;
    buf[2] = 0x23;
    buf[3] = 0x42;

    console.log(buf.readUInt32BE(0));
    console.log(buf.readUInt32LE(0));

    // 0x03042342
    // 0x42230403

### buf.readInt8(offset[, noAssert])

* `offset` Number
* `noAssert` Boolean, Opcional, Por defecto: false
* Devuelve: Number

Lee un entero de 8 bits de un buffer en la posición especificada por el offset con el formato endiano especificado.

Si `noAssert` es true la validación se pasará por alto desde `offset`. Esto significa que `offset` puede estar más allá del final del buffer. Es `false` por defecto.

Es como `buffer.readUInt8`, excepto que los contenidos del buffer se tratan signos complementarios de dos valores.

### buf.readInt16LE(offset[, noAssert])
### buf.readInt16BE(offset[, noAssert])

* `offset` Number
* `noAssert` Boolean, Opcional, Por defecto: false
* Devuelve: Number

Lee un entero de 16 bits de un buffer en la posición especificada por el offset con el formato endiano especificado.

Si `noAssert` es true la validación se pasará por alto desde `offset`. Esto significa que `offset` puede estar más allá del final del buffer. Es `false` por defecto.

Es como  `buffer.readUInt16*`, excepto que los contenidos del buffer contents se tratan signos complementarios de dos valores.

### buf.readInt32LE(offset[, noAssert])
### buf.readInt32BE(offset[, noAssert])

* `offset` Number
* `noAssert` Boolean, Opcional, Por defecto: false
* Devuelve: Number

Lee un entero de 32 bits de un buffer en la posición especificada por el offset con el formato endiano especificado.

Si `noAssert` es true la validación se pasará por alto desde `offset`. Esto significa que `offset` puede estar más allá del final del buffer. Es `false` por defecto.

Es como `buffer.readUInt32*`, excepto que los contenidos del buffer contents se tratan signos complementarios de dos valores.

### buf.readFloatLE(offset[, noAssert])
### buf.readFloatBE(offset[, noAssert])

* `offset` Number
* `noAssert` Boolean, Opcional, Por defecto: false
* Devuelve: Number

Lee un float de 32 bits de un buffer en la posición especificada por el offset con el formato endiano especificado.

Si `noAssert` es true la validación se pasará por alto desde `offset`. Esto significa que `offset` puede estar más allá del final del buffer. Es `false` por defecto.

Ejemplo:

    var buf = new Buffer(4);

    buf[0] = 0x00;
    buf[1] = 0x00;
    buf[2] = 0x80;
    buf[3] = 0x3f;

    console.log(buf.readFloatLE(0));

    // 0x01

### buf.readDoubleLE(offset[, noAssert])
### buf.readDoubleBE(offset[, noAssert])

* `offset` Number
* `noAssert` Boolean, Opcional, Por defecto: false
* Devuelve: Number

Lee un float de 64 bits de un buffer en la posición especificada por el offset con el formato endiano especificado.

Si `noAssert` es true la validación se pasará por alto desde `offset`. Esto significa que `offset` puede estar más allá del final del buffer. Es `false` por defecto.

Ejemplo:

    var buf = new Buffer(8);

    buf[0] = 0x55;
    buf[1] = 0x55;
    buf[2] = 0x55;
    buf[3] = 0x55;
    buf[4] = 0x55;
    buf[5] = 0x55;
    buf[6] = 0xd5;
    buf[7] = 0x3f;

    console.log(buf.readDoubleLE(0));

    // 0.3333333333333333

### buf.writeUInt8(valor, offset[, noAssert])

* `valor` Number
* `offset` Number
* `noAssert` Boolean, Opcional, Por defecto: false

Escribe `valor` en el buffer usando el offset especificado. Téngase en cuenta que `valor` debe de ser un entero de 8 bits sin signo válido.

Si `noAssert` es true no se validará `valor` y `offset`. Esto significa que `valor` puede ser muy largo para la función específica y `offset` más allá del final del buffer llevando aque valores se pierdan silenciosamente. Esto no se debe de hacer a no ser que se esté seguro de su corrección. Por defecto `noAssert` es `false`.

Ejemplo:

    var buf = new Buffer(4);
    buf.writeUInt8(0x3, 0);
    buf.writeUInt8(0x4, 1);
    buf.writeUInt8(0x23, 2);
    buf.writeUInt8(0x42, 3);

    console.log(buf);

    // <Buffer 03 04 23 42>

### buf.writeUInt16LE(valor, offset[, noAssert])
### buf.writeUInt16BE(valor, offset[, noAssert])

* `valor` Number
* `offset` Number
* `noAssert` Boolean, Opcional, Por defecto: false

Escribe `valor` en el buffer en el offset especificado con el formato endiano dado. Nota que `valor` ha de ser un entero sin signo de 16 bits válido.

Si `noAssert` es true no se validará `valor` y `offset`. Esto significa que `valor` puede ser muy largo para la función específica y `offset` más allá del final del buffer llevando aque valores se pierdan silenciosamente. Esto no se debe de hacer a no ser que se esté seguro de su corrección. Por defecto `noAssert` es `false`.

Ejemplo:

    var buf = new Buffer(4);
    buf.writeUInt16BE(0xdead, 0);
    buf.writeUInt16BE(0xbeef, 2);

    console.log(buf);

    buf.writeUInt16LE(0xdead, 0);
    buf.writeUInt16LE(0xbeef, 2);

    console.log(buf);

    // <Buffer de ad be ef>
    // <Buffer ad de ef be>

### buf.writeUInt32LE(valor, offset[, noAssert])
### buf.writeUInt32BE(valor, offset[, noAssert])

* `valor` Number
* `offset` Number
* `noAssert` Boolean, Opcional, Por defecto: false

Escribe `valor` en el buffer en el offset especificado con el formato endiano dado. Nota que `valor` ha de ser un entero sin signo de 32 bits válido.

Si `noAssert` es true no se validará `valor` y `offset`. Esto significa que `valor` puede ser muy largo para la función específica y `offset` más allá del final del buffer llevando aque valores se pierdan silenciosamente. Esto no se debe de hacer a no ser que se esté seguro de su corrección. Por defecto `noAssert` es `false`.

Ejemplo:

    var buf = new Buffer(4);
    buf.writeUInt32BE(0xfeedface, 0);

    console.log(buf);

    buf.writeUInt32LE(0xfeedface, 0);

    console.log(buf);

    // <Buffer fe ed fa ce>
    // <Buffer ce fa ed fe>

### buf.writeInt8(valor, offset[, noAssert])

* `valor` Number
* `offset` Number
* `noAssert` Boolean, Opcional, Por defecto: false

Escribe `valor` en el buffer en el offset especificado con el formato endiano dado. Nota que `valor` ha de ser un entero con signo de 8 bits válido.

Si `noAssert` es true no se validará `valor` y `offset`. Esto significa que `valor` puede ser muy largo para la función específica y `offset` más allá del final del buffer llevando aque valores se pierdan silenciosamente. Esto no se debe de hacer a no ser que se esté seguro de su corrección. Por defecto `noAssert` es `false`.

Es como `buffer.writeUInt8`, excepto que valor se se escriben con signos complementarios de dos valores en el `buffer`.

### buf.writeInt16LE(valor, offset[, noAssert])
### buf.writeInt16BE(valor, offset[, noAssert])

* `valor` Number
* `offset` Number
* `noAssert` Boolean, Opcional, Por defecto: false

Escribe `valor` en el buffer en el offset especificado con el formato endiano dado. Nota que `valor` ha de ser un entero con signo de 16 bits válido.

Si `noAssert` es true no se validará `valor` y `offset`. Esto significa que `valor` puede ser muy largo para la función específica y `offset` más allá del final del buffer llevando aque valores se pierdan silenciosamente. Esto no se debe de hacer a no ser que se esté seguro de su corrección. Por defecto `noAssert` es `false`.

Es como `buffer.writeUInt16*`, excepto que valor se se escriben con signos complementarios de dos valores en el `buffer`.

### buf.writeInt32LE(valor, offset[, noAssert])
### buf.writeInt32BE(valor, offset[, noAssert])

* `valor` Number
* `offset` Number
* `noAssert` Boolean, Opcional, Por defecto: false

Escribe `valor` en el buffer en el offset especificado con el formato endiano dado. Nota que `valor` ha de ser un entero con signo de 32 bits válido.

Si `noAssert` es true no se validará `valor` y `offset`. Esto significa que `valor` puede ser muy largo para la función específica y `offset` más allá del final del buffer llevando aque valores se pierdan silenciosamente. Esto no se debe de hacer a no ser que se esté seguro de su corrección. Por defecto `noAssert` es `false`.

Es como `buffer.writeUInt32*`, excepto que valor se se escriben con signos complementarios de dos valores en el `buffer`.

### buf.writeFloatLE(valor, offset[, noAssert])
### buf.writeFloatBE(valor, offset[, noAssert])

* `valor` Number
* `offset` Number
* `noAssert` Boolean, Opcional, Por defecto: false

Escribe `valor` en el buffer en el offset especificado con el formato endiano dado. Nota que, el comportamiento no está determinado si `valor` no es un float de 32 bit válido.

Si `noAssert` es true no se validará `valor` y `offset`. Esto significa que `valor` puede ser muy largo para la función específica y `offset` más allá del final del buffer llevando aque valores se pierdan silenciosamente. Esto no se debe de hacer a no ser que se esté seguro de su corrección. Por defecto `noAssert` es `false`.

Ejemplo:

    var buf = new Buffer(4);
    buf.writeFloatBE(0xcafebabe, 0);

    console.log(buf);

    buf.writeFloatLE(0xcafebabe, 0);

    console.log(buf);

    // <Buffer 4f 4a fe bb>
    // <Buffer bb fe 4a 4f>

### buf.writeDoubleLE(valor, offset[, noAssert])
### buf.writeDoubleBE(valor, offset[, noAssert])

* `valor` Number
* `offset` Number
* `noAssert` Boolean, Opcional, Por defecto: false

Escribe `valor` en el buffer en el offset especificado con el formato endiano dado. Nota que `valor` ha de ser un double de 64 bit válido.

Si `noAssert` es true no se validará `valor` y `offset`. Esto significa que `valor` puede ser muy largo para la función específica y `offset` más allá del final del buffer llevando aque valores se pierdan silenciosamente. Esto no se debe de hacer a no ser que se esté seguro de su corrección. Por defecto `noAssert` es `false`.

Ejemplo:

    var buf = new Buffer(8);
    buf.writeDoubleBE(0xdeadbeefcafebabe, 0);

    console.log(buf);

    buf.writeDoubleLE(0xdeadbeefcafebabe, 0);

    console.log(buf);

    // <Buffer 43 eb d5 b7 dd f9 5f d7>
    // <Buffer d7 5f f9 dd b7 d5 eb 43>

### buf.fill(valor[, offset][, final])

* `valor`
* `offset` Number, Opcional
* `final` Number, Opcional

Rellena el buffer con el valor especificado. Si el `offset` (por defecto `0`)
y `final` (por defecto `buffer.length`) no se dan se rellenará el buffer por completo.

    var b = new Buffer(50);
    b.fill("h");

## buffer.INSPECT_MAX_BYTES

* Number, Por defecto: 50

Cuantos bits será devueltos cuando se llama `buffer.inspect()` pudiendo ser sobreescrito por módulos de usuario.

Tengase en cuenta que esta propiedad se encuentra en el módulo buffer devuelto por `require('buffer')`, no en el global Buffer, o en una instancia buffer.

## Clase: SlowBuffer

Devuelve un `Buffer` desagrupado `Buffer`.

Para ahorrar sobrecarga por recolección de basura a la hora de crear varios Buffers asignados individualmente, por defector asignaciones por debajo a 4KB son recortadas de un único bufer asignado. Este método mejora rendimiento y memoria ya que v8 no necesita rastrear y limpiar tantos objectos `Persistentes`.

En el caso que un desarrollador necesite mantener una parte pequeña de memoria de un pool por una cantidad de tiempo indeterminado puede que sea apropiado crear un una instancia de `Buffer` desagroupado usando SlowBuffer y copiar las partes relevantes.

    // se necesita una parte pequeña de memoria
    var store = [];

    socket.on('readable', function() {
      var data = socket.read();
      // asignar datos retenidos
      var sb = new SlowBuffer(10);
      // copiar los datos a la nueva referencia
      data.copy(sb, 0, 0, 10);
      store.push(sb);
    });

Aunque esto debería de ser usado de forma marginal y solo como último recurso *despues* de que el desarrollador ha observado activamente retención de memoria indevida en sus aplicaciones.
