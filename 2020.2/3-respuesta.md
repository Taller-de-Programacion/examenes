*Posibles soluciones*

#### 1) ¿Qué diferencia existe entre un constructor por copia y uno por movimiento? Ejemplifique.

La diferencia entre un constructor por copia y uno por movimiento es que el que es por copia
(generalmente) pasa cada uno de los bits de un objeto a otro (dependiendo de cómo esté
implementado el constructor puede que la copia no sea bit a bit). Por otro lado el constructor por
movimiento pasa el “ownership” de un objeto a otro haciendo que el objeto que contenía los datos
originales deje de tenerlos. Ej:

```cpp
#include <vector>

class MiClase {
private:
  std::vector<int> x;
public:
  MiClase() : {}
  MiClase(const MiClase& otro) {
    for (size_t i = 0; i < otro->x.size(); ++i) {
    this->x.push_back(otro->x[i]);
    }
  }
  
  MiClase operator=(const MiClase& otro) {
    for (size_t i = 0; i < otro->x.size(); ++i) {
      this->x.push_back(otro->x[i]);
    }
  }
  
  MiClase(MiClase&& otro) {
    this->x = std::move(otro->x);
    otro->x.empty();
  }
  
  MiClase&& operator=(MiClase&& otro) {
    this->x = std::move(otro->x);
    otro->x.empty();
  }
  
  // Otros métodos
  ~MiClase() {}
 };

```

#### 2) Escriba una rutina que dibuje un rombo azul del tamaño de la pantalla.

```cpp
#include <QApplication>
#include <QPainter>
#include <QWidget>
#include <math.h>
#define WINDOW_SIZE 100

class Rhombus : public QWidget {
public:
  Rhombus(QWidget* parent = nullptr) : QWidget(parent) {}
  void paintEvent(QWidget* e) {
    Q_UNUSED(e);
    QPainter qp(this);
    qp->draw();
  }
  
  void draw() {
    qp->setPen(QPen(“blue”));
    qp->setBrush(QBrush(“blue”));
    qp->traslate(WINDOW_SIZE/2);
    qp->rotate(45);
    int rhombus_side_size = std::sqrt(std::pow(WINDOW_SIZE, 2) + std::pow(WINDOW_SIZE, 2));

    qp->drawRect(0, 0, rhombus_side_size, rhombus_side_size);
  }
};

int main(int argc, char** argv) {
  QApplication app(argc, argv);
  Rhombus rhombus;
  rhombus.resize(WINDOW_SIZE, WINDOW_SIZE);
  rhombus.setWindowTitle(“Rhombus”);
  return app.exec();
}
```

#### 4) ¿Qué es un Deadlock? Ejemplifique.

Un deadlock ocurre cuando un thread bloquea el objeto que modifica y no hay forma que este
lo libere quedando bloqueado por el resto del transcurso del programa porque ningún otro
thread puede acceder al recurso. Ej:

```cpp
class Sumador {
private:
  std::mutex m;
  int x;
public:
  Sumador(int _x) : x(_x) {}
  void inc() {
    m.lock();
    x++;
  }
  // No se libera el recurso (m.unlock()) por lo que los siguientes threads no van a poder acceder a x, creando así un deadlock porque no se puede continuar con el flujo del programa.
  ~Sumador() {}
};

int main() {
  Sumador obj(5);
  std::vector<std::thread> hilos;
  for (size_t i = 0; i < 3; ++i) {
    hilos.emplace_back(&Sumador::inc, &obj);
  }
  
  for (size_t i = 0; i < hilos.size(); ++i) {
    hilos[i].join();
    // El primer thread termina pero no libera el recurso (x) para que los demás hilos puedan acceder
  }
  
  return 0;
}
```

#### 5) Escriba un programa que reciba por línea de comandos un Puerto y una IP. El programa debe conectarse y quedar en escucha de paquetes con la forma “d..d+d..d+...=”. El programa debe imprimir en stdout cada suma hasta que la misma sea 0 (cero). En ese caso debe finalizar ordenadamente.

```c
#include <sys/types.h>
#include <sys/socket.h>
#include <netdb.h>
#include <string.h>
#include <unistd.h>
#include <stdbool.h>

#define SUCCESS 0
#define ERROR -1
#define SOCKET_CLOSED 0
#define SOCKET_ERROR -1

// socket
typedef struct socket{
  int peer;
 } socket_t;
 
static int socket_connect(socket_t* skt, struct addrinfo* results) {
  struct addrinfo* aux = results;
  bool is_connected = false;
  while ((aux != NULL) && !is_connected) {
    skt->peer = socket(aux->ai_family, aux->ai_socktype, aux->ai_protocol);
    if (skt->peer != SOCKET_ERROR) {
      if (connect(skt->peer, aux->ai_addr, aux->ai_addrlen) == SUCCESS) {
        is_connected = true;
      } else {
        close(skt->peer);
      }
    }
    
    aux = aux->ai_next;
  }
  
  if (!is_connected) return ERROR;
  
  return SUCCESS;
}

int socket_init(socket_t* skt, const char* ip, const char* port) {
  struct addrinfo hints, *results;
  memset(&hints, 0, sizeof(hints));
  hints.ai_family = AF_INET;
  hints.ai_socktype = SOCK_STREAM;
  hints.ai_flags = 0;
  int status = getaddrinfo(ip, port, &hints, &results);
  if (status != SUCCESS) {
    freeaddrinfo(results);
    return ERROR;
  }
  
  status = socket_connect(skt, results);
  freeaddrinfo(results);
  return status;
}

int socket_recv(socket_t* skt, char* buffer, const int bytes_to_recv) {
  int total_bytes_recvd = 0;
  bool is_connected = true;
  while (total_bytes_recvd < bytes_to_recv && is_connected) {
    int bytes_recvd = recv(skt->peer, &buffer[total_bytes_recvd], bytes_to_recv - total_bytes_recvd, 0);

    if (bytes_recvd == SOCKET_ERROR || bytes_recvd == SOCKET_CLOSED) {
      is_connected = false;
    } else {
      total_bytes_recvd += bytes_recvd;
    }
  }
  
  return total_bytes_recvd;
}

void socket_uninit(socket_t* skt) {
  shutdown(skt->peer, SHUT_RDWR);
  close(skt->peer);
}

// main
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#define SIZE_BUFFER 10
#define SIZE_NUMBER 30 // Suponiendo que pueda haber un número que entre en un long
#define EQUAL_CHAR ‘=’
#define SUM_CHAR ‘+’

int main(int argc, char** argv) {
  if (argv < 3) {
    printf(“./exec <ip> <port>\n”);
    return -1;
  }
  
  socket_t skt;
  if (socket_init(&skt, argv[1], argv[2]) != SUCCESS) return -1;
  
  char buffer[SIZE_BUFFER], number[SIZE_NUMBER];
  long int total_sum = 0;
  bool is_connected = true;
  int size_number = 0;
  while (is_connected) {
    int bytes_recv = socket_recv(&skt, buffer, SIZE_BUFFER);
    // d..d+d..d+...=
    for (int i = 0; i < bytes_recv && is_connected; ++i) {
      if (buffer[i] == SUM_CHAR) {
        number[size_number] = ‘\0’;
        total_sum += atol(number);
        size_number = 0;
      } else if (buffer[i] == EQUAL_CHAR) {
        number[size_number] = ‘\0’;
        total_sum += atol(number);
        size_number = 0;
        if (total_sum == 0) {
          is_connected = false;
        } else {
          printf(“%ld\n”, total_sum);
          total_sum = 0;
        }
      } else {
        number[size_number] = buffer[i];
        size_number++;
      }
    }
  }
  
  socket_uninit(&skt);
  
  return 0;
}
```

#### 7) Declare una clase a elección considerando:
- #### Atributos que son necesarios
- #### Accesibilidad a la Clase
- #### Incluir los operadores -, ++ (post-incremento), --(pre-incremento), >> (impresión), << (carga desde consola), float

```cpp
class PuntoFlotante {
private:
  int parte_entera, parte_decimal;
public:
  PuntoFlotante(int _parte_entera, int _parte_decimal);
  
  PuntoFlotante operator-(const PuntoFlotante& otro) const;
  
  PuntoFlotante operator++(int)();
  
  PuntoFlotante& operator--();
  
  float operator(float)() const;
  
  ~PuntoFlotante();
};

ostream& operator<<(ostream& out, const PuntoFlotante& num);
istream& operator>>(istream& in, PuntoFlotante& num);


```

#### 8) Implemente una función C++ denominada SoloMuliplos que reciba una lista de elementos numéricos y un elemnto numérico x. La función debe devolver una nueva lista con los elementos de la primera que sean múltiplos de x: 
#### ```std::list<T> SoloMultiplos(std::list<T> a,T x);```

```cpp
std::list<T> SoloMultiplos(std::list<T> a,T x) {
  std::list<T> nueva_lista;
  for (std::list<T>::iterator it = a.begin(); it < a.end(); ++it) {
    if ((*it) % x == 0) {
      nueva_lista.push_back((*it));
    }
  }

  return nueva_lista;
}
```

#### 9) ¿Se puede evitar que un objeto de una clase sea copiado?¿y clonado?. En caso afirmativo explique cómo puede hacerse. En caso negativo explique por qué no se puede.

Sí, es posible evitar que un objeto de una clase sea copiado o clonado. La forma de hacerlo es
poniendo la keyword delete después del constructor por copia:

```cpp
class Ejemplo {
private:
  //atributos
public:
  //Constructor
  //Constructor por copia
  
  Ejemplo(const Ejemplo& otro) = delete;
  // Al colocar la keyword delete el objeto Ejemplo deja de ser copiable
  Ejemplo& operator=(const Ejemplo& otro) = delete;
};
```
