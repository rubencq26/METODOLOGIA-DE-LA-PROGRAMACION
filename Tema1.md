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

## 3. Funciones y Métodos: Interfaces

### Funciones genéricas
  ```cpp
tipo nombre([tipo v1, ..., tipo vN]) {
[declaraciones;]
sentencias;
[return valor_devuelto;]
}
```

- Pueden ser usadas en una expresión o asignación (excepto void).
- Existen durante todo el programa (tiempo de vida global).
- Pueden ser utilizadas en todo el programa (alcance global).
- Pueden ser recursivas y/o pueden llamar a otra función.

 ### Métodos: funciones miembros de una clase
 ```cpp
tipo nombre_clase::nombre_metodo([tipo v1, ..., tipo vN]) {
[declaraciones;]
sentencias;
[return valor_devuelto;]
}
```

Los métodos (funciones miembros):
- Tienen acceso directo (no hay que pasarlo como parámetro) a los atributos privados del objeto que lo invoca y a los atributos privados de los objetos de su clase pasados como argumentos.
- Pueden ser públicos o privados:
- Los métodos privados sólo son accesibles por los métodos de la clase
- Los métodos públicos son accesibles por los métodos de la clase y por los objetos de dicha clase definidos en el programa.

### 3.2.1 Métodos constantes (funciones miembros constantes)
Indican al compilador que **el método no puede modificar el objeto que lo invoca**
```cpp
  tipo nombre_clase::nombre_metodo([tipo v1, …, tipo vN]) const {
[declaraciones;]
sentencias; //si alguna modifica algún atributo -> error
[return valor_devuelto;]
}
```
Un **método const** no puede modificar los atributos de su clase y **sólo puede llamar a otros métodos const de su clase** (aunque si puede llamar a métodos no const de otras clases)

**Los objetos constantes (objetos const) sólo pueden invocar los métodos const de su clase** (al ser objetos constantes el compilador impide que puedan ser modificados y por tanto sólo permite que invoque los métodos const de su propia clase)

**Los objetos y variables constantes sólo se pueden inicializar, no modificar** 
const Punto p(0,0); //un punto de origen no debe cambiar nunca

```cpp
#include <cstdlib>
#include <iostream>
using namespace std;
class Otra {
int x;
public:
void ini(int x=0) { this->x=x; }
int getx() const { return x; }
};
class Punto {
int x, y;
int min() const { return x<y?x:y; }
public:
Punto(int a, int b) { x = a; y = b; }
/*Los métodos de cambio de las coordenadas no
pueden ser invocados por objetos constantes */
void setx(int n) { x = n; } //const nunca
void sety(int n) { y = n; } //const nunca
void en(Punto &p) const { p.x=x; p.y=y; }
void clona(Punto p) { x=p.x; y=p.y; } //const nunca
void cambia(Otra &o) const { int n=min(); o.ini(n); }
/*Los métodos de lectura de coordenadas sí
pueden ser invocados por objetos constantes */
int getx() const { return x; }
int gety() const;
/*La conversión de tipo si puede ser constante */
operator char*() const {
char salida[30];
sprintf(salida, "(%i,%i)",x,y);
return strdup( salida );
}
};
int Punto::gety() const { return y; }
int main(int argc, char *argv[]) {
Punto p1(1,2),p3(2,3);
const Punto p2(5,4);
Otra ot;
cout << p1 << ":" << p2 << ":" << p3 << "\n";
p3.setx(4);
//p2.setx(100); //ERROR: el método no es constante
p1.clona(p2);
//p2.clona(p3); //ERROR: el método no es constante
cout << p1 << ":" << p2 << ":" << p3 << "\n";
p2.en(p3);
p2.cambia(ot);
p1.sety(p3.gety()-1);
p1.setx(ot.getx());
cout << p1 << ":" << p2 << ":" << p3 << "\n";
//p1.min(); //ERROR min es privado
system("PAUSE");
return EXIT_SUCCESS;
}
```

### Métodos constantes (sobrecarga)

**Un método puede sobrecargarse con una versión const y otra no const**

En tiempo de ejecución el compilador ejecutará una versión u otra dependiendo de si el objeto que invoca el método es un objeto constante o no

```cpp
#include <cstdlib>
#include <iostream>
using namespace std;
class Punto {
int x, y;
int min() const { return x<y?x:y; }
public:
Punto(int a, int b) { x = a; y = b; }
void setx(int n) { x = n; } //const nunca
void sety(int n) { y = n; } //const nunca
void clona(Punto &p) const { p.x=x; p.y=y; }
void clona(Punto p) { x=p.x; y=p.y; }
int proce() const;
int proce();
void proce(int n) const;
//void fun() const;
int getx() const { return x; }
int gety() const { return y; }
operator char*() const {
char salida[30];
sprintf(salida, "(%i,%i)",x,y);
return strdup( salida );
}
};
int Punto::proce() const {
return 0;
}
int Punto::proce() {
return 1;
}
void Punto::proce(int n) const {
cout << n << endl;
}
//void Punto::fun() { //no es fun() const!!!; }
int main(int argc, char *argv[]) {
Punto p1(1,2),p3(2,3);
const Punto p2(5,4);
cout << p1 << ":" << p2 << ":" << p3 << "\n";
p1.setx(5);
//p2.setx(5); //ERROR
p1.clona(p2);
p2.clona(p3);
cout << p1 << ":" << p2 << ":" << p3 << "\n";
cout << p1.proce() << "," << p2.proce() << "\n";
p1.proce(10);
p2.proce(15);
//p1.min(); //ERROR min es privado
system("PAUSE");
return EXIT_SUCCESS;
}
```
### Especificador inline para funciones y métodos
Indica al compilador que **sustituya la llamada a una función por su código**
  - La ejecución es más rápida, al no tener que transferir el control.
  - El programa ocupa más espacio al duplicar el código en cada llamada:
```cpp
void ver(int a); // declaración
...
inline void ver(int a) {
. . . // definición
}
```

```cpp
#include <iostream>
using namespace std;
int doble(int n); // prototipo funcion (declaracion)
int factorial(int n);
class complejo {
int real,imag; // parte privada
public:
void ini(int re, int im) {real=re; imag=im;} // inline
void clon(complejo c) { real=c.real; imag=c.imag; } //inline
void suma(int re, int im);
void ver() const;
};
inline void complejo::ver() const {
cout << real << "+" << imag << "i";
}
void complejo::suma(int re, int im) {
real += re; imag = imag + im;
}
inline int doble(int n) { return 2*n; }
int factorial(int n) {
int fact=1;
for(int i=1; i <= n; i++) fact *= i;
return(fact);
}
```

### Funciones amigas(friend)
Una función amiga (**friend**) de una clase es una función o método que **no es miembro de la clase**, pero **tiene permiso para acceder a los elementos privados** de los objetos de dicha clase pasado como parámetro.

Una función amiga de una clase puede ser un método de otra clase o puede ser una función genérica que no pertenece a ninguna clase.

Una función o método puede ser amiga de cuantas clases se quiera.

Para declararla amiga, es necesario incluir su prototipo, precedido por la palabra **friend**,
en la clase para la que va a ser amiga (es indiferente ponerla en la parte pública o
privada: las funciones amigas son siempre accesibles y la accesibilidad de los métodos
amigos la determina la zona donde está definida en la clase a la que pertenece)

Las funciones amigas violan el principio de encapsulamiento de la programación orientada
a objetos, pero es la clase la que decide quiénes son sus amigos.


