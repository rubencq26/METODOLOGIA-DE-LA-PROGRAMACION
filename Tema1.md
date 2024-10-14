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

```cpp
#include <iostream>
using namespace std;
class Punto; //declaracion anticipada para que
class General {
int n;
public:
void inc(Punto p, int x); //no de error aqui
void pinta() const { cout <<"(" << n << ")\n"; }
};
class Punto {
friend Punto suma(Punto p1, const Punto &p2);
friend void General::inc(Punto p, int x);
private:
int x, y;
public:
Punto( int nx=0, int ny=0 ){ x=nx; y=ny; }
void pinta() const {cout <<"("<< x <<","<< y << ")\n"; }
};
//inc hay que definirlo aquí tras definir Punto
void General::inc(Punto p, int x) {
n=p.x+x; //Acceso a atributos privados de p!!
};
Punto suma(Punto p1, const Punto &p2 ) {
Punto res;
res.x = p1.x + p2.x; //Acceso atributos privados!!
res.y = p1.y + p2.y;
return res;
}
int main(int argc, char *argv[]) {
Punto p1(10,5), p2(20,10), p3 = suma(p1, p2);
General g;
p3.pinta();
g.inc(p3, 2);
g.pinta();
system("PAUSE");
return EXIT_SUCCESS;
}
```
Las funciones amigas no son estrictamente necesarias (todo lo que se puede conseguir
con funciones amigas se puede conseguir con funciones no amigas que utilicen la interfaz
pública de la clase). Su uso se justifica sólo por razones de eficiencia


Las funciones amigas no son estrictamente necesarias (todo lo que se puede conseguir
con funciones amigas se puede conseguir con funciones no amigas que utilicen la interfaz
pública de la clase). Su uso se justifica sólo por razones de eficiencia
Con funciones amigas Sin funciones amigas
```cpp
#include <iostream>
using namespace std;
class Punto; //declaracion anticipada para que
class General {
int n;
public:
void inc(Punto p, int x); //no de error
void pinta() const { cout << n << endl; }
};
class Punto {
friend Punto suma(Punto p1, const Punto &p2);
friend void General::inc(Punto p, int x);
private:
int x, y;
public:
Punto(int nx=0, int ny=0) {x=nx; y=ny;}
void pinta() const { cout << "(" << x << ","
<< y << ")\n"; }
};
//inc hay que definirlo tras definir Punto
void General::inc(Punto p, int x) {
n=p.x+x; //Acceso a parte privada de p!!
};
Punto suma(Punto p1, const Punto &p2 ) {
Punto res;
res.x = p1.x + p2.x; //acceso a
res.y = p1.y + p2.y; //parte privada
return res;
}
int main(int argc, char *argv[]) {
Punto p1(10,5), p2(20,10);
Punto p3 = suma(p1, p2);
General g;
p3.pinta();
g.inc(p3, 2);
g.pinta();
system("PAUSE");
return EXIT_SUCCESS;
}
```

Sin funciones amigas
```cpp
#include <iostream>
using namespace std;
class Punto {
private:
int x, y;
public:
Punto(int nx=0, int ny=0) { x=nx; y=ny; }
void pinta() const { cout << "(" << x << ","
<< y << ")\n"; }
int getx() const { return x; }
int gety() const { return y; }
};
class General {
int n;
public:
void inc(Punto p, int x) { n=p.getx()+x; }
void pinta() const { cout << n << endl; }
};
Punto suma(Punto p1, const Punto &p2 ) {
Punto res(p1.getx() + p2.getx(),
p1.gety() + p2.gety());
return res;
}
int main(int argc, char *argv[]) {
Punto p1(10,5), p2(20,10);
Punto p3 = suma(p1, p2);
General g;
p3.pinta();
g.inc(p3, 2);
g.pinta();
system("PAUSE");
return EXIT_SUCCESS;
}
```

### Clases amigas (friend)
Una clase puede declararse como amiga de otra/s clase/s.

Cuando una clase A se declara amiga de otra B, todos los métodos de la clase A pasan a
ser amigos de la clase B

Es la forma más rápida de declarar todos los métodos de una clase como amigos de otra.

```cpp
class Cualquiera {
. . .
friend class Amiga;
};
```

Los métodos de la clase **Amiga** pueden acceder a la parte privada de los objetos de la clase
**Cualquiera** pasados como parámetros, **pero no al revés**.

```cpp
#include <iostream>
using namespace std;
class Punto {
friend class General; //clase amiga
private:
int x, y;
public:
Punto( int nx=0, int ny=0 ){ x=nx; y=ny; }
void pinta() const {cout <<"("<< x <<","<< y << ")\n";}
};
class General {
int n;
public:
void ponACero(Punto &p) {
p.x = 0; //Acceso a parte privada de Punto
p.y = 0;
n = 0;
}
void inc(Punto p, int x) {
n = p.x + x; //Acceso a parte privada de Punto
}
void pinta() const { cout << n << endl; }
};
int main(int argc, char *argv[]) {
Punto p1(10,5);
General g;
p1.pinta();
g.ponACero(p1);
p1.pinta();
g.pinta();
g.inc(p1, -2);
g.pinta();
system("PAUSE");
return EXIT_SUCCESS;
}
```

### Clases amigas (friend) (caso de referencia cruzada)
Si se quiere que dos clases tengan acceso mutuo a los miembros privados de los objetos
de la otra clase pasados como parámetro, cada una debe declararse amiga de la otra.

**Problema**: no podemos utilizar una clase antes de declararla.

**Solución: declarar anticipadamente** (redirigir la declaración de) **la clase que definimos
en segundo lugar**.

**Cuando se redirige una clase, se pueden declarar variables de ese tipo y punteros.**
Básicamente, podemos declarar los prototipos, pero como aún no se han declarado los
atributos y métodos reales de la clase redirigida, no se pueden invocar. La solución es
sólo hacer las declaraciones primero y luego definir los métodos.

```cpp
#include <iostream>
using namespace std;
class Amiga2; // declaración anticipada
class Amiga1 {
friend class Amiga2;
friend int suma(Amiga1 a1, Amiga2 a2);
private:
int privada;
public:
void modificaAmiga2(Amiga2 &a2, int val) const;
/* Aquí no podemos definir el método porque aún
el compilador no ha llegado a leer la definición de
la clase Amiga2 y no sabe que atributos tiene... */
void pinta() const { cout << privada << "\n"; }
};
class Amiga2 {
friend class Amiga1;
friend int suma(Amiga1 a1, Amiga2 a2);
private:
int privada;
public:
void modificaAmiga1(Amiga1 &a1, int val) const {
/* Aquí si podemos porque ya hemos declarado Amiga1 */
a1.privada = val;
}
void pinta() const { cout << privada << "\n"; }
};
int suma(Amiga1 a1, Amiga2 a2 ) {
return (a1.privada+a2.privada);
}
/* Ahora sí que podemos definir el método porque ya
hemos declarado los atributos de la clase Amiga2 */
void Amiga1::modificaAmiga2(Amiga2 &a2, int val) const {
a2.privada = val;
}
int main(int argc, char *argv[]) {
Amiga1 am1;
Amiga2 am2;
am1.modificaAmiga2(am2,10);
am2.modificaAmiga1(am1,20);
am1.pinta(); am2.pinta();
cout << "suma = " << suma(am1, am2) << "\n";
system("PAUSE"); return EXIT_SUCCESS;
}
```

## 4. Parámetros por Valor y por Referencia
Los parámetros son los valores que se le pasan a la función o método al ser llamada.
Cada parámetro se comporta dentro de la función o método como una variable local.

Los parámetros escritos en la llamada a la función se llaman parámetros reales o argumentos.

Los que aparecen en la descripción de la función se llaman parámetros formales.

Los parámetros formales son sustituidos por los reales (argumentos) en el orden de
llamada. Al emparejarse éstos deben coincidir en número y en tipo.

### 4.1 Parámetros por Valor y por Referencia
Parametos **por valor o copia**:
- Los cambios producidos dentro de la
función no afectan a la variable usada
como argumento en la llamada
- En la llamada se pasa el valor (copia) de
la variable usada como argumento.
- El parámetro real puede ser una
constante, variable o una expresión.

Parámetros **por referencia**: 
- Los cambios producidos dentro de la
función afectan a la variable con la que
se realiza la llamada.
- En la llamada se transfiere la propia variable.
- El parámetro real sólo puede ser una
variable, ya que se transfiere ésta.
- Se indica poniendo el carácter & delante del
nombre del parámetro formal.

```cpp
#include <iostream>
using namespace std;
class obj; //declaracion anticipada
void permutar(obj &a, obj &b); //ref
void cambiamal(obj a, obj b); //valor
class obj {
int n;
public:
int get() const { return n; }
void set(int i) { n = i; }
};
void permutar(obj &a, obj &b) {
 cout << "dir. memoria de a: " << &a << endl;
 cout << "dir. memoria de b: " << &b << endl;
 int aux=a.get();
 a.set(b.get());
 b.set(aux);
}
void cambiamal(obj a, obj b) {
 cout << "dir. memoria de a: " << &a << endl;
cout << "dir. memoria de b: " << &b << endl;
 int aux=a.get();
 a.set(b.get());
 b.set(aux);
}

int main(){
obj x, y;
x.set(5); y.set(7);
cout << "dir. memoria de x: " << &x << endl;
cout << "dir. memoria de y: " << &y << endl;
cambiamal(x, y);
cout << "x: " << x.get()
 << " y: " << y.get() << endl;
permutar(x, y);
cout << "x: " << x.get()
 << " y: " << y.get() << endl;
system("Pause"); return 0;

/*Como podemos observar en el main() imprimimos la dirección de memoria del objeto x (0x61fe0c) y del objeto y (0x61fe08)
En cambiamal pasamos los parámetros por copia: a (0x61fde0) es una copia de x (0x61fe0c) y b (0x61fde8) es una copia de y (0x61fe08)
Por eso sus direcciones de memoria son distintas.
En permutar pasamos los parámetros por referencia: la dirección de memoria de a y x es la misma (0x61fe0c) ya que no pasamos una
copia, sino el original. Lo mismo ocurre con con b e y.*/
```

Salida del programa:
```yalm
dir. memoria de x: 0x61fe0c
dir. memoria de y: 0x61fe08
dir. memoria de a: 0x61fde0
dir. memoria de b: 0x61fde8
x: 5 y: 7
dir. memoria de a: 0x61fe0c
dir. memoria de b: 0x61fe08
x: 7 y: 5
```
### 4.2 Variables de tipo por Referencia
Es un alias o sinónimo de una variable (no ocupa memoria ya que no crea una nueva variable)

Se declaran con el operador & y deben ser inicializadas a otra variable
```cpp
int i=2;
int& jref; //declaración no válida
int& iref = i; //declaración válida
iref = 6; //es lo mismo que poner i=6
```
Una función o método puede retornar una variable tipo referencia
```cpp
int& maxref(int& a, int& b) {
if (a >= b) return a;
else return b; //con & devuelve b, no una copia de b
} //sin & devuelve una copia de b, no b
```

Al devolver una referencia, la llamada a la función o método puede estar a la izquierda del
operador de asignación:

```cpp
maxref(i, j) = 0; // asigna un 0 al parametro mayor
```

Peligro al retornar referencias: el retorno puede estar indefinido si no programamos bien
```cpp
int& malprogramado(int n) {
return n; //retorna n que al ser local es destruido al terminar
}
```

### 4.3 Referencias constantes
Cuando la variable que queremos pasar como parámetro por valor a una función o
método ocupa mucha memoria, se suele pasar por referencia por motivos de eficiencia,
ya que al pasarlo por valor el compilador tiene que realizar una copia del parámetro y al
pasarlo por referencia, mediante un puntero o un alias (referencia), no se pierde tiempo ni
memoria en hacer una copia, sino que se pasa directamente el original.

Para evitar que accidentalmente se modifique el parámetro (al ser pasada por referencia, en
vez de por valor) dentro de la función, es conveniente declarar el parámetro constante (const).

- El especificador **const** se puede utilizar tanto con variables (y referencias) como con
punteros (no confundir punteros constantes con punteros a constantes).
- Una **variable const, una referencia const o un puntero a una variable const** no
pueden cambiar el valor durante la ejecución del programa (**sólo puede ser inicializado**).
- Un **objeto const, referencia const o puntero a objeto const** sólo puede invocar métodos
const y no puede cambiar durante la ejecución del programa (**sólo puede ser inicializado**)

**Recuerda:**
- Si por motivos de eficiencia, en vez de pasar un objeto por valor lo pasamos por
referencia (para evitar que el compilador tenga que realizar una copia del parámetro)
por motivos de seguridad, para evitar que el parámetro pueda ser modificado (cuando
se pasa por valor no importa ya que es una copia) la referencia la declaramos constante
debemos etiquetar como constantes aquellos métodos que lo sean, ya que si no, no
podrán ser invocados por el parámetro por referencia contante

**Diferencia entre Referencias constantes**, Punteros constantes y Punteros a constantes
Las **variables y las referencias pueden ser constantes**.
Los punteros pueden ser constantes o apuntar a variables contantes (no es lo mismo).
- Las variables const y las referencias const no puede cambiar su valor.
- Los punteros const pueden cambiar el valor de la variable a la que apuntan, lo que no
pueden es apuntar a un sitio diferente.
- Los punteros a variables const no pueden cambiar el valor de la variable a la que
apuntan, pero pueden apuntar a variables distintas a lo largo de la ejecución del programa.

```cpp
#include <iostream>
using namespace std;
int main() {
 int x, y;
 int *z; //z es un puntero a un entero
 z=&x; *z=6; //z puede apuntar a un sitio y modificar su contenido
 z=&y; *z=7; //y luego apuntar a otro sitio y modificar su contenido
 cout << x << " " << y << " " << *z << endl;
 const int *a; //a es un puntero a una constante entera
 a=&x; //a puede apuntar a un sitio
 a=&y; //y luego apuntar a otro sitio
//*a=6; //ERROR a no puede modificar el contenido al que apunta
 x=2; y=5; // ya que considera ese contenido constante (lo sea o no)
 cout << x << " " << y << " " << *a << endl;
 int *const b=&x; //b es un puntero constante a un entero. Al declararlo tiene
 //que apuntar a algo y no puede apuntar a ningun otro sitio
 *b=6; //el sitio al que apunta se puede
 *b=8; //modificar cuantas veces quiera
//b=&y; //ERROR b no puede apuntar a otro sitio, el puntero es constante
 *b=7;
 cout << x << " " << y << " " << *b << endl;

 x=4; y=8;
 const int * const c=&x; //c es un puntero constante a una constante entera
//c=&y; //ERROR al declararlo tiene que apuntar a algo y no puede
 //apuntar a ningun otro sitio
//*c=6; //ERROR tampoco va a poder modificar el contenido al que
 //apunta ya que considera ese contenido constante
 cout << x << " " << y << " " << *c << endl;
 cout << "------------\n";
//int &d; //ERROR d es una referencia: hay que indicar a qué referencia
 int &d=x; //d es una referencia a x (d es un alias de x)
 d=6; //equivale a poner x=6
 d=y; //equivale a poner x=y
 cout << x << " " << y << " " << d << endl;
 const int &e=x; //e es una referencia a una constante (e es una alias de x)
//e=8; //ERROR e no puede modificar x porque considera que es constante
 x=9; //pero x se puede seguir modificando
 cout << x << " " << y << " " << e << endl;
}
```
Ejemplo: Supongamos una clase que tiene una gran cantidad de datos…

uso habitual(menos eficiente):
```cpp
#include <iostream>
#include <iostream>
using namespace std;
class General {
private:
int x[1000], n;
public:
General(int n, int ini, int inc);
int getn() const { return n; }
int getx(int i) const {return x[i];}
int setx(int i, int v) { x[i]=v; }
void ver()const {for(int i=0;i<n;i++)
 cout << x[i] << " ";}
bool compara(General g) const;
};
bool General::compara(General g)const{
bool v = (n==g.n);
for (int i=0; v && i<n; i++)
 v = x[i]==g.x[i];
return v; //g.n=0 no importa,es copia
}
General::General(int n, int ini, int inc) {
 this->n=n;
 for(int j=0,i=ini; j<n; i+=inc, j++)
 x[j]=i;
}
General suma(General p1, General p2) {
int a=p1.getn(), b=p2.getn();
int n=a<b?a:b;
General res(n, 0, 0);
for(int i=0;i<n;i++)
 res.setx(i, p1.getx(i)+p2.getx(i));
return res;
}
int main(int argc, char *argv[]) {
General a(5,-4,2), b(3,2,5), c=suma(a,b);
c = suma(a, b);
cout << "a: "; a.ver(); cout << endl;
cout << "b: "; b.ver(); cout << endl;
cout << "c: "; c.ver(); cout << endl;
if (!a.compara(b)) cout << "a<>b\n";
a=b;
cout << "a: "; a.ver(); cout << endl;
cout << "b: "; b.ver(); cout << endl;
if (a.compara(b)) cout << "a = b\n";
system("PAUSE");
return EXIT_SUCCESS;
}
```

uso de referencias constantes(mas eficiente):
```cpp
#include <iostream>
#include <iostream>
using namespace std;
class General {
private:
int x[1000], n;
public:
General(int n, int ini, int inc);
int getn() const { return n; }
int getx(int i) const { return x[i]; }
int setx(int i, int v) { x[i]=v; }
void ver() const { for(int i=0; i<n; i++)
 cout << x[i] << " ";}
bool compara(const General &g) const;
};
bool General::compara(const General &g)const {
bool v = (n==g.n);
for (int i=0; v && i<n; i++)
 v = x[i]==g.x[i];
return v; //g.n=0 no permitido, es const
}
General::General(int n, int ini, int inc) {
 this->n=n;
 for(int j=0,i=ini; j<n; i+=inc, j++)
 x[j]=i;
}
General suma(const General &p1, const General &p2){
int a=p1.getn(), b=p2.getn();
int n=a<b?a:b;
General res(n, 0, 0);
for(int i=0;i<n;i++)
 res.setx(i, p1.getx(i)+p2.getx(i));
return res;
}
int main(int argc, char *argv[]) {
General a(5,-4,2), b(3,2,5), c=suma(a,b);
c = suma(a, b);
cout << "a: "; a.ver(); cout << endl;
cout << "b: "; b.ver(); cout << endl;
cout << "c: "; c.ver(); cout << endl;
if (!a.compara(b)) cout << "a<>b\n";
a=b;
cout << "a: "; a.ver(); cout << endl;
cout << "b: "; b.ver(); cout << endl;
if (a.compara(b)) cout << "a = b\n";
system("PAUSE");
return EXIT_SUCCESS;
}
```

## 5. Sobrecarga de Métodos y Operadores
###Sobrecarga de operadores mediante un método (función miembro)
El primer operando debe ser un objeto de esa clase (constituye el argumento implícito) 

Sintaxis:
```cpp
tipo nombre_clase::operator#([tipo operador_derecho]) {
[declaraciones;]
sentencias;
return valor_devuelto;
}
```
**donde # representa el operador a sobrecargar (+, -, *, /, >, <, >=, <=, ==, !=, etc.)**

(operator+, operarator-, operator*, operator/, operator>, operator!=, etc.)

### Sobrecarga del operador de asignación =
Si el programador no lo sobrecarga, el compilador lo sobrecarga (realizando una copia
binaria de los atributos)

Sólo se puede sobrecargar mediante un método (función miembro).

Sintaxis: (permite cadenas de asignaciones y máxima eficiencia)
```cpp
clase & clase::operator=(const clase& operador_derecho);
```
- void operator=(…) permite hacer a=b, pero no encadenar asignaciones a=b=c;
- clase operator=(…) { … return *this; } permite encadenar asignaciones a=b=c; Devuelve copia del objeto
 (x=y=z).metodo(); -> x=y=z; copiax.metodo(); (x=y=z)++  x=y=z; copiax++;
- clase& operator=(…) { … return *this; } permite encadenar asignaciones a=b=c; Devuelve objeto original
 (x=y=z).metodo(); -> x=y=z; x.metodo(); (x=y=z)++ -> x=y=z; x++;
- parámetros referencias constantes -> para que cuando escribamos cd=cd se detecte con (this != &cd)

La sobrecarga por defecto que proporciona el compilador hace una copia binaria de los datos:
```cpp
clase & clase::operator=(const clase& operador_derecho) {
*this=operador_derecho; //llamada recursiva ERROR
if (this != &operador_derecho) { //por si es a=a y no es a=b
 sentencias_que_hacen_una_copia_binaria;
}
 return *this; //con & devuelve el propio objeto implicito
} //sin & devuelve una copia del objeto implicito
```

### Sobrecarga del operador += (o del operador -=, operador/=, operador*=)
Se puede sobrecargar mediante un método o una función no miembro. 

Sintaxis: (permite cadena de += y al ser por referencia máxima eficiencia al no hacer copia)
```cpp
clase & clase::operator+=(const clase &operador_derecho);
clase & operator+=(clase &op_izq, const clase &op_derecho);
```

Ejemplo Sobrecarga Operator=
```cpp
#include <iostream> //cin, cout
using namespace std;
class cadena {
char *s;
int nchar;
public:
cadena(char*); // constructor general
~cadena() { delete [] s; }
int getnchar() const { return nchar; }
const char *gets() const { return s; }
void sets(char *);
void ver() const;
cadena& operator= (const cadena& cd);
cadena& operator+= (const cadena& cd);
};
cadena::cadena(char* c) {
nchar = strlen(c);
s = new char[nchar+1];
strcpy(s, c);
}
void cadena::sets(char* c) {
delete [] s;
nchar = strlen(c);
s = new char[nchar+1];
strcpy(s, c);
}
void cadena::ver() const {
cout << nchar << "," << s << endl;
}

cadena& cadena::operator=(const cadena& cd){
if (this != &cd) { //por si es cd=cd
 nchar = cd.nchar; //para ello cd debe
 delete [] s; //ser por referencia &
 s = new char[nchar + 1];
 strcpy(s, cd.s);
}
return *this;
}

cadena& cadena::operator+=(const cadena& cd){
char *aux=s;
nchar += cd.nchar;
s = new char[nchar + 1];
strcpy(s, aux);
strcat(s, cd.s);
delete [] aux;
return *this;
}
int main() {
cadena c1("toro");
cadena c2("cobra");
cout << "c1:"; c1.ver();
cout << "c2:"; c2.ver();
c2=c1;
cout << "c2:"; c2.ver();
c1.sets("sentado");
cout << "c1:"; c1.ver();
cout << "c2:"; c2.ver();
c2+=c1;
cout << "c1:"; c1.ver();
cout << "c2:"; c2.ver();
system("PAUSE"); return EXIT_SUCCESS;
```
## Sobrecarga del operador []
Se puede sobrecargar mediante un método o una función no miembro. 

Sintaxis: (al devolver una referencia permite modificar lo que se devuelve)
```cpp
tipo1 & clase::operator[](tipo2 i); // miembro
tipo1 & operator[](const clase &op_izq, tipo2 i); //no miembro
```

siendo **tipo1** el tipo indexado y tipo2 el índice (puede ser cualquier tipo, lo normal es int)
y donde & es opcional, dependiendo de si nos interesa o no que devuelva una
referencia a un objeto tipo1 o que devuelva un objeto tipo1.

Si devuelve una referencia, entonces el operador [ ] podrá utilizarse a la derecha, pero
también a la izquierda de una asignación, con el peligro que eso conlleva si la clase tiene
alguna restricción respecto a los valores de sus atributos (véase ejemplo apartado 4.4)

Al devolver una referencia permite modificar lo que se devuelve
```cpp
clase x; tipo1 y,z; int i;
//tipo2 es int
...
x[2]=y; //x.operator[](2)=y
z=x[i];

clase x; tipo1 y,z; char *cad;
//tipo2 es char *
...
x["ana"]=y; //x.operator[]("ana")=y
z=x[cad];
```
Si no queremos permitir que operator[ ] pueda aparecer en la parte izquierda de una
asignación, haremos que no devuelve una referencia o que la referencia sea const.

Ejemplo: sobrecargar el operador [ ] para poder usarlo en una clase cadena, de forma
que permita acceder y/modificar los caracteres indexándolos a partir del 1

```cpp
#include <iostream> //cin, cout
using namespace std;
class cadena {
char *s;
int nchar;
public:
cadena(char*); // constructor general
~cadena() { delete [] s; }
int getnchar() const { return nchar; }
char& operator[](int i);
const char * gets() const { return s; }
};
cadena::cadena(char* c) {
nchar = strlen(c);
s = new char[nchar+1];
strcpy(s, c);
}
char& cadena::operator[](int i) {
if (i <= nchar && i>=1)
 return s[i-1];
else {
 cout << "ERROR\n";
 system("PAUSE");
 exit(1); //aborta el programa
}
}

int main() {
cadena cad("casa");
char h;
cout << cad.gets() << " tiene " <<
 cad.getnchar() << " letras\n";
cad[3] = 'n'; //cad.operator[](3)=’n’;
cout << "Introduzca una letra: ";
cin >> cad[2];//cin>>cad.operator[](2);
cout << cad.gets() << endl;
cout << "la 1ª letra es " << cad[1] << endl;
h = cad[4]; //h=cad.operator[](4);
cout << "la 4ª letra es " << h << endl;
cad[9] = 'k'; //ERROR 9 excede cad
system("PAUSE"); return EXIT_SUCCESS;
}
```
### Sobrecarga de los operadores de inserción << y de extracción>>
Los operadores de **inserción (<<)** y **extracción ( >>)** en los flujos (streams) de E/S **sólo
se pueden sobrecargar mediante una función no miembro.** 

```cpp
friend std::istream& operator>>(std::istream& s, clase &o);
friend std::ostream& operator<<(std::ostream& s, const clase &o);
```

donde istream es el flujo de entrada y ostream el stream de salida y lo gris es opcional
Estos flujos funcionan como cintas transportadoras que entran (>>) o salen (<<) del programa. Se recibe
una referencia al flujo como 1er argumento, se añade o se retira de él la variable que se desee, y se
devuelve siempre como valor de retorno una referencia al flujo (stream) modificado.

istream y ostream se devuelven por referencia para permitir cadenas de << y de >>

o1 << o2 << o3; o1 >> o2 >> o3;

ambas se suelen declarar amigas de la clase para la que se sobrecarga, para poder
acceder a su parte privada, aunque no es obligatorio

en **operator<<** el objeto de la clase para la que se sobrecarga se suede pasar por
referencia (por motivos de eficiencia) **constante** (por seguridad), aunque no es obligatorio.

El C++no tiene instrucciones de E/S, pero si tiene librerías que manejan streams (flujos). La librería <iostream>
proporciona una serie de clases que permiten la E/S por teclado (istream) como por pantalla (ostream).

El C++ tiene predefinidos los objetos streams cin y cout:
- **cin** es un objeto de la clase **istream** que se encarga de la entrada por teclado
- **cout** es un objeto **ostream** que maneja la salida por pantalla.
- << y >> son operadores que están sobrecargados para las clases de streams

```cpp
#include <iostream> //cin, cout
using namespace std;
class frac {
int n, d;
public:
frac() { n=0; d=1; }
frac(int x, int y) { n=x; d=y; }
int getn() const { return n; }
int getd() const { return d; }
void set(int n, int d=1);
friend istream& operator>>(istream& s,
 frac &p);
};
istream& operator>>(istream& s, frac &p) {
cout << "numerador: ";
s >> p.n; //s actúa a modo de cin
do {
 cout << "denominador: ";
 s >> p.d; //s actúa a modo de cin
} while (p.d==0);
return s;
}

ostream& operator<<(ostream &s,
 const frac &p) {
if (p.getd()>0)
 s << p.getn() << "/" << p.getd();
else //s actúa a modo de cout
 s << -p.getn() << "/" << -p.getd();
return s;
}
int main() {
frac a(3,2),b;
cout << a << " , " << b << endl;
cout << "Introduce 2 fracciones \n";
cin >> a >> b;
cout << a << " , " << b << endl;
system("PAUSE"); return EXIT_SUCCESS;
}

```
