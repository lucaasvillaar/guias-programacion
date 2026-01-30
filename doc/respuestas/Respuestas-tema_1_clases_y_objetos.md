<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Clases y Objetos". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: ninguno.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->

# TEMA 1. Clases y objetos

## 1. ¿Cuáles son las cuatro características básicas de la programación orientada a objetos? Describe brevemente cada una

Se identifican cuatro pilares fundamentales: Abstracción, que consiste en aislar los elementos esenciales de un objeto para definir sus características relevantes, ignorando los detalles no esenciales. Encapsulamiento, que permite agrupar datos y comportamientos en una sola unidad, protegiendo el acceso a la información interna mediante niveles de visibilidad.

Por otro lado, la Herencia permite crear nuevas clases a partir de otras ya existentes, facilitando la reutilización de código y la jerarquización de conceptos. Finalmente, el Polimorfismo otorga la capacidad a diferentes objetos de responder de forma distinta a un mismo mensaje o llamada a un método, permitiendo que una misma interfaz se comporte de varias maneras.

## 2. Cita cuatro lenguajes populares que permitan la programación orientada a objetos

Existen numerosos lenguajes que implementan este paradigma. Entre los más destacados se encuentra Java, ampliamente utilizado en entornos corporativos y aplicaciones Android. C++ es otro referente fundamental, ya que añade capacidades de orientación a objetos al lenguaje C original que ya conoces.

También se deben mencionar Python, conocido por su sintaxis clara y versatilidad en ciencia de datos, y C#, desarrollado por Microsoft y muy popular en el desarrollo de aplicaciones de escritorio y videojuegos con el motor Unity.

## 3. Los paradigmas anteriores a la POO, ¿Qué es la **programación estructurada**? y, todavía mejor, ¿Qué es la **programación modular**?

La programación estructurada se basa en la división del código en estructuras de control secuenciales, condicionales e iterativas (bucles). Se enfoca en la lógica del algoritmo y el flujo de ejecución, utilizando funciones y procedimientos para organizar la tarea, muy similar a lo que has trabajado en C estándar.

La programación modular da un paso más allá al organizar el código en unidades separadas e independientes llamadas módulos. Cada módulo agrupa funciones y datos relacionados, permitiendo que el software se divida en piezas que se pueden compilar y probar por separado, facilitando el mantenimiento en proyectos grandes.

## 4. ¿Qué tres elementos definen a un objeto en programación orientada a objetos?

Un objeto se define por su Estado, su Comportamiento y su Identidad. El estado está determinado por los datos o valores almacenados en sus atributos en un momento dado. El comportamiento viene definido por los métodos o funciones que el objeto puede ejecutar y que, generalmente, operan sobre su propio estado.

La identidad es lo que distingue a un objeto de cualquier otro, incluso si tienen el mismo estado y comportamiento. En términos de memoria, esto suele asociarse a la dirección única donde reside el objeto, permitiendo que el sistema diferencie entre dos instancias cuyos datos sean idénticos.

## 5. ¿Qué es una clase? ¿Es lo mismo que un objeto? ¿Qué es una instancia? ¿Todos los lenguajes orientados a objetos manejan el concepto de clase?

Una clase es la plantilla, molde o plano que define cómo será un objeto; especifica qué variables tendrá y qué funciones podrá ejecutar. No es lo mismo que un objeto, ya que la clase es el concepto abstracto (como el plano de una casa), mientras que el objeto es el elemento concreto creado a partir de ese plano (la casa física).

Una instancia es sinónimo de objeto. Se dice que se "instancia una clase" cuando se reserva memoria para crear un nuevo objeto basado en esa definición. No todos los lenguajes orientados a objetos manejan clases; existen lenguajes como JavaScript (en sus versiones clásicas) que utilizan "prototipos" en lugar de clases formales.

## 6. ¿Dónde se almacenan en memoria los objetos? ¿Es igual en todos los lenguajes? ¿Qué es la **recolección de basura**? 

En lenguajes como Java, los objetos se almacenan predominantemente en una zona de memoria dinámica llamada Heap (montón). Esto difiere de variables locales simples que se guardan en el Stack (pila). En C++, el programador puede elegir si un objeto vive en el Stack o en el Heap, pero en Java la gestión es más automática.

La recolección de basura (Garbage Collection) es un proceso automático del sistema que identifica y libera la memoria de aquellos objetos que ya no tienen ninguna referencia apuntando a ellos. Esto evita fugas de memoria, liberando al programador de la tarea manual de usar comandos tipo free() en C o delete en C++.

## 7. ¿Qué es un método? ¿Qué es la **sobrecarga de métodos**? 

Un método es una función que está definida dentro de una clase y que representa una acción que el objeto puede realizar. A diferencia de las funciones globales en C, un método siempre está vinculado a un objeto y tiene acceso a sus atributos internos.

La sobrecarga de métodos ocurre cuando se definen varios métodos con el mismo nombre dentro de la misma clase, pero con diferentes parámetros (distinto número o tipo de datos). El compilador decide cuál ejecutar basándose en los argumentos que se pasen en la llamada, permitiendo realizar acciones similares con diferentes entradas.

## 8. Ejemplo mínimo de clase en Java, que se llame Punto, con dos atributos, x e y, con un método que se llame `calculaDistanciaAOrigen`, que calcule la distancia a la posición 0,0. Por sencillez, los atributos deben tener visibilidad por defecto. Crea además un ejemplo de uso con una instancia y uso del método

public class Punto {
    double x;
    double y;

    double calculaDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }
}

Ejemplo de uso:

public class Principal {
    public static void main(String[] args) {
        Punto p = new Punto();
        p.x = 3.0;
        p.y = 4.0;
        System.out.println("Distancia: " + p.calculaDistanciaAOrigen());
    }
}

## 9. ¿Cuál es el punto de entrada en un programa en Java? ¿Qué es `static` y para qué vale? ¿Sólo se emplea para ese método `main`? ¿Para qué se combina con `final`?

El punto de entrada es el método public static void main(String[] args). La palabra clave static indica que el método pertenece a la clase en sí y no a una instancia específica, por lo que se puede llamar sin crear un objeto con new.

No se emplea solo para main; puede usarse en atributos para crear variables compartidas por todos los objetos de esa clase. Cuando se combina con final, se utiliza para definir constantes globales (ej. static final double PI = 3.1415), asegurando que el valor sea único para la clase y que no pueda ser modificado.

## 10. Intenta ejecutar un poco de Java de forma básica, con los comandos `javac` y `java`. ¿Cómo podemos compilar el programa y ejecutarlo desde linea de comandos? ¿Java es compilado? ¿Qué es la **máquina virtual**? ¿Qué es el *byte-code* y los ficheros `.class`?

Para compilar se utiliza el comando javac NombreClase.java, lo cual genera un archivo .class. Para ejecutarlo se usa java NombreClase. Java es un lenguaje híbrido: se compila a un lenguaje intermedio llamado Byte-code, que no es código máquina directo, sino instrucciones para la Máquina Virtual de Java (JVM).

La JVM actúa como una capa de abstracción que permite que el mismo archivo .class funcione en cualquier sistema operativo (Windows, Linux, Mac) sin cambios. El byte-code es, por tanto, el código "universal" que la JVM interpreta o compila en tiempo real para el hardware específico.

## 11. En el código anterior de la clase `Punto` ¿Qué es `new`? ¿Qué es un **constructor**? Pon un ejemplo de constructor en una clase `Empleado` que tenga DNI, nombre y apellidos

La palabra clave new se encarga de solicitar memoria en el Heap para un nuevo objeto y de invocar al constructor. Un constructor es un método especial que se llama automáticamente al crear el objeto y cuya función principal es inicializar los atributos del mismo.



## 12. ¿Qué es la referencia `this`? ¿Se llama igual en todos los lenguajes? Pon un ejemplo del uso de `this` en la clase `Punto`

La referencia this es un puntero implícito que apunta al objeto actual que está ejecutando el método. Se utiliza para diferenciar atributos de la clase de parámetros locales cuando tienen el mismo nombre. En Python se llama self, mientras que en C++ también se utiliza this.

public void setPosicion(double x, double y) {
    this.x = x; // this.x es el atributo de la clase, x es el parámetro
    this.y = y;
}

## 13. Añade ahora otro nuevo método que se llame `distanciaA`, que reciba un `Punto` como parámetro y calcule la distancia entre `this` y el punto proporcionado

double distanciaA(Punto otroPunto) {
    double dx = this.x - otroPunto.x;
    double dy = this.y - otroPunto.y;
    return Math.sqrt(dx * dx + dy * dy);
}


## 14. El paso del `Punto` como parámetro a un método, es **por copia** o **por referencia**, es decir, si se cambia el valor de algún atributo del punto pasado como parámetro, dichos cambios afectan al objeto fuera del método? ¿Qué ocurre si en vez de un `Punto`, se recibiese un entero (`int`) y dicho entero se modificase dentro de la función? 

En Java, los objetos se pasan por referencia (técnicamente es un paso por valor de la referencia). Si se modifica un atributo del Punto pasado como parámetro dentro de un método, el cambio sí afecta al objeto original fuera del método, ya que ambos apuntan al mismo sitio en el Heap.

Por el contrario, si se recibe un tipo primitivo como un int, este se pasa por copia. Cualquier modificación que se realice sobre ese entero dentro del método es local y desaparece al terminar la función, dejando el valor original intacto, tal como funcionan los tipos simples en C.

## 15. ¿Qué es el método `toString()` en Java? ¿Existe en otros lenguajes? Pon un ejemplo de `toString()` en la clase `Punto` en Java

El método toString() se utiliza para devolver una representación en texto del objeto. Existe en casi todos los lenguajes modernos (en Python es __str__). En Java, si se imprime un objeto con System.out.println(), se llama automáticamente a este método.

@Override
public String toString() {
    return "Punto(x=" + x + ", y=" + y + ")";
}

## 16. Reflexiona: ¿una clase es como un `struct` en C? ¿Qué le falta al `struct` para ser como una clase y las variables de ese tipo ser instancias?


Una clase se parece a un struct en que ambos agrupan datos de diferentes tipos. Sin embargo, al struct le falta la capacidad de incluir comportamiento (métodos) dentro de sí mismo y mecanismos de encapsulamiento (public/private).

Para que un struct sea como una clase, debería permitir definir funciones que pertenezcan a su ámbito y que tengan acceso automático a sus miembros. En C++, de hecho, un struct es prácticamente una clase donde todo es público por defecto.


## 17. Quitemos un poco de magia a todo esto: ¿Como se podría “emular”, con `struct` en C, la clase `Punto`, con su función para calcular la distancia al origen? ¿Qué ha pasado con `this`?

Para emularlo en C, se define el struct y funciones externas que reciban un puntero a dicho struct. El concepto de this desaparece como palabra reservada y debe pasarse explícitamente como el primer argumento de la función, usualmente llamado self o p.

struct Punto {
    double x, y;
};

double calculaDistanciaAOrigen(struct Punto* self) {
    return sqrt(self->x * self->x + self->y * self->y);
}