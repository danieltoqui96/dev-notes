# Hoja de Ayuda: C#

Todo programa en C# comienza dentro de una **clase** y un **namespace**.  
Para que funcione correctamente, se debe incluir la sentencia `using System;` para acceder a funciones básicas como `Console.WriteLine()`.  
El método `Main` es el punto de entrada del programa: ahí comienza la ejecución.

```csharp
// Hola Mundo
using System;

namespace HolaMundo
{
    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine("Hola Mundo");
        }
    }
}
```

## Tipos de datos y variables

### Tipos de datos primitivos

En **C#**, los **tipos de datos primitivos** son aquellos básicos que están integrados en el lenguaje y se usan para almacenar valores simples.

```csharp
// Enteros
byte    edadPequeña = 255;         // 8 bits, 0 a 255
short   numeroCorto = -32000;      // 16 bits
int     numero = 100;              // 32 bits (el más usado)
long    numeroLargo = 100000L;     // 64 bits (agregar 'L')

// Decimales
float   decimalCorto = 3.14f;      // 32 bits, requiere 'f'
double  decimalNormal = 3.1416;    // 64 bits
decimal decimalPreciso = 10.5m;    // Alta precisión, requiere 'm'

// Carácter y texto
char    letra = 'A';               // Un solo carácter

// Booleano
bool    esValido = true;           // true o false
```

### Tipos de datos no primitivos

En **C#**, los **tipos no primitivos** (o tipos por referencia y compuestos) son todos aquellos que **no están directamente representados por un valor simple en memoria**, sino que **referencian objetos**, estructuras o colecciones más complejas.

```csharp
// String
string nombre = "Daniel";

// Objetos y clases
object dato = 123;
Persona persona = new Persona();

// Arreglos
int[] numeros = { 1, 2, 3 };

// Colecciones genéricas
List<string> nombres = new List<string>();
Dictionary<int, string> mapa = new Dictionary<int, string>();

// Enumeraciones
enum Estado { Activo, Inactivo, Suspendido }

// Estructuras
struct Punto {
    public int X;
    public int Y;
}

// Nullable types
int? edadOpcional = null;

// Interfaces
interface IAnimal {
    void HacerSonido();
}
```

### Declaración  e inicialización de variables

```csharp
// Declaración sin asignar
int edad;
edad = 25;                        // ✅ Asignación posterior

// Declaración e inicialización en una línea
int altura = 180;

// ❌ Error si se usa sin asignar:
// Console.WriteLine(edad); // Error: variable no inicializada

// Campos de clase se inicializan automáticamente:
class Persona {
    public int edad;              // valor por defecto: 0
    public string nombre;         // valor por defecto: null
}
```

### Uso de ‘var’ (inferencia de tipo)
```csharp
// El compilador deduce el tipo a partir del valor
var ciudad = "Santiago";         // tipo string
var numero = 42;                 // tipo int

// ❌ No se puede declarar sin asignar:
// var x; // Error: debe asignarse al declarar
```

### Métodos String

```csharp
string texto = "  Hola Mundo  ";
string nombre = "daniel";

// Longitud del string
int largo = texto.Length;                   // 13

// Convertir a mayúsculas / minúsculas
string mayus = nombre.ToUpper();            // "DANIEL"
string minus = nombre.ToLower();            // "daniel"

// Eliminar espacios al inicio y final
string limpio = texto.Trim();               // "Hola Mundo"

// Reemplazar texto
string reemplazado = texto.Replace("Hola", "Adiós"); // "  Adiós Mundo  "

// Verificar si contiene una palabra
bool contiene = texto.Contains("Mundo");    // true

// Verificar si empieza o termina con algo
bool empieza = texto.StartsWith("  H");     // true
bool termina = texto.EndsWith("  ");        // true

// Obtener parte del string
string sub = texto.Substring(2, 4);         // "Hola"

// Comparar cadenas (devuelve 0 si son iguales)
int comparacion = string.Compare("abc", "ABC", true); // 0 (true = ignora mayúsculas)

// Comprobar si está vacío o es null
bool vacio = string.IsNullOrEmpty(nombre);  // false
bool blanco = string.IsNullOrWhiteSpace(" "); // true
```

### Tipos de valor vs tipos de referencia

Cuando pasas una **variable por valor**, se **copia su contenido**.
Cuando pasas una **variable por referencia**, se **comparte la dirección de memoria**, o sea, **modificas el original**.

``` csharp
// por valor
void Cambiar(int x) {
    x = 100;
}

int numero = 5;
Cambiar(numero);
Console.WriteLine(numero); // 🔸 Imprime 5 (no cambia)

// por referencia
void ModificarLista(List<string> lista) {
    lista.Add("Hola");
}

List<string> nombres = new List<string>();
ModificarLista(nombres);
Console.WriteLine(nombres.Count); // 🔸 Imprime 1 (sí cambió)

// pasar valor por referencia
void Cambiar(ref int x) {
    x = 100;
}

int numero = 5;
Cambiar(ref numero);
Console.WriteLine(numero); // 🔸 Imprime 100
```

### Clase Console
```csharp
// Escritura en consola
Console.Write("Texto");             // Imprime sin salto de línea
Console.WriteLine("Texto");         // Imprime con salto de línea al final

// Lectura desde consola
char caracter = (char)Console.Read();         // Lee un solo carácter (como código ASCII)
string cadena = Console.ReadLine();           // Lee una línea completa como string
ConsoleKeyInfo tecla = Console.ReadKey();     // Lee una tecla presionada (sin esperar Enter)
```
