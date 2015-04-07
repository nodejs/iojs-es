# Bienvenido

Muy buenas, te dejamos aquí lo más importante para empezar a colaborar. Estamos organizados en Grupos de Trabajo. En el enlace de abajo tienes un resumen de cada uno

https://github.com/iojs/iojs-es/issues/83

Elige un _máximo de dos grupos de trabajo_ y escribe que quieres participar en el issue correspondiente a ese grupo:

* [Website](https://github.com/iojs/iojs-es/issues/96)
* [Documentación](https://github.com/iojs/iojs-es/issues/94)
* [Evangelización](https://github.com/iojs/iojs-es/issues/95)
* [Revisión](https://github.com/iojs/iojs-es/issues/97)

Cualquier duda, pregúntala en este repositorio o [en gitter](https://gitter.im/iojs/iojs-es).

# Grupos de Trabajo

Cada grupo tiene un repositorio en [la organización iojs-es](http://github.com/iojs-es) que es un fork del repositorio original de iojs. El material se trabaja de forma independiente en cada uno de los repositorios, desde donde una vez revisado por el equipo de revisión, la traducción se agrega al material ya traducido y se hace un Pull Request al repositorio de iojs.

Una vez escojas eres libre de empezar a contribuir. La idea es que hagas lo que puedas cuando puedas. Tu pones el ritmo.

## ¿Dónde está el material pendiente?

Dependiendo del grupo el material pendiente de traducción o revisión va estar en una forma u otra. En cualquier caso siempre puedes usar git para saber qué es lo que hay que actualizar. En la sección siguiente puedes ver una forma de hacerlo.

### Website

Website trabaja con el repositorio [iojs-es/website](https://github.com/iojs-es/website), concretamente con la [carpeta content/es](https://github.com/iojs-es/website/tree/master/content/es) donde se van añadiendo las actualizaciones hechas al iojs/website.

Para estar al día haz lo siguiente

```sh
# las dos primeras lineas solo hay que hacerlas una vez
git fork https://github.com/iojs-es/website.git
git remote add upstream https://github.com/iojs/website.git
# estas dos cada vez que quieras ver si hay algo que actualizar
git remote update
git merge master origin/master
```
esto creará un repositorio que apunta al original de iojs y al fork que tenemos hecho. Con esto, basta con comparar los commits de iojs/website con iojs-es/website y ver qué falta por actualizar

```sh
# commits de iojs/website
git log upstream/master -p
# commits de iojs-es/website
git log origin/master -p
# -p (--pretty :D) muestra el texto/código que se actualizó junto a cada commit
```

Una vez identificado lo que hay que actualizar, se crea una branch local, haces los cambios necesarios y por último push al repositorio iojs-es/website

```sh
# el nombre de la branch no importa ;)
git checkout -b actualiza-<documento>
# actualizamos <documento>
git push origin actualiza-<documento>
```

Hecho el push lo que has actualizado está ya en el repositorio pero falta hacer la Pull Request. Para ello, vas a repositorio y creas una Pull Request pidiendo al Grupo de Trabajo de Revisión que te revise el documento para estar conformes.

Si no recuerdas qué tenias puesto como `origin` y qué como `upstream` haz lo siguiente

```sh
git remote -v
origin	https://github.com/iojs-es/website.git (fetch)
origin	https://github.com/iojs-es/website.git (push)
upstream	https://github.com/iojs/website.git (fetch)
upstream	https://github.com/iojs/website.git (push)
```

y así ya sabes que `git push origin actualiza-<documento>` es el iojs-es/website

### Documentación

Documentación trabaja con el repositorio [iojs-es/io.js](https://github.com/iojs-es/io.js), concretamente con [la carpeta  doc/api/es](https://github.com/iojs-es/io.js/tree/api/doc/api/es) donde se van añadiendo las secciones de la API aún no traducidas.

Estamos a la espera de que io.js integre las traducciones de la API en la web (y aún parece que tardará un tiempo). Así que este grupo puede trabajar sin prisa.

#### Issues de referencia:
 - io.js API localization: open questions. En [ iojs/website#211](https://github.com/iojs/website/issues/211)

### Evangelización

Evangelización trabaja con el repositorio [iojs-es/evangelism](https://github.com/iojs-es/io.js), normalmente traduciendo [weekly-updates](https://github.com/iojs-es/evangelism/tree/master/weekly-updates). Suele ser un artículo a la semana. 

En este momento hay 4 artículos sin traducir empezando por el [weekly-update.2015-03-13.md](https://github.com/iojs-es/evangelism/tree/master/weekly-updates/weekly-update.2015-03-13.md)

Una vez traducido el artículo se publica en [@medium](https://medium.com/@iojs_es). Si estás en evangelización y has traducido un artículo pero no tienes la cuenta para publicar el artículo [habre un issue](https://github.com/iojs/iojs-es/issues/new).

### Revisión

Trabaja en conjunto con cualquiera de los grupos anteriores para garantizar que los documentos estén correctamente traducidos y no haya errores accidentales y/o de contexto. En concreto:

 - Ayuda a construir el [GLOSSARY.md](./GLOSSARY.md)
