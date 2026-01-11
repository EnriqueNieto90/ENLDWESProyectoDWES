# FUNCIONES Y CLASES DE PHP - GUÍA DE ESTUDIO

**Autor:** Enrique Nieto Lorenzo  
**Curso:** DAW2 - IES Los Sauces 2025/2026  
**Asignatura:** DWES - Desarrollo Web en Entorno Servidor

---

## ÍNDICE

- [1. Variables Superglobales](#1-variables-superglobales)
- [2. Funciones](#2-funciones)
  - [2.1 Funciones de Validación de Variables](#21-funciones-de-validación-de-variables)
  - [2.2 Funciones de Salida](#22-funciones-de-salida)
  - [2.3 Funciones de Cadenas y Formato](#23-funciones-de-cadenas-y-formato)
  - [2.4 Funciones de Arrays](#24-funciones-de-arrays)
  - [2.5 Funciones de JSON](#25-funciones-de-json)
  - [2.6 Funciones de Archivos](#26-funciones-de-archivos)
  - [2.7 Funciones de Red y HTTP](#27-funciones-de-red-y-http)
- [3. Clases](#3-clases)
  - [3.1 Clase DateTime](#31-clase-datetime)
  - [3.2 Clase PDO](#32-clase-pdo)
  - [3.3 Clase PDOStatement](#33-clase-pdostatement)
  - [3.4 Clase PDOException](#34-clase-pdoexception)
  - [3.5 Clase DOMDocument](#35-clase-domdocument)
- [4. Referencias Rápidas](#4-referencias-rápidas)

---

## 1. VARIABLES SUPERGLOBALES

Las variables superglobales en PHP son **arrays predefinidos** que están disponibles en todos los ámbitos del script sin necesidad de usar la palabra clave `global`.

### Características

- **Ámbito global:** Accesibles desde cualquier parte del script (funciones, métodos, clases).
- **Predefinidas:** No requieren declaración previa.
- **Arrays asociativos:** Se accede a sus valores mediante claves (índices).

### Tabla de Variables Superglobales

| Variable | Contenido | Uso Típico |
|----------|-----------|------------|
| **`$GLOBALS`** | Contiene **todas las variables** definidas en el ámbito global | Acceder a variables globales desde funciones |
| **`$_SERVER`** | Información del servidor web y entorno de ejecución | `$_SERVER['REMOTE_ADDR']` (IP), `$_SERVER['REQUEST_METHOD']` |
| **`$_GET`** | Variables pasadas por URL (método HTTP GET) | `pagina.php?id=10` → `$_GET['id']` |
| **`$_POST`** | Variables pasadas por método HTTP POST | Recibir datos de formularios (contraseñas, textos) |
| **`$_REQUEST`** | Combinación de `$_GET`, `$_POST` y `$_COOKIE` | Acceder a datos sin importar origen **(uso desaconsejado)** |
| **`$_SESSION`** | Variables de sesión persistentes entre páginas | Mantener login, carrito. Requiere `session_start()` |
| **`$_COOKIE`** | Variables pasadas por cookies HTTP | Leer preferencias de usuario, tokens |
| **`$_FILES`** | Información sobre archivos subidos | Manejar nombre, tamaño, ubicación de uploads |
| **`$_ENV`** | Variables de entorno del sistema operativo | Configuraciones específicas del servidor |

**Documentación:** https://www.php.net/manual/es/language.variables.superglobals.php

---

## 2. FUNCIONES

Una función en PHP es un **bloque de código reutilizable** que realiza una tarea específica. Puede recibir parámetros y devolver valores.

```php
// Ejemplo de función
function saludar($nombre) {
    return "Hola, " . $nombre . "!";
}

echo saludar("Mundo");  // Resultado: Hola, Mundo!
```

### Tabla Resumen de Funciones

| Función | Descripción | Retorna | Cuándo Usar |
|---------|-------------|---------|-------------|
| `is_null()` | Verifica si es estrictamente NULL | bool | Validación estricta de NULL |
| `empty()` | Verifica si está "vacío" | bool | Validar campos de formulario |
| `isset()` | Verifica si existe y no es NULL | bool | Comprobar `$_POST`, `$_GET` |
| `var_dump()` | Muestra tipo y valor detallado | void | Debug y desarrollo |
| `print_r()` | Muestra información legible de arrays | string/bool | Inspeccionar arrays |
| `nl2br()` | Convierte `\n` en `<br>` | string | Mostrar texto con saltos |
| `htmlspecialchars()` | Escapa caracteres HTML | string | Prevenir XSS |
| `json_encode()` | Convierte a JSON | string | APIs y AJAX |
| `json_decode()` | Convierte JSON a PHP | mixed | Leer APIs y AJAX |
| `file_put_contents()` | Escribe en archivo | int/false | Guardar logs, datos |
| `basename()` | Extrae nombre de archivo | string | Procesar rutas |
| `header()` | Envía cabeceras HTTP | void | Redirecciones, descargas |

---

### 2.1 Funciones de Validación de Variables

Funciones que indican si una variable está en un determinado estado.

---

#### is_null()

**Descripción:** Determina si una variable es **estrictamente NULL**. Devuelve `true` solo si el valor es `null`.

**Sintaxis:**
```php
is_null(mixed $var): bool
```

**Ejemplo:**
```php
$a = 0;
$b = null;
$c = "";

var_dump(is_null($a));  // bool(false) - 0 no es NULL
var_dump(is_null($b));  // bool(true)  - null ES NULL
var_dump(is_null($c));  // bool(false) - "" no es NULL
```

**Cuándo usar:** Cuando necesitas verificar específicamente si una variable es NULL, sin considerar otros valores "vacíos".

**Documentación:** https://www.php.net/manual/es/function.is-null.php

---

#### empty()

**Descripción:** Determina si una variable está "vacía". Considera vacíos: `""`, `0`, `"0"`, `null`, `false`, `[]`.

**Sintaxis:**
```php
empty(mixed $var): bool
```

**Ejemplo:**
```php
$cadenaVacia = "";
$cero = 0;
$ceroString = "0";
$arrayVacio = [];
$textoConContenido = "Hola";

var_dump(empty($cadenaVacia));       // bool(true)
var_dump(empty($cero));              // bool(true)
var_dump(empty($ceroString));        // bool(true)
var_dump(empty($arrayVacio));        // bool(true)
var_dump(empty($textoConContenido)); // bool(false)
```

**Cuándo usar:** Validar campos de formulario antes de procesarlos. No genera warning si la variable no existe.

**Documentación:** https://www.php.net/manual/es/function.empty.php

---

#### isset()

**Descripción:** Comprueba si una variable está **declarada** y es **diferente de NULL**.

**Sintaxis:**
```php
isset(mixed $var, mixed ...$vars): bool
```

**Ejemplo:**
```php
$a = "";
$b = null;
$c = 0;

var_dump(isset($a));  // bool(true)  - existe y no es null
var_dump(isset($b));  // bool(false) - es null
var_dump(isset($c));  // bool(true)  - 0 no es null
var_dump(isset($d));  // bool(false) - no existe

// Verificar múltiples variables
var_dump(isset($a, $c));  // bool(true) - ambas existen y no son null
```

**Cuándo usar:** Verificar si existen datos en `$_POST`, `$_GET`, `$_SESSION` antes de usarlos.

**Advertencia:** `isset()` funciona únicamente con variables. Para verificar constantes, usa `defined()`.

**Documentación:** https://www.php.net/manual/es/function.isset.php

---

#### Tabla Comparativa: is_null() vs empty() vs isset()

```
$a - variable con valor no nulo (ej: TRUE)
$b - variable con valor null ($b = null;)
$c - variable no declarada
$d - variable con valor que evalúa a FALSE (ej: "", FALSE, [])
$e - variable declarada pero sin valor asignado
```

| Valor | `is_null()` | `empty()` | `isset()` |
|-------|:-----------:|:---------:|:---------:|
| `null` | ✅ true | ✅ true | ❌ false |
| `""` (cadena vacía) | ❌ false | ✅ true | ✅ true |
| `0` (número) | ❌ false | ✅ true | ✅ true |
| `"0"` (string) | ❌ false | ✅ true | ✅ true |
| `false` | ❌ false | ✅ true | ✅ true |
| `[]` (array vacío) | ❌ false | ✅ true | ✅ true |
| Variable no definida | ⚠️ Warning | ✅ true | ❌ false |

**Regla práctica:**
- Usa `isset()` para comprobar si existe un dato de formulario
- Usa `empty()` para validar si un campo tiene contenido útil
- Usa `is_null()` para verificar específicamente valores NULL

---

### 2.2 Funciones de Salida

---

#### echo

**Descripción:** Muestra una o más cadenas de texto. Es ligeramente más rápido que `print`.

**Sintaxis:**
```php
echo string ...$expressions;
```

**Ejemplo:**
```php
$nombre = "Enrique";
echo "Hola ", $nombre, "!";  // Resultado: Hola Enrique!
echo "Línea 1<br>";
echo "Línea 2";
```

**Documentación:** https://www.php.net/manual/es/function.echo.php

---

#### print

**Descripción:** Muestra una sola cadena de texto. Siempre devuelve `1`.

**Sintaxis:**
```php
print string $expression;
```

**Ejemplo:**
```php
$resultado = print "Texto mostrado";  // $resultado = 1
```

**Documentación:** https://www.php.net/manual/es/function.print.php

---

#### printf

**Descripción:** Muestra una cadena formateada usando especificadores de formato.

**Sintaxis:**
```php
printf(string $format, mixed ...$values): int
```

**Ejemplo:**
```php
$nombre = "Enrique";
$edad = 25;
$precio = 19.5;

printf("Nombre: %s, Edad: %d años", $nombre, $edad);
// Resultado: Nombre: Enrique, Edad: 25 años

printf("Precio: %.2f €", $precio);
// Resultado: Precio: 19.50 €

printf("Número con ceros: %05d", 42);
// Resultado: Número con ceros: 00042
```

**Especificadores comunes:**
- `%s` - String
- `%d` - Entero decimal
- `%f` - Float
- `%.2f` - Float con 2 decimales
- `%05d` - Entero con 5 dígitos (rellena con ceros)

**Documentación:** https://www.php.net/manual/es/function.printf.php

---

#### var_dump()

**Descripción:** Muestra información estructurada y detallada de una variable: **tipo**, **longitud** y **valor**. Es la herramienta de depuración más completa.

**Sintaxis:**
```php
var_dump(mixed $value, mixed ...$values): void
```

**Ejemplo:**
```php
$texto = "Hola";
$numero = 42;
$array = ["a" => 1, "b" => 2];

var_dump($texto);
// Resultado: string(4) "Hola"

var_dump($numero);
// Resultado: int(42)

var_dump($array);
// Resultado: array(2) { ["a"]=> int(1) ["b"]=> int(2) }
```

**Cuándo usar:** Depuración durante el desarrollo. Ver estructura exacta de variables complejas.

**Documentación:** https://www.php.net/manual/es/function.var-dump.php

---

#### print_r()

**Descripción:** Imprime información legible sobre una variable. Ideal para inspeccionar arrays y objetos.

**Sintaxis:**
```php
print_r(mixed $value, bool $return = false): string|bool
```

**Ejemplo:**
```php
$usuario = [
    "nombre" => "Enrique",
    "edad" => 25,
    "cursos" => ["PHP", "JavaScript", "SQL"]
];

print_r($usuario);
// Resultado:
// Array (
//     [nombre] => Enrique
//     [edad] => 25
//     [cursos] => Array (
//         [0] => PHP
//         [1] => JavaScript
//         [2] => SQL
//     )
// )

// Capturar en variable en vez de mostrar
$salida = print_r($usuario, true);
```

**Documentación:** https://www.php.net/manual/es/function.print-r.php

---

#### phpinfo()

**Descripción:** Muestra información completa sobre la configuración de PHP.

**Sintaxis:**
```php
phpinfo(int $flags = INFO_ALL): bool
```

**Ejemplo:**
```php
// Muestra toda la información
phpinfo();

// Solo información de módulos
phpinfo(INFO_MODULES);
```

**Documentación:** https://www.php.net/manual/es/function.phpinfo.php

---

#### highlight_file()

**Descripción:** Colorea (resalta) el código fuente de un archivo PHP usando la sintaxis de HTML.

**Sintaxis:**
```php
highlight_file(string $filename, bool $return = false): string|bool
```

**Ejemplo:**
```php
// Mostrar código de un archivo
highlight_file("mi_script.php");

// Capturar en variable
$codigo = highlight_file("mi_script.php", true);
```

**Cuándo usar:** Mostrar código fuente con colores en una página web.

**Documentación:** https://www.php.net/manual/es/function.highlight-file.php

---

#### exit / die

**Descripción:** Termina la ejecución del script. `die()` es un alias de `exit()`.

**Sintaxis:**
```php
exit(string|int $status = 0): void
```

**Ejemplo:**
```php
// Terminar después de redirección
header("Location: login.php");
exit;

// Terminar con mensaje de error
if (!$conexion) {
    exit("Error: No se pudo conectar a la base de datos");
}

// Terminar con código de estado
exit(1);  // Indica error al sistema
```

**Documentación:** https://www.php.net/manual/es/function.exit.php

---

### 2.3 Funciones de Cadenas y Formato

---

#### nl2br()

**Descripción:** Inserta `<br>` antes de cada salto de línea (`\n`, `\r\n`, `\r`).

**Sintaxis:**
```php
nl2br(string $string, bool $use_xhtml = true): string
```

**Ejemplo:**
```php
$texto = "Primera línea\nSegunda línea\nTercera línea";
echo nl2br($texto);
// Resultado:
// Primera línea<br />
// Segunda línea<br />
// Tercera línea
```

**Cuándo usar:** Mostrar texto de textarea con saltos de línea en HTML.

**Documentación:** https://www.php.net/manual/es/function.nl2br.php

---

#### htmlspecialchars()

**Descripción:** Convierte caracteres especiales en entidades HTML. **Esencial para prevenir XSS**.

**Conversiones:**
- `&` → `&amp;`
- `"` → `&quot;`
- `'` → `&#039;`
- `<` → `&lt;`
- `>` → `&gt;`

**Sintaxis:**
```php
htmlspecialchars(string $string, int $flags = ENT_QUOTES): string
```

**Ejemplo:**
```php
$usuarioMalicioso = '<script>alert("XSS")</script>';

// SIN escapar (PELIGROSO)
echo $usuarioMalicioso;  // Ejecutaría JavaScript

// CON escapar (SEGURO)
echo htmlspecialchars($usuarioMalicioso);
// Resultado: &lt;script&gt;alert(&quot;XSS&quot;)&lt;/script&gt;
```

**Cuándo usar:** **SIEMPRE** al mostrar datos que vienen del usuario.

**Documentación:** https://www.php.net/manual/es/function.htmlspecialchars.php

---

#### str_replace()

**Descripción:** Reemplaza todas las ocurrencias de una subcadena.

**Sintaxis:**
```php
str_replace(string|array $search, string|array $replace, string|array $subject): string|array
```

**Ejemplo:**
```php
$texto = "Hola mundo, mundo cruel";

echo str_replace("mundo", "universo", $texto);
// Resultado: Hola universo, universo cruel

// Reemplazos múltiples
$buscar = ["malo", "feo", "triste"];
$reemplazar = ["bueno", "bonito", "feliz"];
echo str_replace($buscar, $reemplazar, "El día malo y feo me pone triste");
// Resultado: El día bueno y bonito me pone feliz
```

**Documentación:** https://www.php.net/manual/es/function.str-replace.php

---

#### strtolower() / strtoupper()

**Descripción:** Convierte una cadena a minúsculas o mayúsculas.

**Sintaxis:**
```php
strtolower(string $string): string
strtoupper(string $string): string
```

**Ejemplo:**
```php
$texto = "Hola Mundo";

echo strtolower($texto);  // Resultado: hola mundo
echo strtoupper($texto);  // Resultado: HOLA MUNDO
```

**Nota:** Para caracteres especiales (ñ, acentos), usar `mb_strtolower()` y `mb_strtoupper()`.

**Documentación:** https://www.php.net/manual/es/function.strtolower.php

---

#### number_format()

**Descripción:** Formatea un número con separadores de miles y decimales.

**Sintaxis:**
```php
number_format(float $num, int $decimals = 0, ?string $decimal_separator = ".", ?string $thousands_separator = ","): string
```

**Ejemplo:**
```php
$precio = 1234567.891;

echo number_format($precio);
// Resultado: 1,234,568

echo number_format($precio, 2);
// Resultado: 1,234,567.89

// Formato español (punto miles, coma decimales)
echo number_format($precio, 2, ',', '.');
// Resultado: 1.234.567,89
```

**Documentación:** https://www.php.net/manual/es/function.number-format.php

---

### 2.4 Funciones de Arrays

---

#### list()

**Descripción:** Asigna variables desde un array en una sola operación (desestructuración).

**Sintaxis:**
```php
list(mixed $var, mixed ...$vars): array
```

**Ejemplo:**
```php
$datos = ["Enrique", "Nieto", 25];

list($nombre, $apellido, $edad) = $datos;
echo "$nombre $apellido tiene $edad años";
// Resultado: Enrique Nieto tiene 25 años

// Omitir valores
list($nombre, , $edad) = $datos;

// Con arrays asociativos (PHP 7.1+)
$usuario = ["nombre" => "Enrique", "edad" => 25];
["nombre" => $nombre, "edad" => $edad] = $usuario;
```

**Documentación:** https://www.php.net/manual/es/function.list.php

---

#### Funciones de Iteración de Arrays

| Función | Descripción |
|---------|-------------|
| `reset($array)` | Mueve el puntero al primer elemento y lo devuelve |
| `current($array)` | Devuelve el valor del elemento actual |
| `key($array)` | Devuelve la clave del elemento actual |
| `next($array)` | Avanza el puntero y devuelve el siguiente valor |
| `end($array)` | Mueve el puntero al último elemento |

**Ejemplo:**
```php
$colores = ["rojo", "verde", "azul"];

echo reset($colores);    // Resultado: rojo (y puntero al inicio)
echo current($colores);  // Resultado: rojo
echo key($colores);      // Resultado: 0
echo next($colores);     // Resultado: verde
echo current($colores);  // Resultado: verde
```

**Documentación:** https://www.php.net/manual/es/function.reset.php

---

### 2.5 Funciones de JSON

Las funciones JSON en PHP permiten manejar datos en formato JSON (JavaScript Object Notation), crucial para APIs y comunicación AJAX.

---

#### json_encode()

**Descripción:** Convierte un valor PHP (array, objeto) a una cadena JSON.

**Sintaxis:**
```php
json_encode(mixed $value, int $flags = 0, int $depth = 512): string|false
```

**Ejemplo:**
```php
$usuario = [
    "nombre" => "Enrique",
    "edad" => 25,
    "activo" => true
];

echo json_encode($usuario);
// Resultado: {"nombre":"Enrique","edad":25,"activo":true}

// Con formato legible
echo json_encode($usuario, JSON_PRETTY_PRINT);
// Resultado:
// {
//     "nombre": "Enrique",
//     "edad": 25,
//     "activo": true
// }
```

**Cuándo usar:** Enviar datos a APIs, respuestas AJAX, almacenar configuraciones.

**Documentación:** https://www.php.net/manual/es/function.json-encode.php

---

#### json_decode()

**Descripción:** Convierte una cadena JSON a un valor PHP. Solo funciona con cadenas codificadas en UTF-8.

**Sintaxis:**
```php
json_decode(string $json, ?bool $associative = null, int $depth = 512, int $flags = 0): mixed
```

**Ejemplo:**
```php
$json = '{"nombre":"Enrique","edad":25,"activo":true}';

// Como objeto (por defecto)
$objeto = json_decode($json);
echo $objeto->nombre;  // Resultado: Enrique

// Como array asociativo
$array = json_decode($json, true);
echo $array['nombre'];  // Resultado: Enrique
```

**Cuándo usar:** Leer respuestas de APIs, datos almacenados en JSON.

**Documentación:** https://www.php.net/manual/es/function.json-decode.php

---

### 2.6 Funciones de Archivos

---

#### file_get_contents()

**Descripción:** Lee todo el contenido de un archivo en una cadena.

**Sintaxis:**
```php
file_get_contents(string $filename): string|false
```

**Ejemplo:**
```php
// Leer archivo local
$contenido = file_get_contents("config.txt");

// Leer URL
$html = file_get_contents("https://ejemplo.com/pagina.html");
```

**Documentación:** https://www.php.net/manual/es/function.file-get-contents.php

---

#### file_put_contents()

**Descripción:** Escribe contenido en un archivo. Lo crea si no existe.

Esta función sigue estas reglas:
1. Crea el archivo si no existe
2. Abre el archivo
3. Bloquea el archivo si `LOCK_EX` está activado
4. Si `FILE_APPEND` está definido, añade al final. Si no, sobrescribe
5. Escribe los datos y cierra el archivo

**Sintaxis:**
```php
file_put_contents(string $filename, mixed $data, int $flags = 0): int|false
```

**Ejemplo:**
```php
// Sobrescribir archivo
file_put_contents("datos.txt", "Contenido nuevo");

// Añadir al final (append)
file_put_contents("log.txt", "Nueva línea\n", FILE_APPEND);

// Con bloqueo para evitar conflictos
file_put_contents("datos.txt", $contenido, LOCK_EX);
```

**Importante:** Verifica permisos de escritura. Retorna `false` si falla.

**Documentación:** https://www.php.net/manual/es/function.file-put-contents.php

---

#### file_exists()

**Descripción:** Verifica si un archivo o directorio existe.

**Sintaxis:**
```php
file_exists(string $filename): bool
```

**Ejemplo:**
```php
if (file_exists("config.php")) {
    require_once "config.php";
} else {
    echo "Archivo de configuración no encontrado";
}
```

**Documentación:** https://www.php.net/manual/es/function.file-exists.php

---

#### basename()

**Descripción:** Devuelve el nombre del componente final de una ruta.

**Nota:** `basename()` actúa de manera ingenua y no tiene conocimiento del sistema de archivos subyacente.

**Sintaxis:**
```php
basename(string $path, string $suffix = ""): string
```

**Ejemplo:**
```php
echo basename("/var/www/html/index.php");
// Resultado: index.php

echo basename("/var/www/html/index.php", ".php");
// Resultado: index

echo basename($_SERVER['PHP_SELF']);
// Resultado: nombre_del_script_actual.php
```

**Cuándo usar:** Procesar rutas de archivos subidos, mostrar nombres sin ruta completa.

**Documentación:** https://www.php.net/manual/es/function.basename.php

---

#### require_once / include_once

**Descripción:** Incluye y evalúa un archivo. `require` detiene el script si falla, `include` solo muestra warning.

**Sintaxis:**
```php
require_once 'archivo.php';
include_once 'archivo.php';
```

**Ejemplo:**
```php
// Incluir configuración (obligatorio)
require_once 'config/confAPP.php';

// Incluir módulo opcional
include_once 'modulos/estadisticas.php';
```

**Diferencias:**

| Función | Si falla... | Si ya está incluido... |
|---------|-------------|------------------------|
| `require` | Error fatal, detiene script | Lo incluye de nuevo |
| `require_once` | Error fatal, detiene script | Lo omite |
| `include` | Warning, continúa | Lo incluye de nuevo |
| `include_once` | Warning, continúa | Lo omite |

**Documentación:** https://www.php.net/manual/es/function.require-once.php

---

### 2.7 Funciones de Red y HTTP

---

#### header()

**Descripción:** Envía una cabecera HTTP sin procesar al navegador. **Debe llamarse antes de cualquier salida**.

**Sintaxis:**
```php
header(string $header, bool $replace = true, int $response_code = 0): void
```

**Ejemplos de uso:**

```php
// REDIRECCIÓN
header('Location: pagina.php');
exit;  // Siempre usar exit después de redirección

// TIPO DE CONTENIDO
header('Content-Type: application/json');
header('Content-Type: text/html; charset=utf-8');

// DESCARGA DE ARCHIVO
header('Content-Type: application/pdf');
header('Content-Disposition: attachment; filename="documento.pdf"');

// AUTENTICACIÓN BÁSICA
header('WWW-Authenticate: Basic Realm="Área Restringida"');
header('HTTP/1.0 401 Unauthorized');

// CACHÉ
header('Cache-Control: no-cache, no-store, must-revalidate');
header('Pragma: no-cache');
header('Expires: 0');
```

**Error común:** Si aparece "Headers already sent", significa que ya se envió contenido (HTML, espacios, BOM) antes de `header()`.

**Documentación:** https://www.php.net/manual/es/function.header.php

---

#### setcookie()

**Descripción:** Envía una cookie al navegador.

**Sintaxis:**
```php
setcookie(string $name, string $value = "", int $expires_or_options = 0): bool
```

**Ejemplo:**
```php
// Cookie que dura 1 semana
setcookie("idioma", "ES", time() + (7 * 24 * 60 * 60));

// Cookie que expira al cerrar navegador
setcookie("sesion_temp", "abc123");

// Eliminar cookie
setcookie("idioma", "", time() - 3600);
```

**Documentación:** https://www.php.net/manual/es/function.setcookie.php

---

#### session_start()

**Descripción:** Inicia o reanuda una sesión.

**Sintaxis:**
```php
session_start(array $options = []): bool
```

**Ejemplo:**
```php
// Iniciar sesión
session_start();

// Guardar datos
$_SESSION['usuario'] = 'Enrique';
$_SESSION['logueado'] = true;

// Leer datos
echo $_SESSION['usuario'];

// Destruir sesión
session_destroy();
```

**Documentación:** https://www.php.net/manual/es/function.session-start.php

---

## 3. CLASES

Una clase en PHP es una **plantilla** que define propiedades (variables) y métodos (funciones) que tendrán los objetos creados a partir de ella. Las clases son un pilar de la programación orientada a objetos (POO).

```php
// Ejemplo de clase
class Persona {
    public $nombre;
    public $edad;

    public function __construct($nombre, $edad) {
        $this->nombre = $nombre;
        $this->edad = $edad;
    }

    public function presentarse() {
        echo "Hola, soy {$this->nombre} y tengo {$this->edad} años.";
    }
}

$persona1 = new Persona("Enrique", 25);
$persona1->presentarse();  // Resultado: Hola, soy Enrique y tengo 25 años.
```

### Tabla Resumen de Clases

| Clase | Descripción | Uso Principal |
|-------|-------------|---------------|
| `DateTime` | Manipulación de fechas y horas | Cálculos, formateo, zonas horarias |
| `PDO` | Conexión a bases de datos | Conectar MySQL, PostgreSQL, SQLite |
| `PDOStatement` | Resultado de consultas preparadas | Ejecutar queries, obtener resultados |
| `PDOException` | Excepciones de errores de BD | Capturar y manejar errores |
| `DOMDocument` | Manipulación de HTML/XML | Parsear, crear, modificar documentos |

---

### 3.1 Clase DateTime

Clase orientada a objetos para manejar fechas y horas de manera robusta. Es mejor alternativa a la función `date()`.

**Documentación:** https://www.php.net/manual/es/class.datetime.php

---

#### Crear objetos DateTime

```php
// Fecha y hora actual
$ahora = new DateTime();

// Fecha específica
$fecha = new DateTime('2025-12-25');

// Fecha y hora específica
$fechaHora = new DateTime('2025-12-25 18:30:00');

// Desde timestamp
$desdeTimestamp = new DateTime('@1735156200');
```

---

#### Métodos principales

**format() - Formatear fecha**

Devuelve la fecha/hora en el formato especificado como cadena.

```php
$fecha = new DateTime('2025-01-15 14:30:00');

echo $fecha->format('d/m/Y');         // Resultado: 15/01/2025
echo $fecha->format('H:i:s');         // Resultado: 14:30:00
echo $fecha->format('d-m-Y H:i');     // Resultado: 15-01-2025 14:30
echo $fecha->format('l, d F Y');      // Resultado: Wednesday, 15 January 2025
```

**Documentación:** https://www.php.net/manual/es/datetime.format.php

---

**modify() - Modificar fecha**

Modifica un objeto DateTime añadiendo o restando tiempo.

```php
$fecha = new DateTime();

$fecha->modify('+1 day');      // Sumar 1 día
$fecha->modify('-2 weeks');    // Restar 2 semanas
$fecha->modify('+3 months');   // Sumar 3 meses
$fecha->modify('next monday'); // Próximo lunes
```

---

**add() / sub() - Añadir o restar intervalos**

```php
$fecha = new DateTime();

// Añadir 1 mes y 5 días
$fecha->add(new DateInterval('P1M5D'));

// Restar 2 horas
$fecha->sub(new DateInterval('PT2H'));
```

**Documentación:** https://www.php.net/manual/es/datetime.add.php

---

**diff() - Diferencia entre fechas**

Calcula la diferencia entre dos objetos DateTime, devolviendo un objeto DateInterval.

```php
$inicio = new DateTime('2025-01-01');
$fin = new DateTime('2025-12-31');

$diferencia = $inicio->diff($fin);

echo $diferencia->days;                    // Resultado: 364 (días totales)
echo $diferencia->format('%R%a días');     // Resultado: +364 días
echo $diferencia->format('%m meses y %d días'); // Resultado: 11 meses y 30 días
```

**Documentación:** https://www.php.net/manual/es/datetime.diff.php

---

**getTimestamp() - Obtener timestamp UNIX**

Devuelve el timestamp UNIX (segundos desde 1970).

```php
$fecha = new DateTime();
echo $fecha->getTimestamp();  // Resultado: 1736956800
```

**Documentación:** https://www.php.net/manual/es/datetime.gettimestamp.php

---

#### Funciones de Configuración de Zona Horaria

```php
// Establecer zona horaria por defecto
date_default_timezone_set('Europe/Madrid');

// Configuración regional para formatos
setlocale(LC_TIME, 'es_ES.utf8');
```

**Documentación:**
- https://www.php.net/manual/es/function.date-default-timezone-set.php
- https://www.php.net/manual/es/function.setlocale.php

---

### 3.2 Clase PDO

PDO (PHP Data Objects) representa una conexión entre PHP y un servidor de base de datos. Soporta MySQL, MariaDB, PostgreSQL, SQLite, etc.

**Documentación:** https://www.php.net/manual/es/class.pdo.php

---

#### Conexión a Base de Datos

```php
try {
    $miDB = new PDO(
        'mysql:host=localhost;dbname=mi_base;charset=utf8',
        'usuario',
        'contraseña'
    );
    
    // Configurar modo de errores (IMPORTANTE)
    $miDB->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);
    
    echo "Conexión exitosa!";
    
} catch (PDOException $e) {
    echo "Error de conexión: " . $e->getMessage();
} finally {
    unset($miDB);  // Liberar conexión
}
```

---

#### Métodos principales de PDO

**query() - Consulta directa (sin parámetros)**

Ejecuta una sentencia SQL directamente. Solo usar para consultas sin valores variables.

```php
$resultado = $miDB->query('SELECT * FROM usuarios');

while ($fila = $resultado->fetchObject()) {
    echo $fila->nombre;
}
```

**Documentación:** https://www.php.net/manual/es/pdo.query.php

---

**prepare() - Consulta preparada (con parámetros)**

Prepara una sentencia SQL para su ejecución. Fundamental para prevenir inyección SQL.

```php
$consulta = $miDB->prepare('SELECT * FROM usuarios WHERE id = :id');
$consulta->execute([':id' => 5]);
$usuario = $consulta->fetchObject();

// Inserción con parámetros
$sql = 'INSERT INTO usuarios (nombre, email) VALUES (:nombre, :email)';
$consulta = $miDB->prepare($sql);
$consulta->execute([
    ':nombre' => 'Enrique',
    ':email' => 'enrique@ejemplo.com'
]);
```

**Documentación:** https://www.php.net/manual/es/pdo.prepare.php

---

**exec() - Ejecutar sin resultados**

Ejecuta una consulta SQL y devuelve el número de filas afectadas. No devuelve resultados para SELECT.

```php
$filasAfectadas = $miDB->exec("DELETE FROM logs WHERE fecha < '2024-01-01'");
echo "Se eliminaron $filasAfectadas registros";
```

**Documentación:** https://www.php.net/manual/es/pdo.exec.php

---

**Transacciones**

```php
try {
    $miDB->beginTransaction();
    
    $miDB->exec("UPDATE cuentas SET saldo = saldo - 100 WHERE id = 1");
    $miDB->exec("UPDATE cuentas SET saldo = saldo + 100 WHERE id = 2");
    
    $miDB->commit();  // Confirmar cambios
    
} catch (PDOException $e) {
    $miDB->rollBack();  // Revertir cambios
    echo "Error: " . $e->getMessage();
}
```

**Documentación:**
- https://www.php.net/manual/es/pdo.begintransaction.php
- https://www.php.net/manual/es/pdo.commit.php
- https://www.php.net/manual/es/pdo.rollback.php

---

### 3.3 Clase PDOStatement

Representa una consulta preparada y, una vez ejecutada, el conjunto de resultados asociado.

**Documentación:** https://www.php.net/manual/es/class.pdostatement.php

---

#### Métodos principales

**execute() - Ejecutar consulta preparada**

```php
$consulta = $miDB->prepare('INSERT INTO usuarios (nombre, email) VALUES (:nombre, :email)');

$consulta->execute([
    ':nombre' => 'Enrique',
    ':email' => 'enrique@ejemplo.com'
]);
```

**Documentación:** https://www.php.net/manual/es/pdostatement.execute.php

---

**bindParam() / bindValue() - Vincular parámetros**

```php
$consulta = $miDB->prepare('SELECT * FROM usuarios WHERE edad > :edad');

// bindValue: vincula el valor actual
$edad = 18;
$consulta->bindValue(':edad', $edad, PDO::PARAM_INT);

// bindParam: vincula la variable (se evalúa al ejecutar)
$consulta->bindParam(':edad', $edad, PDO::PARAM_INT);
```

**Documentación:** https://www.php.net/manual/es/pdostatement.bindparam.php

---

**fetchObject() - Obtener fila como objeto**

Recupera la siguiente fila y la devuelve como un objeto.

```php
$consulta = $miDB->prepare('SELECT * FROM usuarios WHERE id = :id');
$consulta->execute([':id' => 1]);

$usuario = $consulta->fetchObject();
echo $usuario->nombre;
```

**Documentación:** https://www.php.net/manual/es/pdostatement.fetchobject.php

---

**fetch() - Obtener fila con modo específico**

```php
// Como array asociativo
$fila = $consulta->fetch(PDO::FETCH_ASSOC);
echo $fila['nombre'];

// Como array indexado
$fila = $consulta->fetch(PDO::FETCH_NUM);
echo $fila[0];

// Como ambos
$fila = $consulta->fetch(PDO::FETCH_BOTH);
```

**Documentación:** https://www.php.net/manual/es/pdostatement.fetch.php

---

**fetchAll() - Obtener todas las filas**

```php
$consulta = $miDB->query('SELECT * FROM usuarios');
$usuarios = $consulta->fetchAll(PDO::FETCH_ASSOC);

foreach ($usuarios as $usuario) {
    echo $usuario['nombre'];
}
```

---

**rowCount() - Contar filas afectadas**

Devuelve el número de filas afectadas por la última sentencia DELETE, INSERT o UPDATE.

```php
$consulta = $miDB->prepare('UPDATE usuarios SET activo = 1 WHERE edad > 18');
$consulta->execute();

echo "Usuarios actualizados: " . $consulta->rowCount();
```

**Documentación:** https://www.php.net/manual/es/pdostatement.rowcount.php

---

### 3.4 Clase PDOException

Representa un error emitido por PDO. No debe lanzarse manualmente desde el propio código.

**Documentación:** https://www.php.net/manual/es/class.pdoexception.php

---

#### Métodos principales

```php
try {
    $miDB = new PDO($dsn, $usuario, $password);
    $miDB->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);
    
    $consulta = $miDB->query('SELECT * FROM tabla_que_no_existe');
    
} catch (PDOException $e) {
    // Obtener mensaje de error
    echo $e->getMessage();
    // Resultado: SQLSTATE[42S02]: Base table or view not found...
    
    // Obtener código de error
    echo $e->getCode();
    // Resultado: 42S02
    
    // Obtener archivo y línea
    echo $e->getFile();
    echo $e->getLine();
}
```

**Cuándo usar:** Capturar errores de BD para manejarlos sin exponer información sensible.

**Documentación:**
- https://www.php.net/manual/es/exception.getmessage.php
- https://www.php.net/manual/es/exception.getcode.php

---

### 3.5 Clase DOMDocument

Representa un documento HTML o XML completo; será la raíz del árbol del documento.

**Documentación:** https://www.php.net/manual/es/class.domdocument.php

---

#### Métodos principales

**Cargar HTML**

```php
$doc = new DOMDocument();

// Desde string
$doc->loadHTML('<html><body><h1>Título</h1></body></html>');

// Desde archivo
$doc->loadHTMLFile('pagina.html');

// Guardar
echo $doc->saveHTML();
```

**Documentación:** https://www.php.net/manual/es/domdocument.loadhtml.php

---

**Cargar XML**

```php
$doc = new DOMDocument();

// Desde string
$doc->loadXML('<root><item>Valor</item></root>');

// Desde archivo
$doc->load('datos.xml');

// Guardar
echo $doc->saveXML();
```

**Documentación:** https://www.php.net/manual/es/domdocument.loadxml.php

---

**Obtener elementos**

```php
$doc = new DOMDocument();
$doc->loadHTML('<h1>Título 1</h1><h1>Título 2</h1><p>Párrafo</p>');

// Por nombre de etiqueta
$titulos = $doc->getElementsByTagName('h1');
foreach ($titulos as $h1) {
    echo $h1->nodeValue;  // Resultado: Título 1, Título 2
}

// Por ID
$elemento = $doc->getElementById('mi-id');
```

**Documentación:** https://www.php.net/manual/es/domdocument.getelementsbytagname.php

---

**Crear elementos**

```php
$doc = new DOMDocument('1.0', 'UTF-8');

// Crear elemento raíz
$raiz = $doc->createElement('usuarios');
$doc->appendChild($raiz);

// Crear elemento con contenido
$usuario = $doc->createElement('nombre', 'Enrique');
$raiz->appendChild($usuario);

// Guardar
echo $doc->saveXML();
// Resultado:
// <?xml version="1.0" encoding="UTF-8"?>
// <usuarios><nombre>Enrique</nombre></usuarios>
```

**Documentación:** https://www.php.net/manual/es/domdocument.createelement.php

---

**Validar documento**

```php
$doc = new DOMDocument();
$doc->load('documento.xml');

if ($doc->validate()) {
    echo "¡Este documento es válido!";
}
```

**Documentación:** https://www.php.net/manual/es/domdocument.validate.php

---

## 4. REFERENCIAS RÁPIDAS

### 4.1 Especificadores de Formato (printf)

| Especificador | Descripción | Ejemplo |
|---------------|-------------|---------|
| `%s` | String | `"Hola"` |
| `%d` | Entero decimal | `42` |
| `%f` | Float | `3.141593` |
| `%.2f` | Float con 2 decimales | `3.14` |
| `%05d` | Entero con 5 dígitos (rellena con 0) | `00042` |
| `%x` | Hexadecimal (minúsculas) | `1a` |
| `%X` | Hexadecimal (mayúsculas) | `1A` |
| `%%` | Símbolo % literal | `%` |

---

### 4.2 Formatos de Fecha (DateTime::format)

| Carácter | Descripción | Ejemplo |
|----------|-------------|---------|
| `d` | Día (2 dígitos) | `01` a `31` |
| `j` | Día (sin ceros) | `1` a `31` |
| `m` | Mes (2 dígitos) | `01` a `12` |
| `n` | Mes (sin ceros) | `1` a `12` |
| `Y` | Año (4 dígitos) | `2025` |
| `y` | Año (2 dígitos) | `25` |
| `H` | Hora 24h (2 dígitos) | `00` a `23` |
| `G` | Hora 24h (sin ceros) | `0` a `23` |
| `i` | Minutos | `00` a `59` |
| `s` | Segundos | `00` a `59` |
| `l` | Día de la semana | `Monday` |
| `D` | Día abreviado | `Mon` |
| `F` | Mes completo | `January` |
| `M` | Mes abreviado | `Jan` |

**Ejemplos comunes:**
```php
$fecha->format('d/m/Y');          // 15/01/2025
$fecha->format('Y-m-d');          // 2025-01-15
$fecha->format('d-m-Y H:i:s');    // 15-01-2025 14:30:00
$fecha->format('l, d F Y');       // Wednesday, 15 January 2025
```

---

### 4.3 Constantes PDO Comunes

| Constante | Descripción |
|-----------|-------------|
| `PDO::ATTR_ERRMODE` | Modo de reporte de errores |
| `PDO::ERRMODE_EXCEPTION` | Lanzar excepciones en errores |
| `PDO::FETCH_ASSOC` | Devolver array asociativo |
| `PDO::FETCH_OBJ` | Devolver objeto |
| `PDO::FETCH_BOTH` | Array asociativo e indexado |
| `PDO::PARAM_INT` | Tipo entero para bindParam |
| `PDO::PARAM_STR` | Tipo string para bindParam |

---

**Enrique Nieto Lorenzo**  
**DAW2 - IES Los Sauces - Curso 2025/2026**
