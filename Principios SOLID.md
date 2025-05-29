# Deuda técnica

La deuda técnica es una metáfora que describe el trabajo de desarrollo adicional que se genera cuando el código que se implementa es subóptimo, defectuoso o incompleto. Sus **causas** suelen ser la presión de tiempo, la falta de conocimiento o experiencia, cambios frecuentes en los requisitos, o la priorización de la funcionalidad sobre la calidad del código.

Existen dos tipos principales:

- **Deliberada:** Se asume conscientemente, generalmente para cumplir con plazos ajustados o para entregar una funcionalidad mínima viable rápidamente, con la intención de refactorizarla más tarde.
- **Accidental:** Surge de forma no intencionada debido a malas decisiones de diseño, falta de experiencia, o desconocimiento de las mejores prácticas.

Para gestionarla, estas son las **estrategias**:

1. **Identificación / Seguimiento:** Reconocer dónde existe la deuda técnica en el código. Esto puede hacerse a través de revisiones de código, herramientas de análisis estático o "olores" de código.
2. **Priorización:** Decidir qué deuda técnica abordar primero basándose en su impacto, riesgo y el costo de no resolverla.
3. **Prevención:** Implementar prácticas de desarrollo que eviten la creación de nueva deuda técnica, como seguir principios de diseño, realizar revisiones de código rigurosas y fomentar la calidad.
4. **Refactorización:** Modificar el código existente para mejorar su diseño y estructura sin alterar su comportamiento externo. Es el proceso de pagar la deuda técnica acumulada.

El concepto que nos permite detectar deuda es el de "**olor a código**" (code smell), que son ciertos patrones en el código que, si bien no son errores, pueden indicar problemas de diseño subyacentes o debilidades que conducen a la deuda técnica.

Parámetros a tener en cuenta para analizar la deuda:

1. **Acoplamiento:** Mide cuánto se depende de la estructura interna de otro objeto o módulo. Un alto acoplamiento significa que un cambio en un módulo puede requerir cambios en muchos otros módulos, aumentando la complejidad y el riesgo de errores. Es deseable un bajo acoplamiento.
2. **Rigidez:** La medida de cuánto impacta al resto del código una modificación. Un sistema rígido es aquel donde un pequeño cambio en un lugar provoca una cascada de cambios en otras partes, haciendo el mantenimiento costoso.
3. **Fragilidad:** La medida de cuánto cuesta arreglar el código ante un error al modificar. Un sistema frágil es propenso a romperse en muchos lugares cuando se realiza un cambio en un solo lugar, lo que lleva a un ciclo constante de corrección de errores.
4. **Inseparabilidad (Cohesión):** Es lo opuesto a la cohesión; se refiere a la cantidad de responsabilidades de las clases. Idealmente, las clases deberían tener una única responsabilidad bien definida (alta cohesión). La inseparabilidad alta (baja cohesión) indica que una clase hace demasiadas cosas, lo que dificulta su comprensión, prueba y modificación.
5. **Viscosidad:** Describe qué tan fácil o difícil es hacer lo "correcto" en el código. Un sistema viscoso hace que sea más fácil "hackear" una solución rápida y sucia en una clase existente (aumentando su complejidad), en lugar de crear una nueva clase o estructura adecuada, lo que perpetúa la deuda técnica.

---

# Principios SOLID

Según "Clean Code" de Robert C. Martin (publicado alrededor de 2008), los principios SOLID son cinco principios de diseño de software que facilitan el desarrollo de sistemas de software comprensibles, flexibles y mantenibles. Son una guía para escribir código que sea fácil de extender y modificar a lo largo del tiempo.

---

## **SRP, Single Responsibility Principle (Principio de Responsabilidad Única)**

Una clase debe tener una y solo una razón para cambiar, lo que significa que debe tener una única responsabilidad bien definida. Esto no implica que una clase deba tener un solo método, sino que todas las funcionalidades dentro de esa clase deben estar relacionadas con su única responsabilidad. Separar las responsabilidades ayuda a evitar que los cambios en una parte del sistema afecten a otras partes no relacionadas, y facilita la comprensión y el mantenimiento del código.

Por ejemplo, hagamos una clase `Reporte` que genera un informe, lo imprime y lo guarda en una base de datos.
```java
class Reporte {
    String generarContenido() { /* ... */ }
    void imprimir() { /* ... */ }
    void guardarEnBaseDeDatos() { /* ... */ }
}
```

Esta clase viola el SRP porque tiene tres responsabilidades distintas: generación de contenido, impresión y guardado. Si cambia la forma de imprimir, o la forma de guardar, o el formato del reporte, esta única clase deberá ser modificada.

Según el SRP, sería mejor separar estas responsabilidades en clases distintas:
```java
class GeneradorReporte {
    String generarContenido() { /* ... */ }
}

class ImpresorReporte {
    void imprimir(String contenido) { /* ... */ }
}

class GuardadorReporte {
    void guardarEnBaseDeDatos(String contenido) { /* ... */ }
}
```

De esta manera, si la lógica de impresión cambia, solo `ImpresorReporte` se ve afectado. Si la forma de guardar cambia, solo `GuardadorReporte` necesita ser modificada.

---

## **OCP, Open/Closed Principle (Principio Abierto/Cerrado)**

Las clases y métodos deben estar **abiertos a extensión**, pero **cerrados a modificación**. Esto significa que el comportamiento puede extenderse sin modificar su código fuente existente. La clave para aplicar el OCP es el uso de abstracciones (interfaces o clases abstractas) y polimorfismo. Al definir contratos (interfaces) y permitir que las implementaciones concretas varíen, se puede añadir nueva funcionalidad sin alterar el código que ya funciona y ha sido probado.

Por ejemplo, hagamos un sistema de cálculo de áreas de diferentes formas geométricas. Una primera aproximación podría ser:
```java
class CalculadoraArea {
    double calcularArea(Object forma) {
        if (forma instanceof Circulo) {
            Circulo circulo = (Circulo) forma;
            return Math.PI * circulo.radio * circulo.radio;
        } else if (forma instanceof Rectangulo) {
            Rectangulo rectangulo = (Rectangulo) forma;
            return rectangulo.ancho * rectangulo.alto;
        }
        return 0; // O lanzar una excepción
    }
}
```

Esta clase viola el OCP. Si queremos añadir una nueva forma (por ejemplo `Triangulo`), tendríamos que modificar la clase `CalculadoraArea` para añadir otra condición `if-else`, lo que significa que está cerrada a extensión y abierta a modificación.

Para aplicar el OCP, podemos usar una interfaz `Forma` y permitir que cada forma calcule su propia área:
```java
interface Forma {
    double calcularArea();
}

class Circulo implements Forma {
    double radio;
    // Constructor, etc.
    @Override
    public double calcularArea() {
        return Math.PI * radio * radio;
    }
}

class Rectangulo implements Forma {
    double ancho;
    double alto;
    // Constructor, etc.
    @Override
    public double calcularArea() {
        return ancho * alto;
    }
}

class CalculadoraArea {
    double calcularAreaTotal(Forma[] formas) {
        double areaTotal = 0;
        for (Forma forma : formas) {
            areaTotal += forma.calcularArea(); // Polimorfismo
        }
        return areaTotal;
    }
}
```

Ahora, si necesitamos añadir un `Triangulo`, simplemente creamos una nueva clase `Triangulo` que implemente la interfaz `Forma` y su método `calcularArea()`. La clase `CalculadoraArea` no necesita ser modificada, ya que está abierta a extensión (podemos añadir nuevas formas) pero cerrada a modificación.

---

## **LSP, Liskov Substitution Principle (Principio de Sustitución de Liskov)**

Los objetos de una clase base deben poder ser reemplazados por instancias de sus clases derivadas sin alterar el comportamiento correcto del programa. Esto significa que una subclase debe poder usarse en cualquier lugar donde se espere su clase base sin que el programa se comporte de manera inesperada. Las subclases deben adherirse a los "contratos" definidos por su clase base, lo que incluye:

- Las precondiciones no pueden reforzarse en una subclase.
- Las postcondiciones no pueden debilitarse en una subclase.
- Las invariantes de la clase base deben mantenerse en la subclase.
- Los métodos de la subclase no deben lanzar nuevas excepciones que no estén definidas en la clase base.

Por ejemplo, hagamos una jerarquía de clases para aves:
```java
class Ave {
    void volar() { /* ... */ }
}

class Pato extends Ave {
    @Override
    void volar() { /* vuela como un pato */ }
}

class Pinguino extends Ave {
    @Override
    void volar() {
        throw new UnsupportedOperationException("Los pingüinos no vuelan.");
    }
}
```

Aquí, la clase `Pinguino` viola el LSP. Aunque `Pinguino` es un `Ave`, no puede reemplazar a `Ave` en un contexto donde se espera que `Ave` pueda volar, porque `Pinguino.volar()` lanza una excepción. Esto significa que si tenemos código que funciona con una lista de `Ave`s y espera que todas puedan volar, el programa falla si encuentra un `Pinguino`.

Para respetar el LSP, podemos refactorizar la jerarquía:
```java
interface Volador {
    void volar();
}

class Ave {
    // Propiedades comunes de las aves
}

class Pato extends Ave implements Volador {
    @Override
    public void volar() { /* vuela como un pato */ }
}

class Pinguino extends Ave {
    // No implementa Volador, ya que no vuela
}
```

Ahora, el código interactúa con aves voladoras a través de la interfaz `Volador`, asegurando que solo las aves que realmente pueden volar sean tratadas como tales. Los pingüinos, aunque son aves, no se considerarán `Volador`, manteniendo la consistencia.

---

## **ISP, Interface Segregation Principle (Principio de Segregación de Interfaces)**

Los clientes no deben ser forzados a depender de interfaces que no utilizan. Es mejor dividir una interfaz grande y monolítica en interfaces más pequeñas y específicas, de modo que cada una sea relevante para un conjunto particular de clientes. Esto reduce el acoplamiento y evita que las clases implementen métodos que no necesitan, promoviendo la cohesión.

Poe ejemplo, hagamos una interfaz `Trabajador` con muchos métodos:
```java
interface Trabajador {
    void trabajar();
    void comer();
    void dormir();
    void gestionarProyectos(); // Solo para gerentes
    void programar();         // Solo para desarrolladores
}

class Desarrollador implements Trabajador {
    @Override
    void trabajar() { /* ... */ }
    @Override
    void comer() { /* ... */ }
    @Override
    void dormir() { /* ... */ }
    @Override
    void gestionarProyectos() { /* No aplica, pero debe implementarlo */ }
    @Override
    void programar() { /* ... */ }
}
```

En este caso, `Desarrollador` se ve forzado a implementar `gestionarProyectos()`, aunque no lo use. Esto viola el ISP.

Aplicando el ISP, podemos segregar la interfaz:
```java
interface TrabajadorComun {
    void trabajar();
    void comer();
    void dormir();
}

interface Gerente {
    void gestionarProyectos();
}

interface Programador {
    void programar();
}

class Desarrollador implements TrabajadorComun, Programador {
    @Override
    void trabajar() { /* ... */ }
    @Override
    void comer() { /* ... */ }
    @Override
    void dormir() { /* ... */ }
    @Override
    void programar() { /* ... */ }
}

class GerenteProyecto implements TrabajadorComun, Gerente {
    @Override
    void trabajar() { /* ... */ }
    @Override
    void comer() { /* ... */ }
    @Override
    void dormir() { /* ... */ }
    @Override
    void gestionarProyectos() { /* ... */ }
}
```

Ahora, cada clase solo implementa las interfaces que realmente necesita, haciendo el diseño más limpio y modular.

---

## **DIP, Dependency Inversion Principle (Principio de Inversión de Dependencias)**

Establece que:

1. **Los módulos de alto nivel no deben depender de módulos de bajo nivel. Ambos deben depender de abstracciones.**
2. **Las abstracciones no deben depender de los detalles. Los detalles deben depender de las abstracciones.**

En esencia, este principio sugiere que las dependencias deben apuntar hacia abstracciones (interfaces o clases abstractas) en lugar de implementaciones concretas. Esto invierte la forma tradicional en que el control fluye en un programa, de ahí el término "inversión". Al depender de abstracciones, los módulos de alto nivel se desacoplan de los detalles de implementación de los módulos de bajo nivel, lo que mejora la flexibilidad, la reusabilidad y la capacidad de prueba del código.

Por ejemplo. hagamos una clase `Notificador` que envía notificaciones y depende directamente de una implementación de bajo nivel EmailSender:
```java
class EmailSender {
    void enviarEmail(String destinatario, String mensaje) { /* ... */ }
}

class Notificador {
    private EmailSender emailSender;

    public Notificador() {
        this.emailSender = new EmailSender(); // Dependencia directa de una implementación concreta
    }

    void enviarNotificacion(String destinatario, String mensaje) {
        emailSender.enviarEmail(destinatario, mensaje);
    }
}
```

Esta implementación viola el DIP porque `Notificador` (de alto nivel) depende directamente de `EmailSender` (de bajo nivel). Si queremos cambiar la forma de notificación (por ejemplo SMS), tendríamos que modificar `Notificador`.

Para aplicar el DIP, usamos una abstracción (interfaz):
```java
interface Mensajero {
    void enviar(String destinatario, String mensaje);
}

class EmailSender implements Mensajero {
    @Override
    public void enviar(String destinatario, String mensaje) {
        System.out.println("Enviando email a " + destinatario + ": " + mensaje);
    }
}

class SmsSender implements Mensajero {
    @Override
    public void enviar(String destinatario, String mensaje) {
        System.out.println("Enviando SMS a " + destinatario + ": " + mensaje);
    }
}

class Notificador {
    private Mensajero mensajero; // Depende de la abstracción

    // Inyección de dependencia a través del constructor
    public Notificador(Mensajero mensajero) {
        this.mensajero = mensajero;
    }

    void enviarNotificacion(String destinatario, String mensaje) {
        mensajero.enviar(destinatario, mensaje);
    }
}
```

Ahora, `Notificador` (de alto nivel) no depende de `EmailSender` o `SmsSender` (de bajo nivel), sino de la abstracción `Mensajero`. Podemos "inyectar" diferentes implementaciones de `Mensajero` en `Notificador` sin modificar su código. Esto hace que el `Notificador` sea mucho más flexible y fácil de probar, ya que se pueden sustituir fácilmente los servicios de mensajería.
