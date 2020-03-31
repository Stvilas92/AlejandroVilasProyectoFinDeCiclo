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
