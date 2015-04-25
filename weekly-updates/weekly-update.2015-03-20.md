# io.js liberación 1.6

Esta semana se liberaron dos versiones de io.js, la [v1.6.1](https://iojs.org/dist/v1.6.1/) y la  [v1.6.0](https://iojs.org/dist/v1.6.0/). El registro de cambios completo se encuentra [en GitHub](https://github.com/iojs/io.js/blob/v1.x/CHANGELOG.md).

### Cambios notables

#### 1.6.1

* **path**: la comprobación de tipos en `path.resolve()` [#1153](https://github.com/iojs/io.js/pull/1153) no cubría algunos casos extremos que podían ser invocados en ambientes naturales, siendo el más notable `path.dirname(undefined)`. Esta se ha suavizado para `path.dirname()`, `path.basename()`, y `path.extname()` (Colin Ihrig) [#1216](https://github.com/iojs/io.js/pull/1216).
* **querystring**: optimizciones internas en `querystring.parse()` y `querystring.stringify()` [#847](https://github.com/iojs/io.js/pull/847) previenen que los literales `Number` sean apropiadamente convertidos mediante `querystring.escape()` [#1208](https://github.com/iojs/io.js/issues/1208), exponiendo un punto ciego en el conjunto de pruebas. El bug y las pruebas han sido corregidos (Jeremiah Senkpiel) [#1213](https://github.com/iojs/io.js/pull/1213).

#### 1.6.0

* **node**: se ha añadido la opción `-r` or `--require` por línea de comandas, que puede ser usada para precargar módulos al inicio (Ali Ijaz Sheikh) [#881](https://github.com/iojs/io.js/pull/881).
* **querystring**: `parse()` y `stringify()` ahora son más rápidos (Brian White) [#847](https://github.com/iojs/io.js/pull/847).
* **http**: el método `http.ClientRequest#flush()` ha sido deprecado y se ha reemplazado con `http.ClientRequest#flushHeaders()` para coincidir con el mismo cambio realizado en Node.js v0.12 en [joyent/node#9048](https://github.com/joyent/node/pull/9048) (Yosuke Furukawa) [#1156](https://github.com/iojs/io.js/pull/1156).
* **net**: se permite a `server.listen()` aceptar la opción `port` como un `String`, por ejemplo `{ port: "1234" }`, para coincidir con la misma opción en `net.connect()` de acuerdo a [joyent/node#9268](https://github.com/joyent/node/pull/9268) (Ben Noordhuis) [#1116](https://github.com/iojs/io.js/pull/1116).
* **tls**: further work on the reported memory leak although there appears to be a minor leak remaining for the use-case in question, track progress at [#1075](https://github.com/iojs/io.js/issues/1075).
* **v8**: backport a fix for an integer overflow when `--max_old_space_size` values above `4096` are used (Ben Noordhuis) [#1166](https://github.com/iojs/io.js/pull/1166).
* **platforms**: the io.js CI system now reports passes on **FreeBSD** and **SmartOS** (_Solaris_).
* **npm**: upgrade npm to 2.7.1. See [npm CHANGELOG.md](https://github.com/npm/npm/blob/master/CHANGELOG.md#v271-2015-03-05) for details.

### Known Issues

* Possible remaining TLS-related memory leak(s), details at [#1075](https://github.com/iojs/io.js/issues/1075).
* Surrogate pair in REPL can freeze terminal [#690](https://github.com/iojs/io.js/issues/690)
* Not possible to build io.js as a static library [#686](https://github.com/iojs/io.js/issues/686)
* `process.send()` is not synchronous as the docs suggest, a regression introduced in 1.0.2, see [#760](https://github.com/iojs/io.js/issues/760) and fix in [#774](https://github.com/iojs/io.js/issues/774)
* Calling `dns.setServers()` while a DNS query is in progress can cause the process to crash on a failed assertion [#894](https://github.com/iojs/io.js/issues/894)

# Community Updates

* browserify supports io.js, you can check the announcement [here](https://twitter.com/yosuke_furukawa/status/577150547850969088)
* express.js added [support](https://github.com/strongloop/express/commit/165660811aa9ba5f3733a7b033894f3d9a9c5e60) to io.js
* Over the last two weeks we got access to hardware from Joyent and upstreamed a patch to V8 so we got io.js building. After that we worked on passing tests for both [SmartOS](https://github.com/iojs/build/pull/64) and [FreeBSD](https://github.com/iojs/io.js/pull/1167) which as of two days ago now pass, this was thanks to the amazing work of the build team and [Johan Bergström](https://github.com/jbergstroem)
* [Petka Antonov](https://github.com/petkaantonov) is proposing a workers implementation in io.js under an experimental flag, you can join the discussion [here](https://github.com/iojs/io.js/pull/1159)
* io.js [upgraded](https://github.com/iojs/io.js/pull/1206) openssl to `1.0.1m`

# Upcoming Events

* [NodeConf](http://nodeconf.com/) tickets are on sale, June 8th and 9th at Oakland, CA and NodeConf Adventure for June 11th - 14th at Walker Creek Ranch, CA
* [CascadiaJS](http://2015.cascadiajs.com/) tickets are on sale, July 8th - 10th at Washington State
* [NodeConf EU](http://nodeconf.eu/) tickets are on sale, September 6th - 9th at Waterford, Ireland
