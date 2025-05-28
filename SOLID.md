# Notas sobre Calidad de Código y Principios SOLID

## ¿Qué es la Deuda Técnica?

La deuda técnica representa decisiones de diseño o implementación que son más fáciles a corto plazo, pero que pueden traer problemas a largo plazo. Identificarla y gestionarla reduce el costo futuro de mantenimiento.

- **Identificación y seguimiento**: Detectar dónde y cómo impacta.
- **Priorización**: No toda deuda debe pagarse primero.
- **Prevención**: Adoptar buenas prácticas desde el inicio.
- **Refactorización**: Pagar la deuda técnica mediante reestructuración del código sin cambiar su funcionalidad externa.

---

## Características de un Mal Diseño

### Acoplamiento
- Cuánto dependemos del **cuerpo interno** de otra clase u objeto.
- Un sistema acoplado es más frágil y difícil de mantener o extender.

### Rigidez
- Si cambiar una funcionalidad obliga a modificar muchas partes del sistema.
> _¿Cuánto cambia el sistema cuando queremos modificar algo?_

### Fragilidad
- Cambios pequeños generan efectos colaterales inesperados en otras partes del sistema.
> _¿Cuánto se rompe el sistema cuando tocamos algo?_

### Inseparabilidad
- Una clase hace demasiadas cosas. No se puede reutilizar una parte sin llevarse también lo que no se necesita.

### Viscosidad
- Es más fácil "parchear" una clase existente que hacer las cosas bien creando nuevas clases o módulos, lo cual lleva a un código difícil de mantener.

---

## Principios SOLID

### SRP – Single Responsibility Principle
- Una clase debe tener una **única razón para cambiar**.
- Mejora la cohesión, facilita pruebas y mantenimiento.

### OCP – Open/Closed Principle
- Las entidades (clases, módulos, funciones) deben estar **abiertas a la extensión**, pero **cerradas a la modificación**.
- Usar herencia o composición para extender funcionalidades sin romper lo existente.

### LSP – Liskov Substitution Principle
- Las subclases deben poder usarse en lugar de sus clases base **sin alterar el comportamiento del programa**.
- Requiere definir **contratos claros** y respetar los **invariantes** de la clase base.

### ISP – Interface Segregation Principle
- Es mejor tener muchas **interfaces pequeñas y específicas**, que una grande que obligue a implementar métodos innecesarios.
- Las clases no deberían estar forzadas a depender de métodos que no usan.

### DIP – Dependency Inversion Principle
- Los módulos de alto nivel no deben depender de módulos de bajo nivel.  
  Ambos deben depender de **abstracciones (interfaces o clases abstractas)**.
- Las abstracciones **no deben depender de detalles**, sino que **los detalles deben depender de abstracciones**.

---

## Buenas Prácticas

- **Separar responsabilidades** en diferentes clases.
- Utilizar **interfaces o clases abstractas** para definir comportamientos.
- **Evitar `instanceof` o `pattern matching`** en todos lados: rompe la flexibilidad y el principio LSP.
- Establecer y respetar la **idea general de la clase base**: contratos, efectos secundarios e invariantes.
- Si es difícil ponerle un **nombre claro a una interfaz**, probablemente no sea necesaria.
- Clases con única responsabilidad son **más fáciles de testear** de forma unitaria.
- Diseñar para la extensión en lugar de la modificación.

---

## Resultado

- Diseño más limpio, mantenible y escalable.
- Menor acoplamiento y mayor cohesión.
- Mayor facilidad para testear, extender y refactorizar.
- **Menos deuda técnica y mayor calidad de software.**
