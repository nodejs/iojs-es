# Zlib

    Estabilidad: 3 - Estable

Puedes acceder a este módulo con:

    var zlib = require('zlib');

Esto ofrece enlaces a las clases Gzip/Gunzip, Deflate/Inflate, y
DeflateRaw/InflateRaw. Cada clase recoge las mismas opciones, y
es un readable/writable Stream.

## Ejemplos

Comprimir y descomprimir un archivo se puede hacer a través de una
tubería de fs.ReadStream a un stream de zlib, y de ahí a
fs.WriteStream.

    var gzip = zlib.createGzip();
    var fs = require('fs');
    var inp = fs.createReadStream('input.txt');
    var out = fs.createWriteStream('input.txt.gz');

    inp.pipe(gzip).pipe(out);

Se pueden comprimir y descomprimir datos, en un paso, mediante los
métodos convenientes.

    var input = '.................................';
    zlib.deflate(input, function(err, buffer) {
      if (!err) {
        console.log(buffer.toString('base64'));
      }
    });

    var buffer = new Buffer('eJzT0yMAAGTvBe8=', 'base64');
    zlib.unzip(buffer, function(err, buffer) {
      if (!err) {
        console.log(buffer.toString());
      }
    });

Para usar este módulo en un cliente o servidor HTTP, usa el
header
[accept-encoding](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.3)
en peticiones, y el header
[content-encoding](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.11)
en respuestas.

**Nota: Estos ejemplos están simplificados drásticamente para mostrar
el concepto básico.** La codificación Zlib puede ser costosa, y los
resultados deberían ser almacenados en caché. Vea [Ajustes de Uso de Memoria](#zlib_memory_usage_tuning)
debajo para más información acerca del costo de velocidad/memoria/
compresión involucrado en el uso de zlib.

    // Ejemplo de petición como cliente
    var zlib = require('zlib');
    var http = require('http');
    var fs = require('fs');
    var request = http.get({ host: 'izs.me',
                             path: '/',
                             port: 80,
                             headers: { 'accept-encoding': 'gzip,deflate' } });
    request.on('response', function(response) {
      var output = fs.createWriteStream('izs.me_index.html');

      switch (response.headers['content-encoding']) {
        // o simplemente utiliza zlib.createUnzip() para manejar
        // ambos casos.
        case 'gzip':
          response.pipe(zlib.createGunzip()).pipe(output);
          break;
        case 'deflate':
          response.pipe(zlib.createInflate()).pipe(output);
          break;
        default:
          response.pipe(output);
          break;
      }
    });

    // Ejemplo de servidor
    // Ejecutar una operación gzip en cada petición es un tanto
    // costoso. Sería mucho más eficiente almacenar en caché el
    // buffer comprimido.
    var zlib = require('zlib');
    var http = require('http');
    var fs = require('fs');
    http.createServer(function(request, response) {
      var raw = fs.createReadStream('index.html');
      var acceptEncoding = request.headers['accept-encoding'];
      if (!acceptEncoding) {
        acceptEncoding = '';
      }

      // Nota: Esto no es un parser (analizador) de accept-encoding
      // válido.
      // Véase http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.3
      if (acceptEncoding.match(/\bdeflate\b/)) {
        response.writeHead(200, { 'content-encoding': 'deflate' });
        raw.pipe(zlib.createDeflate()).pipe(response);
      } else if (acceptEncoding.match(/\bgzip\b/)) {
        response.writeHead(200, { 'content-encoding': 'gzip' });
        raw.pipe(zlib.createGzip()).pipe(response);
      } else {
        response.writeHead(200, {});
        raw.pipe(response);
      }
    }).listen(1337);

## zlib.createGzip([opciones])

Genera un nuevo objeto [Gzip](#zlib_class_zlib_gzip) con unas
[opciones](#zlib_options).

## zlib.createGunzip([opciones])

Genera un nuevo objeto [Gunzip](#zlib_class_zlib_gunzip) con unas
[opciones](#zlib_options).

## zlib.createDeflate([opciones])

Genera un nuevo objeto [Deflate](#zlib_class_zlib_deflate) con unas
[opciones](#zlib_options).

## zlib.createInflate([opciones])

Genera un nuevo objeto [Inflate](#zlib_class_zlib_inflate) con unas
[opciones](#zlib_options).

## zlib.createDeflateRaw([opciones])

Genera un nuevo objeto [DeflateRaw](#zlib_class_zlib_deflateraw) con unas
[opciones](#zlib_options).

## zlib.createInflateRaw([opciones])

Genera un nuevo objeto [InflateRaw](#zlib_class_zlib_inflateraw) con unas
[opciones](#zlib_options).

## zlib.createUnzip([opciones])

Genera un nuevo objeto [Unzip](#zlib_class_zlib_unzip) con unas
[opciones](#zlib_options).


## Clase: zlib.Zlib

No se exporta mediante el módulo `zlib`. Está documentada aquí
por que es la base para las clases de compresión y decompresión.

### zlib.flush([tipo], callback)

`tipo` es por defecto `zlib.Z_FULL_FLUSH`.

Limpia datos pendientes. No es recomendable hacer uso de esta
función de forma frívola, pues puede impactar de forma negativa
en la efectividad del algoritmo de compresión.

### zlib.params(nivel, estrategia, callback)

Actualiza dinámicamente el nivel y la estrategia de compresión.
Solo se aplica al algoritmo de desinflado.

### zlib.reset()

Reinicializa el compresor y descompresor a los valores por defecto.
Solo se aplica a los algoritmos de inflación y desinflación.

## Class: zlib.Gzip

Comprime datos usando gzip.

## Class: zlib.Gunzip

Descomprime un stream gzip.

## Class: zlib.Deflate

Comprime datos utilizando desinflación.

## Class: zlib.Inflate

Descomprime datos utilizando inflación.

## Class: zlib.DeflateRaw

Comprime datos utilizando desinflación, y no adhiere la cabecera zlib.

## Class: zlib.InflateRaw

Descomprime un stream de desinflación crudo (sin la cabecera zlib).

## Class: zlib.Unzip

Descomprime un stream comprimido mediante Gzip o desinflación a través
de la identificación automática de la cabecera.

## Métodos de conveniencia

<!--type=misc-->

Todos ellos toman un string o un buffer como primer argumento, un
segundo argumento opcional para proporcionar opciones a las clases
de zlib y un tercero que representa el callback, al que se llamará
finalmente en base a la siguiente estructura: `callback(error, result)`.

Cada método tiene un equivalente `*Sync` (Síncrono), que acepta los
mismos argumentos pero no necesita el callback.

## zlib.deflate(buf[, opciones], callback)
## zlib.deflateSync(buf[, opciones])

Comprime un string con desinflación.

## zlib.deflateRaw(buf[, opciones], callback)
## zlib.deflateRawSync(buf[, opciones])

Comprime un string con desinflación en crudo.

## zlib.gzip(buf[, opciones], callback)
## zlib.gzipSync(buf[, opciones])

Comprime un string con Gzip.

## zlib.gunzip(buf[, opciones], callback)
## zlib.gunzipSync(buf[, opciones])

Descomprime un Buffer con Gunzip.

## zlib.inflate(buf[, opciones], callback)
## zlib.inflateSync(buf[, opciones])

Descomprime un Buffer con inflación.

## zlib.inflateRaw(buf[, opciones], callback)
## zlib.inflateRawSync(buf[, opciones])

Descomprime un Buffer con inflación en crudo.

## zlib.unzip(buf[, options], callback)
## zlib.unzipSync(buf[, options])

Descomprime un Buffer con Unzip.

## Opciones

<!--type=misc-->

Cada clase necesita un objeto de opciones. Todas las opciones son
opcionales.

Ten en cuenta que algunas opciones solo son relevantes a la hora de
comprimir, y son ignoradas por las clases de descompresión.

* flush (por defecto: `zlib.Z_NO_FLUSH`)
* chunkSize (por defecto: 16*1024)
* windowBits
* level (solo compresión)
* memLevel (solo compresión)
* strategy (solo compresión)
* dictionary (solo para inflación y desinflación, no hay diccionario
  predeterminado)

Consulta la descripción de `deflateInit2` e `inflateInit2` en
<http://zlib.net/manual.html#Advanced> para más información.

## Ajuste del Uso de Memoria

<!--type=misc-->

Desde `zlib/zconf.h`, modificado para su uso en io.js:

Los requerimientos de memoria para la desinflación son (en bytes):

    (1 << (windowBits+2)) +  (1 << (memLevel+9))

Esto es: 128K para un tamaño de pantalla de 15 bits + 128K para un
nivel de memoria de 8 (valores por defecto) más algunos kilobytes
para objetos pequeños.

Por ejemplo, si quieres reducir los requerimientos de memoria de 256K
a 128K, establece las opciones a:

    { windowBits: 14, memLevel: 7 }

Por supuesto, esto va a degradar generalmente la compresión (no hay
almuerzo gratis)

Los requerimientos de memoria para la inflación son (en bytes):

    1 << windowBits

Esto es, 32K para un tamaño de pantalla de 15 bits (valor por defecto)
más algunos kilobytes para objetos pequeños.

Todo ello junto a un bloque de buffer de única salida interna con un
tamaño `chunkSize`, que por defecto es de 16K.

La velocidad de compresión de zlib se ve afectada principalmente por
la opción `level`. Un mayor nivel dará lugar a una mejor compresión,
pero llevará más tiempo. Un menor nivel resultará en menor compresión
, pero será mucho más rápida.

En general, opciones de memoria que consuman mayor cantidad de memoria
supondrán que io.js tenga que hacer menos llamadas a zlib, ya que
será capaz de procesar más datos en una única operación `write`. Por
tanto, este es otro factor que afecta la velocidad, a costa del uso
de memoria.

## Constantes

<!--type=misc-->

Todas las constantes definidas en zlib.h también lo están en
`require('zlib')`.
Para un uso habitual, no será necesario establecer ninguna de ellas.
Están documentadas aquí para que su presencia no pille por sorpresa.
Esta sección se ha obtenido casi directamente de la [documentación de
zlib](http://zlib.net/manual.html#Constants). Visita
<http://zlib.net/manual.html#Constants> para más detalles.

Valores permitidos de `flush`.

* `zlib.Z_NO_FLUSH`
* `zlib.Z_PARTIAL_FLUSH`
* `zlib.Z_SYNC_FLUSH`
* `zlib.Z_FULL_FLUSH`
* `zlib.Z_FINISH`
* `zlib.Z_BLOCK`
* `zlib.Z_TREES`

Códigos de resultado para las funciones de compresión y descompresión.
Los valores negativos son errores, los valores positivos se usan para
eventos especiales pero habituales.

* `zlib.Z_OK`
* `zlib.Z_STREAM_END`
* `zlib.Z_NEED_DICT`
* `zlib.Z_ERRNO`
* `zlib.Z_STREAM_ERROR`
* `zlib.Z_DATA_ERROR`
* `zlib.Z_MEM_ERROR`
* `zlib.Z_BUF_ERROR`
* `zlib.Z_VERSION_ERROR`

Niveles de compresión.

* `zlib.Z_NO_COMPRESSION`
* `zlib.Z_BEST_SPEED`
* `zlib.Z_BEST_COMPRESSION`
* `zlib.Z_DEFAULT_COMPRESSION`

Estrategia de compresión.

* `zlib.Z_FILTERED`
* `zlib.Z_HUFFMAN_ONLY`
* `zlib.Z_RLE`
* `zlib.Z_FIXED`
* `zlib.Z_DEFAULT_STRATEGY`

Valores disponibles para el campo `data_type`.

* `zlib.Z_BINARY`
* `zlib.Z_TEXT`
* `zlib.Z_ASCII`
* `zlib.Z_UNKNOWN`

El método de compresión por desinflación (el único soportado en esta
versión).

* `zlib.Z_DEFLATED`

Para inicializar zalloc, zfree, opaque.

* `zlib.Z_NULL`
