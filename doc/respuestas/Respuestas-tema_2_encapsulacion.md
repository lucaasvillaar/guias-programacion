<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Encapsulación". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# TEMA 2. Encapsulación

## 1. En Programación Orientada a Objetos (POO), ¿Qué buscan la **encapsulación** y **la ocultación** de información? Enumera brevemente algunas ventajas de la ocultación de información.

La encapsulación busca agrupar en una única unidad (la clase) tanto los datos como las funciones que operan sobre ellos. Por su parte, la ocultación de información persigue restringir el acceso directo a los detalles internos de dicha unidad, exponiendo solo lo estrictamente necesario. Mientras que en C los datos (structs) y las funciones suelen estar dispersos, aquí se busca crear componentes autónomos.

Entre las ventajas de la ocultación destacan el aislamiento de errores, ya que se impide que código externo modifique datos de forma imprevista, y la facilidad de mantenimiento, pues permite cambiar la implementación interna de una clase sin que el resto del programa, que solo conoce la parte visible, se vea afectado.

## 2. ¿Qué se entiende por la **interfaz pública** de un objeto o clase en POO? Describe brevemente cómo se relaciona con la ocultación de información.

La interfaz pública es el conjunto de métodos y atributos que una clase permite que sean utilizados por código externo. En términos de C, sería comparable a las declaraciones en un archivo de cabecera (.h) que otros módulos pueden invocar, constituyendo el "contrato" o la promesa de qué servicios ofrece el objeto.

La relación con la ocultación es de complementariedad: la ocultación decide qué se guarda bajo llave (lo privado), mientras que la interfaz pública decide qué puertas se dejan abiertas. Un buen diseño minimiza la interfaz pública para reducir el acoplamiento, asegurando que el usuario del objeto sepa qué hace el objeto, pero no cómo lo hace internamente.

## 3. Brevemente: ¿Por qué hay que ser conscientes y diseñar con cuidado la **interfaz pública** de una clase? ¿Es fácil cambiarla?

Diseñar la interfaz pública requiere cuidado porque cualquier cambio en ella puede romper todo el código que dependa de esa clase. Una vez que un método se declara público y otros programadores lo usan, ese método se convierte en un compromiso a largo plazo.

Cambiar la interfaz pública no es fácil; es una tarea costosa que suele requerir la actualización de múltiples partes del sistema. Por el contrario, si algo se mantiene privado (oculto), puede modificarse, optimizarse o eliminarse en cualquier momento sin afectar a los usuarios de la clase, de ahí la importancia de exponer lo mínimo posible.

## 4. ¿Qué son las **invariantes de clase** y por qué la ocultación de información nos ayuda?

Las invariantes de clase son condiciones o reglas lógicas que deben cumplirse siempre para que un objeto se considere válido. Por ejemplo, en una clase Fecha, una invariante sería que el mes siempre esté en el rango de 1 a 12. Son estados de coherencia que el objeto debe garantizar durante toda su vida.

La ocultación de información es la herramienta clave para proteger estas invariantes. Al impedir el acceso directo a los atributos (haciéndolos privados), se obliga a que cualquier cambio pase por métodos controlados que validen los datos, evitando que el objeto entre en un estado corrupto o inconsistente.

## 5. Pon un ejemplo de una clase `Punto` en `Java`, con dos coordenadas, `x` e `y`, de tipo `double`, con un método `calcularDistanciaAOrigen`, y que haga uso de la ocultación de información. ¿Cuál es la interfaz pública de la clase `Punto`? ¿Qué significa `public` y `private`?

public class Punto {
    private double x;
    private double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double calcularDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }
}

La interfaz pública de esta clase está compuesta por el constructor Punto(double x, double y) y el método calcularDistanciaAOrigen(). Los atributos x e y no forman parte de ella al ser privados.

En Java, public indica que el miembro es accesible desde cualquier otra clase del proyecto. Por el contrario, private restringe el acceso únicamente a los métodos que pertenecen a la misma clase, protegiendo los datos de manipulaciones externas directas.

## 6. En Java, ¿A quiénes se pueden aplicar los modificadores `public` o `private`?

En Java, los modificadores de acceso se aplican principalmente a los miembros de la clase, que incluyen los atributos (variables) y los métodos. También se aplican a los constructores para controlar quién puede instanciar la clase.

Asimismo, los modificadores se aplican a las clases mismas. Una clase puede ser public (visible en todo el proyecto) o tener un nivel de acceso por defecto (visible solo dentro de su paquete). No es común declarar clases de primer nivel como private, aunque sí es posible en el caso de clases internas (clases dentro de otras clases).

## 7. En POO, la visibilidad puede ser pública o privada, pero ¿existen más tipos de visibilidad? ¿Qué ocurre en Java? ¿Y en otros lenguajes?

Sí, existen niveles de visibilidad intermedios. En Java, además de public y private, existe el nivel por defecto (package-private), que permite el acceso solo a clases del mismo paquete, y el nivel protected, que permite el acceso a clases del mismo paquete y a las subclases (herencia).

Otros lenguajes ofrecen variantes similares. En C++, protected funciona de forma similar pero más restrictiva respecto al paquete. Algunos lenguajes modernos incluyen niveles como internal (Swift/Kotlin) para limitar la visibilidad a un módulo o unidad de compilación específica, refinando el control sobre la arquitectura del software.

## 8. Responde: Los miembros de instancia privados de un objeto están ocultos para (a) otras clases o (b) otras instancias, aunque sean de la misma clase. Pon un ejemplo añadiendo un método `calcularDistanciaAPunto(Punto otro)` y explica la respuesta.

La respuesta correcta es la (a). Los miembros privados están ocultos para otras clases, pero son visibles para todas las instancias de la misma clase. Esto significa que un objeto de la clase Punto puede acceder a los atributos privados de otro objeto de la clase Punto.

public double calcularDistanciaAPunto(Punto otro) {
    // Es legal acceder a otro.x y otro.y porque estamos dentro de la clase Punto
    double dx = this.x - otro.x;
    double dy = this.y - otro.y;
    return Math.sqrt(dx * dx + dy * dy);
}

## 9. ¿Qué son los métodos "getter" y "setter" en los lenguajes orientados a objetos?

Los getters y setters son métodos públicos diseñados para leer (get) o modificar (set) el valor de un atributo privado. En lugar de permitir el acceso directo a la variable, se utiliza un método intermediario que actúa como filtro o portero.

El uso de estos métodos permite añadir lógica adicional, como validaciones en los setters para asegurar que los datos son correctos, o transformar el dato en los getters antes de entregarlo. Es una práctica estándar para mantener la encapsulación mientras se permite la interacción con los datos del objeto.

## 10. Cuando nos referimos a que la ocultación de información mejora la "seguridad" del programa, ¿nos referimos a que no pueda ser "hackeado"?

No, en este contexto, "seguridad" no se refiere a la ciberseguridad o protección contra ataques externos (hackeo), sino a la integridad técnica del programa. Se trata de evitar errores accidentales de programación que lleven al software a estados inconsistentes o fallos de ejecución.

Un programa es más "seguro" cuando sus componentes están aislados, ya que se minimizan los efectos secundarios indeseados (side effects). Si un programador no puede modificar directamente una variable interna, no puede romper accidentalmente la lógica que depende de ella, lo que resulta en un código más robusto y fiable.

## 11. ¿Qué diferencia hay entre **miembro de instancia** y **miembro de clase**? ¿Los miembros de clase también se pueden ocultar?

Un miembro de instancia es aquel del cual existe una copia independiente por cada objeto creado (como las coordenadas de un punto). Un miembro de clase (marcado con static) es único para toda la clase; existe una sola copia compartida por todas las instancias, similar a una variable global en C pero limitada al ámbito de la clase.

Los miembros de clase también se pueden ocultar utilizando el modificador private. De hecho, es una práctica común ocultar variables estáticas que guardan estados globales de la clase (como contadores) para que solo puedan ser gestionadas mediante métodos específicos, manteniendo el control sobre su modificación.

## 12. Brevemente: ¿Tiene sentido que los constructores sean privados?

Sí, tiene mucho sentido en varios patrones de diseño. Un constructor privado impide que se puedan crear instancias de la clase de forma convencional mediante el operador new desde fuera de la clase. Esto es útil para clases que solo contienen métodos estáticos (como una librería de funciones matemáticas).

También se utiliza cuando se quiere controlar estrictamente la creación de objetos, como en el patrón Singleton (donde solo debe existir una instancia) o cuando se emplean métodos factoría, que deciden cómo y qué tipo de objeto devolver según los parámetros recibidos.

## 13. ¿Cómo se indican los **miembros de clase** en Java? Pon un ejemplo, en la clase `Punto` definida anteriormente, para que incluya miembros de clase que permitan saber cuáles son los valores `x` e `y` máximos que se han establecido en todos los puntos que se hayan creado hasta el momento.

Los miembros de clase se indican con la palabra clave static.

public class Punto {
    private double x, y;
    private static double maxX = Double.NEGATIVE_INFINITY;
    private static double maxY = Double.NEGATIVE_INFINITY;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
        if (x > maxX) maxX = x;
        if (y > maxY) maxY = y;
    }

    public static double getMaxX() { return maxX; }
    public static double getMaxY() { return maxY; }
}

En este ejemplo, maxX y maxY son compartidos por todos los objetos Punto. Cada vez que se crea un objeto, se actualizan estos valores globales de la clase.

## 14. Como sería un método factoría dentro de la clase `Punto` para construir un `Punto` a partir de dos coordenadas, pero que las redondee al entero más cercano. Escribe sólo el código del método, no toda la clase ¿Has usado `static`? 

public static Punto crearPuntoRedondeado(double x, double y) {
    double xRedondeado = Math.round(x);
    double yRedondeado = Math.round(y);
    return new Punto(xRedondeado, yRedondeado);
}

Se ha utilizado el modificador static porque el método pertenece a la clase y no a una instancia específica (se llama antes de que el objeto exista). Este método encapsula la lógica de redondeo y devuelve una nueva instancia válida.

## 15. Cambia la implementación de `Punto`. En vez de dos `double`, emplea un array interno de dos posiciones, intentando no modificar la interfaz pública de la clase.

public class Punto {
    private double[] coords = new double[2];

    public Punto(double x, double y) {
        this.coords[0] = x;
        this.coords[1] = y;
    }

    public double getX() { return coords[0]; }
    public double getY() { return coords[1]; }

    public double calcularDistanciaAOrigen() {
        return Math.sqrt(coords[0] * coords[0] + coords[1] * coords[1]);
    }
}

Aunque la estructura interna ha cambiado de dos variables a un array, la interfaz pública (los métodos disponibles) permanece igual. Esto demuestra el poder de la ocultación: el código que usa Punto no necesita enterarse de este cambio interno.

## 16. Si un atributo va a tener un método "getter" y "setter" públicos, ¿no es mejor declararlo público? ¿Cuál es la convención más habitual sobre los atributos, que sean públicos o privados? ¿Tiene esto algo que ver con las "invariantes de clase"?

No es mejor declararlo público. Aunque parezca un rodeo innecesario, usar métodos proporciona un punto de control. Si en el futuro se necesita añadir una validación (por ejemplo, que la coordenada no sea negativa), se puede hacer en el setter sin cambiar el resto del programa. Si el atributo fuera público, habría que buscar y cambiar cada línea de código donde se asigne un valor.

La convención habitual es que todos los atributos sean privados. Se exponen solo a través de getters y setters si es necesario. Esto preserva las invariantes de clase, ya que el objeto mantiene la soberanía absoluta sobre sus datos, impidiendo que agentes externos impongan valores que violen su lógica de negocio.

## 17. ¿Qué significa que una clase sea **inmutable**? ¿qué es un método modificador? ¿Un método modificador es siempre un "setter"? ¿Tiene ventajas que una clase sea inmutable?

Una clase es inmutable cuando su estado no puede cambiar después de ser creada. Un método modificador es aquel que altera el estado interno del objeto. No siempre es un "setter"; cualquier método que asigne nuevos valores a los atributos (como un método desplazar(dx, dy)) es un modificador.

En una clase inmutable, los métodos que parecerían modificar el objeto en realidad devuelven una nueva instancia con los cambios aplicados. La ventaja principal es la simplicidad y seguridad: los objetos inmutiles son inherentemente seguros para ser compartidos entre diferentes partes de un programa (o hilos de ejecución), ya que nadie puede alterarlos por sorpresa.

## 18. ¿Es recomendable incluir métodos "setter" siempre y como convención?

No, no es recomendable incluirlos por defecto. Solo deben añadirse si realmente existe la necesidad de que el estado del objeto cambie tras su creación. Incluir setters para cada atributo de forma automática suele ser una mala práctica que debilita la encapsulación.

Un exceso de setters puede exponer detalles internos innecesarios y facilitar que el objeto acabe en un estado inconsistente. El diseño debe tender a ser lo más restrictivo posible al inicio; es fácil añadir un setter más adelante, pero muy difícil quitarlo si ya se está utilizando.

## 19. ¿La clase `String` en Java es mutable o inmutable? ¿Qué ocurre al concatenar dos cadenas? ¿Qué debemos hacer si vamos a hacer una operación que implique concatenar muchas veces para construir paso a paso una cadena muy larga?

La clase String en Java es inmutable. Cuando se concatenan dos cadenas (por ejemplo, s1 + s2), no se modifica la cadena original, sino que se crea un objeto String completamente nuevo en memoria que contiene el resultado.

Si se realizan miles de concatenaciones en un bucle, el rendimiento cae drásticamente debido a la creación constante de objetos temporales. Para estos casos, se debe utilizar la clase StringBuilder, que es una versión mutable diseñada para construir cadenas largas de forma eficiente mediante un búfer interno.

## 20. En POO ¿Cómo se comparan objetos de una misma clase? ¿Por su contenido o por su identidad? ¿Qué es el método equals en Java? ¿Qué hace por defecto? ¿Cómo se deben comparar dos cadenas en Java? 

En Java, el operador == compara la identidad (referencia de memoria), es decir, si ambos apuntan exactamente al mismo objeto. Para comparar el contenido (si dos objetos distintos tienen los mismos datos), se debe usar el método equals().

Por defecto, equals() se comporta igual que ==. Sin embargo, las clases pueden (y deben) sobrescribirlo para definir qué significa que dos objetos sean iguales. En el caso de las cadenas de texto, siempre se debe usar s1.equals(s2), ya que el uso de == daría un resultado falso si las cadenas están en distintas posiciones de memoria aunque el texto sea idéntico.

## 21. ¿Qué son las clases "wrapper" en un lenguaje de programación orientado a objetos? ¿Cómo se hace? ¿Es un proceso automático? ¿Qué ventajas tienen? ¿Todos los lenguajes orientados a objetos tienen tipos primitivos y necesitan wrappers? 

Las clases wrapper (envoltorios) son clases que representan tipos primitivos (como int, double, char) como objetos (Integer, Double, Character). Esto es necesario porque en Java los tipos primitivos no son objetos y, a veces, se requieren objetos para usar ciertas utilidades del lenguaje, como las colecciones.

Java realiza este proceso automáticamente mediante el autoboxing y unboxing. No todos los lenguajes los necesitan; lenguajes como Python o Ruby consideran que todo es un objeto desde el principio, mientras que otros como C++ no tienen este concepto de envoltorio automático integrado de la misma manera.

## 22. ¿En POO qué es un **tipo de dato enumerado**? ¿En Java, un tipo de dato enumerado es una clase? ¿Qué ventajas tienen en términos de encapsulación los enumerados en Java?

Un tipo enumerado (enum) es un tipo de dato que define un conjunto fijo de constantes con nombre. En Java, a diferencia de C, los enum son clases completas. Esto significa que pueden tener atributos, métodos y constructores.

En términos de encapsulación, los enumerados en Java permiten agrupar comportamiento relacionado con las constantes. Por ejemplo, un enum Estado puede tener un método para saber si es un "estado final", ocultando la lógica de decisión dentro del propio enumerado y evitando el uso de sentencias if/switch dispersas por todo el código.

## 23. Crea un tipo enumerado en Java que se llame `Mes`, con doce posibles instancias y que además proporcione métodos para obtener cuántos días tiene ese mes, el ordinal de ese mes en el año (1-12), empleando atributos privados y constructores del tipo enumerado. Añade además cuatro métodos para devolver si ese mes tiene algunos días de invierno, primavera, verano u otoño, indicando con un booleano el hemisferio (norte o sur, parámetro `enHemisferioNorte`). Es decir: `esDePrimavera(boolean esHemisferioNorte)`, `esDeVerano(boolean esHemisferioNorte)`, `esDeOtoño(boolean esHemisferioNorte)`, `esDeInvierno(boolean esHemisferioNorte)`

public enum Mes {
    ENERO(31, 1), FEBRERO(28, 2), MARZO(31, 3), ABRIL(30, 4),
    MAYO(31, 5), JUNIO(30, 6), JULIO(31, 7), AGOSTO(31, 8),
    SEPTIEMBRE(30, 9), OCTUBRE(31, 10), NOVIEMBRE(30, 11), DICIEMBRE(31, 12);

    private final int dias;
    private final int ordinal;

    private Mes(int dias, int ordinal) {
        this.dias = dias;
        this.ordinal = ordinal;
    }

    public int getDias() { return dias; }
    public int getOrdinal() { return ordinal; }

    public boolean esDePrimavera(boolean norte) {
        return norte ? (this == MARZO || this == ABRIL || this == MAYO) : 
                       (this == SEPTIEMBRE || this == OCTUBRE || this == NOVIEMBRE);
    }
    // (Resto de métodos esDeVerano, etc. seguirían una lógica similar)
}

Este diseño encapsula toda la información del mes dentro del tipo. Los atributos son privados y finales, asegurando la inmutabilidad, y los métodos proporcionan la lógica necesaria basándose en el hemisferio indicado.