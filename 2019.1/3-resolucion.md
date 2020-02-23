* **Fecha**: 23/07/2019
* **Cuatrimestre**: 1° 2019
* **Tema**: 3

A continuación la resolución de algunos ejercicios correspondiente al archivo 3.pdf

## 1. Declare una clase a elección considerando:
* Atributos que son necesarios
* Accesibilidad a la Clase
* Incluir los operadores +, ++ (post-incremento), --(pre-incremento), >> (impresión), << (carga desde consola), long

### Respuesta:

```cpp
class Complex {
public:
	Complex();

	Complex(float re, float im);

	~Complex() = default;

	Complex operator+(const Complex &c) const;

	// pre-incremento
	Complex &operator++();

	// post-incremento
	Complex operator++(int);

	explicit operator long() const;

	float getRe() const;

	float getIm() const;

	friend std::istream &operator>>(std::istream &input, Complex &c);

private:
	float re;
	float im;
};

std::ostream &operator<<(std::ostream &out, const Complex &c);
```
---

<!--
## 2. ¿Qué es un functor? ¿Qué ventaja ofrece frente a una función convencional? Ejemplifique.

### Respuesta:

Se encuentra en 2019.2/2-resolucion.md, ejercicio 4.

---
 -->
<!--
## 3. Escriba un programa que imprima por salida estándar los números entre 1 y 100, en orden ascendente. Se pide que los números sean contabilizados por una variable global única y que los pares sean escritos por un hilo mientras que los impares sean escritos por otro. Contemple la correcta sincronización entre hilos y la liberación de los recursos utilizados.

### Respuesta:

---
 -->

## 4. Explique qué es (a), (b), (c) y (d), haciendo referencia a su valor y momento de inicialización, su comportamiento y el area de memoria donde residen:

```c
static int a;
int b() {
	static int c;
	char d = 65;
	return c + (int) d;
}
```

### Respuesta:

* `a`: es una variable de tipo entero que reside en el _Data Segment_, se inicializa al invocar el ejecutable, si se usa el compilador `gcc` lo hace por nosotros, pero es recomendable su inicialización explícita su scope es local al bloque donde fue definida (archivo o función si se encuentra entre llaves).
* `b`: es una función que no recibe parámetros y devuelve un entero. En memoria, se haya en el _code segment_, cargándose al momento de invocación del ejecutable.
* `c`: es un entero que en memoria se encuentra en el _Data Segment_, con lifetime durante toda la ejecución del programa y alcance local al bloque donde fue definida, en este caso, sólo puede ser accedida dentro de la función _b_.
* `d`: d es una variable que se ubica en el _Stack Segment_ con scope local a la función donde fue declarada y un _lifetime_ local, en este caso, se crea al invocar la función _b_ y se destruye una vez finalizada su ejecución y sólo dentro de ésta es posible leer o escribir su valor.

---

<!--
## 5. Escriba una rutina que dibuje las dos diagonales de la pantalla en color rojo.

### Respuesta:

---
 -->
<!--
## 6. ¿Por qué las librerías que usan Templates se publican con todo el código fuente y no como un .h y .o/.obj?.

### Respuesta:

---
 -->
<!--
## 7. ¿Qué características debe tener un compilador C para se considerado "portable"?

### Respuesta:

---
 -->
<!--
## 8. Escriba un programa (desde la inicialización hasta la liberación de los recursos) que reciba paquetes de la forma [nnn+nn+....+nnnn] (numeros separados por +, encerrados entre corchetes) e imprima el resultado de la suma de cada paquete por pantalla. Al recibir un paquete vacío (“[]”) debe cerrarse ordenadamente. No considere errores.

### Respuesta:

Muy parecido a 2019.2/2-resolucion.md, ejercicio 1
---
 -->
<!--
## 9. Escriba una función ISO C que permita procesar un archivo texto que contenga frases (1 por línea) sobre sí mismo, sin generar archivos intermedios ni cargar el archivo completo a memoria. El procesamiento consiste en eliminar las palabras de más de 3 letras de cada línea.

### Respuesta:

---
 -->
<!--
## 10. ¿Qué diferencia existe entre un constructor por copia y uno por movimiento? Ejemplifique.

### Respuesta:

 -->