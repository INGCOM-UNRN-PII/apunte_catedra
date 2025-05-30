Provee una **interfaz** para crear **objetos** en una **superclase**, pero permite a las **subclases** alterar el tipo de **objeto** creado.

# Ejemplo en UML

![Factory](https://github.com/user-attachments/assets/14193c6d-3017-4560-8a31-6584d2e6284c)

# Ejemplo en código
```java
// Interfaz de los motores
public interface Motor {
    void arrancar();
    void frenar();
}

// Clases de motores concretos
public class MotorNafta implements Motor {
    @Override
    public void arrancar() {
        System.out.println("Motor a nafta arranca.");
    }

    @Override
    public void frenar() {
        System.out.println("Motor a nafta se detiene.");
    }
}

public class MotorElectrico implements Motor {
    @Override
    public void arrancar() {
        System.out.println("Motor eléctrico se enciende.");
    }

    @Override
    public void frenar() {
        System.out.println("Motor eléctrico frena.");
    }
}

// Clase abstracta que maneja todo tipo de auto
public abstract class Auto {
    public void manejar() {
        // El motor se crea dentro de esta clase
        Motor motor = crearMotor();
        motor.arrancar();
        System.out.println("El auto está andando.");
        motor.frenar();
    }

	// Cada clase de auto concreto implementa su propia versión de este método
    public abstract Motor crearMotor();
}

// Estos son los creadores concretos
public class GolPower extends Auto {
    @Override
    public Motor crearMotor() {
        return new MotorNafta();
    }
}

public class TeslaModeloX extends Auto {
    @Override
    public Motor crearMotor() {
        return new MotorElectrico();
    }
}

// Ejemplo de uso
public class Demo {
    private static Auto auto;

    public static void main(String[] args) {
        configurarAuto();
        cosasAuto();
    }

    static void configurarAuto() {
        // Esta cadena puede ser ingresada por el usuario, definida por
        // una condición, etcétera.
        String tipoAuto = "Tesla";
		
		// Aquí se decide cual Auto instanciar
        if (tipoAuto.equalsIgnoreCase("Tesla")) {
            Auto = new TeslaModeloX();
        } else {
            Auto = new GolPower();
        }
    }

    static void cosasAuto() {
        Auto.manejar();
    }
}
```
