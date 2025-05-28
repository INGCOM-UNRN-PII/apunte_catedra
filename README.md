# Resumen de temas de la materia (trabajo en progreso)

# Tipos de datos en Java

| Tipo    | Tamaño (bits) | Literales                                                                                                                             | Valor por defecto |
| ------- | ------------- | ------------------------------------------------------------------------------------------------------------------------------------- | ----------------- |
| byte    | 8             | Enteros en rango                                                                                                                      | 0                 |
| short   | 16            | Enteros en rango                                                                                                                      | 0                 |
| int     | 32            | Enteros en rango. Con prefijos: `0` octal, `0X/0x` hexadecimal, `0B/0b` binario                                                       | 0                 |
| long    | 64            | Enteros en rango seguidos de `L/l`                                                                                                    | 0L                |
| float   | 32            | Punto flotante seguido de `F/f`                                                                                                       | 0.0F              |
| double  | 64            | Punto flotante seguido de `D/d`                                                                                                       | 0.0D              |
| boolean | 8             | `true` o `false`                                                                                                                      | `false`           |
| char    | 16            | Caracter entre comillas simples `'a'`, también secuencias Unicode `'\u0041'`, octales `'\101'`, o caracteres especiales `'\n'`,`'\t'` | `'\u0000'`        |
Se pueden usar guiones bajos en los literales numéricos:
```java
long enteroLargo = 123_456_789L;
```

Tambien esta [`java.math.BigInteger`](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/math/BigInteger.html) para números enteros de cualquier tamaño y [`java.math.BigDecimal`](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/math/BigDecimal.html) para números decimales de precisión arbitraria.


# Métodos

Son un bloque de código que se ejecuta cuando es llamado. Existen dentro de una `class`
## Sintaxis completa para un método/función

```java
<modificador acceso><otros modificadores><modificador retorno><nombre método>(parámetros) throws <excepciones>{
//código
    return <variable del mismo tipo del método> //excepto si es `void`
}
```
El retorno puede existir en un método `void`, devolviendo el control a quien llama.
## Tipo de acceso
* ninguno : Si no ponemos nada, el método es visible por todo el paquete
* `public` es visible por todas las demás clases.
* `private` sólo es visible dentro de su clase.
* `protected` sólo visible por su clase y sus subclases.
## Otros modificadores
Los únicos modificadores relevantes a la cátedra son
* `static` el método perteneces a la clase, no a sus instancias, y puede llamarse sin una instancia, esto implica que no hay referencia `this`.
* `final` el método no puede ser sobreescrito en subclases.
* `abstract` el método no tiene implementación (no tiene código), sebe ser implementado por subclases, esto implica que la clase en sí es `abstract` también.
## Tipo de retorno
Cualquier tipo primitivo o clase, o `void` si no retorna nada.
## Parámetros
Separados por comas, cada parámetro debe indicar su tipo, como `int valor`. Pueden ingresarse múltiples parámetros con `...`, por ejemplo `String cadenas...` es considerado como un arreglo de `String` de cualquier longitud (incluso cero), esto es llamado `va-args`, argumentos de largo variable.
## Excepciones
Sólo deben declararse excepciones no evitables, del tipo `Exception`, como por ejemplo `IOException`. Las excepciones del tipo `RuntimeException` deben documentarse pero no declararse.

# Entrada y salida de datos
[`java.lang.System`](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/lang/System.html#out)

Para formatear cadenas similar a `C`, puede usarse las instrucciones
```java
// Para imprimir a consola
System.out.printf("cadena formateada %d", numero);
// Para almacenar cadenas con formato
String cadena = String.format("cadena formateada %d", numero)
```
Para imprimir a consola también está `System.out.println()` que agrega automáticamente un salto de línea, aunque no puede aplicarse formato de manera directa.

En **Intellij Idea**, los atajos son:
```java
System.out.println() // sout
System.out.printf()  // souf
```
El modificador de cadena `'%n'` se usa como salto de linea en cualquier plataforma.
## Métodos útiles de String
[`java.lang.String`](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/lang/String.html)
```java
//dada una cadena
String cadena = "sarasa abcdef";
int largo = cadena.length() // 13
char caracter = cadena.charAt(2) // 'r'
String subcadena1 = cadena.substring(4) // "sa abcdef" hasta el final
String subcadena2 = cadena.substring(4, 9) // "sa ab" inicio inclusivo, final exclusivo

String cadena2 = "SARASA AbCdEf"
boolean esIgual = cadena.equalsIgnoreCase(cadena2) // true
boolean empiezaCon = cadena.startsWith("sar") // true
boolean terminaCon = cadena.endsWith("cdef") // true
int indice = cadena.indexOf('a') // 1, busca el primero
int indice2 = cadena.indexOf('a',6) // 7, busca el primero despues del indice especificado. Devuelve -1 sino hay.
int indiceMax = cadena.lastIndexOf('a') // 7, busca el último
String minuscula = cadena.toLowercase // pasa a minusculas
String mayuscula = cadena.toUppercase // pasa a mayusculas
String cambio = cadena.replace("abc", "nuevo") // "sarasa nuevodef"
String[] palabras = cadena.split(" ") //{"sarasa", "abcdef"}, puede usarse cualquier regex como separador
char[] letras = cadena.toCharArray // devuelve un array de char
String otraCadena = String.join("-", letras) // "s-a-r-a-s-a- -a-b-c-d-e-f"
```
Importante a tener en cuenta: **los String son inmutables**, por lo que cualquier modificación a la cadena genera un **nuevo** String.

Y como son inmutables, concatenar en un lazo, crea una nueva cadena en por cada `+`. Para evitar esto, utilizar [`java.lang.StringBuilder`](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/lang/StringBuilder.html) que se explica mas abajo.

## Scanner

La clase `Scanner` se importa con `import java.util.Scanner`. Para usarla se instancia un `Scanner`, y se usan sus métodos `next` para leer datos.

Ejemplo:
```java
Scanner lector = new Scanner(System.in);
int valor = lector.nextInt;
float decimal = lector.nextDouble;
String cadena = lector.next();
String linea = lector.nextLine(); //Lee hasta un "Enter"
```
Si estamos leyendo de un archivo, podemos usar el archivo en lugar de `System.in`.
Importante a tener en cuenta que cerrar el `Scanner` con `Scanner.close()` cierra también el argumento, por lo que no deben cerrarse los `Scanner` que usan `System.in` hasta que hayamos terminado de leer datos.

# Sintaxis de Java

## Constantes
`final tipoDeDato NOMBRE_VARIABLE_SNAKE_CASE = valor`
Se utiliza la palabra clave `final`, que puede ser utilizada en argumentos de funciones y variables locales.
## Operadores
* Asignación `a = b`
* Posfijos `a++ b--`
* Lógicos `&& || !`
* Aritméticos `+ - * / %` Si la operación es entre enteros, `a / b` devuelve un entero.
## Control
### Condicionales
#### if
```java
if(condicion1){
  //se ejecuta si condicion es verdadera
} else if(condicion2){
  //se ejecuta si condicion1 es falsa y condicion 2 es verdadera
} else{
  //se ejecuta si ambas condiciones son falsas
}
```
#### switch
```java
int valor;
//codigo que asigna un valor
switch(valor){
  case 1:
    //si valor  es 1
  case 2:
    //si valor es 2. Como no se usó `break`, esto TAMBIEN se ejecuta despues del caso anterior
    break; //esto sale del switch
  case 3:
    //sól si valor es 3
    break;
  default:
    //si no se cumple ninguno de los casos
}
```
#### while
```java
while(condicion){
  //se ejecuta mientras condicion sea verdadera
  //puede usarse `break` para terminar el bucle
}
```
#### for tradicional
```java
for(inicializacion, condicion, incremento){
  //Inicializacion e incremento pueden tener multiples sentencias separadas por coma. Si hace falta ponermultiples condiciones se hace con &&
  //inicializacion: int i = 0, j = 10
  //condicion: i < j && j > 0
  //incremento: i++, j--
}
```
#### for mejorado (`for-each`)
```java
for(tipoElemento nombreElemento : coleccion){
  //Recorre todos los elementos de una colección que contiene tipoElemento
}
```

# Arreglos

Son secuencias *mutables* de elementos de un único tipo y de largo fijo.

Declaración:
```java
String[] arregloCadenas; //declaración vacía
int[] numeros = {1, 2, 3, 4}; //Declaración explícita
MiClase[] arreglo = new MiClase[5]; //Declaración con tamaño inicial
```

Las matrices son similares:
```java
float[][] miMatrizFloat;
int[][] miMatrizInt = {{1, 2}, {0, 5, -5, -3, 0}, {2, 5, 6, 8}};
String[][][] miMatrizString = new String[3][2][4];
```

# Excepciones

Una excepción es cualquier interrupción en el flujo normal del programa. Se pueden manejar de tres formas:
* **Atajar** - con un bloque `try-catch`
* **Delegar** - Si la excepción ocurre dentro de un método y no la atajamos, la excepción pasa al lugar que llamó al método.
* **Prevenir** - Si sabemos que puede haber una excepción podemos tomar medidas para impedirla, por ejemplo en una división podemos impedir que el divisor sea cero.

## Bloque try-catch
```java
try{
  //código que puede fallar. Si una línea de código arroja una excepción, las lineas siguientes NO se ejecutan.
} catch (TipoExcepcion nombreExcepcion){ //Generalmente se la llama `e`
  //opcional, se ejecuta si el bloque anterior arroja uina excepción
  //Aquí se maneja lo que ocurre luego de la excepción, que puede ser un mensaje de error, una llamada a función, o incluso se puede arrojar otra excepción distinta para delegarla.
} finally {
  //opcional, se ejecuta SIEMPRE después del try. Si no se usó un `catch`, este bloque debe existir.
  //Puede ser util para limpieza, por ejemplo si al leer un archivo salta un error, este bloque se puede usar para cerrar el archivo antes de salir del bloque.
}
```

Para arrojar excepciones, se usa la instrucción `throw` seguido de una instancia de Excepción, que se crea con la palabra reservada `new`. (para mas detalles de esto, ver la parte de Orientación a Objetos)

```java
throw new NombreExcepción(); //sin mensaje
throw new NombreExcepción("mensaje de error"); //con mensaje
throw new NombreExcepcion(Throwable causa);
throw new NombreExcepcion("mensaje", Throwable causa);
```
Suele  arrojarse una excepción cuando se detecta un posible error, o en un bloque `try-catch`. Los `Throwable` son cualquier tipo de error o excepción, aunque típicamente se usa la excepción atajada por el `catch`, por ejemplo:
```java
try{
  String linea miScanner.nextLine();
} catch (FileNotFoundException e){
  throw new LecturaException("No se pudo encontrar el archivo", e);
} catch (IOException e){
  throw new LecturaException("Error de lectura/escritura al leer el archivo", e);
} finally {
  if(miScanner != null){
    miScanner.close();
  }
}
```
Se usa el `Throwable` para que quien sea que ataje esa nueva excepción sepa cual fue el error original. No es necesario en la mayoría de los casos.

## Tipos de excepciones

* **RuntimeException** - Son errores evitables de programación. Ejemplos: `NullPointerException`, `ArithmeticException` (como dividir por cero). Son evitables con código que prevenga este tipo de excepción. Deben documentarse pero no se declaran en la función.
* **Exception** - Ocurren en la ejecución del programa, suelen ser independientes del código. Por ejemplo, que un archivo necesario no exista. algunos ejemplos son `IOException` (de lectura/escritura), `ClassNotFoundException`. En estas excepciones usualmente la IDE muestra un mensaje de error exigiendo que las manejemos o que las declaremos en el método, como
```java
int miMetodo() throws IOException
```
* **Error** - Problemas graves típicamente no manejables. No se manejan ni declaran. Ejemplos: `OutOfMemoryError`, `StackOverflowError`.

**Un detalle sobre atajar excepciones**, En el manejo de excepciones en Java, es fundamental entender qué tipo de excepción estás capturando. Al utilizar un bloque `catch` para `RuntimeException`, `Exception` o `Error`, estás abarcando un espectro muy amplio. En esencia, al capturar cualquiera de estas clases base, estarás "atrapando" todas las situaciones excepcionales que se deriven de ellas.

Es importante atajar solo lo que estamos preparados, porque es posible que suprimamos una excepción que no tenia nada que ver con lo que estábamos programando.

# Funciones y efectos secundarios

En Java, todos los argumentos pasan por valor, lo que significa que:
* **Argumentos primitivos** (`int`, `float`, etc.) - Se pasa a la función una copia del valor actual. Cualquier cambio hecho al valor dentro de la función **no afecta al original**.
* **Referencias a objetos** (incluyendo arreglos) - Se pasa una copia **de la referencia** al objeto. El parámetro **apunta a la misma dirección de memoria** que el objeto original. Cualquier cambio hecho al parámetro **será reflejado en el objeto original**. Para evitar esto, podemos reasignar el parámetro a un **nuevo objeto** dentro de la función. En este sentido, es parecido a pasar punteros en C, con la diferencia que no se puede hacer aritmética de punteros.

Si queremos pasar un arreglo pero no queremos modificar el original podemos pasar una copia (o podemos copiarlo dentro de la función) con la instrucción
```java
//ejemplo con arreglo de `int`
int[] arregloCopia = Arrays.copyof(arregloOriginal, largo);
```
Esta instrucción permite achicar o agrandar el arreglo cambiando el largo. Si el largo nuevo es menor que el original, lo recorta. Si es mayor, agrega elementos vacíos (ceros si son valores numéricos, `null` si son objetos) hasta completar el nuevo largo. El largo del arreglo original se obtiene con `arregloOriginal.length`.

> Ojo que en arreglos, `length` es un atributo y no necesita paréntesis, mientras que en `String`, es un método y lleva paréntesis, como en `cadena.length()`

# Strings y StringBuilder

Si queremos hacer cambios a un `String`, al ser este inmutable, se crea un nuevo `String` con la nueva cadena de caracteres. Esto puede ser costoso en memoria. Para evitar eso, existe la clase `StringBuilder`. Esta clase puede instanciarse de la siguiente manera:
```java
//Crea un SB vacío
StringBuilder sb = new StringBuilder();

//Crea un SB con una capacidad inicial
StringBuilder sb = new StringBuilder(50);

//Crea un SB con una cadena inicial
StringBuilder sb = new StringBuilder("sarasa");
```

Estos son algunos de sus métodos:
```java
//Agregar "algo", que puede ser un String, Object, char[], número, etcétera.
sb.append("hola");
//Puede encadenarse
sb.append("hola").append(',').append(5); //agrega "hola,5"

//Insertar algo en cierta posición
sb.insert(2, "hola"); //

//Eliminar caracteres entre las posiciones "a" y "b-1"
sb.delete(2, 6); // Elimina del 2 al 5.

//Eliminar un caracter en la posición "a"
sb.deleteAt(3);

//Reemplazar caracteres entre las posiciones "a" y "b-1" por una cadena
sb.replace(2, 14, "hola"); // Elimina del 2 al 13 y lo reemplaza por "hola"

//Devuelve el caracter en la posición indicada
char letra = sb.charAt(5);

//Reemplaza el caracter en la posición indicada por uno nuevo
sb.setCharAt(3, '\t');

//Devuelve el largo
int largo = sb.length();

//Devuelve la capacidad (la cantidad total de caracteres que puede contener cin tener que cambiar de tamaño)
int capacidad = sb.capacity();

//Devuelve un String con los caracteres que contiene
String cadena = sb.toString();
```

# Archivos

"Nueva forma", aparece desde **Java 8**.

## java.nio.file.Path
Es una **interfaz** que representa la ruta de una carpeta o archivo. Se usa para manipular nombres de archivo o carpetas, y para interactuar con la clase `java.nio.file.Files` para manipular directamente archivos (copiar, borrar, mover, etc.)
Al igual que `String`, es inmutable, si el `Path` se cambia, se crea uno nuevo.
No depende del sistema operativo, donde Windows usa `\` como separador, UNIX usa `/`, etc.

Métodos útiles:
```java
//Devuelve el nombre
.getFileName()

//Devuelve la carpeta contenedora
.getParent()

//Devuelve la carpeta raiz
.getRoot()

//Verifica si la ruta es absoluta
isAbsolute()

//Convierte la ruta en absoluta (por ejemplo "archivo.java" pasaría a ser "C:\Data\Prog\archivo.java")
.toAbsolutePath()

//Elimina redundancias como "." y ".." (carpeta actual y carpeta padre). Util cuando la ruta fue construida con código.
normalize()
```

Las rutas relativas usan el directorio actual como base. Por ejemplo, el archivo "./archivo.java" es un archivo en la misma carpeta, mientras que "../otraCarpeta/otroArchivo.txt" es un archivo en una carpeta paralela:

> Carpeta Padre ( .. )
>    │             │
>    │             │
>    │             └──────── otraCarpeta
>    │                                       │
> **Carpeta Actual** ( . )              └─  otroArchivo.txt
>    │
>    └─ archivo.java
## java.nio.file.Paths
A pesar del nombre similar, esta clase contiene sólo un método, específicamente un método estático utilitario con una sobrecarga, que se usa para crear objetos del tipo `Path`.
```java
Path miRuta = Paths.get(String primero, String... otros)
```
Este método convierte las cadenas especificadas en un objeto `Path`. Si no se puede crear la ruta (por ejemplo, por contener caracteres ilegales), arroja un `InvalidPathException`, el cual hay que manejar.

Ejemplos:
```java
Path carpetaBase = Paths.get("C:\Data\Prog"); //ruta absoluta
Path miArchivo = Paths.get("archivo.java");  //ruta relativa
```
Al crear un archivo con ruta relativa, Java lo ubica en la carpeta de trabajo del programa. Esta carpeta cambia según de donde se ejecuta el programa.
En **Intellij IDEA**, esta carpeta es el directorio raíz (donde por ejemplo está el Readme.md de los trabajos prácticos.
Desde consola, esta carpeta es la carpeta desde la que se ejecutó el programa, **NO ES** la carpeta donde está el programa. Por ejemplo, si hacemos
```
C:\Data> java c:\Data\Prog\archivo.java
```
el directorio de trabajo será `C:\Data`, desde donde ejecutamos el programa.

## java.nio.file.Files
Esta clase tiene métodos estáticos para operar con archivos o carpetas.

Ejemplos:
```java
// Verifica si existe el archivo, devuelve boolean
Files.exists(Path ruta);

//Devuelve un `DirectoryStream` con el contenido de una carpeta
Files.newDirectoryStream(Path ruta)

//Escribe una cadena en un archivo
Files.writeString(Path ruta, CharSequence cadena, OpenOption... opciones)
```
`DirectoryStream` contiene los archivos de ese directorio y pueden accederse con un for:
```java
DirectoryStream<Path> carpeta = Files.newDirectoryStream(miRuta);
for(Path ruta : carpeta){
//hacer cosas
}
```
`DirectoryStream` también puede usarse con filtros. Por ejemplo
```java
DirectoryStream<Path> carpeta = Files.newDirectoryStream(miRuta, "*.txt");
```
devuelve solo los archivos terminados en ".txt"
Otro tipo de filtro es este otro:
```java
DirectoryStream.Filter<Path> miFiltro = new DirectoryStream.Filter<Path>(){
  public boolean accept(Path file) throws IOException{
    return(Files.size(file) > 8192L);
  }
}
Path ruta = ...
try{
  DirectoryStream<Path> carpeta = Files.newDirectoryStream(ruta, miFiltro);
}
```
El filtro se arma instanciando un `DirectoryStream.Filter<Path>` y declarando internamente el código de su método `accept`, que determina que tipo de archivo se abrirá. Para más información, ver los métodos de filtrado en [Java Docs](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/nio/file/Files.html).

## Formatter
Esta clase recibe un `stream` como `StringBuilder`, un `File` o un `String` con un nombre de archivo. Opcionalmente también puede recibir un `Charset` y un `Locale` para usar caracteres no estándar. 

Ejemplo:
```java
int miEntero = 123;
float miFloat = 6.74f;
Formatter salida = new Formatter(miRuta.toFile());
salida.format("numeros: %d %f", miEntero, miFloat);
salida.close();
```
Este codigo escribe "numeros: 123 6.74" al archivo en `miRuta`, sobreescribiendolo si ya existiera. 

# Tests
Los tests en Java usan la librería `org.junit.jupiter.api`. Lo más usado es:
```java
//Declara que el siguiente método es un test
@Test
//Le da nombre a un test
@DisplayName("nombre")
//Sirve para declarar un método que se ejecutará antes de cada uno de los tests
@BeforeEach
//Sirve para declarar un método que se ejecutará después de cada uno de los tests
@AfterEach
//Sirve para declarar un método que se ejecutará antes de los tests
@BeforeAll
//Sirve para declarar un método que se ejecutará después de los tests
@AfterAll
//Métodos para verificar si las pruebas son correctas
Assertions.assertEquals(esperado, obtenido, mensaje);
Assertions.assertTrue(esperado, mensaje);
Assertions.assertFalse(esperado, mensaje);
```
`@BeforeEach` y `@AfterEach` son particularmente útiles para pruebas con archivos, para abrir/crear los archivos a probar antes de cada test y luego para cerrarlos/eliminarlos después de cada test.

# Programación orientada a objetos

Es una forma de asociar código con datos *bajo control*.
Un **objeto** está compuesto de sus datos, o **estado**, y su código, o **comportamiento**. Esencialmente, un **objeto** es una **instancia** de una **clase**, y existe en memoria con valores específicos para sus datos (su **estado**). Son creados en el *heap*.
Una **clase** es la plantilla para crear el objeto. Define la estructura y **comportamiento** que todos los **objetos** creados a partir de dicha clase tendrán. Tiene **atributos**, que declaran tipos de datos que los **objetos** van a contener. También define el **comportamiento** a través de **métodos**.

<u>Palabras clave:</u>

1) **Método** - El código de una clase. El proceso de llamarlo es la **invocación**. Los métodos **estáticos** de una clase están desacoplados de sus instancias.
2) **Interfaz** - Conjunto de métodos de una clase
3) **Atributo** - La información definida en una clase. Recibe valores específicos al ser instaciada en un objeto, definiendo su estado.
4) **Estado** - Los valores específicos de los atributos en una instancia de objeto.
5) **Construcción** - El acto de tomar una clase y darle valores específicos a sus atributos para generar o instanciar un objeto.
6) **Destrucción** - El acto de tomar un objeto y destruirlo.
7) **Identidad** - La combinación específica de estado que hace que un objeto sea único.
8) **Generalización** - Pasar de algo particular a general. En diagramas, es "subir".
9) **Especialización** - Pasar de algo general a algo más específico. En diagramas, es "bajar".
10) **Herencia** - Es la relación entre clases del mismo *tipo*. La clase que hereda (llamada subclase) especializa a la clase padre. La clase padre (o superclase) generaliza a la clase hija. El comportamiento y atributos pueden ser compartidos por sus subclases. Una clase puede tener muchas subclases, pero solo una superclase. Una subclase se dice que **extiende** a la superclase.
11) **Abstracción** - Es el acto de identificar las partes esenciales de un objeto y representarlas de modo simplificado. 
12) **Composición** - Es una asociación *fuerte*, donde una clase contiene una referencia a otra(s). En composición, el objeto referenciado no puede existir sin su contenedor, como un motor en un auto. El auto absolutamente necesita un motor para andar, y si el auto desaparece, su motor desaparece con él. Usualmente se implementa creando el objeto referenciado al construir el contenedor, o mediante un método del contenedor.
13) **Agregación** - Es una asociación, esta vez  *débil*, donde una clase contiene una referencia a otra(s). En agregación, el objeto referenciado puede existir independientemente al contenedor, como un colectivo y sus pasajeros. Un colectivo tiene pasajeros, pero los pasajeros siguen existiendo si el colectivo deja de existir. Usualmente el objeto referenciado ya existe de manera independiente, y se le pasa como argumento al contenedor en su constructor o en algún método.
14) **Encapsulamiento** - Es el acto de impedir el acceso directo a los atributos.
15) **`this`** - Palabra clave que referencia al objeto en sí.
16) **`super`** - Palabra clave que referencia al objeto padre.
17) **Sobrecarga** - La existencia de varios métodos con el mismo nombre pero con diferentes tipos de argumentos.

## Sintaxis de POO en Java

```java
//el contenedor donde viven las clases
package ar.unrn

//declaración de la clase
public class Contacto{

  //atributos
  private int numero;
  public Contacto propietario; // Los atributos pueden ser otras clases

  //constructor, instrucciones para crear una instancia de la clase
  public Contacto(int n){
    numero = n;
  }

  //puede haber más de uno, con diferentes parámetros
  public Contacto(){
    numero = 0;
  }

  //métodos
  private int incrementar(){ //pueden ser privados
    numero++;
  }

  public int sumarNumero(int otronumero){ //pueden ser públicos
    return numero + otroNumero;
  }
}
```

Para instanciar una clase en un objeto, se usa la instrucción `new`
```java
Contacto pepito = new Contacto(7);
```

### Modificadores de acceso

* **`public`** - Accesible por todas las demás clases. Para mantener encapsulamiento no debe usarse en atributos.
* **`protected`** - Accesible por la misma clase y sus subclases.
* **`private`** - Accesible sólo por objetos dentro de la misma clase.
* _ninguno_ - Accesible por todas las clases del mismo paquete.

## Sobrecarga de métodos

La sobrecarga permite la existencia de varios métodos con el mismo nombre, pero con distintos parámetros. Un ejemplo de sobrecarga son los dos constructores en el código de ejemplo de arriba. Uno de los constructores no tiene parámetros, mientras que el otro tiene un parámetro de tipo `int`.

# Clase `Object`

La clase `Object` es la clase padre superior a todas las clases, esto es, todas las clases en Java son subclases de `Object` o de alguna de sus subclases.
Tiene tres métodos que todos sus descendientes heredan.
* **`equals(Object o)`**  - Devuelve `true` si el objeto es igual al objeto `o`.
* **`hashCode()`** - Devuelve un número "único" que representa al objeto.
* **`toString()`** - Devuelve una representación en forma de `String` del objeto. Este método se llama automáticamente al concatenar con una cadena.

## `@Override`
Estos métodos (y cualquier método heredado que no sea `final`) pueden ser reescritos por sus subclases. Se marca dicha reescritura con la etiqueta `@Override`, por ejemplo, en la clase `Contacto` de arriba podríamos agregar este método:
```java
@Override
String toString(){
  return String.format("Número %d", numero);
}
```
El "Override" es distinto a la sobrecarga, porque la sobrecarga cambia los argumentos, mientras que el "Override" usa los mismos argumentos que el método original.

La anotacion `@Override` se utiliza para indicar que estamos sobreescribiendo un método de la super-clase, esta existe para forzar a que el método sea uno de la clase superior, en lugar de una sobrecarga. Tengan en cuenta que una clase puede sobrecargar sin sobreescribir, viceversa y ambas.

## Override de equals y hashCode
Estos métodos pueden ser reescritos, pero deben ser consistentes entre sí.
El método `equals` es lo que le da identidad a los objetos, cuando queremos ver si dos objetos son "iguales", usamos este método. Sin embargo, la igualdad puede depender de varios factores. En algunos casos, queremos una igualdad estricta, esto quiere decir que dos objetos son iguales si **todo** su estado coincide, pero en otros casos con que sólo algunas partes sean iguales nos alcanza. En estos casos, haremos un "Override" de tanto `equals` como de `hashCode`, que están relacionados.

Un `equals` debe cumplir con las tres reglas matemáticas de la igualdad, es decir:
1.  **Reflexividad**
    Para cualquier referencia no nula `A`, la expresión $A == A$ debe ser siempre `true`. Esto significa que un objeto siempre debe ser igual a sí mismo.

2.  **Simetría**
    Para cualquier referencia no nula `A` y `B`, si la expresión $A == B$ es `true`, entonces la expresión $B == A$ también debe ser `true`. En otras palabras, si `A` es igual a `B`, entonces `B` también debe ser igual a `A`.

3.  **Transitividad**
    Para cualquier referencia no nula `A`, `B` y `C`, si la expresión $A == B$ es `true` y la expresión $B == C$ también es `true`, entonces la expresión $A == C$ debe ser `true`. Esto implica que si `A` es igual a `B` y `B` es igual a `C`, `A` también debe ser igual a `C`.

El **Intellij IDEA** ofrece un "wizard" muy robusto para reescribir `equals` y `hashCode`, pero veamos ejemplos con la siguiente clase:
```java
public class Persona{
  private String nombre;
  private String apellido;
  private String dni;
  private int id;
}
```
Una filosofía de implementación de `equals` es decir que dos personas son iguales si todos esos atributos son iguales, en cuyo caso no hace falta reescribir el método. Pero otra filosofía podría ser que dos personas son iguales si su `dni` y `id` son los mismos, independientemente del nombre y apellido que tengan asociados. En cuyo caso, un `equals` y `hashCode` adecuados son:
```java
//Este método ignora las reglas de estilo de un solo return por método
@Override
public boolean equals(Object o){
  
  //primero vemos si los objetos son iguales de entrada
  if (this == o){
    return true;
  }
  
  //después vemos si el objeto argumento es nulo, encuyo caso no puede ser igual
  if (o == null){
    return false;
  }
  
  //luego vemos si son de la misma clase
  if (getClass != o.getClass()){
    return false;
  }
  
  //Si llegamos hasta acá,los objetos son de la misma clase y no son nulos,
  //así que podemos castear el objeto a la clase adecuada sin temor a que falle.
  Persona otro = (Persona) o;
  
  //Finalmente verificamos si los atributos que nos interesan son iguales.
  //En nuestro ejemplo, verificamos id y dni
  if(!dni.equals(otro.dni)){ //usamos el equals de la clase String!!
    return false;
  }
  if(id != otro.id) //Aquí usamos la igualdad numérica.
  //Un detalle interesante a tener en cuenta es que en esta verificación estamos accediendo a los atributos id y dni del otro objeto, aunque sean `private`. Esto es porque estamos aún dentro de la clase "plantilla" de ambos objetos.

  //Finalmente, pasadas las verificaciones, podemos decir que los objetos son iguales
  return true
}

//El método hashCode debe usar los mismos atributos que equals. Así como en equals pudimos usar el metodo equals de String, aquí podemos usar el método estático `Objects.hash(Object... valores)` para generar nuestro hashCode
@Override
public int hashCode(){
  return Objects.hash(dni, id);
}
```

De todas formas, para `hashCode` prefieran utililizar las funciones de libreria como (`java.util.Objects#hash(Object...)`[https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/Objects.html#hash(java.lang.Object...)]