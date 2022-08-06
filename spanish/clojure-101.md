#Clojure 101

>"We should aim for simplicity because simplicity is a prerequisite for reliability"
— Edsger W. Dijkstra

¿Cuáles son las características de un desarrollador 10x? ¿ Cuál es el secreto para mantenernos entre el top de los mejores dentro de una industria tan cambiante y competitiva?. Ciertamente el mundo del software esta cambiando nuestro entorno, de una forma nunca antes vista; el software en sí, nos presenta una oportunidad para enfrentarnos a múltiples desafíos todos los días, poniendo a prueba nuestra imaginación y habilidades para resolver problemas.

Invertir en conocimiento siempre tendrá un gran retorno, dominar el arte del software y las herramientas que nos permiten moldear el mundo dónde vivimos, es parte esencial de nuestra carrera. Ahora bien, cuando se trata de resolver problemas con software, la mejor forma de hacerlo es siendo pragmáticos, diseñando soluciones simples y mantenibles, y para ello necesitamos de las mejores herramientas (PC, IDE, terminal, lenguaje de programación, servicios en la nube, editores de texto, etc).

Para mí, **Clojure** es una de estás herramientas, es tal vez el lenguaje con mayor énfasis en simplicidad y resolución de problemas de negocio, información, concurrencia, quitando del medio toda la *ceremonía y complejidad innecesaria*; Complejidad que no hace parte del dominio del negocio, sino de los mecanismos y reglas, impuestas por algunos lenguajes, que nos hacen caer en un mundo de abstracciones incorrectas y al final: en código sucio y verboso.


```java
// Hola mundo en Java
public class Main {
    public static void main(final String[] args) {
        System.out.println("Hello world!");
    }
}
```
```clojure
; Hola mundo en Clojure
(println "Hello world!")
```

Una de las características más importantes de Clojure es que: es un dialecto de Lisp; en esté artículo vamos a explorar uno de los elementos más importantes que componen el core de este tipo de lenguajes, la sintaxis. -pendientes funciones y data-.

##Sintaxis

###Formas

Todo el código de Clojure, mantiene una estructura uniforme : operaciones, y representaciones literales de estructuras de datos.

> `;` es el inicio de un comentario en clojure

```clojure
;(operador operando1 operando2)
(+ 1 2)
; => 3

(str "My fist article " "in DEV")
; => "My fist article in DEV"
```

Clojure mantiene consistencia en todas sus estructuras de datos, a diferencia de lenguajes como Java, donde la estructura cambia con respecto a la clase o datos que están siendo manipulados.

```java
System.out.println(1 + 2);
// => 3
System.out.println("My first article ".concat("in DEV"));
// => "My fist article in DEV"
```

###Flujos de control
Las estructuras de control en Clojure son consideradas expresiones, dado que producen valores, a diferencia de las sentencias que producen efectos secundarios y en ocasiones valores.

```clojure
(if (> 3 1)
  "3 es mayor que 1"
  "???")
; =>  "3 es mayor que 1"
```
En Java es posible si usamos el if -ternary-, el modo tradicional es una sentencia:

```java
public String isGreaterThan() {
    return (3 > 1) ? "3 es mayor que 1" :
            "???";
}
```

###Inmutabilidad

Clojure es un lenguaje Funcional donde las estructuras de datos son inmutables, permitiendo que la información sea compartida entre hilos y funciones sin problemas.

```clojure
; Información de dominio, Clojure es orientado a datos!!
(def person 
  {:first-name "Jorge"
   :last-name "Tovar"
   :age 30
   :occupation "Programmer"})
; Consulta
(:occupation person) 
; => "Programmer"
; La actualización sobre estructuras inmutables no modifica nada de la definición inicial
(assoc person :occupation "Devops")
; => {:first-name "Jorge", :last-name "Tovar", :age 30, :occupation  "Devops"}
; Consulta a la estructura original
person
; => {:first-name "Jorge", :last-name "Tovar", :age 30, :occupation "Programmer"}
```

###Valores con nombre
Clojure es un lenguaje donde las funciones son elementos de primera clase, además cuenta con el REPL, para que puedas probar todas tus ideas y tener feedback de forma inmediata.

![Alt Text](https://dev-to-uploads.s3.amazonaws.com/i/22x42ibk279loegjjqxb.png)

Clojure es orientada a datos, por lo tanto el lenguaje promueve el uso de valores inmutables y funciones; a diferencia de los lenguajes tradicionales OOP, donde la asignación y modificación de variables es el default.


```clojure
; En Clojure lo normal es tener funciones que te retornan valores 
(def x (user-message :simplicity)); => "Clojure loves simplicity"
(def y (user-message :other)); => "Clojure loves functional programming!"
```

En Python lo más común es hacer lo mismo haciendo uso de variables:

```python
def user_message(message):
    if message == "simplicity":
        x = "Python loves " + "dynamic types"
    else:
        x = "Python " + "loves OOP"
    return x
```
###Conclusión
Clojure es un lenguaje que centra su filosofía en simplicidad, funciones puras, libertad de enfoque y la expresividad del código, elementos fundamentales para crear software robusto.

El paradigma funcional per se, es un mundo interesante, que permite una forma más limpia de crear sistemas, y si al igual que yo y muchos de los desarrolladores que conozco, eres apasionado por aprender cosas nuevas y útiles, te recomiendo esté lenguaje, un punto de vista totalmente diferente de como resolver los problemas - minimizando la complejidad y las líneas de código -.

### Referencias 
[Clojure for the brave](https://www.braveclojure.com/introduction/)
