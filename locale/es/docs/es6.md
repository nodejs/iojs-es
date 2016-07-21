---
title: ECMAScript 2015 (ES6) and beyond
layout: docs.hbs
---
# ECMAScript 2015 (ES6) y mas alla

Node.Js esta construyendo en nuevas versiones de [motor V8](https://developers.google.com/v8/). Manteniendo dia a dia, con las ultimas versiones, podemos asegurar que tendera nuevas caracteristas de [JavaScript ECMA-262 specification](http://www.ecma-international.org/publications/standards/Ecma-262.htm) que seran llevadas por los desarrolladores Node.js a su tiempo y forma, así como las continuas mejoras de rendimiento y estabilidad.

Las caracteristicas de ECMAScript 2015 (ES6), se pueden dividir en tres grandes grupos:

* Caracteristicas **shipping**, en el cual se motor V8 considera estable, se **activan automaticamente** en NodeJs en la version por defecto, y **NO** requiere ningun tipo de argumento inicial.
* Caracteristicas **Staged**, estas caracteristicas no son consideradas estables, por lo tanto requiere el argumento `--harmony` al iniciar el runtime.
* Caracteristicas **In progress**, estas se necesitan ser activadas individualmente, con la respectivo argumento de `--harmony`, no es recomendable por que esta **en prueba**. Nota: Estos argumentos y caracteristicas pueden cambiar o eliminarse sin previo aviso por el motor V8.

## Cuales son las caracteristicas de Node.Js en la version por defecto?

En la Web [node.green](http://node.green) provee una excelente resumen de las caracteristicas de ECMAScript, soportadas en varias versiones de Node.JS.

## Cuales son las caracteristicas en **in progress** ?

Las nuevas caracteristicas, que estamos incorporando para el motor V8. Generalmente se hablan, y se espera que sean lanadas en las versiones futuras de Node.Js, pero no sabemos cuando.

Usted puede ver la lista de *in progress*, estas caracteristicas esta disponibles en cada version final de Node.Js puede verlas con el argumento `--v8-options`. Recuerde que estas caracteristicas estan incompletas y posiblemente no estan completas en el motor V8, usuelas bajo su propio riesgo.

```bash
node --v8-options | grep "in progress"
```

## En mi infrastuctura usuando el argumento `--harmony`. Deberia quitarlo?

La comportamiento actual del argumento `--harmony` en Nodejs, estas caracteristicas esten activas en **staged**. Pero antes de todo, es un sinonimo a `--es_staging`. Como se mencionó anteriormente, estos son características que no han sido considerados estables aún completamente. Si quieres ir a lo seguro, especialmente en entornos de producción, considerar la eliminación de este argumento del runtime hasta que las caracteristicas esten forma predeterminada en V8 y, en consecuencia, sobre Node.js. Si se mantiene esta activada, usted debe estar preparado para más actualizaciones Node.js para romper su codigo dado que la semantica pueda cambiar.

## ¿Cómo encuentro la versión del motor V8 con una versión particular de Node.js?

Node.js porve una simple via para ver la lista de **todas** las dependecias y sus respectivas versiones, que se entregan con el binario, estan especificados en el objeto global `process`. En este caso el motor V8, se puede ejecutar en su terminal asi.

```bash
node -p process.versions.v8
```
