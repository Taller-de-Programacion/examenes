*Posibles soluciones*

#### 2) ¿Qué función se utiliza para lanzar hilos?¿Qué parámetros tiene y para qué sirven? Ejemplifique.

En C++ se pueden lanzar hilos al momento de crear un objeto de tipo std::thread que recibe
por parámetro el método con el cual se quiere usar el hilo y su objeto relacionado. Ej:

```cpp
#include <thread>
#include <vector>
class MiClase {
private:
  int x;
public:
  MiClase(int _x) : x(_x) {}
  void agregar() {
    for (int i = 0; i < 10; ++i) this->x += i;
  }
  ~MiClase() {}
};

int main() {
  std::vector<std::thread> main_threads;
  MiClase obj_1(2), obj_2(3);
  // Acá se lanza el thread, al momento de crear el objeto thread
  main_threads.emplace_back(&MiClase::agregar, &obj_1);
  main_threads.emplace_back(&MiClase::agregar, &obj_2);
  for (size_t i = 0; i < main_threads.size(); ++i) {
    main_threads[i].join();
  }
  return 0;
}
```

#### 4) Implemente una función C++ denominada DUPLICAR que reciba dos listas de elementos y devuelva una nueva lista duplicando los elementos de la primera que están en la segunda:
#### std::list<T> Duplicar(std::list<T> a,std::list<T> b);

```cpp
// Se asumió que la nueva lista tiene los elementos de a que están en b duplicados y los que
// no están en b están sin duplicar.

std::list<T> Duplicar(std::list<T> a,std::list<T> b) {
  std::list<T> nueva_lista;
  
  for (std::list<T>::iterator it_a = a.begin(); it_a != a.end(); ++it_a) {
    bool esta_en_b = false;
    nueva_lista.push_back((*it_a));
  
    for (std::list<T>::iterator it_b = b.begin(), it_b != b.end() && !esta_en_b; ++it_b) {
      if ((*it_a) == (*it_b)) esta_en_b = true;
    }
  
    if (esta_en_b) {
      nueva_lista.push_back((*it_a));
    }
  }
  
  return nueva_lista;
  }
```

#### 5) Declare una clase a elección considerando:
#### - Atributos que son necesarios
#### - Accesibilidad a la Clase 
#### - Incluir los operadores *, --(pre-decremento), ++(post-incremento), << (impresión), >>(carga desde consola), functor
  
```cpp
class PunteroEntero {
private:
  int* ptr;
public:
  PunteroEntero(int* _ptr);
  
  int operator*() const;
  PunteroEntero& operator--(int)();
  
  PunteroEntero operator++();
  
  void operator();
  ~PunteroEntero();
};
  
ostream& operator<<(ostream& out, const PunteroEntero& ptr_ent);
istream& operator>>(istream& in, PunteroEntero& ptr_ent);
  
```
  
#### 10) ¿Qué es la compliación condicional? Ejemplifique
  
La compilación condicional sucede cuando se tiene algún condicional del precompilador que
se verifica al momento de compilar, una macro que esté o no definida (puede definirse al
momento de compilar con el flag -D<nombre_macro>). Ej:

```c
#ifndef PRINT
#define PRINT(str) printf(“%s”, s)
#endif
```

