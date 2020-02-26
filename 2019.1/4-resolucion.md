* **Fecha**: 06/08/2019
* **Cuatrimestre**: 1° 2019
* **Tema**: 4

A continuación la resolución de algunos ejercicios correspondiente al archivo 4.pdf
<!--
## 1. ¿Qué es una macro de C? Enumere buenas prácticas para su definición.

### Respuesta:

Se encuentra en 2019.2/1-resolucion.md, ejercicio 6
---
 -->
<!--
## 2. Describa el proceso de transformación de código fuente a un ejecutable. Precise las etapas y las tareas desarrolladas en cada una de ellas.

### Respuesta:

Se encuentra en 2019.2/1-resolucion.md, ejercicio 7
---
 -->

## 3. Explique breve y concretamente qué es f: `char (*f) (int *, char[3]);`

### Respuesta:

f es un puntero a una función que recibe como parámetros un puntero a entero y un array de 3 char, devolviendo un char.

---

## 4. Escribir un programa ISO C que procese el archivo “nros_2bytes_bigendian.dat” sobre sí mismo, eliminando los número múltiplos de 7.

### Respuesta:

```c
#include <stdlib.h>
#include <stdio.h>
#include <string.h>
#include <stdint.h>

#include <arpa/inet.h>	// ntohs
#include <unistd.h>		// truncate
#include <sys/types.h>	// truncate

#define FILENAME "nros_2bytes_bigendian.dat"
#define ELEMENTS 1

int main(int argc, char const *argv[]) {
	FILE* fp_read = fopen(FILENAME, "rb");
	FILE* fp_write = fopen(FILENAME, "r+b");

	if ((fp_read == NULL) || (fp_write == NULL)) {
		printf("Error\n");
		return 1;
	}

	int16_t number = 0;

	while (!feof(fp_read)) {
		if (fread(&number, sizeof(int16_t), 1, fp_read)) {
			number = (int16_t) ntohs(number);
			if (number % 7 != 0) {
				number = (int16_t) htons(number);
				fwrite(&number, sizeof(int16_t), ELEMENTS, fp_write);
			}
		}
	}

	fclose(fp_read);

	if (!feof(fp_write)) {
		fflush(fp_write);
		long offset = ftell(fp_write);
		truncate(FILENAME, offset);	// No es C standard
	}

	fclose(fp_write);

	return 0;
}
```

---

## 5. Implemente una función C++ denominada DobleSegunda que reciba dos listas de elementos y devuelva una nueva lista duplicando los elementos de la primera si están en la segunda: `std::list<T> DobleSegunda(std::list<T> a,std::list<T> b);`

### Respuesta:

```cpp
template <class T>
std::list<T> DobleSegunda(std::list<T> a,std::list<T> b) {
	std::list<T> result;

	for (auto& item_a: a) {
		result.push_back(item_a);
		auto it = std::find(b.begin(), b.end(), item_a);
		if (it != b.end()) {
			result.push_back(item_a);
		}
	}

	return result;
}
```

---

<!--
## 6. Escriba un programa que reciba por línea de comandos un Puerto y una IP. El programa debe aceptar una unica conexión e imprimir en stdout todo lo recibido. Al recibir el texto ‘FIN’ debe finalizar el programa sin imprimir dicho texto.

### Respuesta:

Muy parecido a 2019.2/1-resolucion.md, ejercicio 10
---
 -->

## 7. Escriba el .H de una biblioteca de funciones ISO C para números complejos. Incluya, al menos, 4 funciones.

### Respuesta:

```c
typedef struct complex {
	float _re;  // private
	float _im;  // private
} complex_t;

void complex_init(complex_t *self, float re, float im);

void complex_destroy(complex_t *self);

void complex_add(complex_t *self, complex_t *a);

float complex_module(const complex_t *self);

void complex_print(const complex_t *self);
```
---

<!--
## 8. Explique qué es cada uno de los siguientes, haciendo referencia a su inicialización, su comportamiento y el area de memoria donde residen:
* a) Una variable global static
* b) Una variable local static
* c) Un atributo de clase static.

### Respuesta:

Se encuentra en 2019.2/1-resolucion.md, ejercicio 2

---
 -->
<!--
## 9. ¿Cómo se logra que 2 threads accedan (lectura/escritura) a un mismo recurso compartido sin
que se generen problemas de consistencia? Ejemplifique.

### Respuesta:

Se encuentra en 2019.2/2-resolucion.md, ejercicio 5.

---
 -->
<!--
## 10. Indique la salida del siguiente programa:

```cpp
class A {
	A() {
		cout << "A()" << endl;
	}
	~A() {
		cout << "~A()" << endl;
	}
};

class B : public A {
	B() {
		cout << "B()" << endl;
	}
	~B() {
		cout << "~B()" << endl;
	}
};

int main () {
	B b;
	return 0;
}
```

### Respuesta:

Se encuentra en 2019.2/1-resolucion.md, ejercicio 8.
 -->