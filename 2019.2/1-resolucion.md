* **Fecha**: 10/12/2019
* **Cuatrimestre**: 2° 2019
* **Tema**: 1

A continuación la resolución de algunos ejercicios correspondiente al archivo 1.pdf

## 1. Explique breve y concretamente qué es f:
```c
int (*f) (short int *, char[3]);
```

### Respuesta:

`f` es un puntero a función que toma como parámetros un puntero a `short int` y un `array` de 3 `char` retornando un `int`.

---

## 2. Explique qué es cada uno de los siguientes, haciendo referencia a su inicialización, su comportamiento y el area de memoria donde residen:
* a) Una variable global static
* b) Una variable local static
* c) Un atributo de clase static.

### Respuesta:

a) Una **variable global static** en memoria se encuentra en el _data segment_ mientras que su _scope_ es local al archivo donde haya sido declarada y el _lifetime_ es desde el inicio hasta la finalización del proceso.
b) Una **variable local static** reside en el _data segment_, su _scope_ es local al bloque de código donde haya sido declarada y el _lifetime_ comenzará la primera vez que se acceda al bloque donde haya sido declarada hasta el final del proceso.
c) Un **atributo de clase static** en memoria se encuentra en el _data segment_, su _lifetime_ es global y su _scope_ se encuentra en su función de la visibilidad que se le haya dado al atributo dentro de la definición de la clase

---

## 3. Escribir un programa ISO C que procese el archivo “nros2bytes.dat” sobre sí mismo, duplicando los enteros de 2 bytes múltiplos de 3.

### Respuesta:

```c
#include <stdlib.h>
#include <stdio.h>
#include <string.h>
#include <stdint.h>

#define FILENAME "nros2bytes.dat"

int main(int argc, char const *argv[]) {
	FILE* fp_read = fopen(FILENAME, "rb");
	FILE* fp_write = fopen(FILENAME, "r+b");

	if ((fp_read == NULL) || (fp_write == NULL)) {
		return 1;
	}

	int16_t number = 0;

	while (!feof(fp_read)) {
		if (fread(&number, sizeof(int16_t), 1, fp_read)) {
			if (number % 3 == 0) {
				int offset = ftell(fp_read);
				fseek(fp_write, offset, SEEK_SET);
				int16_t next_number = 0;
				while (!feof(fp_read)) {
					fwrite(&number, sizeof(int16_t), 1, fp_write);
					if (fread(&next_number, sizeof(int16_t), 1, fp_read)) {
						number = next_number;
					}
				}
				fseek(fp_read, offset + sizeof(int16_t), SEEK_SET);
				fseek(fp_write, offset + sizeof(int16_t), SEEK_SET);
			}
		}
	}

	fclose(fp_read);
	fclose(fp_write);

	return 0;
}
```
---

<!-- ## 4. ¿Cómo se logra que 2 threads accedan (lectura/escritura) a un mismo recurso compartido sin que se generen problemas de consistencia? Ejemplifique.

### Respuesta:

Se encuentra en 2019.2/2-resolucion.md, ejercicio 5.

--- -->

## 5. Escriba el .H de una biblioteca de funciones ISO C para cadenas de caracteres. Incluya, al menos, 4 funciones.

### Respuesta:

```c
#ifndef __STRING_H__
#define __STRING_H__

typedef struct string {
	char* _data;	// privado
} string_t;

// PRE :
// POST: Inicializa estructura
void string_init(string_t* self);

// PRE : self inicializado mediante string_init
// POST: Libera recursos asociados a self
void string_destroy(string_t* self);

// PRE : self inicializado mediante string_init
// POST: self contendrá el contenido de text
void string_set_text(string_t* self, const char* text);

// PRE : self inicializado mediante string_init
// POST: retorna el largo de la cadena contenida en self
size_t string_length(string_t* self);

// PRE : self y other inicializado mediante string_init
// POST: concatena en self el contenido de other
void string_concat(string_t* self, string_t* other);

// PRE : self y copy inicializado mediante string_init
// 		 0 > position > length
// POST: copia en copy el contenido de hasta length caracteres de self desde la posición position
void string_copy(string_t* self, string_t* copy, size_t position, size_t length);

#endif	// __STRING_H__
```
---

## 6. ¿Qué es una macro de C? Detalle las buenas prácticas para su definición. Ejemplifique

### Respuesta:

Una macro es un fragmento de código con un identificador, que al ser invocado se expande con su definición. Existen dos tipos, _Object-like_, un identificador que será reemplazado por un valor; y _function-like_, macros cuyo identificador recibe una lista de parámetros entre paréntesis. El encargado de hacer este reemplazo es el pre-procesador.

---

## 7. Describa el proceso de transformación de código fuente a un ejecutable. Precise las etapas y las tareas desarrolladas en cada una de ellas.

### Respuesta:

El proceso de compilación consta de varias etapas:

1. **Precompilación**: aquí se analizan las sentencias para el precompilador, las que comienzan con `#`, la sustitución de las declaraciones de macros, `#define`, por sus respectivos valores y el reemplazo de las sentencias `#include` por el contenido del archivo indicado. En conjunto con estas keywords, también existen las sentencias condicionales `#ifndef`, o no realizar más un reemplazo, `#undef`

2. **Compilación**: En esta etapa se realiza el análisis sintáctico, traducción, a instrucciones de lenguaje ensamblador, y ensamblado. La salida de esta estapa son archivos objeto, capaz de ser interpretado por un procesador, pero aún no ejecutable

3. **Link-edición**: esta es la última etapa del proceso, aquí en enlazan los distintos archivos objeto generando finalmente el ejecutable, se resuelven las últimas dependencias, definiciones en otros archivos por ejemplo. Existen dos formas de linkear, una **estática**, en esta se incluyen en tiempo de compilación dentro del ejecutable todo el código necesario, permitiendo ser portable a distintas computadoras que compartan arquitectura al costo de un archivo de mayor tamaño respecto a la segunda forma de linkeo, dinámico, aquí al contrario se genera un ejecutable de menor tamaño pero que requiere la existencia de los archivos objeto en la máquina donde se corra, resolviendo las dependencias en tiempo de ejecución.

---

## 8. Indique la salida del siguiente programa:
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

El funcionamiento de constructores y destructores en clases derivadas es similar al de una pila, los construtores ejecutando un `push` mientras que los destructores un `pop`, por lo que la salida del programa será:

```
A()
B()
~B()
~A()
```

---

## 9. Implemente una función C++ denominada `Sacar` que reciba dos listas de elementos y devuelva una nueva lista con los elementos de la primera que no están en la segunda: `std::list<T> Sacar(std::list<T> a,std::list<T> b)`;

### Respuesta:

```cpp
template<class T>
std::list<T> Sacar(std::list<T> a, std::list<T> b) {
	std::list<T> result;

	for (auto& item_a: a) {
		typename std::list<T>::iterator it = std::find(b.begin(), b.end(), item_a);
		if (it == b.end()) {
			result.push_back(item_a);
		}
	}

	return result;
}
```

---

<!-- ## 10. Escriba un programa que reciba por línea de comandos un Puerto y una IP. El programa debe establecer una unica conexión, quedar en escucha e imprimir en stdout todo lo recibido. Al recibir el texto ‘FINAL’ debe finalizar el programa sin imprimir dicho texto.

### Respuesta:

--- -->
