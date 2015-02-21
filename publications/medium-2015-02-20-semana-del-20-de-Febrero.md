## io.js Semana del 20 de Febrero de 2015
Versión 1.3.0, MongoDB, la hoja de ruta y más.

## Publicado io.js 1.3.0
Cambios notables incluídos:

* **url**: `url.resolve('/path/to/file', '.')` ahora devuelve `/path/to/` con la barra diagonal, `url.resolve('/', '.')` devuelve `/` [#278](https://github.com/iojs/io.js/pull/278) (Amir Saboury)
* **tls**: El paquete de cifrado por defecto usado por `tls` y `https` ha sido modificado por uno que logra *Perfect Forward Secrecy* (Confidencialidad Directa Perfecta) con todos los navegadores web modernos. Adicionalmente, los cifrados RC4 inseguros fueron excluidos. Si absolutamente requieres RC4, por favor especifica tus propios paquetes de cifrado. [#826](https://github.com/iojs/io.js/pull/826) (Roman Reiss)

## Eventos Notables en la Comunidad
* **Gobierno de Node** - [William Bert](https://twitter.com/williamjohnbert) creó http://nodegovernance.io/ para alertar a Scott Hammond, CEO de Joyent, del deseo de la comunidad de que el modelo de gobierno abierto de io.js sea la base del Comité Técnico de la Fundación Node. La respuesta de la comunidad fue _fantástica_! 
* **Mejora de Rendimiento de Node.js y io.js** - Raygun.io hizo tests de rendimiento con Node.js y io.js recientemente, y ambos están mejorando el rendimiento en cada versión! [Leer el artículo completo](https://raygun.io/blog/2015/02/node-js-performance-node-js-vs-io-js/).
* **LTTng Básico** - [LTTing Básico](https://asciinema.org/a/16785) con io.js por el usuario jgalar en asciinema
* **[Diapositivas de la hoja de ruta de io.js](http://roadmap.iojs.org/)** - Diapositivas de la hoja de ruta actual de io.js. 

### Soporte Añadido para io.js 
* [TravisCI](https://travis-ci.org/) agregó io.js. El día que la última actualización semanal fue publicada, Hiro Asari (あさり) [twiteó](https://twitter.com/hiro_asari/status/566268486012633088) que cerca del 10% de los proyectos Node estaban corriendo io.js.
* @thlorenz actualizó [nad](https://github.com/thlorenz/nad), Node Addon Developer, para [soportar io.js](https://twitter.com/thlorenz/status/566328088121081856)
* [Catberry.js](https://github.com/catberry/catberry) agregó soporte para io.js.
* Módulo node oficial de mongodb soporta io.js en [v. 2.0.16 2015-02-16](https://github.com/mongodb/node-mongodb-native/blob/2.0/HISTORY.md).
* [The Native Web](http://www.thenativeweb.io/) ahora tiene un [contenedor Docker para io.js](https://registry.hub.docker.com/u/thenativeweb/iojs/)
* [DNSChain](https://github.com/okTurtles/dnschain) de [okTurtles](https://okturtles.com/) agregó soporte para io.js.
* [TDPAHACLPlugin](https://github.com/neilstuartcraig/TDPAHACLPlugin) y [TDPAHAuthPlugin](https://github.com/neilstuartcraig/TDPAHAuthPlugin) para [actionHero](http://www.actionherojs.com/) ahora soportan io.js.
* [node-sass](https://npmjs.org/package/node-sass) agregó soporte para io.js 1.2 en node-sass [v. 2.0.1](https://github.com/sass/node-sass/issues/655)
* [total.js](https://www.totaljs.com/) agregó soporte para io.js en [v. 1.7.1](https://github.com/totaljs/framework/releases/tag/v1.7.1) 
* [Clever Cloud](https://www.clever-cloud.com/) agregó [soporte para io.js](https://www.clever-cloud.com/blog/features/2015/01/23/introducing-io.js/)

## Reunión de Grupo de Trabajo de io.js
* Reunión del Grupo de Trabajo de Tracing de io.js - 19 de Feb., 2015: [YouTube](https://www.youtube.com/watch?v=wvBVjg8jkv0) - [Minutas](https://docs.google.com/document/d/1_ApOMt03xHVkaGpTEPMDIrtkjXOzg3Hh4ZcyfhvMHx4/edit)
* Reunión del Grupo de Trabajo de Build de io.js - 19 de Feb., 2015: [YouTube](https://www.youtube.com/watch?v=OKQi3pTF7fs) - [SoundCloud](https://soundcloud.com/iojs/iojs-build-wg-meeting-2015-02-19) - [Minutas](https://docs.google.com/document/d/1vRhsYBs4Hw6vRu55h5eWTwDzS1NctxdTvMMEnCbDs14/edit)
* Reunión del Comité Técnico de io.js - 18 de Feb., 2015: [YouTube](https://www.youtube.com/watch?v=jeBPYLJ2_Yc) - [SoundCloud](https://soundcloud.com/iojs/iojs-tc-meeting-meeting-2015-02-18) - [Minutas](https://docs.google.com/document/d/1JnujRu6Rfnp6wvbvwCfxXnsjLySunQ_yah91pkvSFdQ/edit)
* Reunión del Grupo de Trabajo de Sitio Web de io.js - 16 de Feb., 2015: [YouTube](https://www.youtube.com/watch?v=UKDKhFV61ZA) - [SoundCloud](https://soundcloud.com/iojs/iojs-website-wg-meeting-2015-02-16) - [Minutas](https://docs.google.com/document/d/1R8JmOoyr64tt-QOj27bD19ZOWg63CujW7GeaAHIIkUs/edit)
