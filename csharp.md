# Hoja de Ayuda: C#

<details>
<summary><strong>Índice</strong></summary>

- [Estructura de un Programa en C#](#estructura-de-un-programa-en-c)
- [Tipos de Datos y Variables](#tipos-de-datos-y-variables)
- [Cadenas de texto: métodos comunes](#cadenas-de-texto-métodos-comunes)
- [Entrada y Salida por Consola](#entrada-y-salida-por-consola)
- [Conversiones de Tipos](#conversiones-de-tipos)
- [Convenciones de Nombres en C#](#convenciones-de-nombres-en-c)

</details>

## Estructura de un Programa en C#

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

## Tipos de Datos y Variables

### Tipos de Datos Primitivos

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

### Tipos de Datos No Primitivos

En **C#**, los **tipos no primitivos** (o tipos por referencia y compuestos) son todos aquellos que **no están representados directamente por un valor simple en memoria**, sino que **referencian objetos** u estructuras más complejas.

```csharp
// String (cadena de texto)
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

### Tipos de Valor vs Tipos de Referencia

Un **tipo de valor** almacena directamente el dato, mientras que un **tipo de referencia** almacena una referencia (puntero) al dato. Esta diferencia afecta el comportamiento al asignar o pasar variables a métodos. Por ejemplo, al pasar una variable a un método por valor se envía una copia, y al pasarla por referencia, el método puede modificar el original:

``` csharp
// Pasaje por valor
void Cambiar(int x) {
    x = 100;
}

int numero = 5;
Cambiar(numero);
Console.WriteLine(numero); // 🔸 Imprime 5 (no cambia)

// Pasaje por referencia (referencia por defecto, e.g. objetos)
void ModificarLista(List<string> lista) {
    lista.Add("Hola");
}

List<string> listaNombres = new List<string>();
ModificarLista(listaNombres);
Console.WriteLine(listaNombres.Count); // 🔸 Imprime 1 (sí cambió)

// Forzar pasaje por referencia con ref (para tipos de valor)
void Cambiar(ref int x) {
    x = 100;
}

int otroNumero = 5;
Cambiar(ref otroNumero);
Console.WriteLine(otroNumero); // 🔸 Imprime 100
```

### Declaración e Inicialización de Variables

La declaración de una variable establece su tipo y nombre. Se puede **declarar** una variable sin asignarle valor inmediatamente, pero **no se puede usar** una variable local sin inicializar (produce error de compilación). Los campos de clase, en cambio, se inicializan automáticamente con valores por defecto (por ejemplo `0` para números, `null` para objetos).

```csharp
// Declaración sin asignar (luego se asigna valor)
int edad;
edad = 25;                        // ✅ Asignación posterior

// Declaración e inicialización en una línea
int altura = 180;

// ❌ Error si se usa sin inicializar:
// Console.WriteLine(edad); // Error: variable no inicializada

// Campos de clase se inicializan automáticamente:
class Persona {
    public int edad;              // valor por defecto: 0
    public string nombre;         // valor por defecto: null
}
```

### Inferencia de Tipo con var

La palabra clave `var` permite declarar variables locales sin especificar el tipo explícitamente; el compilador deduce el tipo a partir del valor asignado. **Nota**: es obligatorio asignar un valor en la declaración cuando se usa `var` (no se puede declarar primero y asignar después).

```csharp
// El compilador infiere el tipo según el valor asignado
var ciudad = "Santiago";         // tipo string
var numero = 42;                 // tipo int

// ❌ No se puede usar var sin inicializar:
// var x; // Error: debe asignarse al declarar
```

### Constantes y campos de solo lectura (`readonly`)

La palabra clave `const` permite definir una **constante**, es decir, un valor que se define una vez y no puede cambiar durante la ejecución del programa. Se declara usando `const` antes del tipo. Por otro lado, un campo marcado como `readonly` también define un valor inmutable, pero su valor se puede asignar **en tiempo de ejecución** (en la declaración o en un constructor) en lugar de en tiempo de compilación.

```csharp
// Ejemplo de const
const tipo nombre = valor;
const double PI = 3.1416;
const int MaxIntentos = 5;
const string MensajeBienvenida = "¡Hola!";

// Ejemplo de readonly
class Configuracion
{
    public readonly int Limite;

    public Configuracion(int limiteUsuario)
    {
        Limite = limiteUsuario; // Se asigna en el constructor
    }
}
```

| Característica   | `const` (constante)                                   | `readonly` (solo lectura)                                      |
|------------------|------------------------------------------------------|----------------------------------------------------------------|
| Asignación        | En tiempo de compilación (debe asignarse al declararla). | En tiempo de ejecución (en la declaración o en el constructor). |
| Ámbito            | Implícitamente estático (valor compartido, no depende de instancias). | Puede ser de instancia o estático (añadiendo `static`).         |
| Tipos permitidos  | Literales constantes (`números`, `char`, `string`, `enum`) y expresiones evaluables en compilación. | Cualquier tipo de dato, incluyendo objetos o estructuras.     |


## Cadenas de texto: métodos comunes

El tipo `string` ofrece muchos métodos útiles para manipular texto (cadenas de caracteres). A continuación, algunos de los métodos más comunes de la clase `string`:

```csharp
string texto = "  Hola Mundo  ";
string nombre = "daniel";

// Longitud de la cadena
int largo = texto.Length;                   // 13

// Convertir a mayúsculas / minúsculas
string mayus = nombre.ToUpper();            // "DANIEL"
string minus = nombre.ToLower();            // "daniel"

// Eliminar espacios en extremos
string limpio = texto.Trim();               // "Hola Mundo"

// Reemplazar texto
string reemplazado = texto.Replace("Hola", "Adiós"); // "  Adiós Mundo  "

// Verificar contenido
bool contiene = texto.Contains("Mundo");    // true

// Verificar comienzo o final específicos
bool empieza = texto.StartsWith("  H");     // true
bool termina = texto.EndsWith("  ");        // true

// Subcadena
string sub = texto.Substring(2, 4);         // "Hola"

// Comparar cadenas (retorna 0 si son iguales)
int comparacion = string.Compare("abc", "ABC", true); // 0 (true = ignora mayúsculas)

// Comprobar cadena vacía o solo espacios
bool vacio = string.IsNullOrEmpty(nombre);   // false
bool blanco = string.IsNullOrWhiteSpace(" "); // true
```

## Entrada y Salida por Consola

La clase `Console` proporciona métodos para interactuar con la consola: escribir en la salida estándar o leer desde la entrada estándar.

```csharp
// Escritura en consola
Console.Write("Texto");             // Imprime sin salto de línea
Console.WriteLine("Texto");         // Imprime con salto de línea al final

// Lectura desde consola
char caracter = (char)Console.Read();         // Lee un solo carácter (como código ASCII)
string cadena = Console.ReadLine();           // Lee una línea completa como string
ConsoleKeyInfo tecla = Console.ReadKey();     // Lee una tecla presionada (sin esperar Enter)
```

## Conversiones de Tipos

En C# se pueden convertir valores de un tipo a otro de forma **implícita** o **explícita**. Una conversión **implícita** ocurre automáticamente cuando no hay posibilidad de pérdida de información, mientras que una conversión **explícita** (cast) se requiere cuando hay riesgo de pérdida de datos o los tipos no son directamente compatibles.

### 🔄 Conversión Implícita

Se realiza automáticamente cuando **no hay pérdida de datos**. Solo ocurre entre tipos compatibles (por ejemplo, de `int` a `long`, o de `float` a `double`).

```csharp
int numero = 100;
long numeroGrande = numero;         // Implícita (int -> long)
float decimalCorto = 5.5f;
double decimalLargo = decimalCorto; // También implícita (float -> double)
```
### ⚠️ Conversión Explícita (cast)

Se utiliza cuando puede haber **pérdida de datos** o al convertir entre tipos incompatibles. Se realiza anteponiendo el tipo deseado entre paréntesis. **Nota**: para convertir cadenas de texto a valores numéricos u otros tipos se deben usar métodos específicos (no es posible con un cast directo).

```csharp
double valorDecimal = 10.75;
int entero = (int)valorDecimal;   // Pierde la parte decimal (entero = 10)

long grande = 99999;
short pequeño = (short)grande;    // Posible pérdida de datos (si excede rango de short)
```

### Conversión de cadenas: Parse y TryParse
La conversión desde texto a un tipo numérico o booleano se realiza mediante métodos de *parsing*. Por ejemplo, `int.Parse` intenta convertir un `string` en entero y lanza una excepción si el formato no es válido. En cambio, `int.TryParse` realiza la conversión de forma segura, devolviendo un valor booleano que indica si tuvo éxito (y proporcionando el resultado convertido via un parámetro `out`).

```csharp
// Parse (convierte string a tipo específico, lanza excepción si falla)
int numero = int.Parse("123");             // OK -> 123 (int)
double decimal1 = double.Parse("3.14");    // OK -> 3.14 (double)
bool estado = bool.Parse("true");          // OK -> true (bool)

// int.Parse("abc"); // ❌ Lanza FormatException (formato inválido)

// TryParse (convierte string de forma segura, sin excepciones)
string entrada = "456";
int resultado;
if (int.TryParse(entrada, out resultado))
    Console.WriteLine("Número válido: " + resultado);
else
    Console.WriteLine("Entrada no válida");
```

| Característica        | `Parse` (ej. `int.Parse`)                      | `TryParse` (ej. `int.TryParse`)                                |
|------------------------|------------------------------------------------|---------------------------------------------------------------|
| Valor devuelto         | Valor convertido del tipo de destino.         | `bool` (`true` si la conversión tuvo éxito, `false` si falló). El valor convertido se obtiene mediante un parámetro `out`. |
| Si el formato es inválido | Lanza una excepción (`FormatException`).       | No lanza excepción; simplemente devuelve `false`.             |


## Convenciones de Nombres en C#

En C#, existen convenciones ampliamente utilizadas para nombrar los distintos elementos del código:

- **Clases** → `PascalCase`  
  Ejemplo: `Persona`, `ProductoController`

- **Métodos** → `PascalCase`  
  Ejemplo: `CalcularSueldo()`, `ObtenerDatos()`

- **Argumentos** → `camelCase`  
  Ejemplo: `string nombre`, `int cantidadTotal`

- **Variables locales** → `camelCase`  
  Ejemplo: `contador`, `precioFinal`

