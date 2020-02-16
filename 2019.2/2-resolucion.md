* Fecha: 17/12/2019
* Cuatrimestre: 2°/2019
* Tema: 2

A continuación la resolución de algunos ejercicios correspondiente al archivo `2.pdf`

## 2. Explique breve y concretamente qué es f:
```c
int (*f) (short *, char[4]);
```

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

---

## 3. Analice el siguiente código y determine lo que se imprime (valor de Pi)
```c
main() {
	int *Pi = 1000;
	Pi++;
	printf("Pi apunta a la dirección: %l", (long)Pi);
}
```

### Respuesta:

`Pi` es una variable que almacena la dirección de memoria cuyo contenido es un `int`. El operador `++` se basa en el tamaño del objeto apuntado, al aplicarlo sobre Pi, apuntará al siguiente. Asumiendo una arquitectura de 64 bits y `sizeof(int) == 4` entonces la salida del código anterior será:

```c
Pi apunta a la dirección: 1004
```
