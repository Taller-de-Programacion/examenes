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

<!-- ## 2. Explique qué es cada uno de los siguientes, haciendo referencia a su inicialización, su comportamiento y el area de memoria donde residen:
* a) Una variable global static
* b) Una variable local static
* c) Un atributo de clase static.

### Respuesta:

a) Una **variable global static** en memoria se encuentra en el _data segment_ mientras que su _scope_ es local al archivo donde haya sido declarada.
b) Una **variable local static** reside en el _data segment_ y su _scope_ es local al bloque de código donde haya sido declarada.
c)

--- -->

## 3. Escribir un programa ISO C que procese el archivo “nros2bytes.dat” sobre sí mismo, duplicando los enteros de 2 bytes múltiplos de 3.

### Respuesta:

```c
int main(int argc, char const *argv[]) {
	FILE* fp_read = fopen("nros2bytes.dat", "rb");
	FILE* fp_write = fopen("nros2bytes.dat", "r+b");

	if ((fp_read == NULL) || (fp_write == NULL)) {
		printf("Error\n");
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

--- -->

<!-- ## 5. Escriba el .H de una biblioteca de funciones ISO C para cadenas de caracteres. Incluya, al menos, 4 funciones.

### Respuesta:

--- -->

## 6. ¿Qué es una macro de C? Detalle las buenas prácticas para su definición. Ejemplifique

### Respuesta:

Una macro es un fragmento de código con un identificador, que al ser invocado se expande con su definición. Existen dos tipos, _Object-like_, un identificador que será reemplazado por un valor; y _function-like_, macros cuyo identificador recibe una lista de parámetros entre paréntesis. El encargado de hacer este reemplazo es el pre-procesador.

---

<!-- ## 7. Describa el proceso de transformación de código fuente a un ejecutable. Precise las etapas y las tareas desarrolladas en cada una de ellas.

### Respuesta:

--- -->

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

<!-- ## 9. Implemente una función C++ denominada `Sacar` que reciba dos listas de elementos y devuelva una nueva lista con los elementos de la primera que no están en la segunda: `std::list<T> Sacar(std::list<T> a,std::list<T> b)`;

### Respuesta:

--- -->

<!-- ## 10. Escriba un programa que reciba por línea de comandos un Puerto y una IP. El programa debe establecer una unica conexión, quedar en escucha e imprimir en stdout todo lo recibido. Al recibir el texto ‘FINAL’ debe finalizar el programa sin imprimir dicho texto.

### Respuesta:

--- -->
