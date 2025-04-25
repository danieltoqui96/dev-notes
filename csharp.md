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
### Convenciones para nombres y estándares en C#

En C#, existen convenciones ampliamente utilizadas para nombrar distintos elementos del código:

- **Clases** → `PascalCase`  
  Ejemplo: `Persona`, `ProductoController`

- **Métodos** → `PascalCase`  
  Ejemplo: `CalcularSueldo()`, `ObtenerDatos()`

- **Argumentos** → `camelCase`  
  Ejemplo: `string nombre`, `int cantidadTotal`

- **Variables locales** → `camelCase`  
  Ejemplo: `contador`, `precioFinal`

### Conversiones de tipos en C#

En C# se pueden convertir valores de un tipo a otro. Esto se puede hacer de forma **implícita** o **explícita**.

---

#### 🔄 Conversión implícita

Se realiza automáticamente cuando **no hay pérdida de datos**.  
Solo ocurre entre tipos compatibles (por ejemplo, de `int` a `long`, o de `float` a `double`).

```csharp
int numero = 100;
long numeroGrande = numero;       // Conversión implícita
float decimalCorto = 5.5f;
double decimalLargo = decimalCorto; // También implícita
```

####⚠️ Conversión explícita (cast)

Se necesita cuando puede haber **pérdida** de datos o tipos incompatibles.
Se hace usando **cast** entre paréntesis.

```csharp
double decimalLargo = 10.75;
int entero = (int)decimalLargo;   // Pierde la parte decimal (entero = 10)

long grande = 99999;
short pequeño = (short)grande;    // Posible pérdida de datos

string numeroTexto = "123";
int numero = int.Parse(numeroTexto);           // Conversión explícita desde string
int seguro = Convert.ToInt32(numeroTexto);     // Otra forma con manejo de errores
```
#### Parsing

El **parsing** es el proceso de convertir un `string` a otro tipo de dato (como `int`, `double`, `bool`, etc.).

```csharp
// Parse
//Convierte un `string` a un tipo determinado.  
//⚠️ Lanza excepción (`FormatException`) si el texto no tiene el formato correcto.
int numero = int.Parse("123");             // OK
double decimal1 = double.Parse("3.14");    // OK
bool estado = bool.Parse("true");          // OK

// int.Parse("abc"); // ❌ Error en tiempo de ejecución

// TryParse
// Convierte de forma segura, devolviendo true o false según si la conversión fue exitosa.
// Evita excepciones si el formato no es válido.

string entrada = "456";
int resultado;

if (int.TryParse(entrada, out resultado))
    Console.WriteLine("Número válido: " + resultado);
else
    Console.WriteLine("Entrada no válida");
```
### Constantes
Una **constante** es un valor que se define una vez y **no puede cambiar** durante la ejecución del programa.

Se declara usando la palabra clave `const`.

```csharp
const tipo nombre = valor;
const double PI = 3.1416;
const int MaxIntentos = 5;
const string MensajeBienvenida = "¡Hola!";
```

