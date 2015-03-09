# Assert

    Estabilidad: 5 - Bloqueada

Este módulo se usa para escribir pruebas unitarias para tus aplicaciones, puedes acceder con `require('assert')`.

## assert.fail(actual, esperado, mensaje, operador)

Lanza una excepción mostrando los valores para `actual` y `esperado` separados por el operador proporcionado.

## assert(valor, mensaje), assert.ok(valor[, mensaje])

Prueba si el valor es válido, esto es equivalente a `assert.equal(true, !!valor, mensaje);`

## assert.equal(actual, esperado[, mensaje])

Prueba superficialmente, la igualdad por coerción con el operador de igualdad ( `==` ).

## assert.notEqual(actual, esperado[, mensaje])

Tests shallow, coercive non-equality with the not equal comparison operator ( `!=` ).

## assert.deepEqual(actual, esperado[, mensaje])

Pruebas de igualdad por profundidad.

## assert.notDeepEqual(actual, esperado[, mensaje])

Pruebas cualquier desigualdad por profundidad.

## assert.strictEqual(actual, esperado[, mensaje])

Pruebas igualdad estricta, tal y como son determinadas por el operador de igualdad estricta ( `===` ).

## assert.notStrictEqual(actual, esperado[, mensaje])

Pruebas de igualdad no estricta, tal y como son determinadas por el operador de igualdad no estricta ( `!==` ).

## assert.throws(bloque[, error][, mensaje])

Espera que `bloque` lance un error. `error` puede ser un constructor, `RegExp` o una functión de validación.

Validar instanceof usando el constructor:

    assert.throws(
      function() {
        throw new Error("Valor inválido");
      },
      Error
    );

Validar un mensaje de error usando RegExp:

    assert.throws(
      function() {
        throw new Error("Valor inválido");
      },
      /valor/
    );

Validar un error con con una función:

    assert.throws(
      function() {
        throw new Error("Valor inválido");
      },
      function(err) {
        if ( (err instanceof Error) && /valor/.test(err) ) {
          return true;
        }
      },
      "Valor inesperado"
    );

## assert.doesNotThrow(bloque[, mensaje])

Espera que `bloque` no lance un error, ver `assert.throws` para una descripción más detallada.

## assert.ifError(valor)

Prueba si valor no es un valor falos, lanza un error si el valor es verdadero. Útil cuando se está probando el primer argumento, `error`para callbacks.
