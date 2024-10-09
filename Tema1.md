# Tema 1
## 1. Programación estructurada
Funciones
```cpp
// declaracion o prototipo
double potencia(int base, int exponente);
...
// definicion
double potencia(int base, int exponente) {
double resultado;
resultado = ... ;
return resultado;
}
...
valor = potencia(2,3); // llamada
valor = potencia(7,2); // llamada
```

## 2. Ámbitos globales, locales y de clase

| Variable local                                 | Variable global  |
|------------------------------------------------|------------------|
| **Es declarada dentro de un bloque o función**     | **Es declarada fuera de cualquier bloqueo función**, al inicio del programa|
| Sólo puede ser usada dentro del bloque en el que está. No puede usarse fuera  | Puede ser usada en cualquier parte del programa, y en cualquier función (no hay que pasarla como parámetro) |
| Sólo existe durante la ejecución del bloque en el que está: se crea al entrar en el bloque, se destruye al salir. | Existe durante toda la ejecución del programa. |

Una variable local y global pueden tener el mismo nombre (la variable local oculta a la global)

### Variable miembro (atributo) de clase
| Variable miembro privada |  Variable miembro pública |
|------------------------------------------------|------------------|
| **Es declarada en la parte private: de la clase** | Es declarada en la parte public: de la clase. |
| No es visible fuera de la clase. | Es visible fuera de la clase. |
| Sólo la puede usar las funciones miembros (métodos) de la clase  |  La puede usar los métodos de la clase y los objetos de dicha clase que haya en el programa. | 

Una variable miembro puede tener el mismo nombre que una variable local y una global (la variable local oculta a la de clase y a la global, la de clase oculta a la global)

Mediante el operador de resolución de ámbito :: podemos acceder a la que nos interese ::vble accede a la global Ejemplo: clase::vble accede a la de clase

```cpp
#include <iostream>
using namespace std;
void ver();
class clase {
int a;
public:
int aget() { return a; }
void aset(int i) { a = i; }
void ver();
};
int a; clase x; // global
void clase::ver(){
a=2; //de clase
int a=0; //local
a++; //local
clase::a++; //de clase
::a=3; //global
cout << "A " << ::a;
cout << " X.a " << clase::a;
cout << " a " << a << endl;
}

void ver(){
a=0; //global
x.aset(4); //global
int a=5; //local
clase x; //local
a++; x.aset(2); //local
::x.aset(::x.aget()+1); ::a++; //global
cout << "A " << ::a << " X.a " << ::x.aget();
cout << " a " << a << " x.a " << x.aget() << endl;
}
int main(){
a=2; x.aset(0); //global
cout << "A " << a << " X.a " << x.aget() << endl;
ver();
cout << "A " << a << " X.a " << x.aget() << endl;
x.ver();
system("Pause"); return 0;
}
```

Salida del Programa:
A 2 X.a 0
A 1 X.a 5 a 6 x.a 2
A 1 X.a 5
A 3 X.a 3 a 1
