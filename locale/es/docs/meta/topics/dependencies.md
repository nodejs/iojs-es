---
title: Dependencies
layout: docs.hbs
---

# Dependecias

Aqui estan todas las dependecias de Node.Js con las cuales trabaja

There are several dependencies that Node.js relies on to work the way it does.

- [Librerias](#Librerias)
  - [V8](#V8)
  - [libuv](#libuv)
  - [http-parser](#http-parser)
  - [c-ares](#c-ares)
  - [OpenSSL](#OpenSSL)
  - [zlib](#zlib)
- [Herramientas](#Herramientas)
  - [npm](#npm)
  - [gyp](#gyp)
  - [gtest](#gtest)

## Librerias

### V8

El V8 es una libreria que provee a Node.Js con un motor de JavaScript, en el cual los controles esta via V8 C++ API. V8 esta mantenido por Google, lo usa en Chrome.

- [Documentacion](https://v8docs.nodesource.com/)

### libuv

Es  una dependecia muy importate, una libreria en C que se usa en abstracto de non-blocking I/O de operaciones de una interfaz consistente a traves de todo ello apoyado del sistema operativo. Esta libreria proporciona mecanismos para menejar el sistema de archivos, DNS, red, subprocesos, pipes (Tuberias), manejo de señales (signal.h), polling y streaming. Esto incluye un grupo de subprocesos para la descarga de trabajo, en los cuales para algunas cosas que no se pueden hacer de forma asíncrona a nivel de sistema operativo.

- [Documentacion](http://docs.libuv.org/)

### http-parser

http-parser, es una libreria ligera hecha en C. Esa diseñada para no hacer ninguna llamada del sistema o asignaciones, por lo que tiene una huella muy pequeña de memoria por solicitud.

- [Documentacion](https://github.com/joyent/http-parser/)

### c-ares

Para algunas peticiones de DNS, Node.Js usa esta libreria en C. Se usa atravez del modulo "DNS" en JavaScript en `resolve()`. El resto de la bibloteca esta usado por la funcion `lookup()`, que se usa como un multihilo en `getaddrinfo(3)` y que llama a libuv. La razon por la cual se usa, es por que soporta `/etc/hosts`, `/etc/resolv.conf` y `/etc/svc.conf`, pero no cosas como mDNS

- [Documentacion](http://c-ares.haxx.se/docs.html)

### OpenSSL

Esta libreria se usa extensamente en los modulos de `tls` y `crypto`. Esta provee implementaciones ya probados, con muchas funciones criptográficas que la web moderna se basa en la seguridad.

- [Documentacion](https://www.openssl.org/docs/)

### zlib

Para una rapida comprecion y descomprecion, Node.Js usa esta libreria estandar de la industria, tambien conocido por su uso en gzip y libpng. Node.Js usa esta para sincronización, compresión y transmisión asíncrona y la descompresión.

- [Documentacion](http://www.zlib.net/manual.html)

## Herramientas

### npm

Esta herramienta, se usa como una modulardad y con eso viene la necesidad de un gestor de paquetes. Por eso se creo [NPM](http://npmjs.com), esta herramienta tiene la mayor secion de paquetes creados por a comunidad de JavaScript, lo que hace la construcion de aplicaciones en Node.Js rapida y facil.

- [Documentacion](https://docs.npmjs.com/)

### gyp

Un sistema de construcion por gyp, un generador de proyectos basado en python copiado de V8. Este puede generar arcichos de proyectos para el uso de multiples plataformas o sistemas operativos. Esta herramienta es indispensable para la compliacion de las anteriores librerias, que son dependecias necesarias de NodeJs.

- [Documentacion](https://chromium.googlesource.com/external/gyp/+/master/docs/UserDocumentation.md)

### gtest

Es una herramienta que testea el codigo, en nativo, en el cual toma Chromium. Permite una prueba en C/C++ sin necesidad de ejecutar todo el paquete.

- [Documentacion](https://code.google.com/p/googletest/wiki/V1_7_Documentation)
