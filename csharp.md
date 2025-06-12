# C# – Guía de referencia rápida

## Índice

- [Estructura de un Programa en C#](#estructura-de-un-programa-en-c)
- [Tipos de datos](#tipos-de-datos)
- [Conversiones de tipo](#conversiones-de-tipo)
- [Variables](#variables)
- [Operadores](#operadores)
- [Estructuras de control](#estructuras-de-control)
- [Métodos](#métodos)
- [Excepciones](#excepciones)
- [Arrays y Colecciones](#arrays-y-colecciones)
- [Programación Orientada a Objetos](#programación-orientada-a-objetos)
- [Otros conceptos útiles](#otros-conceptos-útiles)

## Estructura de un Programa en C#

Todo programa en C# sigue una estructura básica que define su punto de entrada y organización. Con las versiones modernas del lenguaje, esta estructura se ha simplificado notablemente.

**Estructura Clásica**

Esta es la estructura tradicional que encontrará en muchos proyectos y tutoriales. Define explícitamente todos los contenedores necesarios.

```csharp
using System;

namespace MiAplicacion
{
    class Program
    {
        static void Main(string[] args)
        {
            // Este es el punto de entrada del programa
            Console.WriteLine("¡Hola, C#!");
        }
    }
}
```

A continuación se detalla el propósito de cada componente clave de esta estructura.

Componente | Ejemplo | Propósito
---------- | ------- | ---------
Directiva `using` | `using System;` | Importa un namespace para poder usar sus clases (como `Console`) sin escribir su nombre completo (`System.Console`).
`namespace` | `namespace MiAplicacion` | Contenedor lógico para organizar el código y evitar colisiones de nombres entre clases.
`class` | `class Program` | Plantilla para crear objetos. Todo el código ejecutable en C# debe estar dentro de una clase.
Método `Main` | `static void Main(...)` | El punto de entrada del programa. Es el primer método que se ejecuta al iniciar la aplicación.
Parámetros `Main` | `(string[] args)` | Es un array de strings que permite recibir argumentos pasados por la línea de comandos al ejecutar el programa.

<details> <summary><strong>Ver la versión moderna (Top-Level Statements)</strong></summary>

A partir de C# 9, se puede omitir toda la estructura ceremonial (`namespace`, `class`, `Main`). El compilador la genera automáticamente por detrás, simplificando enormemente el código.

El mismo programa anterior se puede escribir simplemente así:

```csharp
using System;

Console.WriteLine("Hola, C#!");
```

Esta sintaxis es ideal para programas pequeños, scripts, y especialmente para quienes están aprendiendo, ya que les permite enfocarse directamente en la lógica sin distracciones.

</details>

<p align="right"> <a href="#índice">⬆️</a> </p>

## Tipos de datos

En C#, las variables deben tener un tipo definido que determina qué clase de datos pueden almacenar y cómo se comportan.

**Declaración de Variables**

Es el primer paso para usar una variable. La práctica moderna favorece la inferencia de tipo con var cuando el tipo es obvio.

```csharp
// 1. Declaración explícita: se especifica el tipo de dato.
int edad = 30;

// 2. Inferencia de tipo con 'var': el compilador deduce el tipo.
var nombre = "Ana";       // El compilador infiere que es un 'string'.
var estaActivo = true;   // El compilador infiere que es un 'bool'.
```

**Tipos de Valor vs. Tipos de Referencia**

Esta distinción es fundamental para entender cómo se comporta el programa.
- **Tipos de Valor (`int`, `double`, `bool`, `struct`)**: Almacenan el dato directamente. Al pasarlos a un método, el método recibe una **copia**.
- **Tipos de Referencia (`string`, `array`, `List<T>`)**: Almacenan una **referencia** (una dirección de memoria) al objeto. Un método puede usar esta referencia para modificar el objeto original.

```csharp
void ModificarDatos(int numeroCopia, List<int> listaOriginal)
{
    numeroCopia = 100;      // Solo modifica la copia local.
    listaOriginal.Add(100); // Modifica el objeto original.
}

var miNumero = 10;
var miLista = new List<int> { 10 };
ModificarDatos(miNumero, miLista);

Console.WriteLine($"Número original: {miNumero}");         // Salida: 10 (no cambió)
Console.WriteLine($"Elementos en lista: {miLista.Count}"); // Salida: 2 (sí cambió)
```

**Cadenas de Texto (`string`)**

Es un tipo de referencia para manipular texto. Su característica más importante es que es **inmutable**: cualquier operación de modificación no altera la cadena original, sino que devuelve una nueva cadena con el resultado.

```csharp
// Declaración básica de una cadena de texto
string saludo = "Hola, mundo!";
var nombreUsuario = "Ana"; // También se puede usar 'var'
```

**Interpolación de Cadenas**

Es la forma moderna y recomendada para construir cadenas. Se usa el prefijo `$` y las variables se encierran en llaves `{}`.

```csharp
var lenguaje = "C#";
var version = 11;

// Forma moderna y recomendada
var mensaje = $"Bienvenido a {lenguaje} {version}!";
Console.WriteLine(mensaje); // Salida: Bienvenido a C# 11!

// Los métodos de manipulación devuelven una nueva cadena
var saludo = " hola mundo ";
var saludoFormateado = saludo.Trim().ToUpper(); // " HOLA MUNDO " -> "HOLA MUNDO"
Console.WriteLine($"Original: '{saludo}', Formateado: '{saludoFormateado}'");
```

Método Común | Descripción
------------ | -----------
`string.IsNullOrEmpty(str)` | Comprueba si una cadena es `null` o vacía (`""`).
`string.IsNullOrWhiteSpace(str)` | Comprueba si es `null`, vacía o solo contiene espacios.
`.ToUpper()` / `.ToLower()` | Devuelve una nueva cadena en mayúsculas o minúsculas.
`.Trim()` | Devuelve una nueva cadena sin espacios al inicio y al final.
`.Split(separador)` | Divide la cadena en un array de subcadenas.
`.Contains(subcadena)` | Comprueba si la cadena contiene un texto específico.

<details> <summary><strong>Ver más sobre Tipos y Cadenas</strong></summary>

- **Valores Nulos (`null`) y Tipos Nulables (`?`)**: `null` representa la ausencia de valor. Los tipos de referencia pueden ser `null`, pero los de valor no. Para permitir que un tipo de valor sea nulo, se le añade un `?` (ej: `int? edad = null;`). Es fundamental comprobar si una variable es nula antes de usarla para evitar errores.
- **Strings Literales (`@`)**: Para escribir texto que contiene muchos caracteres de escape, como rutas de archivo, se puede usar el prefijo `@`. Esto le dice al compilador que ignore las secuencias de escape.
  ```csharp
  string ruta = @"C:\Usuarios\MiUsuario\Documentos"; // Más limpio que "C:\\Usuarios\\..."
  ```
- **Tabla de Tipos de Datos Comunes**:

Tipo | Uso Común | Ejemplo | Familia
---- | --------- | ------- | -------
`int` | Números enteros. | `10`, `-50` | Valor
`double` | Números con decimales. Es el tipo por defecto. | `3.14`, `-0.01` | Valor
`decimal` | Alta precisión para cálculos financieros. | `9.99m` (sufijo `m`) | Valor
`bool` | Valores lógicos de verdadero o falso. | `true`, `false` | Valor
`char` | Un solo carácter de texto. | `'A'` (comillas simples) | Valor
`string` | Secuencias de texto. | `"Hola Mundo"` | Referencia

</details>

<p align="right"> <a href="#índice">⬆️</a> </p>

## Conversiones de tipo

Dado que C# es un lenguaje de tipado estático, una variable no puede cambiar de tipo una vez declarada. Sin embargo, a menudo es necesario convertir un valor de un tipo a otro. Estas operaciones se conocen como conversiones o "casting".

**1. Conversiones Implícitas (Seguras)**

Ocurren automáticamente cuando el compilador sabe que la conversión es segura y no habrá pérdida de datos. Esto sucede típicamente al convertir de un tipo numérico más pequeño a uno más grande.

```csharp
int miEntero = 123;
long miLong = miEntero; // Conversión implícita de int a long, es seguro.

double miDouble = miEntero; // Conversión implícita de int a double.
```

La jerarquía de conversiones implícitas seguras sigue un orden lógico:

`byte` → `short` → `int` → `long` → `float` → `double`

**2. Conversiones Explícitas (Casting)**

Se deben solicitar manualmente cuando existe el riesgo de perder información, como al convertir de un tipo más grande a uno más pequeño. Se utiliza el operador de "cast" `(tipo)` para forzar la conversión.

```csharp
double miDouble = 123.45;
// int miEntero = miDouble; // Error de compilación: no se puede convertir implícitamente.

int miEntero = (int)miDouble; // Conversión explícita (casting).
Console.WriteLine(miEntero);  // Salida: 123 (se pierde la parte decimal).
```

**3. Conversiones con Clases de Ayuda (`Parse`, `TryParse`, `Convert`)**

Para convertir entre tipos no compatibles, como un `string` a un `int`, no se puede usar el casting. En su lugar, se utilizan métodos especializados.

**`TryParse` (Método recomendado y seguro)**

Intenta convertir un `string` a otro tipo. Si tiene éxito, devuelve `true` y el valor convertido; si falla, devuelve `false` y no lanza una excepción. Es ideal para manejar entradas de usuario o datos externos.

```csharp
string textoNumero = "123";
if (int.TryParse(textoNumero, out int numeroConvertido))
{
    Console.WriteLine($"Conversión exitosa: {numeroConvertido}");
}
else
{
    Console.WriteLine("La cadena no es un número válido.");
}
```

**`ToString()` (Conversión a string)**

Prácticamente cualquier variable puede convertirse a su representación en `string` usando el método `.ToString()`.

```csharp
int numero = 42;
string textoDelNumero = numero.ToString(); // "42"

DateTime fecha = DateTime.Now;
string textoFecha = fecha.ToString("yyyy-MM-dd"); // "2025-06-12"
```

Situación | Método Recomendado | Ejemplo
--------- | ------------------ | -------
De tipo numérico menor a mayor | Implícita (automática) | `long l = mi_entero;`
De tipo numérico mayor a menor | Explícita (Casting) | `int i = (int)mi_double;`
De string a un tipo (`int`, `double`, etc.) | `TryParse` | `int.TryParse(s, out int i)`
De cualquier tipo a `string` | `.ToString()` | `mi_numero.ToString();`

<details> <summary><strong>Ver más sobre Conversiones</strong></summary>

- **`Parse` vs. `TryParse`**: El método `int.Parse("123")` hace lo mismo que `TryParse`, pero si la conversión falla, **lanza una excepción** que detiene el programa si no se maneja. Usa `Parse` solo cuando estés 100% seguro de que el formato del `string` es correcto.
- **Clase `System.Convert`**: Proporciona un conjunto de métodos para conversiones, como `Convert.ToInt32()`, `Convert.ToDouble()`, etc. Una ventaja es que `Convert.ToInt32(null)` devuelve `0`, mientras que `int.Parse(null)` lanza una excepción. Es una alternativa robusta para muchos escenarios.
</details>

<p align="right"> <a href="#índice">⬆️</a> </p>

## Variables

Declaración e inicialización de variables; `var` infiere tipo.

```csharp
int  edad = 30;       // declaración y asignación
var nombre = "Ana";   // infiere string
float altura;
altura = 1.75f;       // asignación posterior
```

<details><summary>Ver más</summary>

- Variables locales deben inicializarse antes de usar.
- Campos de clase se inicializan por defecto (0, null, false).

**Constantes vs readonly**

|            | `const`             | `readonly`                    |
| ---------- | ------------------- | ----------------------------- |
| Asignación | Solo en declaración | Declaración o constructor     |
| Tiempo     | Compilación         | Ejecución                     |
| Ámbito     | Siempre estático    | Instancia o estático opcional |

```csharp
const double PI = 3.1416;
readonly int  max;
public Clase(int valor) { max = valor; }
```

</details>

<p align="right">
  <a href="#índice">⬆️</a>
</p>

## Operadores

Permiten realizar cálculos, comparaciones y lógica en expresiones.

```csharp
int x = 10;
x += 5;
Console.WriteLine(x);         // 15
Console.WriteLine(x != 0);    // True
bool dentroRango = (x > 0 && x < 20);
Console.WriteLine(dentroRango ? "Dentro de rango" : "Fuera de rango");  // Dentro de rango
```

| Categoría   | Operadores              | Ejemplo          |
| ----------- | ----------------------- | ---------------- |
| Aritméticos | `+`, `-`, `*`, `/`, `%` | `a + b`          |
| Asignación  | `=`, `+=`, `-=` …       | `x += 5`         |
| Comparación | `==`, `!=`, `>`, `<`…   | `a >= b`         |
| Lógicos     | `&&`, `\|\|`, `!`       | `a && b`         |
| Incremento  | `++`, `--`              | `i++`            |
| Ternario    | `? :`                   | `x>0? "sí":"no"` |

<details><summary>Ver más</summary>

- **Operador condicional nulo** `?.`: si el operando izquierdo es `null`, la expresión completa devuelve `null` (en lugar de arrojar error).
- **Coalescencia nula** `??:` devuelve el operando derecho si el izquierdo es `null`.
- **Asignación nula** `??=`: asigna el operando derecho a la variable izquierda solo si esta es `null`.
</details>

<p align="right">
  <a href="#índice">⬆️</a>
</p>

## Estructuras de control

Dirigen el flujo: decisiones y bucles.

```csharp
// if-else
int edad = 18;
if (edad < 18) Console.WriteLine("Menor");
else Console.WriteLine("Adulto");

// for
for (int i = 0; i < 3; i++)
    Console.WriteLine(i);

// while
int j = 0;
while (j < 3) { Console.WriteLine(j); j++; }
```

<details><summary>Ver más</summary>

- `switch` para múltiples casos.
- `break` sale de un bucle o un `case` en `switch`; `continue` pasa a la siguiente iteración del bucle.
- `do { … } while (condición);` garantiza ejecutar el bloque al menos una vez.
</details>

<p align="right">
  <a href="#índice">⬆️</a>
</p>

## Métodos

Bloques reutilizables de código; se definen con modificador, tipo de retorno y nombre en PascalCase.

```csharp
public static int Sumar(int x, int y) => x + y;

static void Main()
{
    int r = Sumar(3,4);
    Console.WriteLine(r);  // 7
}
```

<p align="right">
  <a href="#índice">⬆️</a>
</p>

## Excepciones

Control de errores en tiempo de ejecución con `try-catch` (y bloque `finally` opcional).

```csharp
try
{
    int r = 10 / int.Parse("0");
}
catch (DivideByZeroException)
{
    Console.WriteLine("No dividir por cero");
}
finally
{
    Console.WriteLine("Termina intento");
}
```

<details><summary>Ver más</summary>

- **Lanzar una excepción**: usar `throw new Exception("Mensaje");` (idealmente una excepción específica) para generar un error manualmente.
- **Excepciones comunes**: `ArgumentNullException`, `InvalidOperationException`, `FormatException`, etc.
</details>

<p align="right">
  <a href="#índice">⬆️</a>
</p>

## Arrays y Colecciones

Estructuras para agrupar y manipular conjuntos de elementos. Los arrays tienen un tamaño fijo, mientras que las colecciones son dinámicas.

### Arrays

Son colecciones de tamaño fijo y tipo específico. Se clasifican según sus dimensiones.

**Arrays Unidimensionales**

La forma más simple de array: una lista secuencial de elementos.

```csharp
// Declarar e inicializar un array de tamaño fijo
var nombres = new string[3] { "Ana", "Luis", "Marcos" };

// Acceder a un elemento por su índice
Console.WriteLine($"El segundo elemento es: {nombres[1]}"); // Salida: Luis

// Obtener la cantidad de elementos
Console.WriteLine($"Tamaño del array: {nombres.Length}"); // Salida: 3
```

**Arrays Multidimensionales (Matrices)**

Son estructuras rectangulares (como una tabla o una rejilla) donde cada fila tiene la misma cantidad de columnas.

```csharp
// Declarar una matriz de 2 filas y 3 columnas
var matriz = new int[2, 3] 
{
    { 1, 2, 3 }, 
    { 4, 5, 6 }
};

// Acceder al elemento en la fila 1, columna 2
Console.WriteLine($"Elemento [1, 2]: {matriz[1, 2]}"); // Salida: 6
```

**Arrays Irregulares (Jagged)**

Son "arrays de arrays", donde cada array interno puede tener una longitud diferente, creando una estructura escalonada.

```csharp
// Declarar un array irregular con 3 arrays internos
var irregular = new int[3][];
irregular[0] = new int[] { 1, 2 };
irregular[1] = new int[] { 3, 4, 5 };
irregular[2] = new int[] { 6 };

// Acceder al tercer elemento del segundo array interno
Console.WriteLine($"Elemento [1][2]: {irregular[1][2]}"); // Salida: 5
```
| Tipo    | Descripción                                                        |
| ---     | ------------------------------------------------------------------ |
| `T[]`   | **Unidimensional:** Una sola fila de elementos.                    |
| `T[,]`  | **Multidimensional:** Matriz rectangular con filas y columnas.     |
| `T[][]` | **Irregular (Jagged):** Un array cuyos elementos son otros arrays. |


<details> <summary>Ver más</summary>
    
- **Tamaño Fijo**: El tamaño de un array y sus dimensiones se establecen en su creación y no pueden cambiar. Redimensionar un array implica crear una nueva instancia y copiar los elementos, lo cual es una operación costosa.
- **Inicialización**: Al crear un array con `new`, sus elementos se inicializan con el valor por defecto del tipo (ej: 0 para int, null para objetos, false para bool).
- **Acceso**: En matrices, se usa una sola tupla de índices (`[fila, columna]`). En arrays irregulares, se usan múltiples corchetes para acceder a los arrays anidados (`[índice_externo][índice_interno]`).
- **Uso**: Las matrices son ideales para estructuras de datos tabulares como tableros de ajedrez o imágenes. Los arrays irregulares son útiles cuando las sub-listas varían en tamaño, como almacenar los días de cada mes.
</details>

**Colecciones Genéricas: `List<T>`**

Es la colección dinámica más utilizada. Ofrece flexibilidad para agregar o quitar elementos y garantiza la seguridad de tipos.

```csharp
// Crear una lista dinámica de enteros
var numeros = new List<int> { 10, 20, 30 };

// Agregar un nuevo elemento al final
numeros.Add(40);

// Quitar un elemento por su valor
numeros.Remove(20);

// Acceder a un elemento por su índice
Console.WriteLine($"Primer número: {numeros[0]}"); // Salida: 10
```

**Listas Anidadas (Simulando Matrices)**

Las colecciones como `List<T>` no tienen un concepto nativo de "multidimensionalidad" como los arrays. Sin embargo, se pueden anidar para lograr un comportamiento similar al de un array irregular, pero con tamaño dinámico.

```csharp
// Lista de listas para simular una estructura irregular
var listaDeListas = new List<List<int>>
{
    new List<int> { 1, 2 },
    new List<int> { 3, 4, 5 }
};

// Agregar una nueva "fila" (una nueva lista)
listaDeListas.Add(new List<int> { 6, 7 });

// Acceder a un elemento
Console.WriteLine($"Elemento [1][2]: {listaDeListas[1][2]}"); // Salida: 5
```

<details> <summary>Ver más</summary>

- **Ventajas**: A diferencia de los arrays, las listas pueden crecer y encogerse dinámicamente con `Add()`, `Remove()` y otros métodos.
- **Simulación de Matriz**: Para simular una matriz rectangular con listas (`List<List<T>>`), es responsabilidad del programador asegurarse de que todas las listas internas tengan el mismo tamaño.
- **Otras Colecciones Útiles**: `Dictionary<TKey, TValue>` (pares clave-valor), `HashSet<T>` (elementos únicos), `Queue<T>` (FIFO) y `Stack<T>` (LIFO).
</details>

**Iteración de Colecciones**

Existen varias formas de recorrer los elementos de un array o una colección.

```csharp
var frutas = new List<string> { "Manzana", "Pera", "Naranja" };

// 1. Bucle foreach (preferido por su simplicidad)
foreach (var fruta in frutas)
{
    Console.WriteLine(fruta);
}

// 2. Bucle for (útil cuando se necesita el índice)
for (var i = 0; i < frutas.Count; i++)
{
    Console.WriteLine($"Índice {i}: {frutas[i]}");
}
```

<details> <summary><strong>Ver más sobre Iteración</strong></summary>
    
- **Iterar Matrices (`T[,]`)**: Se usan bucles anidados y el método `GetLength(dimensión)` para obtener el tamaño de cada dimensión.
    ```csharp
    var matriz = new int[,] { { 1, 2, 3 }, { 4, 5, 6 } };
    for (int i = 0; i < matriz.GetLength(0); i++) // Dimensión 0: Filas
    {
        for (int j = 0; j < matriz.GetLength(1); j++) // Dimensión 1: Columnas
        {
            Console.Write(matriz[i, j] + " ");
        }
        Console.WriteLine();
    }
    ```
- **Iterar Arrays Irregulares (`T[][]`)**: Se usa Length para el array exterior y luego Length para cada array interior.
  ```csharp
    var irregular = new int[][] 
    {
        new int[] { 1, 2 },
        new int[] { 3, 4, 5 },
    };
    for (int i = 0; i < irregular.Length; i++)
    {
        for (int j = 0; j < irregular[i].Length; j++)
        {
            Console.Write(irregular[i][j] + " ");
        }
        Console.WriteLine();
    }
  ```
- **`List<T>.ForEach()`**: Es una alternativa concisa a `foreach` que usa una expresión lambda: `frutas.ForEach(fruta => Console.WriteLine(fruta));`. No permite `break` ni `continue`.
</details>

<p align="right">
  <a href="#índice">⬆️</a>
</p>

## Programación Orientada a Objetos

La POO permite modelar entidades del mundo real en clases con datos (campos) y comportamiento (métodos), instanciándolas como objetos.

```csharp
// Definición de clase con constructor, propiedad y destructor
class Persona
{
    public string Nombre { get; set; }      // Propiedad automática
    public int Edad { get; set; }

    public Persona(string nombre, int edad)  // Constructor
    {
        Nombre = nombre;
        Edad = edad;
    }

    ~Persona()                              // Destructor (finalizador)
    {
        // Cleanup si fuera necesario
    }
}

// Crear y usar objeto
static void Main()
{
    var p = new Persona("Ana", 28);
    Console.WriteLine($"{p.Nombre}, {p.Edad} años");
}
```

| Elemento      | Sintaxis                      | Nota                                   |
| ------------- | ----------------------------- | -------------------------------------- |
| Clase         | `class Nombre { … }`          | Define tipo de referencia              |
| Constructor   | `public Clase(...) { … }`     | Mismo nombre que la clase              |
| Propiedad     | `public T Prop { get; set; }` | Atajos para campo privado              |
| Destructor    | `~Clase() { … }`              | Se invoca al recolector de basura (GC) |
| Instanciación | `new Clase(args)`             | Reserva y retorna objeto               |

<details><summary>Ver más</summary>
    
- **Encapsulamiento**: usar `private` para campos y exponer sólo con propiedades públicas.
- `this`: referencia al objeto actual (instancia en curso), útil para distinguir campos de parámetros o invocar constructores sobrecargados.
- **Propiedades con lógica** (propiedades completas):
    ```csharp
    private int edad;
    public int Edad
    {
        get => edad;
        set { if (value >= 0) edad = value; }
    }
    ```
- **Herencia y polimorfismo**: `class Empleado : Persona { … }` (hereda de Persona); usar miembros `virtual`/`override` para sobreescribir comportamiento en la subclase.
</details>

<p align="right">
  <a href="#índice">⬆️</a>
</p>

## Otros conceptos útiles

Conceptos adicionales útiles en C#, incluyendo la interacción por consola y las convenciones de nomenclatura.

### Entrada y salida por consola

Lectura y escritura básica en la consola con la clase `Console`.

```csharp
Console.Write("Nombre: ");
string nombre = Console.ReadLine();
Console.WriteLine($"Hola, {nombre}");
```

### Convenciones de nombres

Convenciones de nomenclatura para mejorar la legibilidad y mantenimiento del código:

- **Clases y métodos**: `PascalCase` → `MiClase`, `ObtenerDatos()`
- **Variables y parámetros**: `camelCase` → `totalItems`, `nombreUsuario`
- **Constantes**: `UPPER_SNAKE_CASE` o `PascalCase` → `MAX_INTENTOS`

<p align="right">
  <a href="#índice">⬆️</a>
</p>
