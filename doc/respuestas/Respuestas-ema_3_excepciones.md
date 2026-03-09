<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Excepciones". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# TEMA 3. Excepciones

## 1. Empecemos un tema sobre control de errores en lenguajes de programación, con algo básico. En C, donde no existen las excepciones, pongamos un ejemplo de una raíz que toma número flotante positivo. Queremos controlar el error si la función recibe un número negativo. El usuario debe ser informado pero desde fuera de la función `raiz` ¿Cómo indicamos ese error?. Enumera dos opciones diferentes de diseñar, poniendo un ejemplo de código de cada una.

En el lenguaje C, al carecer de un sistema nativo de excepciones, el control de errores se basa tradicionalmente en el diseño de las firmas de las funciones. La primera opción consiste en utilizar el valor de retorno de la función para señalizar un estado anómalo. Se reserva un valor que no sea un resultado válido (como un número negativo en una raíz cuadrada) para indicar que ha ocurrido un error, obligando al llamador a verificar dicho valor mediante una estructura condicional.

La segunda opción habitual es el uso de paso por referencia mediante punteros. En este diseño, la función devuelve un código de estado (por ejemplo, un entero donde 0 es éxito y 1 es error) y el resultado real del cálculo se almacena en una dirección de memoria proporcionada por el usuario. Esto separa claramente el flujo de control del dato obtenido, permitiendo una gestión más robusta.

// Opción 1: Valor de retorno especial
float raiz_opcion1(float n) {
    if (n < 0) return -1.0; // Valor centinela para error
    return sqrt(n);
}

// Opción 2: Código de estado y puntero
int raiz_opcion2(float n, float *resultado) {
    if (n < 0) return 1; // Error
    *resultado = sqrt(n);
    return 0; // Éxito
}

## 2. Brevemente ¿Qué es una **"excepción"**? ¿Con qué objetivo las usa un programador cuando implementa funciones o cuando las llama?

Una excepción es un evento anómalo que ocurre durante la ejecución de un programa y que interrumpe el flujo normal de las instrucciones. Técnicamente, se trata de una señal que indica que una función no ha podido cumplir con su "contrato" o tarea encomendada debido a una condición inesperada, como una división por cero, la falta de un archivo o, como en el ejemplo anterior, un argumento inválido.

El programador utiliza las excepciones con el objetivo de separar la lógica de negocio (el "camino feliz" del código) del manejo de errores. Al implementar una función, se lanzan excepciones para delegar la responsabilidad de la decisión sobre qué hacer ante un error a quien tenga el contexto suficiente para resolverlo. Al llamar a funciones, el programador las usa para centralizar la gestión de fallos en bloques específicos, evitando que el código principal se llene de comprobaciones if-else constantes.


## 3. Reescribe el mismo ejemplo de raiz, pero en Java, metiendo ese método en una clase `Calculadora` y llama a dicho método desde el método `main`, mostrando cómo se puede controlar desde fuera.

En Java, el control se realiza mediante objetos que representan el error. A continuación se presenta la implementación donde la función valida el parámetro y, en caso de ser negativo, crea y lanza una instancia de IllegalArgumentException.

El método main actúa como el nivel superior que decide cómo reaccionar ante el fallo, envolviendo la llamada en un bloque protector que captura el objeto de error y muestra un mensaje informativo al usuario.

public class Calculadora {
    public double raiz(double n) {
        if (n < 0) {
            throw new IllegalArgumentException("No se puede calcular la raíz de un negativo.");
        }
        return Math.sqrt(n);
    }

    public static void main(String[] args) {
        Calculadora calc = new Calculadora();
        try {
            double res = calc.raiz(-5.0);
            System.out.println("Resultado: " + res);
        } catch (IllegalArgumentException e) {
            System.out.println("Error detectado: " + e.getMessage());
        }
    }
}

## 4. ¿Qué es **"lanzar"** una excepción? ¿Qué es **"controlar"** o **"capturar"** una excepción? ¿Qué es que se **"propague"** una excepción? ¿Qué le va ocurriendo a las funciones en la pila de llamadas por donde se va propagando la excepción? ¿Las funciones que no la controlan se reanudan después de alguna forma? Explica con el mismo ejemplo anterior en Java de la raíz cuadrada.

Lanzar una excepción (throw) es el acto de crear un objeto de error y entregarlo al entorno de ejecución de Java para notificar un fallo. Capturar o controlar (catch) es definir un bloque de código que se hace responsable de procesar ese objeto de error. La propagación es el viaje que realiza la excepción hacia atrás a través de la pila de llamadas (del método raiz al main, y de ahí a la JVM) buscando un bloque catch compatible.

Cuando una excepción se propaga, las funciones en la pila de llamadas se interrumpen abruptamente. No se ejecutan las líneas de código posteriores al punto donde ocurrió el error o donde se llamó a la función que falló. Es fundamental entender que las funciones que no controlan la excepción no se reanudan jamás; la ejecución de ese hilo de programa termina para esas funciones y sus marcos en la pila son eliminados (unwinding).

En el ejemplo de la raíz, si el main no tuviera el bloque try-catch, la excepción saldría de raiz, llegaría a main, causaría el cierre de main y finalmente la Máquina Virtual de Java (JVM) imprimiría el error por consola y detendría el programa. Si se captura, el programa continúa exclusivamente desde el punto inmediatamente posterior al bloque catch.


## 5. ¿Qué ventajas tiene frente a C, la **"propagación natural"** de las excepciones a través de la pila (*stack*) de llamadas?

La principal ventaja frente a C es que la propagación es automática y obligatoria. En C, si una función devuelve un código de error y el programador olvida deliberada o accidentalmente comprobar ese valor, el programa continúa ejecutándose en un estado inconsistente, lo que suele derivar en errores mucho más difíciles de depurar (como punteros nulos o corrupción de memoria).

En Java, la excepción "salta" por encima de los niveles que no saben qué hacer con ella hasta encontrar un manejador adecuado. Esto permite escribir un código mucho más limpio, ya que no es necesario que cada función intermedia en una jerarquía de llamadas tenga lógica de comprobación de errores; simplemente dejan pasar la excepción hacia arriba, asegurando que el error no será ignorado.

## 6. En orientación a objetos, ¿las excepciones suelen ser objetos? ¿Qué ventajas tiene esto en términos de encapsulación? ¿Podemos entonces crear excepciones personalizadas?

Efectivamente, en los lenguajes modernos de objetos, las excepciones son instancias de clases. Esto supone una gran ventaja de encapsulación, ya que una excepción no es solo un código numérico ciego (como un "Error 404"), sino un objeto que puede contener datos relevantes sobre el contexto del fallo: el estado de las variables en ese momento, el nombre del recurso que falló o una traza de la pila.

Gracias a la herencia, es posible crear excepciones personalizadas extendiendo las clases base de la jerarquía de excepciones de Java (como Exception o RuntimeException). Esto permite que el programador defina tipos de error específicos para su dominio de negocio, como una SaldoInsuficienteException, que puede llevar dentro un campo con el saldo actual, facilitando que el manejador tome decisiones más inteligentes.

## 7. En relación con las ventajas de la encapsulación, comparando el ejemplo en C con Java. ¿Qué **información esencial** lleva cualquier **objeto excepción** que es muy útil tener cuando se llega a un manejador?

A diferencia de un simple entero en C, cualquier objeto excepción en Java hereda de la clase Throwable, lo que le garantiza llevar consigo dos piezas de información críticas: el mensaje descriptivo (String message) y la traza de la pila (stack trace). El mensaje ofrece una explicación legible para humanos sobre la causa del error, mientras que la traza indica la secuencia exacta de llamadas a métodos que condujeron al fallo.

Además, al ser objetos, pueden almacenar el estado del sistema en el momento del error. Mientras que en C un -1 no dice nada sobre por qué falló la raíz, un objeto excepción en Java podría incluir el valor negativo exacto que causó el conflicto, permitiendo al manejador generar un log mucho más detallado y útil para la depuración técnica.

## 8. En Java, sobre el bloque **"try-catch"**, ¿se pueden tener más de un bloque `catch`? ¿cuántos bloques `catch` se ejecutan?

Es perfectamente posible, y a menudo recomendable, tener múltiples bloques catch asociados a un solo bloque try. Esto se hace para dar un tratamiento diferenciado a distintos tipos de errores que pueden surgir en un mismo fragmento de código (por ejemplo, un error de red frente a un error de formato de datos).

Sin embargo, solo se ejecuta un único bloque catch por cada excepción lanzada: el primero cuya clase coincida o sea una superclase del objeto lanzado. Por esta razón, el orden es crítico; se deben colocar siempre las excepciones más específicas (hijas) arriba y las más genéricas (padres) abajo, pues de lo contrario las genéricas "atraparían" todo e impedirían que el código específico se ejecute.

## 9. Si las excepciones producen rupturas en el código llamador, ¿cómo podemos garantizar que se ejecuta siempre finalmente un código necesario para cierre de ficheros, liberacion de recursos, antes de que continúe propagándose la excepción? Pon un ejemplo en Java con `finally`, tanto con `catch` como sin él.

Para garantizar la ejecución de tareas críticas de limpieza se utiliza el bloque finally. Este bloque se ejecuta siempre, sin importar si el código dentro del try terminó con éxito o si se lanzó una excepción (haya sido capturada o no). Es el lugar ideal para cerrar flujos de datos o liberar memoria, evitando fugas de recursos.

// Ejemplo con catch: manejo el error y cierro recurso
try {
    fichero.abrir();
    fichero.leer();
} catch (IOException e) {
    System.out.println("Error de lectura");
} finally {
    fichero.cerrar(); // Se ejecuta siempre
}

// Ejemplo sin catch: la excepción se propaga, pero el finally se ejecuta antes de irse
try {
    recurso.conectar();
    recurso.operar();
} finally {
    recurso.desconectar(); // Se garantiza la desconexión
}

## 10. En Java, el bloque `finally` puede ir sin `catch`? ¿Se ejecuta siempre tanto si ocurre como si no ocurre una excepción? ¿Y si hay un `return` en medio del `try`?

Sí, el bloque finally puede aparecer inmediatamente después de un try sin necesidad de un catch. Esta estructura se utiliza cuando no se desea manejar el error en ese punto (se prefiere que se propague hacia arriba), pero se tiene la responsabilidad de dejar el sistema en un estado limpio antes de abandonar el método.

Incluso si existe una sentencia return dentro del bloque try, Java garantiza que el bloque finally se ejecute antes de que el control regrese efectivamente al llamador. Es una de las reglas más estrictas del lenguaje para asegurar la integridad de los recursos; el finally tiene prioridad sobre la salida normal del método.

## 11. En Java, qué son las excepciones **"controladas"** y las **"no controladas"**? ¿Qué papel juega `RuntimeException`? Pon un ejemplo de excepciones típicas controladas y no controladas que incluso nosotros mismos podríamos usar. Haz dos listas con 3 o 4 ejemplos de situación donde se suele preferir una excepción controlada y donde se suele preferir una excepción no controlada.

Las excepciones controladas (checked) son aquellas que el compilador obliga a gestionar (mediante try-catch o throws); derivan de Exception. Las no controladas (unchecked) son aquellas que el compilador ignora en tiempo de desarrollo y derivan de RuntimeException. Estas últimas suelen representar errores de programación (bugs) o condiciones irrecuperables.

## 12. ¿Qué es y para qué se usa `throws`? ¿Por qué es alternativa a capturar una excepción controlada?

La palabra clave throws se utiliza en la firma de un método para declarar qué excepciones controladas puede llegar a lanzar durante su ejecución. Es, esencialmente, un aviso legal para quien llame a ese método: "Si me usas, ten en cuenta que puede ocurrir este error y tendrás que gestionarlo".

Es una alternativa al try-catch porque permite delegar la responsabilidad. En lugar de capturar la excepción localmente (cuando quizás no sabemos cómo solucionarla), usamos throws para dejar que la excepción suba por la pila de llamadas hasta un nivel superior que tenga una estrategia de recuperación clara.

## 13. Pon un ejemplo en Java de firma de método que incluya `throws`, de una función que abre un fichero pero que declara que no le interesa menejar la excepción de si el fichero no existe, sino que se propague hacia arriba. Eso sí, acuérdate del `finally`.

public void procesarConfiguracion(String ruta) throws FileNotFoundException {
    FileReader lector = null;
    try {
        lector = new FileReader(ruta);
        // ... realizar operaciones ...
    } finally {
        if (lector != null) {
            try { lector.close(); } catch (IOException e) { /* ignorar al cerrar */ }
        }
    }
}

Nótese que el método no tiene bloque catch para FileNotFoundException, por lo que si el archivo no existe, la excepción sale del método hacia el llamador, pero el bloque finally asegura que el recurso se intente cerrar si llegó a abrirse algo.

## 14. ¿Podemos poner en `throws` excepciones no controladas, como `RuntimeException`? ¿Debería el método llamador entonces poner `try-catch` en ese caso? ¿Qué sentido tendría?

Técnicamente, Java permite incluir excepciones no controladas en la cláusula throws, aunque no es obligatorio por ley del compilador. El método llamador no está obligado a poner un try-catch ni a volver a declararlas, ya que el compilador no realiza ninguna verificación sobre ellas.

El sentido de hacer esto es puramente documentativo. Sirve para informar explícitamente a otros programadores que usan nuestra API de que existe un riesgo común de esa excepción (por ejemplo, una IllegalArgumentException si pasan datos mal formados), fomentando que el llamador escriba un código más defensivo aunque no esté forzado a ello.

## 15. ¿Cuándo se recomienda usar excepciones controladas, como `IOException`, y cuándo no controladas como `IllegalArgumentException`? ¿Existen en todos los lenguajes ambas opciones? En los que sólo existe una opción, ¿cuál es la más habitual?

Se recomienda usar controladas para condiciones de error de las que el programa razonablemente puede recuperarse (errores externos). Las no controladas se reservan para errores de lógica del programador o precondiciones violadas, donde lo más sano suele ser que el hilo de ejecución falle para no operar con datos corruptos.

Java es casi único en su insistencia con las excepciones controladas. En otros lenguajes como C++, C#, Python o Kotlin, solo existen las excepciones no controladas (todas funcionan como RuntimeException). La tendencia moderna en la industria favorece este último enfoque, argumentando que las excepciones controladas ensucian demasiado el código y a menudo terminan siendo capturadas con bloques vacíos por pereza del programador.

## 16. ¿Tiene sentido lanzar excepciones dentro del `catch`? ¿Se puede relanzar la misma excepción capturada? ¿Cuándo tendría sentido hacer esto último? Pon ejemplos de ambos casos.

Lanzar excepciones dentro de un catch es una práctica común. Se puede relanzar la misma excepción si el bloque catch solo se ha usado para realizar una tarea secundaria (como registrar el error en un log) pero no para solucionarlo, permitiendo que la excepción original siga su camino hacia arriba.

También se puede lanzar una excepción nueva (traducción de excepciones) para cambiar el nivel de abstracción. Por ejemplo, si una base de datos falla, el catch captura la SQLException técnica y lanza una ErrorAccesoDatosException propia, mucho más comprensible para el resto de la aplicación.

// Caso 1: Relanzar la misma
catch (IOException e) {
    logger.error("Fallo al leer", e);
    throw e; 
}

// Caso 2: Lanzar una nueva (Traducción)
catch (SQLException e) {
    throw new ErrorDeNegocioException("La base de datos no responde.");
}

## 17. ¿En qué consiste que una excepción sea la **"causa"** de otra excepción? Pon un ejemplo en Java, donde capturemos una excepción de bajo nivel y la encapsulemos en otra personalizada de alto nivel. Cuando una excepción sale por pantalla y tiene una causa, ¿se ve?

El concepto de causa (o encadenamiento de excepciones) consiste en envolver una excepción original dentro de una nueva. Esto permite mantener la trazabilidad: informamos de un error de alto nivel pero conservamos el objeto del error original que lo provocó, evitando perder la información técnica de "qué pasó realmente al fondo".

Cuando una excepción con causa sale por pantalla, la JVM imprime la traza de la excepción principal y, justo debajo, una sección que comienza con "Caused by:", seguida de la traza de la excepción original. Esto es fundamental para el diagnóstico, ya que permite ver toda la genealogía del fallo desde la superficie hasta la raíz.


try {
    abrirConexionSocket();
} catch (IOException e) {
    // Encapsulamos la IOException como la causa de nuestra excepción personalizada
    throw new ConnectionException("Fallo crítico en la red", e);
}