# Addons

Los Addons son objetos compartidos (shared objects), dinámicamente enlazados.
Pueden proporcionar conexión para bibliotecas de C y C++. La API (por el momento)
es bastante compleja, requiriendo el conocimiento de varias bibliotecas:

  - V8 Javascript, una biblioteca de C++. Usada para interconectar con Javascript:
    Crear objetos, llamar funciones, etc. Documentada, en su mayoría, en
    el archivo de cabecera `v8.h` (`deps/v8/include/v8.h` en el árbol de
    directorios de io.js), disponible también de manera
    [online](http://izs.me/v8-docs/main.html)

  - [libuv](https://github.com/libuv/libuv), biblioteca para bucle de eventos en C.
    En cualquier momento en el que se necesite esperar a que un descriptor de
    archivo pueda ser leído, esperar un temporizador, o esperar a que una señal
    sea recibida, necesitarás libuv. Es decir, si realizas algún tipo de
    E/S, necesitas usar libuv.

  - Bibliotecas internas de io.js. La más importante es la clase `node::ObjectWrap`
    que será, probablemente, de la cual derives en la mayoría de tus proyectos.

  - Otros. Hecha un vistazo en `deps/` para ver qué más hay disponible.

io.js compila estáticamente todas sus dependencias en un único ejecutable.
Cuando compilas tu módulo, no necesitas preocuparte por enlazar ninguna de
estas bibliotecas.

Todos los ejemplos siguientes están disponibles para
[descargar](https://github.com/SametSisartenep/ejemplos-addon-node)
y deberían usarse como punto de partida para tu propio Addon.

## Hola mundo

Para empezar, hagamos un pequeño Addon que es el equivalente en C++ al
siguiente código en Javascript:

    module.exports.hola = function() { return 'mundo'; };

Primero creamos el archivo `hola.cc`:

    // hola.cc
    #include <node.h>

    using namespace v8;

    void Method(const FunctionCallbackInfo<Value>& args) {
      Isolate* isolate = Isolate::GetCurrent();
      HandleScope scope(isolate);
      args.GetReturnValue().Set(String::NewFromUtf8(isolate, "mundo"));
    }

    void init(Handle<Object> exports) {
      NODE_SET_METHOD(exports, "hola", Method);
    }

    NODE_MODULE(hola, init)

Observa que todos los Addons de io.js deben exportar una función de
inicialización:

    void Initialize (Handle<Object> exports);
    NODE_MODULE(module_name, Initialize)

Después de `NODE_MODULE` no hay punto y coma `;` ya que no es una función
(Échale un vistazo a `node.h`).

El `module_name` será el nombre final del archivo binario (módulo)
(menos el sufijo *.node*).

El código fuente tiene que compilarse en `hola.node`, el Addon binario.
Para ello, creamos un archivo llamado `binding.gyp`, que describe la
configuración para compilar el módulo en un formato similar a JSON.
Este archivo es compilado mediante
[node-gyp](https://github.com/TooTallNate/node-gyp).

    {
      "targets": [
        {
          "target_name": "hola",
          "sources": [ "hola.cc" ]
        }
      ]
    }

El siguiente paso es generar los archivos de compilación del proyecto
para nuestro sistema. Utiliza `node-gyp configure`.

Ahora tendrás, o bien un `Makefile` (en sistemas Unix) o bien un
archivo `vcxproj` (en Windows) en el directorio `build/`. Después
ejecuta el comando `node-gyp build`.

Ya tienes tu archivo empaquetado (bindings file) `.node` compilado! Los archivos
empaquetados se encuentran en `build/Release/`.

Ahora puedes usar tu addon binario en el proyecto `hola.js` de io.js
a través de un `require` al módulo `addon.node` recién compilado:

    // hello.js
    var addon = require('./build/Release/addon');

    console.log(addon.hello()); // 'world'

Por favor, mira los siguientes modelos para más información ó
<https://github.com/arturadib/node-qt> para ver un ejemplo en producción.


## Modelos de Addon

Abajo se muestran algunos modelos de Addon para ayudarte a empezar.
Consulta la [referencia online de v8](http://izs.me/v8-docs/main.html)
para ayudarte con las diversas llamadas de v8, y la
[Guía de Embebedores](http://code.google.com/apis/v8/embed.html)
de v8 para una explicación de varios conceptos como manejadores (handles),
ámbitos (scopes), plantillas de funciones (function templates), etc.

Con el fín de utilizar estos ejemplos, necesitas compilarlos mediante
`node-gyp`. Crea el siguiente archivo `binding.gyp`:

    {
      "targets": [
        {
          "target_name": "addon",
          "sources": [ "addon.cc" ]
        }
      ]
    }

En situaciones donde hayan más de un archivo `.cc`, simplemente
añade el nombre del archivo al Array de `sources`, e.g:

    "sources": ["addon.cc", "myexample.cc"]

Ahora que tienes el archivo `binding.gyp` listo, puedes configurar y
compilar el addon:

    $ node-gyp configure build


### Argumentos de funciones

El siguiente modelo muestra cómo obtener argumentos de llamadas
a funciones en Javascript y devolver un resultado. Este es el
único archivo que vas a necesitar, `addon.cc`:

    // addon.cc
    #include <node.h>

    using namespace v8;

    void Add(const FunctionCallbackInfo<Value>& args) {
      Isolate* isolate = Isolate::GetCurrent();
      HandleScope scope(isolate);

      if (args.Length() < 2) {
        isolate->ThrowException(Exception::TypeError(
            String::NewFromUtf8(isolate, "Wrong number of arguments")));
        return;
      }

      if (!args[0]->IsNumber() || !args[1]->IsNumber()) {
        isolate->ThrowException(Exception::TypeError(
            String::NewFromUtf8(isolate, "Wrong arguments")));
        return;
      }

      double value = args[0]->NumberValue() + args[1]->NumberValue();
      Local<Number> num = Number::New(isolate, value);

      args.GetReturnValue().Set(num);
    }

    void Init(Handle<Object> exports) {
      NODE_SET_METHOD(exports, "add", Add);
    }

    NODE_MODULE(addon, Init)

Puedes probarlo con el siguiente fragmento de Javascript:

    // test.js
    var addon = require('./build/Release/addon');

    console.log( 'This should be eight:', addon.add(3,5) );


### Callbacks

Puedes pasar funciones de Javascript a una función de C++, y ejecutarlas
desde ahí. Aquí está el archivo `addon.cc`:

    // addon.cc
    #include <node.h>

    using namespace v8;

    void RunCallback(const FunctionCallbackInfo<Value>& args) {
      Isolate* isolate = Isolate::GetCurrent();
      HandleScope scope(isolate);

      Local<Function> cb = Local<Function>::Cast(args[0]);
      const unsigned argc = 1;
      Local<Value> argv[argc] = { String::NewFromUtf8(isolate, "hello world") };
      cb->Call(isolate->GetCurrentContext()->Global(), argc, argv);
    }

    void Init(Handle<Object> exports, Handle<Object> module) {
      NODE_SET_METHOD(module, "exports", RunCallback);
    }

    NODE_MODULE(addon, Init)

Fíjate que en este ejemplo utilizamos la función `Init()` con dos argumentos,
siendo el último un objeto `module` completo. Esto le permite al addon
sobreescribir completamente el objeto `exports` con una única función
en lugar de añadir la función como una propiedad del mismo.

Para probarlo, ejecuta el siguiente código de Javascript:

    // test.js
    var addon = require('./build/Release/addon');

    addon(function(msg){
      console.log(msg); // 'hello world'
    });


### Fábrica de objetos

Puedes crear y distribuir objetos nuevos desde una función en C++
con este modelo de `addon.cc` que devuelve un objeto con la propiedad
`msg` que emite la cadena de caracteres (string) introducida en
`createObject()`:

    // addon.cc
    #include <node.h>

    using namespace v8;

    void CreateObject(const FunctionCallbackInfo<Value>& args) {
      Isolate* isolate = Isolate::GetCurrent();
      HandleScope scope(isolate);

      Local<Object> obj = Object::New(isolate);
      obj->Set(String::NewFromUtf8(isolate, "msg"), args[0]->ToString());

      args.GetReturnValue().Set(obj);
    }

    void Init(Handle<Object> exports, Handle<Object> module) {
      NODE_SET_METHOD(module, "exports", CreateObject);
    }

    NODE_MODULE(addon, Init)

Para probarlo en Javascript:

    // test.js
    var addon = require('./build/Release/addon');

    var obj1 = addon('hello');
    var obj2 = addon('world');
    console.log(obj1.msg+' '+obj2.msg); // 'hello world'


### Fábrica de funciones

Este modelo muestra cómo crear y distribuir una función en Javascript
que se conecta a una función de C++:

    // addon.cc
    #include <node.h>

    using namespace v8;

    void MyFunction(const FunctionCallbackInfo<Value>& args) {
      Isolate* isolate = Isolate::GetCurrent();
      HandleScope scope(isolate);
      args.GetReturnValue().Set(String::NewFromUtf8(isolate, "hello world"));
    }

    void CreateFunction(const FunctionCallbackInfo<Value>& args) {
      Isolate* isolate = Isolate::GetCurrent();
      HandleScope scope(isolate);

      Local<FunctionTemplate> tpl = FunctionTemplate::New(isolate, MyFunction);
      Local<Function> fn = tpl->GetFunction();

      // omit this to make it anonymous
      fn->SetName(String::NewFromUtf8(isolate, "theFunction"));

      args.GetReturnValue().Set(fn);
    }

    void Init(Handle<Object> exports, Handle<Object> module) {
      NODE_SET_METHOD(module, "exports", CreateFunction);
    }

    NODE_MODULE(addon, Init)

Para probarlo:

    // test.js
    var addon = require('./build/Release/addon');

    var fn = addon();
    console.log(fn()); // 'hello world'


### Conectando objetos de C++

Aquí vamos a crear un conector para el objeto/clase `MyObject` de C++,
el cual se podrá instanciar a través del operador `new` en Javascript.
Primero prepara el módulo principal, `addon.cc`:

    // addon.cc
    #include <node.h>
    #include "myobject.h"

    using namespace v8;

    void InitAll(Handle<Object> exports) {
      MyObject::Init(exports);
    }

    NODE_MODULE(addon, InitAll)

Entonces, en `myobject.h`, haz que tu conector herede de la clase
`node::ObjectWrap`:

    // myobject.h
    #ifndef MYOBJECT_H
    #define MYOBJECT_H

    #include <node.h>
    #include <node_object_wrap.h>

    class MyObject : public node::ObjectWrap {
     public:
      static void Init(v8::Handle<v8::Object> exports);

     private:
      explicit MyObject(double value = 0);
      ~MyObject();

      static void New(const v8::FunctionCallbackInfo<v8::Value>& args);
      static void PlusOne(const v8::FunctionCallbackInfo<v8::Value>& args);
      static v8::Persistent<v8::Function> constructor;
      double value_;
    };

    #endif

Y en `myobject.cc`, implementa los diversos métodos que vas a exponer.
Aquí vamos a exponer el método `plusOne`, añadiéndolo al prototipo
(prototype) del constructor:

    // myobject.cc
    #include "myobject.h"

    using namespace v8;

    Persistent<Function> MyObject::constructor;

    MyObject::MyObject(double value) : value_(value) {
    }

    MyObject::~MyObject() {
    }

    void MyObject::Init(Handle<Object> exports) {
      Isolate* isolate = Isolate::GetCurrent();

      // Prepare constructor template
      Local<FunctionTemplate> tpl = FunctionTemplate::New(isolate, New);
      tpl->SetClassName(String::NewFromUtf8(isolate, "MyObject"));
      tpl->InstanceTemplate()->SetInternalFieldCount(1);

      // Prototype
      NODE_SET_PROTOTYPE_METHOD(tpl, "plusOne", PlusOne);

      constructor.Reset(isolate, tpl->GetFunction());
      exports->Set(String::NewFromUtf8(isolate, "MyObject"),
                   tpl->GetFunction());
    }

    void MyObject::New(const FunctionCallbackInfo<Value>& args) {
      Isolate* isolate = Isolate::GetCurrent();
      HandleScope scope(isolate);

      if (args.IsConstructCall()) {
        // Invoked as constructor: `new MyObject(...)`
        double value = args[0]->IsUndefined() ? 0 : args[0]->NumberValue();
        MyObject* obj = new MyObject(value);
        obj->Wrap(args.This());
        args.GetReturnValue().Set(args.This());
      } else {
        // Invoked as plain function `MyObject(...)`, turn into construct call.
        const int argc = 1;
        Local<Value> argv[argc] = { args[0] };
        Local<Function> cons = Local<Function>::New(isolate, constructor);
        args.GetReturnValue().Set(cons->NewInstance(argc, argv));
      }
    }

    void MyObject::PlusOne(const FunctionCallbackInfo<Value>& args) {
      Isolate* isolate = Isolate::GetCurrent();
      HandleScope scope(isolate);

      MyObject* obj = ObjectWrap::Unwrap<MyObject>(args.Holder());
      obj->value_ += 1;

      args.GetReturnValue().Set(Number::New(isolate, obj->value_));
    }

Pruébalo con:

    // test.js
    var addon = require('./build/Release/addon');

    var obj = new addon.MyObject(10);
    console.log( obj.plusOne() ); // 11
    console.log( obj.plusOne() ); // 12
    console.log( obj.plusOne() ); // 13

### Fábrica de objetos conectados

Esto es útil cuando quieres crear objetos nativos sin tener que recurrir
al operador `new` en Javascript, e.g:

    var obj = addon.createObject();
    // instead of:
    // var obj = new addon.Object();

Vamos a registrar nuestro método `createObject` en `addon.cc`:

    // addon.cc
    #include <node.h>
    #include "myobject.h"

    using namespace v8;

    void CreateObject(const FunctionCallbackInfo<Value>& args) {
      Isolate* isolate = Isolate::GetCurrent();
      HandleScope scope(isolate);
      MyObject::NewInstance(args);
    }

    void InitAll(Handle<Object> exports, Handle<Object> module) {
      MyObject::Init();

      NODE_SET_METHOD(module, "exports", CreateObject);
    }

    NODE_MODULE(addon, InitAll)

En `myobject.h`, ahora incluimos el método estático `NewInstance` que
se encarga de instanciar el objeto (es decir, hace el trabajo de `new`
en Javascript):

    // myobject.h
    #ifndef MYOBJECT_H
    #define MYOBJECT_H

    #include <node.h>
    #include <node_object_wrap.h>

    class MyObject : public node::ObjectWrap {
     public:
      static void Init();
      static void NewInstance(const v8::FunctionCallbackInfo<v8::Value>& args);

     private:
      explicit MyObject(double value = 0);
      ~MyObject();

      static void New(const v8::FunctionCallbackInfo<v8::Value>& args);
      static void PlusOne(const v8::FunctionCallbackInfo<v8::Value>& args);
      static v8::Persistent<v8::Function> constructor;
      double value_;
    };

    #endif

La implementación es similar al anterior `myobject.cc`:

    // myobject.cc
    #include <node.h>
    #include "myobject.h"

    using namespace v8;

    Persistent<Function> MyObject::constructor;

    MyObject::MyObject(double value) : value_(value) {
    }

    MyObject::~MyObject() {
    }

    void MyObject::Init() {
      Isolate* isolate = Isolate::GetCurrent();
      // Prepare constructor template
      Local<FunctionTemplate> tpl = FunctionTemplate::New(isolate, New);
      tpl->SetClassName(String::NewFromUtf8(isolate, "MyObject"));
      tpl->InstanceTemplate()->SetInternalFieldCount(1);

      // Prototype
      NODE_SET_PROTOTYPE_METHOD(tpl, "plusOne", PlusOne);

      constructor.Reset(isolate, tpl->GetFunction());
    }

    void MyObject::New(const FunctionCallbackInfo<Value>& args) {
      Isolate* isolate = Isolate::GetCurrent();
      HandleScope scope(isolate);

      if (args.IsConstructCall()) {
        // Invoked as constructor: `new MyObject(...)`
        double value = args[0]->IsUndefined() ? 0 : args[0]->NumberValue();
        MyObject* obj = new MyObject(value);
        obj->Wrap(args.This());
        args.GetReturnValue().Set(args.This());
      } else {
        // Invoked as plain function `MyObject(...)`, turn into construct call.
        const int argc = 1;
        Local<Value> argv[argc] = { args[0] };
        Local<Function> cons = Local<Function>::New(isolate, constructor);
        args.GetReturnValue().Set(cons->NewInstance(argc, argv));
      }
    }

    void MyObject::NewInstance(const FunctionCallbackInfo<Value>& args) {
      Isolate* isolate = Isolate::GetCurrent();
      HandleScope scope(isolate);

      const unsigned argc = 1;
      Handle<Value> argv[argc] = { args[0] };
      Local<Function> cons = Local<Function>::New(isolate, constructor);
      Local<Object> instance = cons->NewInstance(argc, argv);

      args.GetReturnValue().Set(instance);
    }

    void MyObject::PlusOne(const FunctionCallbackInfo<Value>& args) {
      Isolate* isolate = Isolate::GetCurrent();
      HandleScope scope(isolate);

      MyObject* obj = ObjectWrap::Unwrap<MyObject>(args.Holder());
      obj->value_ += 1;

      args.GetReturnValue().Set(Number::New(isolate, obj->value_));
    }

Pruébalo con:

    // test.js
    var createObject = require('./build/Release/addon');

    var obj = createObject(10);
    console.log( obj.plusOne() ); // 11
    console.log( obj.plusOne() ); // 12
    console.log( obj.plusOne() ); // 13

    var obj2 = createObject(20);
    console.log( obj2.plusOne() ); // 21
    console.log( obj2.plusOne() ); // 22
    console.log( obj2.plusOne() ); // 23


### Distribuyendo objetos conectados

Además de conectar y devolver objetos de C++, puedes distribuirlos
desconectándolos mediante la función de ayuda (helper function)
`node::ObjectWrap::Unwrap` de io.js. En el siguiente `addon.cc` introducimos
una función `add()` que puede hacer uso de dos objetos `MyObject`:

    // addon.cc
    #include <node.h>
    #include <node_object_wrap.h>
    #include "myobject.h"

    using namespace v8;

    void CreateObject(const FunctionCallbackInfo<Value>& args) {
      Isolate* isolate = Isolate::GetCurrent();
      HandleScope scope(isolate);
      MyObject::NewInstance(args);
    }

    void Add(const FunctionCallbackInfo<Value>& args) {
      Isolate* isolate = Isolate::GetCurrent();
      HandleScope scope(isolate);

      MyObject* obj1 = node::ObjectWrap::Unwrap<MyObject>(
          args[0]->ToObject());
      MyObject* obj2 = node::ObjectWrap::Unwrap<MyObject>(
          args[1]->ToObject());

      double sum = obj1->value() + obj2->value();
      args.GetReturnValue().Set(Number::New(isolate, sum));
    }

    void InitAll(Handle<Object> exports) {
      MyObject::Init();

      NODE_SET_METHOD(exports, "createObject", CreateObject);
      NODE_SET_METHOD(exports, "add", Add);
    }

    NODE_MODULE(addon, InitAll)

Para hacer las cosas más interesantes, vamos a introducir un un método
público en `myobject.h`, así podemos probar valores privados después de
desconectar el objeto:

    // myobject.h
    #ifndef MYOBJECT_H
    #define MYOBJECT_H

    #include <node.h>
    #include <node_object_wrap.h>

    class MyObject : public node::ObjectWrap {
     public:
      static void Init();
      static void NewInstance(const v8::FunctionCallbackInfo<v8::Value>& args);
      inline double value() const { return value_; }

     private:
      explicit MyObject(double value = 0);
      ~MyObject();

      static void New(const v8::FunctionCallbackInfo<v8::Value>& args);
      static v8::Persistent<v8::Function> constructor;
      double value_;
    };

    #endif

La implementación de `myobject.cc` es similar a los anteriores:

    // myobject.cc
    #include <node.h>
    #include "myobject.h"

    using namespace v8;

    Persistent<Function> MyObject::constructor;

    MyObject::MyObject(double value) : value_(value) {
    }

    MyObject::~MyObject() {
    }

    void MyObject::Init() {
      Isolate* isolate = Isolate::GetCurrent();

      // Prepare constructor template
      Local<FunctionTemplate> tpl = FunctionTemplate::New(isolate, New);
      tpl->SetClassName(String::NewFromUtf8(isolate, "MyObject"));
      tpl->InstanceTemplate()->SetInternalFieldCount(1);

      constructor.Reset(isolate, tpl->GetFunction());
    }

    void MyObject::New(const FunctionCallbackInfo<Value>& args) {
      Isolate* isolate = Isolate::GetCurrent();
      HandleScope scope(isolate);

      if (args.IsConstructCall()) {
        // Invoked as constructor: `new MyObject(...)`
        double value = args[0]->IsUndefined() ? 0 : args[0]->NumberValue();
        MyObject* obj = new MyObject(value);
        obj->Wrap(args.This());
        args.GetReturnValue().Set(args.This());
      } else {
        // Invoked as plain function `MyObject(...)`, turn into construct call.
        const int argc = 1;
        Local<Value> argv[argc] = { args[0] };
        Local<Function> cons = Local<Function>::New(isolate, constructor);
        args.GetReturnValue().Set(cons->NewInstance(argc, argv));
      }
    }

    void MyObject::NewInstance(const FunctionCallbackInfo<Value>& args) {
      Isolate* isolate = Isolate::GetCurrent();
      HandleScope scope(isolate);

      const unsigned argc = 1;
      Handle<Value> argv[argc] = { args[0] };
      Local<Function> cons = Local<Function>::New(isolate, constructor);
      Local<Object> instance = cons->NewInstance(argc, argv);

      args.GetReturnValue().Set(instance);
    }

Pruébalo con:

    // test.js
    var addon = require('./build/Release/addon');

    var obj1 = addon.createObject(10);
    var obj2 = addon.createObject(20);
    var result = addon.add(obj1, obj2);

    console.log(result); // 30
