* **Fecha**: 02/07/2019
* **Cuatrimestre**: 1° 2019
* **Tema**: 1

A continuación la resolución de algunos ejercicios correspondiente al archivo 2.pdf

## 1. Declare la clase Número para encapsular una cadena numérica larga. Incluya al menos: Constructor(unsigned long), Constructor default y Constructor move; Operador <<, (), =, long y ++(int). Implemente el operador >>.

### Respuesta:

`Numero.h`

```cpp
#ifndef __NUMERO_H__
#define __NUMERO_H__

#include <iostream>

class Numero {
public:
	// Constructor(unsigned long)
	explicit Numero(unsigned long);

	// Constructor default
	Numero();

	// Constructor copia para post-incremento
	Numero(Numero &);

	// Constructor move
	Numero(Numero &&);

	~Numero() = default;

	// Para operator<<
	unsigned long getNumber() const;

	void operator()();

	Numero &operator=(const Numero &);

	explicit operator long() const;

	Numero operator++(int);

	friend std::istream &operator>>(std::istream &, Numero &);

private:
	unsigned long number;
};

std::ostream &operator<<(std::ostream &, const Numero &);

#endif    // __NUMERO_H__
```

`Numero.cpp`

```cpp
std::istream &operator>>(std::istream &in, Numero &num) {
	in >> num.number;

	return in;
}
```
---

<!--
## 2. ¿Qué significa que una función es blocante? ¿Cómo subsanaría esa limitación en término de mantener el programa ‘vivo’ ?

### Respuesta:

Se encuentra en 2019.1/5-resolucion.md, ejercicio 5.
---
-->
<!--
## 3. Explique qué son los métodos virtuales y para qué sirven. De un breve ejemplo donde su uso sea imprescindible.

### Respuesta:

---
-->
<!--
## 4. Escribir un programa C que procese el archivo “numeros.txt” sobre sí mismo (sin crear archivos intermedios y sin subir el archivo a memoria). El procesamiento consiste en leer grupos de 4 caracteres hexadecimales y reemplazarlos por los correspondientes dígitos decimales (que representen el mismo número leído pero en decimal).

### Respuesta:

Se encuentra en 2019.1/5-resolucion.md, ejercicio 7.
---
-->
<!--
## 5. Explique qué se entiende por “compilación condicional”. Ejemplifique mediante código.

### Respuesta:

Se encuentra en 2019.1/5-resolucion.md, ejercicio 4.
---
-->
<!--
## 6. Escriba un programa C que tome 2 cadenas por línea de comandos: A y B; e imprima la cadena A después de haber duplicado todas las ocurrencias de B..
`ej.: reemp.exe “Este es el final” final ----\-> Este es el final final`

### Respuesta:

Se encuentra en 2019.1/5-resolucion.md, ejercicio 10.
---
-->

## 7. Escriba las siguientes definiciones/declaraciones:
* a) Definición de una la función SUMA, que tome dos enteros largos con signo y devuelva su suma. Esta función sólo debe ser visible en el módulo donde se la define.
* b) Declaración de un puntero a puntero a entero sin signo.
* c) Definición de un caracter solamente visible en el módulo donde se define.

### Respuesta:

* a)
```c
static int SUMA(const int a, const int b) {
	return a + b;
}
```
* b) `unsigned int (*(*a));`
* c) `static char c = 0;`

---


## 8. ¿Qué valor arroja sizeof(int)? Justifique.

### Respuesta:

El valor dependerá del compilador y la arquitectura donde se compile el fuente. Si el caso es una arquitectura de 32 bits, el valor será 4.

---

<!--
## 9. Describa el concepto de loop de eventos (events loop) utilizado en programación orientada a eventos y, en particular, en entornos de interfaz gráfica (GUIs).

### Respuesta:

Se encuentra en 2019.2/2-resolucion.md, ejercicio 6.
---
-->
<!--
## 10. ¿Qué ventaja ofrece un lock raii frente al tradicional lock/unlock?

### Respuesta:
-->
