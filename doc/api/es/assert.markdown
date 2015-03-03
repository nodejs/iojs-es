# Assert

    Estabilidad: 5 - Bloqueada

Este módulo se usa para escribir pruebas unitarias para tus aplicaciones, puedes acceder con `require('assert')`.

## assert.fail(actual, esperado, mensage, operador)

Lanza una execepción mostrando los valores para `actual` y `esperado` separado por el operador proporcionado.

## assert(valor, mensage), assert.ok(valor[, mensage])

Prueba si el valor es válido, esto es equivalente a `assert.equal(true, !!valor, mensage);`

## assert.equal(actual, esperado[, mensage])

Prueba superficialmente, la igualdad por coerción con el operador de igualdad ( `==` ).

## assert.notEqual(actual, esperado[, mensage])

Tests shallow, coercive non-equality with the not equal comparison operator ( `!=` ).

## assert.deepEqual(actual, esperado[, mensage])

Pruebas de igualdad por profundidad.

## assert.notDeepEqual(actual, esperado[, mensage])

Pruebas cualquier desigualdad por profundidad.

## assert.strictEqual(actual, esperado[, mensage])

Pruebas igualdad estricta, tal y como son determinadas por el operador de igualdad estricta ( `===` ).

## assert.notStrictEqual(actual, esperado[, mensage])

Pruebas de igualdad no estricta, tal y como son determinadas por el operador de igualdad no estricta ( `!==` ).

## assert.throws(bloque[, error][, mensage])

Espera que `bloque` lance un error. `error` puede ser un constructor, `RegExp` o una functión de validación.

Validar instanceof usando el constructor:

    assert.throws(
      function() {
        throw new Error("Valor inválido");
      },
      Error
    );

Validar un mensage de error usando RegExp:

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

## assert.doesNotThrow(bloque[, mensage])

Espera que `bloque` no lance un error, ver `assert.throws` para una descripción más detallada.

## assert.ifError(valor)

Prueba si valor no es un valor falos, lanza un error si el valor es verdadero. Útil cuando se está probando el primer argumento, `error`para callbacks.
