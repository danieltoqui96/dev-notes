# C# – Guía de referencia rápida

## Índice

<details>
<summary><strong>Ver índice</strong></summary>

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
</details>

## Estructura de un Programa en C#

Todo programa en C# reside en un namespace y una clase; el método `Main` es su punto de entrada.

```csharp
using System;

namespace MiAplicacion
{
    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine("¡Hola, C#!");
        }
    }
}
```

<details>
<summary>Ver más</summary>

A partir de C# 9, es posible escribir un programa _sin_ definir explícitamente la clase `Program` ni el método `Main` (característica conocida como _top-level statements_). El código puede escribirse directamente en el archivo fuente, y el compilador generará automáticamente un `Main` implícito en tiempo de compilación.

</details>

<p align="right">
  <a href="#índice">⬆️</a>
</p>

## Tipos de datos

C# ofrece tipos primitivos para valores simples y tipos por referencia para estructuras más complejas.

```csharp
// Primitivos
int    numero    = 42;
double real      = 3.14;
bool   activo    = true;
char   letra     = 'A';

// Referencia
string texto    = "Hola";
int[]  arreglo  = {1,2,3};
```

| Tipo      | Tamaño   | Rango / Notas                  |
| --------- | -------- | ------------------------------ |
| `byte`    | 8 bits   | 0 a 255                        |
| `short`   | 16 bits  | -32 768 a 32 767               |
| `int`     | 32 bits  | -2 147 483 648 a 2 147 483 647 |
| `long`    | 64 bits  | ‘L’ al final                   |
| `float`   | 32 bits  | sufijo ‘f’                     |
| `double`  | 64 bits  | predeterminado para decimales  |
| `decimal` | 128 bits | sufijo ‘m’, alta precisión     |
| `char`    | 16 bits  | Un solo carácter Unicode       |
| `bool`    | 1 bit    | true o false                   |

### Cadenas de texto

Manipulación de `string` con métodos esenciales.

```csharp
string s = "  Hola C#  ";
Console.WriteLine(s.Trim().ToUpper()); // "HOLA C#"
Console.WriteLine(s.Substring(2,4));   // "Hola"
```

| Método         | Descripción             |
| -------------- | ----------------------- |
| `Trim()`       | Quita espacios extremos |
| `ToUpper()`    | Mayúsculas              |
| `ToLower()`    | Minúsculas              |
| `Contains()`   | Busca subcadena         |
| `Replace(a,b)` | Reemplaza texto         |

<details><summary>Ver más</summary>

**Tipos no primitivos** (_reference types_): Clases, arrays, cadenas, `object`, genéricos, `enum`, `struct`, `interface`.

**Valor vs referencia**:

- Tipos de valor (`int`, `struct`): almacenan dato directamente.
- Tipos de referencia (`string`, `List<T>`): almacenan puntero al objeto.
</details>

<p align="right">
  <a href="#índice">⬆️</a>
</p>

## Conversiones de tipo

Transformaciones comunes entre números, cadenas y fechas

```csharp
// 1. Números: implícita y explícita
int    a = 10;
long   b = a;           // implícita widening
double d = 9.7;
int    i = (int)d;      // explícita narrowing

// 2. De string a valor
if (int.TryParse("123", out int v))
    Console.WriteLine(v);               // 123
DateTime dt = DateTime.Parse("2025-05-24");

// 3. A string
string s1 = i.ToString();
string s2 = dt.ToString("yyyy-MM-dd");

```

| Origen                | Destino  | Método                         |
| --------------------- | -------- | ------------------------------ |
| `int` → `long`        | numérico | implícita                      |
| `double` → `int`      | numérico | explícita `(int)d`             |
| `string` → `int`      | entero   | `int.Parse` / `int.TryParse`   |
| `string` → `DateTime` | fecha    | `DateTime.Parse` / `TryParse`  |
| cualquier tipo        | `string` | `ToString()` / `ToString(fmt)` |

<details><summary>Ver más</summary>

- **Usa TryParse** para evitar excepciones en formatos inválidos.
- **Parse** lanza excepción si falla. - Para enums: `Enum.TryParse<T>(...)`.
</details>

<p align="right">
  <a href="#índice">⬆️</a>
</p>

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
