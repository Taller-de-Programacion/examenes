* **Fecha**: 17/12/2019
* **Cuatrimestre**: 2° 2019
* **Tema**: 2

A continuación la resolución de algunos ejercicios correspondiente al archivo 2.pdf

## 1.

Escriba un programa (desde la inicialización hasta la liberación de los recursos) que reciba paquetes de la forma nnn+nn+....+nnnn= (numeros separados por +, seguidos de =) e imprima el resultado de la suma de cada paquete por pantalla. Al recibir un paquete vacío (“=”) debe cerrarse ordenadamente. No considere errores..

### Respuesta:

```c
#define _POSIX_C_SOURCE 200112L

#include <stdlib.h>		// strtol

#include <errno.h>		// errno, strerror
#include <stdio.h>		// printf
#include <string.h>		// memset
#include <stdbool.h>	// bool

#include <sys/types.h>
#include <sys/socket.h>
#include <netdb.h>		// struct addrinfo
#include <unistd.h>		// close

#include <ctype.h>		// isdigit

int main(int argc, char const *argv[]) {
	const char* service = argv[1];	// puerto

	int accept_socket_fd = 0;
	int client_socket_fd = 0;

	struct addrinfo hints;
	struct addrinfo* ptr;

	memset(&hints, 0, sizeof(struct addrinfo));
	hints.ai_family = AF_INET;			// IPv4
	hints.ai_socktype = SOCK_STREAM;	// TCP
	hints.ai_flags = AI_PASSIVE;		// Server: AI_PASSIVE

	getaddrinfo(NULL, service, &hints, &ptr);

	accept_socket_fd = socket(ptr->ai_family, ptr->ai_socktype, ptr->ai_protocol);

	bind(accept_socket_fd, ptr->ai_addr, ptr->ai_addrlen);

	listen(accept_socket_fd, 1);

	client_socket_fd = accept(accept_socket_fd, NULL, NULL);

	char buffer = 0;
	int operand = 0;
	int result = 0;
	bool turn_off = false;

	while (!turn_off) {
		size_t received = 0;
		while (received < 1) {
			received = recv(client_socket_fd, &buffer, 1, MSG_NOSIGNAL);
		}

		if (isdigit(buffer)) {
			operand = operand * 10 + strtol(&buffer, NULL, 10);
		} else if (buffer == '+') {
			result += operand;
			operand = 0;
		} else if (buffer == '=') {
			result += operand;
			operand = 0;
			if (result == 0) {
                turn_off = true;
			} else {
				printf("%d\n", result);
				result = 0;
			}
		}
	}

	freeaddrinfo(ptr);

	shutdown(accept_socket_fd, SHUT_RDWR);
	close(accept_socket_fd);

	shutdown(client_socket_fd, SHUT_RDWR);
	close(client_socket_fd);

	return 0;
}
```

---


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

---

## 4. ¿Qué es un functor? ¿Qué ventaja ofrece frente a una función convencional? Ejemplifique.

### Respuesta:

Un _functor_, o _function object_, es una clase que sobrecarga el operador `()`, de esta forma, permite el tratamiento de sus instancias como punteros a funciones ejecutando el comportamiento deseado definido en el método que sobrecarga el operador `operator ()`.

Al ser un objeto, además de poseer el comportamiento definido en la función, posee las ventajas de almacenar estado de forma desacoplada respecto al llamado y evita el uso de variables globales.

Un ejemplo de aplicación de un functor sería un sumador:

```c++
#include <iostream>

class Sumador {
public:
	Sumador(): total(0) {}
	~Sumador() {}
	void operator()(int value) {
		this->total += value;
	}
private:
	int total;
};

int main(int argc, char const *argv[]) {
	int numbers[] = {1, 2, 3, 4, 5};

	Sumador sumador;

	for (int i : numbers) {
		sumador(i);
	}

	return 0;
}
```

---

## 5. ¿Cómo se logra que 2 threads accedan (lectura/escritura) a un mismo recurso compartido sin que se generen problemas de consistencia? Ejemplifique.

### Respuesta:

Tras identificar la porción de código que accede al recurso compartido, la _critical section_, se bloquea la ejecución por parte de un hilo mediante el uso de mutex, el que adquiera este, será el único que podrá acceder al recurso, mientras que todos los demás deberán esperar hasta que el mutex sea liberado. Este último paso es clave, ya que de no realizarse se producirá un _dead lock_.

---

## 6. Describa el concepto de loop de eventos (events loop) utilizado en programación orientada a eventos y, en particular, en entornos de interfaz gráfica (GUIs).

### Respuesta:

El loop de eventos es el patrón que permite la ejecución de las distintas acciones encoladas en la cola de eventos por parte de los distintos actores de un programa, input del usuario, eventos temporales o automáticos. Si un evento desencolado, cuenta con un manejador, o _handler_, dentro del loop, se procede a ejecutar la acción respectiva.

```
while se debe continuar:
	evento := obtener el siguiente elemento de la cola de eventos
	if evento == salir:
		se debe continuar := false
	else if existe manejador para evento:
		ejecutar manejador
```

---

## 7. Considere la estructura `struct ejemplo { int a; char b;}`. ¿Es verdad que `sizeof (ejemplo)=sizeof(a) +sizeof(b)`? Justifique.

### Respuesta:

No es cierto, ya que depende de la arquitectura y del compilador. Debe tenerse en cuenta la alineación y el padding del procesador y los flags utilizados en el momento de la compilación.

---

## 8. ¿En qué consiste el patrón de diseño RAII? Ejemplifique.

### Respuesta:

RAII es un patrón que consiste en encapsular la inicialización y liberación de recursos en el constructor y destructor de la clase respectivamente, ya sea memoria en el heap, un archivo o un socket. De esta forma se simplifican tanto el desarrollo, como el chequeo de errores.

---

## 9. Escribir un programa ISO C que procese el archivo de enteros de 2 bytes bigendian cuyo nombre es recibido como parámetro. El procesamiento consiste en eliminar los número múltiplos de 3, trabajando sobre el mismo archivo (sin archivos intermedios ni en memoria).

### Respuesta:

```c
#include <stdio.h>		// FILE
#include <string.h>
#include <stdlib.h>
#include <unistd.h>		// truncate
#include <sys/types.h>	// truncate

#define NUM_SIZE 2
#define BASE 10

int main(int argc, char const *argv[]) {
	char buffer[NUM_SIZE];
	memset(buffer, 0, NUM_SIZE);

	FILE* fp_read = fopen(argv[1], "rb");
	FILE* fp_write = fopen(argv[1], "r+b");

	if ((fp_read == NULL) || (fp_write == NULL)) {
		exit(EXIT_FAILURE);
	}

	while (!feof(fp_read)) {
		if (fread(buffer, sizeof(char), NUM_SIZE, fp_read)) {
			short int number = (short int) strtol(buffer, NULL, BASE);

			if (number % 3 != 0) {
				fwrite(buffer, sizeof(char), NUM_SIZE, fp_write);
			}
		}
	}

	fclose(fp_read);

	if (!feof(fp_write)) {
		fflush(fp_write);
		long offset = ftell(fp_write);
		// No es C standard
		truncate(argv[1], offset);
	}

	fclose(fp_write);

	return 0;
}
```

<!-- ---

## 10. Implemente una función C++ denominada `DobleSiNo` que reciba dos listas de elementos y devuelva una nueva lista duplicando los elementos de la primera que no están en la segunda: `std::list<T> DobleSiNo(std::list<T> a,std::list<T> b)`;

### Respuesta:
-->
