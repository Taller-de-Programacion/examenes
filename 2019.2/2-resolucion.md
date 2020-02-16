* Fecha: 17/12/2019
* Cuatrimestre: 2°/2019
* Tema: 2

A continuación la resolución de algunos ejercicios correspondiente al archivo `2.pdf`

## 2. Explique breve y concretamente qué es f:
```c
int (*f) (short *, char[4]);
```

---

### Respuesta:

Tomando `(*f) == X`, la expresión se convierte en:
```c
int X (short *, char[4]);
```

Así, la expresión anterior es la firma de una función que toma como parámetros un puntero a `short` y un arreglo de 4 elementos del tipo `char`, retornando un `int`, por lo tanto:

```c
int (*f) (short *, char[4]);
```

Es la declaración de la variable `f` como el puntero a una función con parámetros: puntero a `short` y un arreglo de 4 elementos del tipo `char`; retornando un entero `int`.