Proporciona una interfaz para crear **familias de objetos relacionados o dependientes** sin especificar sus clases concretas. Es una "fábrica de fábricas". 
Se usa cuando:
* El sistema debe ser independiente de cómo se crean, componen y representan sus productos.
* Un sistema debe configurarse con una de varias familias de productos.
* Una familia de objetos relacionados está diseñada para ser utilizada junta, y se necesita asegurar esta consistencia.
* Se quiere proporcionar una biblioteca de clases de productos y se quiere revelar solo sus interfaces, no sus implementaciones.
Para usar este patrón, se define una interfaz "Fábrica Abstracta" que declara métodos para crear cada tipo de producto abstracto dentro de una familia. Luego, se crean "Fábricas Concretas", cada una de las cuales implementa la interfaz de la fábrica abstracta para producir una variante específica de la familia de productos. El cliente utiliza la fábrica abstracta para crear los productos sin conocer las clases concretas.
Suele implementarse usando composición, donde la fábrica abstracta se inyecta en el cliente, y es la fábrica abstracta la que delega la creación de los objetos a sus fábricas concretas (que a su vez pueden usar Factory Methods).

El patrón Abstract Factory suele implementarse usando Factory Methods. Cada método de creación dentro de una Fábrica Concreta de un Abstract Factory puede ser, en sí mismo, un Factory Method.

Diferencias:
* **Factory Method:** Se centra en la creación de un **único tipo de objeto**, dejando la decisión de la subclase a instanciar.
* **Abstract Factory:** Se enfoca en la creación de una **familia completa de objetos relacionados**, asegurando que todos los productos de esa familia sean compatibles entre sí. Proporciona un nivel más alto de abstracción.

## Ejemplo UML

![Abstract Factory](https://github.com/user-attachments/assets/299d4561-5d1b-4286-9a71-00b70df2314f)

## Ejemplo en código
```java
/**
 * Esta clase usa la fábrica que se le pase como argumento. 
 * A esta clase no le importa cual fábrica concreta usar.
 */
public class Aplicacion {
    private final Boton boton;
    private final Checkbox checkbox;

	// El constructor recibe una FabricaGUI, que es la interfaz abstracta
	// que las fábricas concretas implementan.
    public Aplicacion(FabricaGUI fabrica) {
        boton = fabrica.crearBoton();
        checkbox = fabrica.crearCheckbox();
    }

    public void paint() {
        boton.paint();
        checkbox.paint();
    }
}

/**
 * La fábrica abstracta asume que hay varias familias de productos,
 * estructurados en jerarquías separadas de clases. Todos los productos de 
 * la misma familia usan una misma interfaz.
 * 
 * Esta es la interfaz específica para botones.
 */
public interface Boton {
    void paint();
}

/**
 * Todas las familias de productos tienen que tener las mismas variantes,
 * en este caso Mac/Windows.
 *
 * Esta es la variante de boton para Mac.
 */
public class BotonMac implements Boton {

    @Override
    public void paint() {
        System.out.println("Se ha creado un BotonMac");
    }
}

/**
 * Todas las familias de productos tienen que tener las mismas variantes,
 * en este caso Mac/Windows.
 *
 * Esta es la variante de boton para Windows.
 */
public class BotonWindows implements Boton {

    @Override
    public void paint() {
        System.out.println("Se ha creado un BotonWindows");
    }
}

/**
 * Esta es la segunda familia de productos, Tambíen vienen en veriones
 * para Mac o Windows.
 */
public interface Checkbox {
    void paint();
}

/**
 * Todas las familias de productos tienen que tener las mismas variantes,
 * en este caso Mac/Windows.
 *
 * Esta es la variante de checkbox para Mac.
 */
public class CheckboxMac implements Checkbox {

    @Override
    public void paint() {
        System.out.println("Se ha creado un CheckboxMac");
    }
}

/**
 * Todas las familias de productos tienen que tener las mismas variantes,
 * en este caso Mac/Windows.
 *
 * Esta es la variante de checkbox para Windows.
 */
public class CheckboxWindows implements Checkbox {

    @Override
    public void paint() {
        System.out.println("Se ha creado un CheckboxWindows");
    }
}

//A continuacion van las fabricas

/**
 * Esta fábrica abstracta conoce todos los tipos (abstractos) de productos
 */
public interface FabricaGUI {
    Boton crearBoton();
    Checkbox crearCheckbox();
}

/**
 * Cada fábrica concreta extiende a la fábrica base y es responsable de crear
 * una sola variedad de producto.
 * Esta fábrica en particular crea la variedad Mac
 */
public class FabricaMac implements FabricaGUI {

    @Override
    public Boton crearBoton() {
        return new BotonMac();
    }

    @Override
    public Checkbox crearCheckbox() {
        return new CheckboxMac();
    }
}

/**
 * Cada fábrica concreta extiende a la fábrica base y es responsable de crear
 * una sola variedad de producto.
 * Esta fábrica en particular crea la variedad Windows
 */
public class FabricaWindows implements FabricaGUI {

    @Override
    public Boton crearBoton() {
        return new BotonWindows();
    }

    @Override
    public Checkbox crearCheckbox() {
        return new CheckboxWindows();
    }
}

//Demo
public class Demo {

    /**
     * La aplicación elige el tipo de fábrica y la crea en tiempo de ejecución,
     * usualmente en su inicialización, dependiendo de ciertas variables que
     * el usuario puede elegir.
     */
    private static Aplicacion configurarApp() {
        Aplicacion app;
        FabricaGUI fabrica;
        String osName = System.getProperty("os.name").toLowerCase();
        if (osName.contains("mac")) {
            fabrica = new FabricaMac();
        } else {
            fabrica = new FabricaWindows();
        }
        app = new Aplicacion(fabrica);
        return app;
    }

    public static void main(String[] args) {
        Aplicacion app = configurarApp();
        app.paint();
    }
}
```
