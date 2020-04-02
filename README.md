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
