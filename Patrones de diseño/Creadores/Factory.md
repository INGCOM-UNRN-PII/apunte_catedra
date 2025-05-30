El patrón **Factory Method** (Método de Fábrica) es un patrón de diseño creacional que permite crear objetos sin especificar la clase exacta del objeto que se creará. En lugar de usar el operador `new` directamente, se delega la creación de objetos a un método de "fábrica" en una subclase. Esto hace que el código sea más flexible y extensible, ya que se pueden introducir nuevos tipos de objetos sin modificar el código cliente existente. Es especialmente útil cuando:

- Una clase no puede anticipar la clase de objetos que necesita crear.
- Una clase quiere que sus subclases especifiquen los objetos a crear.
- Las clases delegan la responsabilidad a una de las varias subclases auxiliares.

# Ejemplo UML

![Factory](https://github.com/user-attachments/assets/ef279c9c-945e-4bf4-98b3-fbdde8d3bbce)

# Ejemplo en código

```java
// La clase creadora declara el método fábrica, que debe devolver un
// objeto de un producto.
// Las subclases del creador implementarán este método para producir
// tipos específicos de productos.
public abstract class Dialogo {
    
    public void crearVentana() {
        // El creador no sabe qué producto concreto se producirá,
        // pero sabe que todos los productos concretos implementan
        // la interfaz Boton.
        Boton botonOk = crearBoton(); 
        botonOk.mostrar();
    }

    // Este es el "método fábrica" que deben implementar las subclases
    public abstract Boton crearBoton();
}

public class DialogoHTML extends Dialogo {
    // Esta subclase concreta implementa el método fábrica
    // para producir un producto HTML (BotonHtml).
    @Override
    public Boton crearBoton() {
        return new BotonHtml();
    }
}

public class DialogoWindows extends Dialogo {
    // Esta subclase concreta implementa el método fábrica
    // para producir un producto de Windows (BotonWindows).
    @Override
    public Boton crearBoton() {
        return new BotonWindows();
    }
}

// La interfaz del producto. Declara las operaciones comunes que todos los productos concretos deben implementar.
public interface Boton {
    void mostrar();
    void clickHecho();
}

// Un producto concreto que implementa la interfaz del producto.
public class BotonHtml implements Boton {
    public void mostrar() {
        System.out.println("<Boton>Boton HTML</Boton>");
        clickHecho();
    }

    public void clickHecho() {
        System.out.println("Click! Botón HTML dice - 'Hola mundo!'");
    }
}

// Otro producto concreto que implementa la interfaz del producto.
public class BotonWindows implements Boton {
    public void mostrar() {
        System.out.println("<Boton>Boton Windows</Boton>");
        clickHecho();
    }

    public void clickHecho() {
        System.out.println("Click! Botón Windows dice - 'Hola mundo!'");
    }
}

public class Demo {
    private static Dialogo dialogo;

    public static void main(String[] args) {
        configurar();
        hacerCosas();
    }

    static void configurar() {
        // La lógica de configuración decide qué creador concreto instanciar,
        // basándose en algún criterio (en este caso, el sistema operativo).
        // Esto desacopla el código del cliente de la creación de
        // productos concretos.
        if (System.getProperty("os.name").equals("Windows 10")) {
            dialogo = new DialogoWindows();
        } else {
            dialogo = new DialogoHTML();
        }
    }

    static void hacerCosas() {
        // El cliente trabaja con la interfaz del creador y su método fábrica,
        // sin preocuparse por la clase concreta del producto que se crea.
        dialogo.crearVentana();
    }
}
```
