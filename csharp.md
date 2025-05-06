# C# Cheat Sheet

## Índice

<details>
<summary><strong>Ver índice</strong></summary>

- [Estructura de un Programa en C#](#estructura-de-un-programa-en-c#)
- [Tipos de datos](#tipos-de-datos)
- [Variables](#variables)
- [Operadores](#operadores)
- [Estructuras de control](#estructuras-de-control)
- [Métodos](#métodos)
- [Excepciones](#excepciones)
- [Cadenas de texto](#cadenas-de-texto)
- [Entrada y salida por consola](#entrada-y-salida-por-consola)
- [Conversiones de tipos](#conversiones-de-tipos)
- [Convenciones de nombres](#convenciones-de-nombres)
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

<p align="right">
  <a href="#índice">⬆️ volver</a>
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

<details><summary>Ver más</summary>

**Tipos no primitivos** (_reference types_):
Clases, arrays, cadenas, `object`, genéricos, `enum`, `struct`, `interface`.

**Valor vs referencia**:

- Tipos de valor (`int`, `struct`): almacenan dato directamente.
- Tipos de referencia (`string`, `List<T>`): almacenan puntero al objeto.
</details>

<p align="right">
  <a href="#índice">⬆️ volver</a>
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
  <a href="#índice">⬆️ volver</a>
</p>

Permiten realizar cálculos, comparaciones y lógica en expresiones.
Permiten realizar cálculos, comparaciones y lógica en expresiones.
Permiten realizar cálculos, comparaciones y lógica en expresiones.
Permiten realizar cálculos, comparaciones y lógica en expresiones.
Permiten realizar cálculos, comparaciones y lógica en expresiones.
Permiten realizar cálculos, comparaciones y lógica en expresiones.

## Operadores

Permiten realizar cálculos, comparaciones y lógica en expresiones.

| Categoría   | Operadores              | Ejemplo          |
| ----------- | ----------------------- | ---------------- |
| Aritméticos | `+`, `-`, `*`, `/`, `%` | `a + b`          |
| Asignación  | `=`, `+=`, `-=` …       | `x += 5`         |
| Comparación | `==`, `!=`, `>`, `<`…   | `a >= b`         |
| Lógicos     | `&&`, `\|\|`, `!`       | `a && b`         |
| Incremento  | `++`, `--`              | `i++`            |
| Ternario    | `? :`                   | `x>0? "sí":"no"` |

<p align="right">
  <a href="#índice">⬆️ volver</a>
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
- `break` sale de bucle o case; `continue` salta a siguiente iteración.
- `do while` garantiza al menos una ejecución.
</details>

<p align="right">
  <a href="#índice">⬆️ volver</a>
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
  <a href="#índice">⬆️ volver</a>
</p>

## Excepciones

Control de errores en tiempo de ejecución con `try-catch(-finally)`.

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

<p align="right">
  <a href="#índice">⬆️ volver</a>
</p>

## Cadenas de texto

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

<p align="right">
  <a href="#índice">⬆️ volver</a>
</p>

## Entrada y salida por consola

Lectura y escritura usando `Console`.

```csharp
Console.Write("Nombre: ");
string nombre = Console.ReadLine();
Console.WriteLine($"Hola, {nombre}");
```

<p align="right">
  <a href="#índice">⬆️ volver</a>
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
  <a href="#índice">⬆️ volver</a>
</p>

## Convenciones de nombres

Guías para legibilidad y mantenimiento:

- **Clases y métodos**: `PascalCase` → `MiClase`, `ObtenerDatos()`
- **Variables y parámetros**: `camelCase` → `totalItems`, `nombreUsuario`
- **Constantes**: `UPPER_SNAKE_CASE` o `PascalCase` → `MAX_INTENTOS`

<p align="right">
  <a href="#índice">⬆️ volver</a>
</p>
