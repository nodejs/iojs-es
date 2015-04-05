# Assert

    Estabilidad: 5 - bloqueada

Este módulo se usa para escribir pruebas unitarias para tus aplicaciones, puedes acceder con `require('assert')`.

## assert.fail(actual, expected, message, operador)

Lanza una excepción mostrando los valuees para `actual` y `expected` separados por el operador proporcionado.

## assert(value, message), assert.ok(value[, message])

Prueba si el value es verdadero, esto es equivalente a `assert.equal(true, !!value, message);`

## assert.equal(actual, expected[, message])

Prueba superficialmente, la igualdad por coerción con el operador de igualdad ( `==` ).

## assert.notEqual(actual, expected[, message])

Prueba superficialmente, desigualdad por coerción con el operador de comparación de desigualdad ( `!=` ).

## assert.deepEqual(actual, expected[, message])

Prueba la igualdad por profundidad.

## assert.notDeepEqual(actual, expected[, message])

Prueba por cualquier desigualdad en profundidad.

## assert.strictEqual(actual, expected[, message])

Prueba igualdad estricta, tal y como son determinadas por el operador de igualdad estricta ( `===` ).

## assert.notStrictEqual(actual, expected[, message])

Prueba de igualdad no estricta, tal y como es determinado por el operador de desigualdad estricto ( `!==` ).

## assert.throws(block[, error][, message])

Espera que `block` lance un error. `error` puede ser un constructor, `RegExp` o una función de validación.

Validar instanceof usando el constructor:

    assert.throws(
      function() {
        throw new Error("valor inválido");
      },
      Error
    );

Validar un message de error usando RegExp:

    assert.throws(
      function() {
        throw new Error("valor inválido");
      },
      /value/
    );

Validación de error personalizada:

    assert.throws(
      function() {
        throw new Error("valor inválido");
      },
      function(err) {
        if ( (err instanceof Error) && /valor/.test(err) ) {
          return true;
        }
      },
      "valor inesperado"
    );

## assert.doesNotThrow(block[, message])

Espera que `block` no lance un error, ver `assert.throws` para una descripción más detallada.

## assert.ifError(value)

Prueba si value no es un value falso, lanza un error si el value es verdadero. Útil cuando se está probando el primer argumento, `error`para callbacks.
