* **Fecha**: 13/08/2019
* **Cuatrimestre**: 1° 2019
* **Tema**: 5

A continuación la resolución de algunos ejercicios correspondiente al archivo 1.pdf
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

a) `int *(*a)`
b) `long int RESTA(long int a, long int b)`
c) `static char c`

---


## 3. Declare la clase TELEFONO para encapsular una cadena numérica correspondiente a un teléfono. Incluya al menos: `Constructor(area, numero)`, `Constructor move` y `Constructor de Copia`; `Operador <<`, `==`, `=`, `long` y `>>`. Implemente el operador `>>`.

### Respuesta:

---
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
<!-- ## 4. Explique qué se entiende por “compilación condicional”. Ejemplifique mediante código.

### Respuesta:

---
-->
<!-- ## 5. ¿Qué significa que una función es blocante? ¿Cómo subsanaría esa limitación en término de mantener el programa ‘vivo’ ?

### Respuesta:

---
-->
<!-- ## 6. Explique qué son los métodos virtuales puros y para qué sirven. De un breve ejemplo donde su uso sea imprescindible.

### Respuesta:

---
-->
<!-- ## 7. Escribir un programa C que procese el archivo `numeros.txt` sobre sí mismo (sin crear archivos intermedios y sin subir el archivo a memoria). El procesamiento consiste en leer nros hexadecimales de 4 símbolos y reemplazarlos por su valor decimal (en texto).

### Respuesta:

---
-->
<!-- ## 8. ¿Qué es el polimorfismo? Ejemplifique mediante código.

### Respuesta:

---
-->
<!-- ## 9. ¿Qué función utiliza para esperar la terminación de un thread? Ejemplifique mediante código.

### Respuesta:

---
-->
<!-- ## 10. Escriba un programa C que tome 2 cadenas por línea de comandos: A y B; e imprima la cadena A después de haber duplicado todas las ocurrencias de B..
`ej.: reemp.exe “El final está aprobado” aprobado -> El final está aprobado aprobado`

### Respuesta:

---
-->