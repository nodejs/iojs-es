---
title: ES6 y más allá
layout: docs.hbs
---
# ES6 y más allá

Node.Js esta construyendo en nuevas versiones con [motor V8](https://developers.google.com/v8/). Manteniendo dia a dia, con las ultimas versiones, podemos asegurar que tendera nuevas caracteristas de [especificacion en JavaScript ECMA-262 ](http://www.ecma-international.org/publications/standards/Ecma-262.htm) que seran llevadas por los desarrolladores Node.js a su tiempo y forma, así como las continuas mejoras de rendimiento y estabilidad.

Las características de ECMAScript 2015 (ES6), se pueden dividir en tres grandes grupos:

* Características **shipping**, en el cual se motor V8 considera estable, se **activan automáticamente** en NodeJs en la versión por defecto, y **NO** requiere ningún tipo de argumento inicial.
* Características **Staged**, estas son consideradas como **inestables**, por lo tanto requiere el argumento `--harmony` al iniciar el runtime.
* Características **In progress**, estas se necesitan ser activadas individualmente, con la respectivo argumento de `--harmony`, no es recomendable porque está **en prueba**. Nota: Estos argumentos y características pueden cambiar o eliminarse sin previo aviso por el motor V8.

## ¿Cuales son las características de Node.Js en la versión por defecto?

En la Web [node.green](http://node.green) provee un excelente resumen de las caracteristicas de ECMAScript, soportadas en varias versiones de Node.JS.

## ¿Cuales son las características en **in progress** ?

Las nuevas características, que estamos incorporando para el motor V8. Generalmente se hablan, y se espera que sean lanzadas en las versiones futuras de Node.Js, pero no sabemos cuando.

Usted puede ver la lista de *in progress*, estas caracteristicas estan disponibles en cada versión final de Node.Js puede verlas con el argumento `--v8-options`. Recuerde que estas características están incompletas en el motor V8, uselas bajo su propio riesgo.

```bash
node --v8-options | grep "in progress"
```

## En mi infraestructura se esta usando el argumento `--harmony`. ¿Debería quitarlo?

La comportamiento actual del argumento `--harmony` en Nodejs, estas características están activas en **staged**. Pero antes de todo, es un sinónimo a `--es_staging`. Como se mencionó anteriormente, estas son características, aun son inestables. Si quieres ir a lo seguro, especialmente en entornos de producción, deberia considerar la eliminación de ese argumento del runtime hasta que las características esten forma predeterminada en V8 y, en consecuencia, en Node.js. Si, mantiene el argumento activado, usted debe estar preparado para más actualizaciones Node.js para romper su código dado que la semántica pueda cambiar.

## ¿Cómo encuentro la versión del motor V8 con una versión particular de Node.js?

Node.js porvee un simple objeto de **todas** las dependencias y sus respectivas versiones, que se entregan el binario, están especificados en el objeto global `process`. En este caso el motor V8, se puede ejecutar en su terminal así.

```bash
node -p process.versions.v8
```
