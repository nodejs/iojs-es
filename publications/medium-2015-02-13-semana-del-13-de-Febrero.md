## Soporte de io.js añadido por...
* [Postmark](http://blog.postmarkapp.com/post/110829734198/its-official-were-getting-cozy-with-node-js)
* [node-serialport](https://github.com/voodootikigod/node-serialport/issues/439)
* [Microsoft Azure](http://azure.microsoft.com/en-us/documentation/articles/web-sites-nodejs-iojs/)

## io.js rompe las 10,000 estrellas en GitHub
El 13 de Febrero, io.js llegó al objetivo de 10,000 estrellas en GitHub. No podríamos haber hecho esto sin el soporte de la increíble comunidad tras JavaScript ¡Gracias a todos!

## Publicado io.js 1.2.0
* **stream**: Construcción más simple para stream ([readable-stream/issues#102](https://github.com/iojs/readable-stream/issues/102))
* **dns**: `lookup()` ahora soporta una opción boolean 'all', por defecto false pero cuando es activadada hará a este método devolver un array de todos los nombres resueltos para una dirección, véase, ([iojs/pull#744](https://github.com/iojs/io.js/pull/744))
* **assert**: Elimina la propiedad prototype para comparación en `deepEqual()` (https://github.com/iojs/io.js/pull/636); introduce un método deepStrictEqual() para comparar deepEqual() pero hace igualdad estricta en primitivas (https://github.com/iojs/io.js/pull/639).
* **docs**: Gran actualización de documentación, véase commits individuales; nueva página de Errores estudiando errores en JavaScript, temas específicos de V8, y detalles específicos a errores con io.js.
* **tracing**: Añadir [LTTng](http://lttng.org/) (Linux Trace Toolkit Next Generation) cuando se compila con `--with-lttng opción`. Los puntos Trace coinciden con los aquellos disponibles para DTrace y ETW. ([iojs/issues#702](https://github.com/iojs/io.js/pull/702))
* **npm** actualización a 2.5.1
* **libuv** actualización a 1.4.0, véase libuv [ChangeLog](https://github.com/libuv/libuv/blob/v1.x/ChangeLog)
* Añadir nuevos colaboradores:
  * Aleksey Smolenchuk (@lxe)
  * Shigeki Ohtsu (@shigeki)

## Abrió nuestras puertas a la communidad internacional
Véase el [artículo original](https://medium.com/@mikeal/how-io-js-built-a-146-person-27-language-localization-effort-in-one-day-65e5b1c49a62) en Medium.
* Añadido contribuidores interesados a los equipos de su idioma.
* Equipos registraron cuentas en Twitter para sus equipos y otras redes sociales de interés.
* Equipos llevaron a cabo sus propias maneras de trabajar juntos, y se convirtieron más en "organizadores de comunidad", en oposición a sólo "traductores".

### Estadísticas por Localizaciones:

* 146 personas se registraron para ayudar con localizaciones el primer día (más de 160 registradas ahora)
* 27 comunidades de idiomas creadas el primer día (en este momento llegando a 29)

### Comunidades de Idioma

* [`iojs-bn`](https://github.com/iojs/iojs-bn) Comunidad Bengalí
* [`iojs-cn`](https://github.com/iojs/iojs-cn) Comunidad China
* [`iojs-cs`](https://github.com/iojs/iojs-cs) Comunidad Checa
* [`iojs-da`](https://github.com/iojs/iojs-da) Comunidad Danesa
* [`iojs-de`](https://github.com/iojs/iojs-de) Comunidad Alemana
* [`iojs-el`](https://github.com/iojs/iojs-el) Comunidad Griega
* [`iojs-es`](https://github.com/iojs/iojs-es) Comunidad Española
* [`iojs-fa`](https://github.com/iojs/iojs-fa) Comunidad Persa
* [`iojs-fi`](https://github.com/iojs/iojs-fi) Comunidad Finesa
* [`iojs-fr`](https://github.com/iojs/iojs-fr) Comunidad Francesa
* [`iojs-he`](https://github.com/iojs/iojs-he) Comunidad Hebrea
* [`iojs-hi`](https://github.com/iojs/iojs-hi) Comunidad India
* [`iojs-hu`](https://github.com/iojs/iojs-hu) Comunidad Húngara
* [`iojs-id`](https://github.com/iojs/iojs-id) Comunidad Indonesia
* [`iojs-it`](https://github.com/iojs/iojs-it) Comunidad Italiana
* [`iojs-ja`](https://github.com/iojs/iojs-ja) Comunidad Japonesa
* [`iojs-ka`](https://github.com/iojs/iojs-ka) Comunidad Georgiana
* [`iojs-kr`](https://github.com/iojs/iojs-kr) Comunidad Coreana
* [`iojs-mk`](https://github.com/iojs/iojs-mk) Comunidad Macedonia
* [`iojs-nl`](https://github.com/iojs/iojs-nl) Comunidad Holandesa
* [`iojs-no`](https://github.com/iojs/iojs-no) Comunidad Noruega
* [`iojs-pl`](https://github.com/iojs/iojs-pl) Comunidad Polaca
* [`iojs-pt`](https://github.com/iojs/iojs-pt) Comunidad Portuguesa
* [`iojs-ro`](https://github.com/iojs/iojs-ro) Comunidad Rumana
* [`iojs-ru`](https://github.com/iojs/iojs-ru) Comunidad Rusa
* [`iojs-sv`](https://github.com/iojs/iojs-sv) Comunidad Sueca
* [`iojs-tr`](https://github.com/iojs/iojs-tr) Comunidad Turca
* [`iojs-tw`](https://github.com/iojs/iojs-tw) Comunidad Taiwanesa
* [`iojs-uk`](https://github.com/iojs/iojs-uk) Comunidad Ucraniana

## io.js y Node.js
Véase el [artículo original](https://medium.com/@iojs/io-js-and-a-node-js-foundation-4e14699fb7be) en Medium.
* Scott Hammond, CEO de Joyent, expresó su deseo de llevar io.js de vuelta a node.js.

#### En solo unos pocos meses io.js...
* Ha crecido a 23 miembros activos del equipo core
* Tiene varios grupos de trabajo
* Tiene 29 equipos de localización
* Ha atraído más contribuidores al projecto que nunca en la historia de node.js
* Ha sido capaz de publicar software de calidad a un buen ritmo con el soporte de una comunidad excepcional.

> Estamos impacientes en dejar todo esto tras nosotros pero no podemos sacrificar el progreso que hemos hecho o los principios o gobernación abierta que nos han traído hasta aquí.

### El Futuro
* Charlas con la fundación node.js están en proceso.
* Una vez la fundación tenga un modelo de gobernación técnica se podrá ver un issue en la cuenta de GitHub de io.js sobre sobre si io.js debe unirse.

    * Esto será discutido y votado abiertamente en una reunión TC pública seguida de las reglas de gobernación construidas.

> Para la comunidad, nada ha cambiado.

### Qué hacer ahora
* Continúa enviando PRs a io.js
* Únete a uno de los 27 [equipos de localización](https://github.com/iojs/website/issues/125)
* Contribuye a grupos de io.js ([streams](https://github.com/iojs/readable-stream), [website](https://github.com/iojs/website), [evangelism](https://github.com/iojs/website/labels/evangelism), [tracing](https://github.com/iojs/tracing-wg), [build](https://github.com/iojs/build), [roadmap](https://github.com/iojs/roadmap)) y
* Continúa adoptando io.js en tus aplicaciones.
