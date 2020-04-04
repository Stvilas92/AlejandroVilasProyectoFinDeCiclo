# AlejandroVilasProyectoFinDeCiclo
Proyecto de fin de ciclo. Carrera de desarrollador de spring en OpenWebinars


## 30/03/2020

Empezado el primer curso de la carrera de _'Desarrollador de Spring'_, el cual se titula _'Curso de Spring Core'_
En el día de hoy he hecho la parte de introducción , que ha constado de los siguientes apartados.

###### * Presentación del curso.
###### * Introducción a Spring. 
¿Que és Spring?¿De donde surgió? Donde se explica un poco de la historia de la creación de Spring y en
que consiste Spring como tal.
###### * Instalación del entorno de trabajo.
En esta parte nos muestra como instalar _Spring Tool Suit_, para desarrollar aplicaciones 
con Spring. En esta parte he instalado el programa de una forma distinta a la que hace el profesor, puesto que el instala la herramienta acoplada a Eclipse. Mientras que por mi parte, para hacer esta experiencia útil hacia mi puesto de trabajo, he 
instalado la _Spring Tool Suit_ como una dependecia _Maven_ en _Intell IJ_ , que es la forma que lo hago en el trabajo y cumplen las mismas funciones.
###### * Estructura de una aplicación empresarial y patrones de diseño.
Aquí he aprendido lo que es una aplicación empresarial y como se suele estructurar. Además también he tocado lo que son los patrones de diseño, los tipos de patrones de diseño propuestos por el 
_Gang of Four_ (Viendolos por encima), y que patrones de diseño utilizaremos; inyección de dependecias, singleton, prototype y proxy.
###### * Inversión de control e inyección de dependencias.
He visto lo que es la inversión de control, principio de patrón de diseño cuyo objectivo es desacoplar objetos. También hemos visto como la inversión de control controla el flujo del programas para lograr sus 
fines. También nos habla de la inyección de dependencias como una forma de inversión de control, y nos pone algunos ejemplos
de como optimizar código en base a la inyección de dependencias.

## 31/03/2020

Hoy he visto los dos primeros videos de la parte de _Contenedor de inversion de control_ .

###### * Contenedor de IoC.
Donde se explica brevemente el esquema de un contenedor IoC, de que clases se compone y a través de cual se ejecuta (ApplicationContext, BeanFactory principalmente). También se explican los beans, que son y como se forman.

###### * Mi primer bean.
Aquí hemos creado un _"Hola mundo"_ de Spring.Hemos creado un proyecto maven que consta de un fichero XML en el cual se define la configuración de los beans.

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">


    <bean id="saludator" class="beans.Saludator"></bean>


</beans>
```

La clase a la que se hace referencia en el bean.
```
package beans;

public class Saludator {
    public void saludo(){
        System.out.println("Hola");
    }
}
```

Y finalmente el main, donde creamos ApplicationContext a partir de xml donde está la configuración de los beans, y cargamos el bean _saludator_ y ejecutamos la función que tiene, _saludo_ .

```
public class App {
    public static void main(String args[]){
        ApplicationContext applcationContext = new ClassPathXmlApplicationContext("beans.xml");

        Saludator s = (Saludator) applcationContext.getBean("saludator");
        s.saludo();
        ((ClassPathXmlApplicationContext)applcationContext).close();

    }
}
```

Y por último vemos la salida en consola.

```
"C:\Program Files\Java\jdk-11.0.1\bin\java.exe" "-javaagent:C:\Program Files\JetBrains\IntelliJ IDEA Community Edition 2018.2.5\lib\idea_rt.jar=53892:C:\Program Files\JetBrains\IntelliJ IDEA Community Edition 2018.2.5\bin" -Dfile.encoding=UTF-8 -classpath C:\Users\alexv\OneDrive\Escritorio\PrimerEjemplo\target\classes;C:\Users\alexv\.m2\repository\org\springframework\spring-context\5.2.5.RELEASE\spring-context-5.2.5.RELEASE.jar;C:\Users\alexv\.m2\repository\org\springframework\spring-aop\5.2.5.RELEASE\spring-aop-5.2.5.RELEASE.jar;C:\Users\alexv\.m2\repository\org\springframework\spring-beans\5.2.5.RELEASE\spring-beans-5.2.5.RELEASE.jar;C:\Users\alexv\.m2\repository\org\springframework\spring-core\5.2.5.RELEASE\spring-core-5.2.5.RELEASE.jar;C:\Users\alexv\.m2\repository\org\springframework\spring-jcl\5.2.5.RELEASE\spring-jcl-5.2.5.RELEASE.jar;C:\Users\alexv\.m2\repository\org\springframework\spring-expression\5.2.5.RELEASE\spring-expression-5.2.5.RELEASE.jar App
Hola

Process finished with exit code 0
```

## 02/04/2020

Hoy he visto los dos últimos videos de la parte de _Contenedor de inversion de control_ y los vídeos de la parte _Ámbito y ciclo de vida de 
un bean_

#### Inyección de dependecias : Vía setter vs vía constructor.
En este video nos explica dos formas de inyección de dependencias. Añade propiedades a los beans del proyecto en un xml, puede
hacerlo por el constructor o por setter. También nos da un ejemplo de como los veans pueden depender unos de otros.

Ejemplo de inyección vía setter:
*Clase*
```
public class Saludator {
    String mensaje;

    public void saludo(){
        System.out.println(mensaje != null ? mensaje : "Hola");
    }

    public void setMensaje(String mensaje) {
        this.mensaje = mensaje;
    }
}
```

*Xml*
```
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="saludator" class="beans.Saludator">
        <property name="mensaje" value="Hola a todos"></property>
    </bean>

</beans>
```

Ejemplo de inyección vía comstructor:
*Clase*
```
public class Saludator {
    String mensaje;

    public  Saludator(String mensaje) {
        this.mensaje = mensaje;
    }
    
    public void saludo(){
        System.out.println(mensaje != null ? mensaje : "Hola");
    }
}
```

*Xml*
```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="saludator" class="beans.Saludator"
        >
        <constructor-arg name="mensaje" value="Hola a todos"></constructor-arg>
    </bean>

</beans>
```


#### Inyección automática
En este apartado vemos como spring se encarga de buscar automáticamente las dependencias especificando por tipo, nombre o constructor.
También nos informa de las desventajas confictos y errores que se pueden tener debido a la inyección automática.

Aquí tenemos un ejemplo de inyección automática.

Ejemplo de inyección vía comstructor:
*Clase Saludator*
```
public class Saludator {
    String mensaje;

    public  Saludator(String mensaje) {
        this.mensaje = mensaje;
    }
    
    public void saludo(){
        System.out.println(mensaje != null ? mensaje : "Hola");
    }
}
```

*Clase EmailService*
```
public class EmailService {
    private Saludator saludator;

    public void enviarMail(String destinatario){
        System.out.println("Enviando mensaje a " + destinatario);
        System.out.println(saludator.mensaje);
    }

    public void setSaludator(Saludator saludator) {
        this.saludator = saludator;
    }
}
```

*Xml*
```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="saludator" class="beans.Saludator">
        <constructor-arg name="mensaje" value="Hola a todos"></constructor-arg>
    </bean>

    <bean id="email" class="beans.EmailService" autowire="byType"> </bean>
</beans>
```

Aquí finaliza la parte de inversión de control y empieza la de ámbito y ciclo de vida de un bean.

#### Singleton y prototype
Nos explica los ámbitos _Singleton_ y  _Prototype_ , el primero crea una instancia de clase común para todos los objetos y el segundo
hace una instancia para cada objeto en tiempo de ejecución.

#### Otros ámbitos
Aquí se comenta de forma breve otros ámbitos de bean que nos podemos encontrar, sobre todo en applicaciones web como son,
_Request_ , _Session_ y _Application_ .

#### Manejo de ciclo de vida de un bean
En este video nos explica como podemos alterar el comportamiento de un bean durante su ciclo de vida. Tenemos dos formas.
-Mediante las interfaces proporcionadas por Spring, _IntialazingBean_ y _DisposableBean_ , en las cuales sobreescribimos los métodos de 
cada una de ellas para poner nuestro código cuando el bean se inicializa y cuando se destruye.
-Mediante XML, aquí tenemos dos opciones:

* Especificar los metodos que cambian el comportamiento al inicio y destrucción en cada bean individualmente. Lo que significa que 
       el bean que querramos modificar deberá tener los métodos especificados.
* Especificar los metodos que cambian el comportamiento al inicio y destrucción para todos los beans. Lo que significa que todos los
       beans deberán tener los métodos especificados.
       
## 04/04/2020
Hoy he visto los videos de como configurar las dependencias, sus ámbitos y su ciclo de vida vistas en el día anterior, a través de distintas
herramientas. La primera sección trata sobre _Configuración basada en anotaciones_ y el segundo en _Configuración a través de java_ .
Empezamos con la configuración basada en anotaciones.
La confinguración en anotaciones, como se puede intuir por su título, se realiza mediante la escritura de tags ('@<Expresión>"') 
sobre los métodos, constructores o propiedades que le dan distintas caracteristicas a la clase en la que se encuentran al ejecutarse
el controlador de inversión. Los tags los vemos en los videos de la sección.

#### Uso de @Required
El tag Required nos indica que una dependencia de una propiedad se tiene que instanciar si o si, en el caso de que no se encuentre un bean 
que nos permite instanciarla, nos saltará una excepción. También está el uso del tag @Nullable, que nos indica lo contrario, que una 
dependencia puede ser nula.

#### Uso de autowired

Autowired nos proporciona que una dependencia sea instanciada mediante inyección automática vista anteriormente. Esto hace que no sea 
necesario declarar la inyección automática en el xml. La inyección automática realizada por tags siempre será _byType_ , y si no encuentra 
un bean del mismo tipo , saltará una excepción.
También podemos superponer el tag required o ponerlo entre parentesis después del tag autowired.
Ejemplo de superponer los tags.
```
@Autowired
@Required
private Saludator saludator;

```

Ejemplo de ponerlo entre paréntesis.
```
@Autowired(required = true)
private Saludator saludator;
```

Por último añadir que autowired tiene por defecto ``` required = false ```

#### Uso de primary y qualifier.

Primary nos indica que si hay varios  beans de un tipo, el que tenga la anotación primary será el que tenga prioridad si se hace una
inyección automática _byType_ . 
Qualifier nos permite seleccionar un bean específico de un tipo a la hora de hacer la inyección automática. En el tag qualifier tenemos
que poner entre parentesis al qur nos queremos referir.
Imaginemos que tenemos dos beans saludator _SaludatorEs_ y _SaludatorEn_ , para saludar en diferentes idiomas. Para saludar en español
tendríamos que poner lo siguiente:
```
@Autowired
@Qualyfier("saludatorEn")
private Saludator saludator;
```

#### Uso de post constructor  y post destroy.

Los dos tags nos permiten infuir en el ciclo de vida de un bean. Los tags se ponen encima de los métodos que se desean que se ejecuten
una vez creado el bean y una vez se destruya.

#### Uso de estereotipos.

*Escaneo de componentes*. En el XML podemos especificar un _package_ para que busque nuestros beans, y reducir así la busqueda por
todo el proyecto y agilizar el programa. Se hace mediante:
```
   <beans>
       <context:component-scan
              base-package="nombre-del-package"
       />
   </beans>
```

Encima de la declaración de cada clase que querramos que sea un componente(bean) debemos poner la anotación @Component, y así de forma
automática será declarada como bean y nos ahorraríamos mucha escritura en el xml a parte de declarar los beans de forma más simple
y sencilla.

Tipos de componentes.
-@Component es el bean general.
-@Service indica un tipo de lógica de negocio, orientado a las clases servicio.
-@Controller clase para recibir y procesar las peticiones recibidas.
-@Repository clase de acceso a datos.

Aquí empezamos la parte de configuración java.

#### Uso de @Configuration
Con la configuración mediante código java, estamos prescindiendo de el xml para realizar la configuración en java, mediante el tag
_configuration_ . El tag _configuration_ nos indica que en esa clase está la configuración del proyecto.
En esa clase se pueden crear los beans mediante el tag @Bean (El cual se explica en el siguiente video), o podemos especificar que escanee
componentes mediante el tag _@ComponentScan("basePackages")_ .

Ejemplo con beans:
```
   @Configuration
   public class appConfiguration{
       
       @Bean
       public Saludator saludator(){
              return new Saludator();
       }
   }
```

Ejemplo con component scan, suponiendo que el bean esté en el package _components_
```
   @Configuration
   @ComponentScan("components")
   public class appConfiguration{
   }
```

#### Uso de @Bean
Es una anotación a nivel de método que nos va a permitir definir un bean. El tipo de bean será el tipo que retornemos (No puede ser void)
y el nombre del bean será el nombre del método.
Los argumentos del método serán tratados como dependencias, por lo que el contenedor de inversión se encargará de inyectarlas.
Se pueden usar los tags vistos anteriormente para modificar el ámbito y el ciclo de vida de un bean (@Primary por ejemplo).

```
   @Configuration
   public class appConfiguration{
       
       @Bean(init)
       @Primary
       public Saludator saludator(){
              return new Saludator();
       }
   }
```

Uso de *@value* en tipos primitivos. Sirve para inyectar valores en tipos primitivos. Pueden ser valores de un archivo .properties
(Es el mas usado en mi entorno de trabajo), variables de entorno o valores por defecto.
Código
```
   @Component
   public class Saludator{
       
       @Value(${"mensaje"})
       private String mensajeHola;
       
       @Value("Adiós")
       private String mensajeAdios;
       
       public void Saluda(){
              System.out.print(mensajeHola + mensajeAdios);
       }
   }
```

Properties
```
mensaje=Adiós a todos
```

Para especificar el fichero de propiedades lo hacemos en la clase de configuración mediante _@PropertySource_ . En el siguiente ejemplo, mostramos donde está el fichero .properties , usando la palabra resevada classpath.
```
   @Configuration
   @PropertySource("classpath:/application.properties")
   public class appConfiguration{
       
       @Bean(init)
       @Primary
       public Saludator saludator(){
              return new Saludator();
       }
   }
```


