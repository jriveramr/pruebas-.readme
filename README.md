# 🧠 Proyecto: Análisis Semántico y Tabla de Símbolos en Compiladores

Documento que aborda el papel del **análisis semántico** y la **tabla de símbolos** en el proceso de construcción de un compilador, 
como continuación natural de los pasos vistos en análisis léxico y sintáctico.

---

## 📌 Introducción
- Contexto: análisis léxico y sintáctico completados
- Necesidad del análisis semántico
- Objetivo de este paso
- Repaso: análisis léxico y sintáctico como etapas previas.
- Limitaciones: el árbol sintáctico no garantiza que el programa sea válido lógicamente.
- Importancia: el análisis semántico permite validar **restricciones de tipos, declaraciones y ámbitos**.

---

## 📚 Marco Teórico

### 🔍 Análisis Semántico
El análisis semántico es una fase crítica en el proceso de compilación que se encarga de verificar que el programa fuente sea semánticamente coherente con las reglas del lenguaje. A diferencia del análisis sintáctico, que valida la estructura del código, el análisis semántico se enfoca en el significado de las construcciones, asegurando que las operaciones sean válidas y que los tipos de datos sean compatibles.

Según Aho, Lam, Sethi y Ullman (2014), el analizador semántico utiliza el árbol sintáctico y la información contenida en la tabla de símbolos para realizar estas verificaciones. Una de sus funciones principales es el chequeo de tipos (type checking), donde se comprueba que los operadores tengan operandos compatibles. Por ejemplo, si un lenguaje exige que los índices de arreglos sean enteros, el compilador debe reportar un error si se intenta usar un número flotante como índice.

Además, el análisis semántico puede aplicar conversiones de tipo implícitas, conocidas como coerciones. Estas permiten que, por ejemplo, un operador aritmético funcione con un entero y un flotante, convirtiendo automáticamente el entero en flotante para mantener la consistencia. En estos casos, el árbol sintáctico se enriquece con nodos adicionales que representan dichas conversiones, como el operador inttofloat.

Complementando esta visión, el autor destacan que el análisis semántico también recolecta información de tipos y la almacena en el árbol sintáctico o en la tabla de símbolos, preparando el terreno para la generación de código intermedio. Esta etapa actúa como un filtro lógico que previene errores de ejecución y garantiza que el código tenga sentido más allá de su forma.

Por su parte, la tabla de símbolos es una estructura de datos fundamental que almacena información sobre los identificadores del programa: variables, funciones, tipos, constantes, entre otros. Cada entrada en la tabla contiene atributos como el nombre del símbolo, su tipo, su alcance (scope) y, en algunos casos, su valor. Esta tabla es consultada constantemente durante el análisis semántico para validar declaraciones, detectar duplicados y verificar el uso correcto de los elementos del lenguaje.

En conjunto, el análisis semántico y la tabla de símbolos permiten que el compilador actúe como un intérprete lógico del código fuente, asegurando que cada instrucción tenga sentido dentro del contexto del lenguaje y evitando errores que podrían pasar desapercibidos en fases anteriores.

🧠 Resumen de funciones del Análisis Semántico
El análisis semántico cumple varias funciones esenciales dentro del compilador:

Verificación de tipos en expresiones Se asegura de que los operadores tengan operandos compatibles. Por ejemplo, no se permite sumar una cadena con un número si el lenguaje no lo admite.

Chequeo de declaraciones y uso de variables Valida que las variables estén declaradas antes de usarse y que no se repitan en el mismo ámbito.

Control de ámbitos (scope) Verifica que los identificadores se usen dentro del contexto donde fueron definidos, respetando las reglas de visibilidad.

Revisión de parámetros y retorno en funciones Comprueba que las funciones reciban el número correcto de argumentos y que el tipo de retorno sea el esperado.

Aplicación de coerciones de tipo En algunos lenguajes, se permite convertir automáticamente un tipo en otro compatible. Por ejemplo, convertir un entero en flotante para una operación aritmética mixta. Esto se refleja en el árbol sintáctico con nodos adicionales como inttofloat.

### Ejemplos de errores semanticos
| Tipo de error | Ejemplo   | 
|--------|--------|
| Variable no declarada    | x = 5; sin haber declarado x   | 
| Asignación incompatible de tipos     | int x = "hola"; | 
| Número incorrecto de argumentos     | Llamar a suma(2) cuando la función espera dos parámetros | 
| Índice de arreglo no entero    | array[3.5] en un lenguaje que exige índices enteros |

### 🗂️ Tabla de Símbolos
- **Definición:** estructura que almacena información sobre identificadores del programa.  
- **Información típica almacenada:**
  - Nombre del símbolo.  
  - Tipo de dato.  
  - Ámbito (global, local).  
  - Valor (si aplica).  
- **Funciones principales:**
  - Registrar símbolos al declararse.  
  - Consultar símbolos al usarse.  
  - Manejar múltiples ámbitos mediante pilas o estructuras anidadas.  

---

## ⚙️ Funciones del Análisis Semántico
1. ✅ **Verificación de tipos** → asegura compatibilidad de operaciones.  
2. ✅ **Control de declaración y uso de variables** → evita identificadores desconocidos.  
3. ✅ **Manejo de ámbitos** → distingue variables globales y locales.  
4. ✅ **Otras verificaciones** → retorno correcto en funciones, uso de constantes, etc.  

---

 Integración con el intérprete
   - Cómo se realiza el análisis semántico usando el AST
   - Manejo de errores semánticos
   - Ejemplo de implementación



## 🏗️ Implementación de una Tabla de Símbolos
- Qué información almacena
   - Estructuras de datos típicas (tablas hash, árboles, etc.)
   - Gestión de ámbitos (pila de tablas de símbolos)
   - Ejemplo práctico con pseudocódigo

- **Estructuras de datos comunes:**
  - Diccionarios (*hash maps*).  
  - Pilas de tablas (para manejar bloques y funciones).  
- **Operaciones básicas:**
  - `insertar(nombre, tipo, ámbito)`  
  - `consultar(nombre)`  
  - `entrarÁmbito()` y `salirÁmbito()`  
- **Ejemplo de representación simplificada:**  

```txt
Tabla Global:
 ├── a : int
 └── b : string
```

---

## 🧪 Ejemplo Práctico

### 📄 Código de entrada
```txt
int a;
a = 10;
string b;
b = a + "hola";
```

### 🌳 Árbol Sintáctico Simplificado
Estructura jerárquica de declaraciones y asignaciones.

![Árbol sintáctico de ejemplo](img/arbol_sintactico.png "Árbol sintáctico")

### 🗂️ Tabla de Símbolos
| Nombre | Tipo   | Ámbito  | Valor |
|--------|--------|---------|-------|
| a      | int    | global  | 10    |
| b      | string | global  | ""    |

![Tabla de símbolos representada gráficamente](img/tabla_simbolos.png "Tabla de símbolos")

### ✅ Verificación Semántica
- `a = 10;` → **válido**.  
- `b = a + "hola";` → **error de tipos** (`int + string`).  

---

## 📝 Conclusión
  - Cómo el análisis semántico prepara para la generación de código
   - Relación con la fase de ejecución en el intérprete
- El análisis semántico garantiza que el programa sea **lógicamente válido**.  
- La tabla de símbolos funciona como la **memoria del compilador** durante la verificación.  
- Ambos elementos preparan el terreno para la **generación de código**.  
- En ANTLR, se implementan mediante **listeners/visitors** que recorren el árbol sintáctico y actualizan/consultan la tabla.  

---

## 📚 Referencias
- Aho, A., Lam, M., Sethi, R., & Ullman, J. (2006). *Compilers: Principles, Techniques, and Tools*. Pearson.  
- Documentación oficial de ANTLR: [https://www.antlr.org/](https://www.antlr.org/)  
- Videos del Prof. Jaime A. Pavlich-Mariscal (Pontificia Universidad Javeriana).  
