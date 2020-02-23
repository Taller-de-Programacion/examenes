* **Fecha**: 13/08/2019
* **Cuatrimestre**: 1° 2019
* **Tema**: 5

A continuación la resolución de algunos ejercicios correspondiente al archivo 5.pdf
<!--
## 1. Escriba una rutina (para ambiente gráfico Windows o Linux) que dibuje un triángulo amarillo del tamaño de la ventana.

### Respuesta:

---
-->
## 2. Escriba las siguientes definiciones/declaraciones:
* a) Declaración de un puntero a puntero a entero largo con signo.
* b) Definición de la función RESTA, que tome dos enteros largos con signo y devuelva su resta. Esta función debe ser visible en todos los módulos del programa.
* c) Definición de un caracter solamente visible en el módulo donde se define.

### Respuesta:

* a) `int *(*a)`
* b) `long int RESTA(long int a, long int b)`
* c) `static char c`

---

## 3. Declare la clase TELEFONO para encapsular una cadena numérica correspondiente a un teléfono. Incluya al menos: `Constructor(area, numero)`, `Constructor move` y `Constructor de Copia`; `Operador <<`, `==`, `=`, `long` y `>>`. Implemente el operador `>>`.

### Respuesta:

```cpp
class Telefono {
public:
	// constructor
	Telefono(std::string area, std::string numero);
	// constructor move
	Telefono(Telefono&& other);
	// constructor copia
	Telefono(const Telefono &other);
	// destructor
	~Telefono();

	//operator <<
	std::ostream& operator<<(std::ostream& out, const Telefono& a);
	//operator ==
	bool operator== (const Telefono& a, const Telefono& a);
	//operator =
	Telefono& operator= (const Telefono& other);
	//operator long
	operator long() const;
	//operator >>
	std::istream& operator>>(std::istream& out, const Telefono& a);

private:
	std::string area;
	std::string numero;
};

std::istream& Telefono::operator>>(std::istream& in, const Telefono& a) {
	"(" >> a.area >> ") " >> a.numero >> in;
	return in;
}
```

---

## 4. Explique qué se entiende por "compilación condicional". Ejemplifique mediante código.

### Respuesta:

El preprocesador permite la compilación condicional de algunas partes del archivo fuente. Este comportamiento está controlado por las directivas `#if`, `#else`, `#elif`, `#ifdef`, `#ifndef` y `#endif`. De esta forma el código que se encuentre dentro de estos bloques sólo se compilará cuando la condición se evalúe verdadera.

```c
int main(int argc, char const *argv[]) {
#ifdef TRUE
	printf("Evaluó verdadera\n");
#else
	printf("Evaluó falsa\n");
#endif
	return 0;
}
```

---

## 5. ¿Qué significa que una función es blocante? ¿Cómo subsanaría esa limitación en término de mantener el programa "vivo"?

### Respuesta:

Una función es blocante cuando se ejecuta hasta que se cumpla una condición, como la función `accept`, una vez invocada, el programa no avanza hasta la conexión de un cliente. Para evitar la limitación y ejecutar las siguientes instrucciones, una solución es ejecutar la función blocante en un hilo aparte.

---

## 6. Explique qué son los métodos virtuales puros y para qué sirven. De un breve ejemplo donde su uso sea imprescindible.

### Respuesta:

Un método virtual puro es aquel que no admite implementación dentro de la clase donde se declara, se identifica como `virtual <nombre_metodo> = 0`.

Dado que en C++ no existe una modificador `abstract`, al definir un método virtual puro en una clase, la clase que lo contiene se torna abstracta, es decir, que no pueden crearse instancias de ella.

<!-- ---

## 7. Escribir un programa C que procese el archivo `numeros.txt` sobre sí mismo (sin crear archivos intermedios y sin subir el archivo a memoria). El procesamiento consiste en leer nros hexadecimales de 4 símbolos y reemplazarlos por su valor decimal (en texto).

### Respuesta:

-->
---

## 8. ¿Qué es el polimorfismo? Ejemplifique mediante código.

### Respuesta:

El polimorfismo es la capacidad de variar el comportamiento para un mismo método en función del objeto que lo invoque.

```cpp
class Figura {
public:
	Figura() {}
	virtual ~Figura() {}
	virtual int get_perimetro() const = 0;
	virtual int get_area() const = 0;
};

class Rectangulo : public Figura {
public:
	Rectangulo(const int base, const int altura): base(base), altura(altura) {}
	~Rectangulo() {}
	int get_perimetro() const override {
		return (2 * this->base) + (2 * this->altura);
	}
	int get_area() const override {
		return this->base * this->altura;
	}
private:
	const int base;
	const int altura;
};

class Cuadrado: public Figura {
public:
	Cuadrado(const int lado) : lado(lado) {}
	~Cuadrado() {}
	int get_perimetro() const override {
		return 4 * this->lado;
	}
	int get_area() const override {
		return this->lado * this->lado;
	}
private:
	const int lado;
};
```

<!-- ---

## 9. ¿Qué función utiliza para esperar la terminación de un thread? Ejemplifique mediante código.

### Respuesta:

-->
---

## 10. Escriba un programa C que tome 2 cadenas por línea de comandos: A y B; e imprima la cadena A después de haber duplicado todas las ocurrencias de B..
`ej.: reemp.exe “El final está aprobado” aprobado -> El final está aprobado aprobado`

### Respuesta:

```c
#include <stdlib.h>
#include <string.h>
#include <stdio.h>

#define DELIMITER " "

void duplicate(char dst[], char substr[]) {
	char *ptr = dst;

	while ((ptr = strstr(ptr, substr)) != NULL) {
		// puntero al inicio de la dirección para la palabra a duplicar
		char *dup_word_start = ptr + strlen(substr);
		// si substr es la última palabra de dst piso el '\0' con un espacio
		*dup_word_start = ' ';
		dup_word_start++;

		// puntero al fin de la dirección para la palabra a duplicar
		char *dup_word_end = dup_word_start + strlen(substr) + 1;

		// desplazo el texto a la derecha
		strncpy(dup_word_end, dup_word_start, strlen(dup_word_start));

		// duplico la palabra
		strncpy(dup_word_start, substr, strlen(substr));
		dup_word_start[strlen(substr)] = ' ';
		ptr = dup_word_end;
	}
}

int main(int argc, char *argv[]) {
	size_t max_length = strlen(argv[1]) * 2;

	char *result = malloc(sizeof(char) * max_length);
	memset(result, 0, sizeof(char) * max_length);
	strncpy(result, argv[1], strlen(argv[1]));

	char *word = strtok(argv[2], DELIMITER);
	while (word != NULL) {
		duplicate(result, word);
		word = strtok(NULL, DELIMITER);
	}

	printf("%s\n", result);

	free(result);

	return 0;
}
```
