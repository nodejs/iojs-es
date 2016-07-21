---
title: Dependencies
layout: docs.hbs
---

# Dependencias

Aquí están todas las dependencias de Node.Js con las cuales trabaja

- [Librerías](#Librerías)
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

## Librerías

### V8

El V8 es una librería que provee a Node.Js con un motor de JavaScript, en el cual los controles esta vía V8 C++ API. V8 está mantenido por Google, lo usa en Chrome.

- [Documentacion](https://v8docs.nodesource.com/)

### libuv

Es  una dependencia muy importante, una librería en C que se usa en abstracto de non-blocking I/O de operaciones de una interfaz consistente a través de todo ello apoyado del sistema operativo. Esta librería proporciona mecanismos para manejar el sistema de archivos, DNS, red, subprocesos, pipes (Tuberías), manejo de señales (signal.h), polling y streaming. Esto incluye un grupo de subprocesos para la descarga de trabajo, en los cuales para algunas cosas que no se pueden hacer de forma asíncrona a nivel de sistema operativo.

- [Documentacion](http://docs.libuv.org/)

### http-parser

http-parser, es una librería ligera hecha en C. Está diseñada para no hacer ninguna llamada del sistema o asignaciones, por lo que tiene una huella muy pequeña de memoria por solicitud.

- [Documentacion](https://github.com/joyent/http-parser/)

### c-ares

Para algunas peticiones de DNS, Node.Js usa esta librería en C. Se usa a través del módulo "DNS" en JavaScript en `resolve()`. El resto de la biblioteca está usado por la función `lookup()`, que se usa como un multihilo en `getaddrinfo(3)` y que llama a libuv. La razón por la cual se usa, es porque soporta `/etc/hosts`, `/etc/resolv.conf` y `/etc/svc.conf`, pero no cosas como mDNS

- [Documentacion](http://c-ares.haxx.se/docs.html)

### OpenSSL

Esta librería se usa extensamente en los modulos de `tls` y `crypto`. Esta provee implementaciones ya probados, con muchas funciones criptográficas que la web moderna se basa en la seguridad.

- [Documentacion](https://www.openssl.org/docs/)

### zlib

Para una rápida compresión y descompresión, Esta es librería estándar de la industria, también conocido por su uso en gzip y libpng. Node.Js usa esta para sincronización, compresión y transmisión asíncrona y la descompresión.

- [Documentacion](http://www.zlib.net/manual.html)

## Herramientas

### npm

Esta herramienta, se usa como una modularidad y con eso viene la necesidad de un gestor de paquetes. Por eso se creó [NPM](http://npmjs.com), esta herramienta tiene la mayor sección de paquetes creados por la comunidad de JavaScript, lo que hace la construcción de aplicaciones en Node.Js rapida y facil.

- [Documentacion](https://docs.npmjs.com/)

### gyp

Un sistema de construcción por gyp, un generador de proyectos basado en python copiado de V8. Este puede generar archivos de proyectos para el uso de múltiples plataformas o sistemas operativos. Esta herramienta es indispensable para la compilación de las anteriores librerías, que son dependencias necesarias de NodeJs.

- [Documentacion](https://chromium.googlesource.com/external/gyp/+/master/docs/UserDocumentation.md)

### gtest

Es una herramienta que testea el código, en nativo, en el cual toma Chromium. Permite una prueba en C/C++ sin necesidad de ejecutar todo el paquete.

- [Documentacion](https://code.google.com/p/googletest/wiki/V1_7_Documentation)
