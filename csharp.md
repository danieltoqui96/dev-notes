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

## Conversiones de tipos

Cast explícito e implícito; `Parse` vs `TryParse`.

```csharp
double d = 9.7;
int    i = (int)d;            // cast, pierde .7

if (int.TryParse("123", out int v))
    Console.WriteLine(v);     // 123
```

| Conversión     | Implícita | Explícita (cast) | Parse vs TryParse           |
| -------------- | --------- | ---------------- | --------------------------- |
| `int`→`long`   | ✓         | —                | —                           |
| `double`→`int` | —         | ✓ `(int)d`       | `int.Parse`, `int.TryParse` |

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

Estructuras para agrupar elementos del mismo tipo y manipularlos de forma ordenada.

```csharp
// Array unidimensional y longitud
int[] numeros = {1, 2, 3, 4};
Console.WriteLine(numeros.Length);  // 4

// foreach
foreach (var n in numeros)
    Console.Write($"{n} ");

// Array multidimensional
int[,] matrix = { {1,2}, {3,4} };
Console.WriteLine(matrix[1,0]);     // 3

// Array irregular (jagged)
int[][] jagged = {
    new int[] {1,2},
    new int[] {3,4,5}
};
Console.WriteLine(jagged[1].Length); // 3

// Pasar array como parámetro
static int Sumar(int[] a)
{
    int s = 0;
    foreach (var x in a) s += x;
    return s;
}

// ArrayList vs List<T>
var al = new System.Collections.ArrayList {1, "dos", 3};
var list = new List<int> {1, 2, 3};
```

| Tipo        | Descripción                                     |
| ----------- | ----------------------------------------------- |
| `T[]`       | Array fijo de elementos tipo `T`                |
| `T[,]`      | Matriz rectangular de 2 o más dimensiones       |
| `T[][]`     | “Jagged”: array de arrays con longitudes libres |
| `ArrayList` | Colección no genérica, boxing/unboxing          |
| `List<T>`   | Colección genérica, dinámica y tipada           |

<details><summary>Ver más</summary>

- **Redimensionar**: `Array.Resize(ref numeros, 6);` cambia el tamaño (rellena con valores por defecto si crece).
- **Inicializar**: `var a = new int[5];` todos elementos inicializan a 0.
- **Índice y rango** (.NET Core+): `numeros[^1]` (último elemento), `numeros[1..3]` (subarray del índice 1 al 2)
- **Colecciones avanzadas**: `Dictionary<TKey,TValue>`, `HashSet<T>`, `Queue<T>`, `Stack<T>`, etc.
- **Conversión**: `list.ToArray()`, `array.ToList()` (LINQ)
- **ArrayList**: En código moderno se prefiere `List<T>` por seguridad de tipos y rendimiento.
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
- **this**: referencia al objeto actual (instancia en curso), útil para distinguir campos de parámetros o invocar constructores sobrecargados.
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
