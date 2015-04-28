# io.js liberación 1.6

Esta semana se liberaron dos versiones de io.js, la [v1.6.1](https://iojs.org/dist/v1.6.1/) y la  [v1.6.0](https://iojs.org/dist/v1.6.0/). El registro de cambios completo se encuentra [en GitHub](https://github.com/iojs/io.js/blob/v1.x/CHANGELOG.md).

### Cambios notables

#### 1.6.1

* **path**: la comprobación de tipos en `path.resolve()` [#1153](https://github.com/iojs/io.js/pull/1153) no cubría algunos casos extremos que podían ser invocados en ambientes naturales, siendo el más notable `path.dirname(undefined)`. Esta se ha suavizado para `path.dirname()`, `path.basename()`, y `path.extname()` (Colin Ihrig) [#1216](https://github.com/iojs/io.js/pull/1216).
* **querystring**: optimizciones internas en `querystring.parse()` y `querystring.stringify()` [#847](https://github.com/iojs/io.js/pull/847) previenen que los literales `Number` sean apropiadamente convertidos mediante `querystring.escape()` [#1208](https://github.com/iojs/io.js/issues/1208), exponiendo un punto ciego en el conjunto de pruebas. El bug y las pruebas han sido corregidos (Jeremiah Senkpiel) [#1213](https://github.com/iojs/io.js/pull/1213).

#### 1.6.0

* **node**: se ha añadido la opción `-r` or `--require` por línea de comandos, que puede ser usada para precargar módulos al inicio (Ali Ijaz Sheikh) [#881](https://github.com/iojs/io.js/pull/881).
* **querystring**: `parse()` y `stringify()` funcionan más rápidamente (Brian White) [#847](https://github.com/iojs/io.js/pull/847).
* **http**: el método `http.ClientRequest#flush()` ha sido deprecado y se ha reemplazado con `http.ClientRequest#flushHeaders()` para coincidir con el mismo cambio realizado en Node.js v0.12 en [joyent/node#9048](https://github.com/joyent/node/pull/9048) (Yosuke Furukawa) [#1156](https://github.com/iojs/io.js/pull/1156).
* **net**: se permite a `server.listen()` aceptar la opción `port` como un `String`, por ejemplo `{ port: "1234" }`, para coincidir con la misma opción en `net.connect()` de acuerdo a [joyent/node#9268](https://github.com/joyent/node/pull/9268) (Ben Noordhuis) [#1116](https://github.com/iojs/io.js/pull/1116).
* **tls**: hay que seguir trabajando en la pérdida de memoria reportada, aunque parece que la fuga restante para el caso de uso en cuestión es menor, siga el progreso en [#1075](https://github.com/iojs/io.js/issues/1075).
* **v8**: se hace backport de una solución para el desbordamiento de enteros cuando valores de `--max_old_space_size` mayores a `4096` son usados (Ben Noordhuis) [#1166](https://github.com/iojs/io.js/pull/1166).
* **platforms**: se reporta que la CI para io.js funciona en **FreeBSD** y **SmartOS** (_Solaris_).
* **npm**: actualizado a la versión 2.7.1. Ver el [npm CHANGELOG.md](https://github.com/npm/npm/blob/master/CHANGELOG.md#v271-2015-03-05) para más detalles.

### Problemas conocidos

* Posibles pérdidas de memoria relacionadas a TLS, más detalles en [#1075](https://github.com/iojs/io.js/issues/1075).
* Un par suplente en el REPL puede congelar la terminal [#690](https://github.com/iojs/io.js/issues/690)
* No es posible compilar io.js como una librería estática [#686](https://github.com/iojs/io.js/issues/686)
* `process.send()` no es una acción síncrona como la documentación sugiere, una regresión introducida en el 1.0.2, vea [#760](https://github.com/iojs/io.js/issues/760) y la solución en el [#774](https://github.com/iojs/io.js/issues/774)
* Llamar a `dns.setServers()` mientras una consulta DNS está en progreso puede ocasionar que el proceso falle en una afirmación fallida [#894](https://github.com/iojs/io.js/issues/894)

# Actualizaciones de la comunidad

* browserify ahora soporta io.js, puedes ver el anuncio [aquí](https://twitter.com/yosuke_furukawa/status/577150547850969088)
* express.js añadió  [soporte](https://github.com/strongloop/express/commit/165660811aa9ba5f3733a7b033894f3d9a9c5e60) para io.js
* En las dos últimas semanas se tuvo acceso a hardware de Joyent y se subió un parche a V8 que se pudo compilar en io.js. . Después de esto, trabajamos en pasar los test tanto para [SmartOS](https://github.com/iojs/build/pull/64) como [FreeBSD](https://github.com/iojs/io.js/pull/1167) los cuales desde hace dos días funcionan, esto fue gracias al increible trabajo del equipo de compilación y de [Johan Bergström](https://github.com/jbergstroem)
* [Petka Antonov](https://github.com/petkaantonov) está proponiendo una implementación de workers en io.js bajo un flag experimental, puedes unirte a la discusión [aquí](https://github.com/iojs/io.js/pull/1159)
* io.js [actualizó](https://github.com/iojs/io.js/pull/1206) openssl a `1.0.1m`

# Próximos eventos

* Los tickets para la [NodeConf](http://nodeconf.com/) están ya a la venta, el evento será del 8 y 9 de junio en Oakland, CA y la NodeConf Adventure del 11 al 14 de junio en Walker Creek Ranch, CA
* Los tickets para [CascadiaJS](http://2015.cascadiajs.com/) están ya a la venta, el evento será del 8 al 10 de julio en el Estado de Washington
* Los tickets para la [NodeConf EU](http://nodeconf.eu/) están ya a la venta, el evento será del 6 al 9 de Septiembre en Waterford, Irlanda
