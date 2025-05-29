# 📘 Documentación y Patrones de Diseño

## 🧾 Documentación

### 📌 Clases

Las clases deben incluir su documentación explicando:

- Qué **representan**
- Qué **promocionan**
- Cómo se **implementan**

### 📌 Genéricos

Los `@param` genéricos deben documentarse especificando que representan un **tipo genérico** que será utilizado por la clase o método.

### 📌 Atributos

Los atributos se pueden documentar de dos formas:

- `///` Documentación **de línea**
- `/** ... */` Documentación **de bloque**, pero **breve** (una línea es suficiente).

Ejemplo:

```java
/// Cantidad de elementos
private int cantidad;

/**
 * Nombre del usuario
 */
private String nombre;
```

### 📌 Referencias cruzadas
Para hacer referencias a otros métodos o elementos del código:
```java
/**
 * @see #verificar(int)
 */
```
# Patrones de Diseño

## ¿Qué son los patrones de diseño?

Los **patrones de diseño** son soluciones reutilizables a problemas comunes en el desarrollo de software. Representan experiencias previas condensadas en una forma reutilizable y comprensible.

- **Vocabulario común**: Permiten describir soluciones de diseño complejas con una sola palabra, lo que facilita la documentación y la comunicación.
- **Experiencia**: Son soluciones probadas a situaciones comunes en el diseño de programas.
- **Flexibles**: No son reglas estrictas, sino guías que se pueden adaptar según las necesidades del proyecto.
- **Universales**: No son exclusivos de la programación orientada a objetos.

## ¿Cuándo utilizarlos?

- Cuando se identifica un problema común que puede beneficiarse de una solución conocida y estandarizada.
- Al diseñar estructuras que puedan crecer, cambiar o adaptarse fácilmente.

## ¿Cuándo no utilizarlos?

- Si no hay un problema que lo amerite.
- Cuando su inclusión complejiza innecesariamente el diseño.

---

## Tipos de Patrones

- **Creacionales**: Se centran en cómo se crean los objetos.
- **Estructurales**: Se centran en cómo se organizan las clases y objetos.
- **De Comportamiento**: Se centran en cómo interactúan los objetos y cómo se distribuyen las responsabilidades.

---

## Patrones Creacionales

- **Factory Method**: Proporciona una interfaz para crear objetos en una superclase, mientras permite a las subclases alterar el tipo de objetos que se crearán. Útil cuando no se puede anticipar el tipo de objeto.
- **Abstract Factory**: Permite producir familias de objetos relacionados sin especificar sus clases concretas. Todo lo creado es compatible entre sí.
- **Builder**: Permite crear un objeto complejo paso a paso. Separa la construcción del objeto de su representación.
- **Prototype**: Crea nuevos objetos mediante la clonación de un objeto existente. Más simple que algunos otros patrones creacionales.
- **Singleton**: Garantiza que solo exista una instancia de una clase. Similar a una variable global, pero puede dificultar el testing.

---

## Patrones Estructurales

- **Adapter**: Convierte la interfaz de una clase en otra que el cliente espera.
- **Bridge**: Desacopla una abstracción de su implementación para que ambas puedan evolucionar de forma independiente.
- **Composite**: Compone objetos en estructuras de árbol para representar jerarquías parte-todo.
- **Decorator**: Añade responsabilidades adicionales a un objeto de forma dinámica y transparente envolviéndolo.
- **Facade**: Proporciona una interfaz unificada y simplificada a un conjunto de interfaces en un subsistema complejo.
- **Flyweight**: Gestiona datos compartidos para soportar grandes cantidades de objetos pequeños de manera eficiente.
- **Proxy**: Proporciona un intermediario para otro objeto a fin de controlar el acceso a este. Tipos comunes:
  - **Virtual**: Crea el objeto real solo cuando es necesario (carga perezosa).
  - **Protección**: Controla el acceso en base a permisos del cliente.
  - **Remoto**: Encapsula la comunicación con un objeto en otro proceso o servidor.
  - **Registro**: Registra llamadas y argumentos antes de delegar al objeto real.
  - **Caché**: Almacena resultados para evitar operaciones repetidas.

---

## Patrones de Comportamiento

- **Chain of Responsibility**: Pasa una solicitud a lo largo de una cadena de objetos hasta que uno la procesa.
- **Command**: Encapsula una solicitud como un objeto independiente que contiene toda la información necesaria. Permite parametrizar, almacenar y deshacer operaciones.
- **Iterator**: Permite recorrer una colección de elementos sin exponer su estructura interna.
- **Mediator**: Define un objeto que encapsula cómo interactúan un conjunto de objetos, promoviendo un bajo acoplamiento.
- **Memento**: Permite guardar y restaurar el estado interno de un objeto sin violar su encapsulamiento.
- **Observer**: Implementa un sistema de notificaciones. Cuando un objeto cambia, notifica automáticamente a sus observadores.
- **State**: Permite que un objeto modifique su comportamiento cuando su estado cambia. El objeto parece cambiar de clase.
- **Strategy**: Define una familia de algoritmos, los encapsula y los hace intercambiables sin modificar el cliente.
- **Template Method**: Define el esqueleto de un algoritmo y deja que las subclases implementen ciertos pasos.
- **Visitor**: Permite separar un algoritmo de la estructura de objetos sobre la que opera. Ideal para recorrer elementos y aplicar operaciones específicas a cada uno.



