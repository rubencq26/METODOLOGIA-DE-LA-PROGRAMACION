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
