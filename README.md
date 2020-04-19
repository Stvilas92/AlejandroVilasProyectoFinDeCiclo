# Alejandro Vilas Proyecto Fin De Ciclo
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



## 08/04/2020

El último video de este curso trata de hacer un miniproyecto en el cual usemos los conceptos aprendids en este curso. En cada uno de los 
va explicando por partes como hacer el proyecto.

#### Introducción a MovieAdvisor
Movie Advisor es el nombre del proyecto. En este video nos cuenta la utilidad y estructura del proyecto. El proyecto consiste en leer 
películas de un archivo CSV. Para hacerlo, el usuario dispndrá de una serie de comandos para buscar las películas desde terminal.
Las busquedas de peliculas será con filtros , id, genero etc.

#### Creación del proyecto y modelo de datos
Será un proyecto Maven en el que utilizaremos Java 8 y se incluirá el CSV en la carpeta resources. Se crea la clase _Pelicula_ (Aunque en el
curso se llama _Fila_, me pareció mas intuitivo Película). Se trata de una clase simple, solo con propiedades, getters y setters:
```

public class Pelicula {
    private int id;
    private Date fecha;
    private int year;
    private List<String> generos;
    
    Getters
    ...
    
    Setters
    ...
}

```

#### Repositorio y acceso a datos (Parte 1)

Creamos la configuración del proyecto. Será una configuración mixta entre configuración java y anotaciones.
Creamos una interfaz _PeliculasDAO_ para todas las peliculas que la implementen realicen ciertas operaciones (Buscar por título etc).
Creamos la clase _PeliculaDaoImp_, que utilizaremos para acceder a los datos en memoria de las películas. Tendrá la una lista de películas e
implementará la interfaz _PeliculasDAO_. La clase _PeliculaDAOImp_ será un bean, concretamente un repositorio.
Creamos otra clase _ReadUtil_, la cual se encargará de cargar del fichero CSV todas las películas. Solo contará con un metodo estático en 
el cual cargamos las películas de CSV.

Clase _ReadUtil_
```
public class UtilFilmFileReader {

	public static List<Pelicula> readFile(final String path, final String separator, final String listSeparator) {
		List<Pelicula> result = new ArrayList<>();


		try {
			// @formatter:off
			result = Files
						.lines(Paths.get(ResourceUtils.getFile(path).toURI()))
						.skip(1)
						.map(line -> {
							String[] values = line.split(separator);
							return new Film(Long.parseLong(values[0]), values[1], values[2],
											Arrays.asList(values[3].split(listSeparator)));
					}).collect(Collectors.toList());
 			// @formatter:on


		} catch (Exception e) {
			System.err.println("Error de lectura del fichero de datos: imdb_data");
			System.exit(-1);
		}

		return result;

	}

}
```

#### Repositorio y acceso a datos (Parte 2)

Crearemos un fichero .properties donde estarán las propiedades path,separator y list_separator, que utilizaremos para leer del CSV.
```
file.path=classpath:imdb_data.csv
file.csv.separator=;
file.csv.list_separator=,
```
En la configuración del proyecto tenemos que indicarle donde se encuentra el fichero .properties  con ``` @PropertySource("classpath:/movieadvisor.properties")```
También usaremos la clase de conficguración para cargar las propiedades y poner los metodos getter para acceder a ellas.

```
@Configuration
@ComponentScan(basePackages="com.openwebinars.movieadvisor")
@PropertySource("classpath:/movieadvisor.properties")
public class AppConfig {
	
	@Value("${file.path}")
	public String file;
	
	@Value("${file.csv.separator}")
	public String separator;
	
	@Value("${file.csv.list_separator}")
	public String listSeparator;
	
	public String getFile() {
		return file;
	}
	
	public String getSeparator() {
		return separator;
	}
	
	public String getListSeparator() {
		return listSeparator;
	}
	
}
```

Usaremos la clase configuración para cargar las propiedades en la clase _PeliculaDAOImp_ , en la misma, al ser un bean, accedemos al ciclo de 
vida y en la función del bean cargamos el csv. Ya que tenemos las peliculas, sobreescribiremos los métodos de la interfaz _PeliculasDAOImp_ 
para que realice las operaciones de la interfaz _PeliculasDao_

```
@Repository
public class PeliculaDAOImp implements PeliculasDao {
	
	public List<Pelicula> peliculas = new ArrayList<>();
	
	@Autowired
	private AppConfig appConfig;
	
	@PostConstruct
	public void init() {
		peliculas = UtilFilmFileReader.readFile(appConfig.getFile(), appConfig.getSeparator(), appConfig.getListSeparator());
	}

	public Pelicula findById(long id) {
		Optional<Pelicula> result = peliculas.stream().filter(f -> f.getId() == id).findFirst();
		return result.orElse(null);
	}

	public Collection<Pelicula> findAll() {		
		return peliculas;
	}

	public void insert(Pelicula pelicula) {
		peliculas.add(pelicula);
	}

	public void edit(Pelicula pelicula) {
		int index = getIndexOf(film.getId());
		if (index != -1)
			peliculas.set(index, film);
	}

	public void delete(long id) {
		int index = getIndexOf(id);
		if (index != -1)
			peliculas.remove(index);

	}
	
	private int getIndexOf(long id) {
		boolean encontrado = false;
		int index = 0;
		
		while (!encontrado && index < peliculas.size()) {
			if (peliculas.get(index).getId() == id)
				encontrado = true;
			else
				index++;
		}
		
		return (encontrado) ? index : -1;
	}

}
```

#### Servicios

Se encargarán de procesar las peticiones del usuario para mostrarle los datos que el mismo desee.
Se usarán Predicados, los cuales usaremos para encadenar métodos en caso de que el usuario nos pida varios tipos de datos.
Para trabajar con los conjuntos de datos crearemos una interfaz _PeliculaQueryService_ cuyos métodos se sobrescribirán en una
nueva clase _PeliculaQueryServiceImp_ la cual usaremos para buscar peliculas mediante busquedas con filtros (Por id, fecha etc).
La clase será un bean , en concreto un service.
```

@Service
public class PeliculaQueryServiceImpl implements PeliculaQueryService{
	
	@Autowired
	PeliculaDao dao;

	private Predicate<Pelicula> predicate;
	
	@PostConstruct
	public void init() {
		predicate = null;
	}

	public Collection<Pelicula> exec() {
		
		// @formatter:off
		return dao.findAll()
				.stream()
				.filter(predicate)
				.collect(Collectors.toList()); 
		// @formatter:on

	}

	public PeliculaQueryServiceImpl anyGenre(String... genres) {
		Predicate<Pelicula> pAnyGenre = (pelicula -> Arrays.stream(genres).anyMatch(film.getGenres()::contains));
		predicate = (predicate == null) ? pAnyGenre : predicate.and(pAnyGenre);
		return this;
	}

	public PeliculaQueryServiceImpl allGenres(String... genres) {
		Predicate<Pelicula> pAllGenres = (pelicula -> Arrays.stream(genres).allMatch(film.getGenres()::contains));
		predicate = (predicate == null) ? pAllGenres : predicate.and(pAllGenres);
		return this;
	}

	public PeliculaQueryServiceImpl year(String year) {
		Predicate<Pelicula> pYear = (pelicula -> pelicula.getYear().equalsIgnoreCase(year));
		predicate = (predicate == null) ? pYear : predicate.and(pYear);
		return this;
	}

	public PeliculaQueryServiceImpl betweenYears(String from, String to) {
		Predicate<Pelicula> pBetweenYears = (pelicula -> {
			LocalDate fromYear = LocalDate.of(Integer.parseInt(from), 1, 1);
			LocalDate toYear = LocalDate.of(Integer.parseInt(to), 1, 3);
			LocalDate filmYear = LocalDate.of(Integer.parseInt(film.getYear()), 1, 2);

			return peliculaYear.isAfter(fromYear) && peliculaYear.isBefore(toYear);
		});
		
		predicate = (predicate == null) ? pBetweenYears : predicate.and(pBetweenYears);

		return this;
	}

	public PeliculaQueryServiceImpl titleContains(String title) {
		Predicate<Pelicula> pTitleContains  = (pelicula -> pelicula.getTitle().toLowerCase().contains(title.toLowerCase()));
		predicate = (predicate == null) ? pTitleContains : predicate.and(pTitleContains);
		
		return this;
	}

}
```
Crearemos por último la clase _PeliculasService_ donde montaremos todas las consultas sobre los datos que se pueden hacer desde _PeliculaDaOImp_ y _PeliculaQueryServiceImpl_ . También será un bean servicio.


```
@Service
public class PeliculasService {

	@Autowired
	PeliculaDao peliculaDao;

	@Autowired
	PeliculaQueryServiceImp queryService;

	public Collection<String> findAllGenres() {
		List<String> result = null;

		// @formatter:off
		result = peliculaDao.findAll()
				.stream()
				.map(f -> f.getGenres())
				.flatMap(lista -> lista.stream())
				.distinct()
				.sorted()
				.collect(Collectors.toList());

		// @formatter:on

		return result;
	}

	public Collection<Pelicula> findByAnyGenre(String... genres) {

		return queryService.anyGenre(genres).exec();

	}

	public Collection<Pelicula> findByAllGenres(String... genres) {
		return queryService.allGenres(genres).exec();
	}

	public Collection<Pelicula> findByYear(String year) {
		return queryService.year(year).exec();
	}

	public Collection<Pelicula> findBetweenYears(String from, String to) {
		return queryService.betweenYears(from, to).exec();
	}

	public Collection<Pelicula> findByTitleContains(String title) {
		return queryService.titleContains(title).exec();
	}

	public Collection<Pelicula> findAll() {
		return peliculaDao.findAll();
	}
}
```


#### Ejecución de la app

Aquí crearemos los elementos que faltan.
Creamos la clase _MovieAdvisorApp_ desde la que lanzamos el programa.
Creamos la clase _MovieAdvisorHelp_ en la cual cargaremos menssajes de ayuda y error que se mostrarán al usuario. Los mensajes los 
cargaremos de un fichero txt _hel.txt_ el cual contiene todos los mensajes de ayuda que se le mostrarán al usuario. La clase será un bean general el cual señalaremos con el tag Component

```
@Component
public class MovieAdvisorHelp {

	private String help;

	@PostConstruct
	public void init() {
		try {
			// @formatter:off
			help = Files
					.lines(Paths.get(ResourceUtils.getFile("classpath:ayuda.txt").toURI()))
					.collect(Collectors.joining("\n")); 
			// @formatter:on

		} catch (IOException e) {
			System.err.println("Error cargando el texto de ayuda");
			System.exit(-1);
		}
	}

	public String getHelp() {
		return help;
	}

}

```
Creamos la clase MovieAdvisorRunApp, la cual actuaría de manera parecida a un _controller_ y es donde estará la lógica del programa. 
Para interactuar con el usuario se proporcionarán una serie de comandos que usará para acceder a las differentes querys del servicio
_PeliculaService_ . En mi opinión el código para la interfaz es un poco lioso y creo que hubiese sido más sencillo utilizar un _controller_
normal. También será un component, el cual instanciaremos la clase _MovieAdvisorApp_.

```
@Component
public class MovieAdvisorRunApp {

	@Autowired
	PeliculaService peliculaService;
	
	@Autowired
	PeliculaQueryServiceImp peliculaQueryService;

	@Autowired
	MovieAdvisorHelp help;

	public void run(String[] args) {

		if (args.length < 1) {
			System.out.println("Error de sintaxis");
			System.out.println(help.getHelp());
		} else if (args.length == 1) {
			switch (args[0].toLowerCase()) {
			case "-lg":
				peliculaService.findAllGenres().forEach(System.out::println);
				break;
			case "-h":
				System.out.println(help.getHelp());
				break;
			default:
				System.out.println("Error de sintaxis");
				System.out.println(help.getHelp());

			}
		} else if (args.length % 2 != 0) {
			System.out.println("Error de sintaxis");
			System.out.println(help.getHelp());
		} else if (args.length > 8) {
			System.out.println("Error de sintaxis");
			System.out.println(help.getHelp());
		} else {
			List<String[]> argumentos = new ArrayList<>();

			for (int i = 0; i < args.length; i += 2) {
				argumentos.add(new String[] { args[i], args[i + 1] });
			}
			
			boolean error = false;

			for (String[] argumento : argumentos) {
				switch (argumento[0].toLowerCase()) {
				case "-ag":
					peliculaQueryService.anyGenre(argumento[1].split(","));
					break;
				case "-tg":
					peliculaQueryService.allGenres(argumento[1].split(","));
					break;
				case "-y":
					peliculaQueryService.year(argumento[1]);
					break;
				case "-b":
					String[] years = argumento[1].split(",");
					peliculaQueryService.betweenYears(years[0], years[1]);
					break;
				case "-t":
					peliculaQueryService.titleContains(argumento[1]);
					break;
				default: error = true;
						 System.out.println("Error de sintaxis");
						 System.out.println(help.getHelp());
				}

			}
			
			if (!error) {
				Collection<Pelicula> result = peliculaQueryService.exec();
				System.out.printf("%s\t%-50s\t%s\t%s\n","ID","Título", "Año", "Géneros");
				if (result != null) {
					result.forEach(f -> System.out.printf("%s\t%-50s\t%s\t%s\n", 
							f.getId(), f.getTitle(), f.getYear(), 
							f.getGenres().stream().collect(Collectors.joining(", "))));
				} else {
					System.out.println("No hay películas que cumplan esos criterios. Lo sentimos");
				}
			}
		}

	}

}
```


Por último nos queda ejecutar el método run creado anteriormente en la clase _MovieAdvisorRunApp_ en la clase _MovieAdvisorApp_ que 
se encargará de lanzar la aplicación.
```
public class MovieAdvisorApp {

	public static void main(String[] args) {
		
		ApplicationContext appContext = new AnnotationConfigApplicationContext(AppConfig.class);
		
		MovieAdvisorRunApp runApp = appContext.getBean(MovieAdvisorRunApp.class);
		
		runApp.run(args);
		
		((AnnotationConfigApplicationContext) appContext).close();

	}
}
```
Y ya estaría la aplicación montada. Muy importante acordarse de especificar la versión de java 8 , tanto para java como para java compiler.


## 09/04/2020
 
 Comienzo con el segundo curso de la carrera _Spring Boot y Spring MVC_ . Este curso lo había empezado en octubre y estoy en el 25% del curso que es donde lo retomaré haciendo un breve resumen de lo que he visto
 
 ### Spring Boot
 
 En un conjunto de videos se nos explica que es _Spring Boot_ y cuales son sus características. _Spring Boot_ es una herramienta para 
 crear con mayor facilidad aplicaciones basadas en el framework spring, sin crear la configuración en xml y configurando automaticamente
 librerías y dependencias.
 
 ### Spring Web MVC
 
 De esta parte me he visto los cinco primeros videos en los cuales se explica:
 -Como funciona una aplicación web. Peticiones, metodos HTTP etc.
 -Se explican algunos patrones de diseño, haciendo especial hincapié en el modelo vista controlador.
 -Estructura de un proyecto web. Modelos, controladores, vistas, servicios etc.
 -Conceptos de Java EE. Servlets , web.xml, filters, contextos y algunos beans especiales como _HandlerMapping_ .
 -Que es un controlador y que estructura y características tiene.
 
 A partir de aquí empiezo con los videos que no he visto de este curso.
 
 #### Las vistas
 
 Spring MVC es completamente independiente de la vista, podemos cambiar la tecnología de la vista independientemente de lo que tengamos en
 los otros componentes. En este curso se usará la tecnología _Theymeleaf_ para las vistas, en mi trabajo solemos usar React.
 Theymeleaf es una plantilla que usa código HMTL + CSS + JavaScript.
 Spring Boot se encarga de configurar _Theymeleaf_ a traves de los beans _TemplateEngine_, _TemplateResolver_ y _ViewResolver_ . También
 podemos cambiar la configuración en el fichero _application.properties_ , donde se alojan las propiedades del proyecto.
 
 
 #### @RequestParam
 Nos explica el parámetro _@RequestParam_, que es igual al mismo en la tecnología _JavaRX_ visto en Acceso a datos y se usa de la misma
 forma poniendo el tag el un argumento de un método HTTP.
 ```
 public Response recibeDatos(@RequestParam Datos datos){}
 ```
 Por defecto , la propiedad _required_ de el parámetro es _true_. Para cambiarla, debemos poner parentesis después del tag poniendo
 ``` (required = false) ``` .
 
 #### @PathVariable
 Nos explica el parámetro _@PathVariable, que al igual que_@RequestParam_ es igual al _athVariable_ visto en Acceso a datos y se usa de la
 misma forma.
 
 #### Recursos Estáticos
 
 Por recursos estáticos entedemos ficheros HTML, imágenes fichero etc.  Se almacenarán el _%ClassPath%/src/resurces/static_ y serán mapeados
 automáticamente por Spring Boot. Si deseaos usarlos en HTML por ejemplo, no tendrmos más que escribir ``` src/<nombre_del_recurso>``` .
 
 #### WebJars
 
 Forma de integrar librerías externas en nuestro proyecto utilizando maven. Todas las dependencias de Maven serán mapeadas a la ruta
 _/webjars/**_ . Esto nos permite integrar tecnologías como boostrap e ir acutalizando sus versiones a través del tiempo de forma
 fácil y sencilla.
 
 ## 10/04/2020
 
  #### Formularios
  Se harán a traves De _Thyemeleaf_ , Spring tratará los campos del formulario con un bean llamado _CommandObject_ , que debe tener tantos
  objetos como campos el formulario. La acción del formulario será llamar al controlador encargado de recibir los dats del formulario y
  llamar a los servcios asociados que ejecuten la lógica de negocio correspondiente.
  En los inputs deberos referenciar a la propiedad del objeto _CommandObject_ que vayamos a rellenar.
  Ejemplo con empleados:
  ```
  <form action="#"
  	th:action="@{/empleado/new/submit}"
	th:object="${/empleadoForm}"
	method="post">
  <input type="text" id="idObject" th:field=*{campo}
  ```
  
  #### Formularios de edición
  A diferencia del apartado anterior, este formulario nos permite modificar datos. Para ello, el objeto _CommandObject_ no debe ir vacío
  , es decir , sus propiedades deben tener valores. La forma de ejecución es igual a la anterior , solo cambiarán las URLs, puesto que
  en el controlador tiene que haber otro método para modificar datos que lleven a una lógica de negocio distinta a la de crear
  datos.
  
## 14/04/2020

#### Validación de datos
Se pueden validar los datos que recibimos de un formulario mediante tags de validación . Los más comunes son:
- @NotNull El valor no puede ser nulo.
- @Email El valor tiene que ser un email.
- @Max mayor o igual que el valor especificado.
- @Min menor o igual que el valor especificado.
- @NotEmpty, aplicada a colecciones, no debe estar vacía.
- @Size aplicada a colecciones, no debe tener un tamaño especificado.

```
public class Empleado{
	@Min(0)
	private id;
	
	@NotNull
	private dni
}
```

En el controller donde se insertan los datos del formulario, tenemos que especificar eque el objeto que recibimos debe ser válido mediante
el tag _@Valid_

```
@PostMapping("empleado/new")
public void addEmpleado(@Valid @ModelAtribute("empleadoForm") Empleado empleado, BindingResult bindingResult ){
	if(bindingResult.hasErrors()){
		return "form";
	}
	servicio.add(empleado);
}
```
Se pueden personalizar los mensajes por defecto que la aplicación muestra si hay error poniendo una propiedad clave=valor dentro del
tag de validación.

´´´
public class Empleado{
	@Min(value = 0, message="El campo id de tener un valor mas alto que 0")
	private id;
	
	@NotNull(message="Tiene que existir un DNI")
	private dni
}
´´´
También se pueden hacer configurables los mensajes mediante el archivo .properties . Para ello debemos insertar dos nuevos beans en la configuración del proyecto. En el siguiente ejemplo vamos a suponer que los mensajes estarían en el archivo eeror.properties.

```
@Configuration
public class MyConfig {
	
	@Bean
	public MessageSource messageResource() {
		ReloadableResourceBundleMessageSource messageSource =
				new ReloadableResourceBundleMessageSource();
		
		messageSource.setBasename("classpath:errors");
		messageSource.setDefaultEncoding("UTF-8");
		
		
		return messageSource;
	}
	
	@Bean
	public LocalValidatorFactoryBean getValidator() {
		LocalValidatorFactoryBean bean = new LocalValidatorFactoryBean();
		bean.setValidationMessageSource(messageResource());
		return bean;
	}
}

```

El archivo errors.properties
```
errors.id="Id mal introducida, debe ser mayor que 0"
errors.dni="Debes insertar un DNI"
```

El objeto empleados

´´´
public class Empleado{
	@Min(value = 0, message="${errors.id}")
	private id;
	
	@NotNull(message=${errors.dni})
	private dni
}
´´´

#### Subida de ficheros
Para ello vamos a utilizar mensaje multiparte, que no deja de ser un mensaje con diferentes partes en formatos distintos, con lo que 
podemos rellenar los campos del formulario y subir ficheros al mismo tiempo. Debe existir en el formulario un input tipo _file_ .
Para recibir el mensage multiparte con el fichero del formulario debemos usar la clase _Multipart_.

´´´
public class Empleado{
	@Min(value = 0, message="${errors.id}")
	private id;
	
	@NotNull
	Image image;
}
´´´
El controller
```
@PostMapping("empleado/new")
public void addEmpleado(@Valid @ModelAtribute("empleadoForm") Empleado empleado, BindingResult bindingResult,
	@RequestParam("file")Multipart file
	if(bindingResult.hasErrors()){
		return "form";
	}
	if(!file.isEmpty()){
		//Lógica de almacenamiento de fichero
	}
	servicio.add(empleado);
}
```

#### Servicio de almacenamiento de ficheros
La idea es hacer un servicio de un almacenamiento de ficheros que funcienorá por lo menos en nuestro local, haciendolo configurable 
con diferentes beans y porperties. La implementación se hace en el siguiente video
El servicio se usará utlizando la siguiente interfaz
```
public interface StorageService {

    void init();

    String store(MultipartFile file, long id);

    Stream<Path> loadAll();

    Path load(String filename);

    Resource loadAsResource(String filename);

    void deleteAll();

}
```

OpenWebinars nos proporciona el servicio que procesa los ficheros utilizando la anterior interfaz anteriormente creada. Podemos ver
como el path del proyecto es configurable a traves de un archivo .properties .
```
@Service
public class FileSystemStorageService implements StorageService{

    private final Path rootLocation;
		
    @Autowired
    public FileSystemStorageService(StorageProperties properties) {
        this.rootLocation = Paths.get(properties.getLocation());
    }
    
    @Override
    public String store(MultipartFile file, long id) {
        String filename = StringUtils.cleanPath(file.getOriginalFilename());
        String extension = StringUtils.getFilenameExtension(filename);
        String storedFilename = Long.toString(id) + "." + extension;
        try {
            if (file.isEmpty()) {
                throw new StorageException("Failed to store empty file " + filename);
            }
            if (filename.contains("..")) {
                throw new StorageException(
                        "Cannot store file with relative path outside current directory "
                                + filename);
            }
            try (InputStream inputStream = file.getInputStream()) {
                Files.copy(inputStream, this.rootLocation.resolve(storedFilename),
                    StandardCopyOption.REPLACE_EXISTING);
                return storedFilename;
            }
        }
        catch (IOException e) {
            throw new StorageException("Failed to store file " + filename, e);
        }
        
    }
    
    @Override
    public Stream<Path> loadAll() {
        try {
            return Files.walk(this.rootLocation, 1)
                .filter(path -> !path.equals(this.rootLocation))
                .map(this.rootLocation::relativize);
        }
        catch (IOException e) {
            throw new StorageException("Failed to read stored files", e);
        }

    }

    @Override
    public Path load(String filename) {
        return rootLocation.resolve(filename);
    }
    
    @Override
    public Resource loadAsResource(String filename) {
        try {
            Path file = load(filename);
            Resource resource = new UrlResource(file.toUri());
            if (resource.exists() || resource.isReadable()) {
                return resource;
            }
            else {
                throw new StorageFileNotFoundException(
                        "Could not read file: " + filename);

            }
        }
        catch (MalformedURLException e) {
            throw new StorageFileNotFoundException("Could not read file: " + filename, e);
        }
    }
    
    @Override
    public void deleteAll() {
        FileSystemUtils.deleteRecursively(rootLocation.toFile());
    }
    
    @Override
    public void init() {
        try {
            Files.createDirectories(rootLocation);
        }
        catch (IOException e) {
            throw new StorageException("Could not initialize storage", e);
        }
    }

}
```

#### Implementación de la subidad de ficheros en nuestro proyecto.
Cada vez que creemos un empleado hay que almacenarlo en el fichero y guardarlo en el fichero.
Utilizaremos el método serveFile, que nos devolverá el fichero como respuesta a una petición. 
Añadiremos un método en el controller para cargar el fichero y que además nos reponda un _Response_ HTTP que nos indique
si la petición se ha procesado correctamente o se han producido errores. el ```.+```en el filename significa que el valor sera ``` 
valor.valor```
```
@GetMapping("/files/{filename:.+}")
	@ResponseBody
	public ResponseEntity<Resource> serveFile(@PathVariable String filename) {
		Resource file = storageService.loadAsResource(filename);
		return ResponseEntity.ok().body(file);
	}
```
Una vez tenemos cargado el fichero nos deberemos ir a modificar os métodos que utilizabamos para añadir empleados en el controller

```
@Controller
public class EmpleadoController {

	@Autowired
	private EmpleadoService servicio;
	
	@Autowired
	private StorageService storageService;

	@GetMapping({ "/", "/empleado/list" })
	public String listado(Model model) {
		model.addAttribute("listaEmpleados", servicio.findAll());
		return "list";
	}

	@GetMapping("/empleado/new")
	public String nuevoEmpleadoForm(Model model) {
		model.addAttribute("empleadoForm", new Empleado());
		return "form";
	}

	@PostMapping("/empleado/new/submit")
	public String nuevoEmpleadoSubmit(@Valid @ModelAttribute("empleadoForm") Empleado nuevoEmpleado,
			BindingResult bindingResult, @RequestParam("file") MultipartFile file) {

		if (bindingResult.hasErrors()) {			
			return "form";	
			
		} else {
			if (!file.isEmpty()) {
				String avatar = storageService.store(file, nuevoEmpleado.getId());
				nuevoEmpleado.setImagen(MvcUriComponentsBuilder
						.fromMethodName(EmpleadoController.class, "serveFile", avatar).build().toUriString());
			}
			servicio.add(nuevoEmpleado);
			return "redirect:/empleado/list";
		}
	}

	@GetMapping("/empleado/edit/{id}")
	public String editarEmpleadoForm(@PathVariable long id, Model model) {
		Empleado empleado = servicio.findById(id);
		if (empleado != null) {
			model.addAttribute("empleadoForm", empleado);
			return "form";
		} else
			return "redirect:/empleado/new";
	}

	@PostMapping("/empleado/edit/submit")
	public String editarEmpleadoSubmit(@Valid @ModelAttribute("empleadoForm") Empleado nuevoEmpleado,
			BindingResult bindingResult) {

		if (bindingResult.hasErrors()) {			
			return "form";	
		} else {
			servicio.edit(nuevoEmpleado);
			return "redirect:/empleado/list";
		}
	}
	
	@GetMapping("/files/{filename:.+}")
	@ResponseBody
	public ResponseEntity<Resource> serveFile(@PathVariable String filename) {
		Resource file = storageService.loadAsResource(filename);
		return ResponseEntity.ok().body(file);
	}

}
```
En el método _MvcUriComponentsBuilder.fromMethodName_ motaríamos una uri por partes, utilizando el método al que hacemos referencia
como argumneto, en este caso, _serveFile_ .


## 15/04/2020

#### Aplicación de web segura con Spring Security
Lo primero es importar en maven la dependencia de Spring-Starter-Security.
```
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-security</artifactId>
    <version>2.2.6.RELEASE</version>
</dependency>
```
La seguridad responde a quién es cada usuario y que permisos tiene. Autenticación y autorización.
Crearemos una clase llamada _SecurityConfig_ , que tendrá los tags @Configuration y @EnableWebSecurity y debe de heredar de 
WeSecurityAdapter.
Autenticación, se hará en memoria de forma muy sencilla. Lo hacemos sobreescribiendo el método configure. Vamos a hacer que para 
hacer los roles de administrador de una aplicación, el usuario se tenga que identificarse con el nombre _admin_ y con la contraseña 
_admin_. Además la contrseña no estará encriptada.

```
@Override
protected void configure(AutenticationManagerBuilder autenticationManagerBuilder) throws Exception{
	auth.
		inMemotyApplication().
		passwordEncoder(NoOpPasswordEncoder.getInstance()).
		withUser("Admin").
		password("Admin").
		roles("ADMIN");
}
```

Con este código:
* Todas las peticiones requieren autenticación.
* Generación automática de un login.
* Previenen ataques CSRF entre otros
* Se integran los métodos http de la clase server.


Autorización , es my simple, simplemente hay que indicar que peticiones requieren de autorización y cuales no.

```
@Override
protected void configure(AutenticationManagerBuilder autenticationManagerBuilder) throws Exception{
	http.
		authorizeRequests().
		anyRequest().
		authenticated();
}
```

#### Implementación del login con spring security

Vamos a autorizar con un login a diferentes usuarios un aplicación web. Lo primero es la parte visual, que utilizaremos una plantilla
de login de html, para crear un login.
```
<!DOCTYPE html>
<html lang="es" xmlns:th="http://www.thymeleaf.org">
<head>
<meta charset="utf-8">
<meta http-equiv="X-UA-Compatible" content="IE=edge">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Formulario de login</title>
<meta name="description" content="">
<meta name="author" content="">


<!-- Bootstrap core CSS -->
<!-- Custom styles for this template -->
<link href="/webjars/bootstrap/css/bootstrap.min.css" rel="stylesheet">

<link href="/css/signin.css" rel="stylesheet">


      <script src="https://oss.maxcdn.com/html5shiv/3.7.3/html5shiv.min.js"></script>
<!-- HTML5 shim and Respond.js for IE8 support of HTML5 elements and media queries -->
<!--[if lt IE 9]>
      <script src="https://oss.maxcdn.com/respond/1.4.2/respond.min.js"></script>
    <![endif]-->
            <label for="username" class="sr-only">Username</label> <input
</head>

<body>

    <div class="container">

        <form class="form-signin" th:action="@{/login}" method="post">
            <h2 class="form-signin-heading">Por favor, introduzca sus datos</h2>
                for="password" class="sr-only">Password</label> <input
                type="text" id="username" name="username" class="form-control"
                placeholder="Username" required autofocus> <label
                type="password" name="password"  id="password" class="form-control"
    ================================================== -->
                placeholder="Password" required>
            <button class="btn btn-lg btn-primary btn-block" type="submit">Sign
                in</button>
        </form>

    </div>
    <!-- /container -->

    <!-- Bootstrap core JavaScript
</html>
    <!-- Placed at the end of the document so the pages load faster -->
    <script src="/webjars/jquery/jquery.min.js"></script>
    <script src="/webjars/bootstrap/js/bootstrap.min.js"></script>


</body>
```

Y como vimos antes en el método configure debemos indicar que autorizamos a ls usuarios que se logeen en el login anterior. También
hay que permitir que los elementos web y css estén habilitados, porque si no, no nos cargará el login.

```
@Override
protected void configure(AutenticationManagerBuilder autenticationManagerBuilder) throws Exception{
	http.
		authorizeRequests()
		.anyMatchers("/webjars/**","/css/**).permitAll()
		.anyRequest()
		.authenticated()
		.formLogin()
		.loginPage("/login")
		.permitAll();
		
}
```

#### Manejo de sesiones con srping session

Utilizadas para almacenar claves valor que sobreviven entre diferentes peticiones. Una sesión es única por cada usuario. Esto 
ayuda a la lógica de nuestra aplicación (Un carrito de compra por ejemplo). Con @Autowired, spring maneja e inyecta el trabajo
con sesiones con el bean HttpSession.

#### Integración de spring session en nuestro proyecto
Actulizaremos el pomp para añadir las dependencias necesarias.
```
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
    <version>2.2.6.RELEASE</version>
</dependency>
<dependency>
    <groupId>org.springframework.session</groupId>
    <artifactId>spring-session-data-redis</artifactId>
    <version>2.2.2.RELEASE</version>
</dependency>
```

Modificamos la configuración mediante el archivo .properties de nuestro proyecto.
```
spring.session.store-type=redis
```

Redis es un framework el cual se encarga del manejo de sesiones con spring. Spring se encarga de crear un filtro _springSessionRepositoryFIlter_
el cual se encarga de redirigir a redis el manejo de beans HttpSession. En el fichero properties se pueden configrurar otros
aspectos de la sesion como el timeout y el modo de volcado de datos.



## 18/04/2020

Hoy empiezo la parte de _Spring Data JPA_ dentro del curso de _Spring Boot y Spring MVC_

#### Introducción a Spring Data
#### Integración de las entidades en nuestro proyecto.
Pongo los dos videos porque primero se explica la teoría y después la implementación, la cual muestro en los códigos de ejemplo.
Vamos a ver una herramienta para tener persitencia de información, usando bases de datos. Se plantea _Spring JPA_ para hacer mas sencilla
la interacción con la base de datos. _Spring Data_ usa el framework _Hibernate_, que se encarga de relacionar nuestra aplicación con
la base de datos y ejecutar operaciones CRUD.
 _Spring Data_ trabaja tanto con bases relacionales como no relacionales.

#### Entidades

En un modelo entidad-relacion de una base de datos relacional, necesitamos que las entidades puedan ser facilmente accesibles. Para ello
mapeamos una clase Java normal a una entidad. Para ello se nos proporciona un mapeo automático con el tag @Entity, que colocamos sobre
la declaración de la clase. Como requisito, la clase debe tener una propiedad que se usada como clave primaria, y señalada con el tag
@Id.
```
@Entity
public class Persona{
	@Id
	private String dni;

}
```
Podemos controlar las tablas y columns con los tags @Table y @Column
```
@Entity
@Table(name = "Persona")
public class Persona{
	@Id
	@Column(name = "Dni")
	private String dni;

}
```
Se pueden autogenerar valores con el tag @GeneratedValue
```
@Entity
@Table(name = "Persona")
public class Persona{
	@Id
	@GeneratedValue(strategy = GenerationType.AUTO)
	@Column(name = "Dni")
	private String dni;

}
```
Hay cuatro estrategias para generar valores :
-AUTO, hibernate escoge cual es la mejor estrategia para generar valores.
-SEQUENCE, se generan valores en base a una secuencia.
-IDENTITY, se utiliza un campo auto numérico.
-TABLE, se utiliza una tabla extra especial.

Para asociar entidades podemos usar los tags de relaciones, @ManyToMany , @OneToOne,  @OneToMany y @ManyToOne .
Para rellenar campos se pueden usar los mismos tags de validación de datos que en los formularios.

#### Repositorios
#### Integración de repositorios en nuestro proyecto.
Los repositorios son la interfaz principal de Spring data. Permiten hacer operaciones CRUD sobre la entidad que especifiquemos y el tipo
de id que maneja. Para la clase persona anterior el repositorio sería.
```
public interface PersonaRepository extends CrudRepositoty<Persona,Long>
```
Para crear una persona en la base de datos debemos usar el método _save_ del repositorio. Para buscarla podemos usar _findAll()_
```
Persona persona = new Persona("56756789F");
personaRepository.save(persona);
personaRepository.findAll().foreach(System.out::print);
```
Para guardar las operaciones lógicas con la base de datos se suele crear un servicio, que autoimplemente los repositorios y entidades
necesarias. Aquí tenemos un ejemplo de un servicio con operaciones básicas, como serían añadir una persona, modificarla, borrarla , buscar todas 
las personas o buscar por id.
```
@Service
public class PersonaServiceDB implements PersonaService {
	
	@Autowired
	private PersonaRepository repositorio;

	@Override
	public Persona add(Empleado p) {
		return repositorio.save(p);
	}

	@Override
	public List<Persona> findAll() {
		return repositorio.findAll();
	}

	@Override
	public Persona findById(long id) {
		return repositorio.findById(id).orElse(null);
	}

	@Override
	public Persona edit(Persona p) {
		return repositorio.save(p);
	}
	
	public void delete(Persona p) {
		repositorio.delete(p);
	}

}
```
Para que la conexión a la base de datos funcione necesitamos incorporar los drivers. En mi caso lo hago desde el archivo properties:
```
spring.datasource.url=jdbc:localhost:1433/dbejemplo
spring.datasource.username=sa
spring.datasource.password=1234
```

#### Consultas básicas
#### Otras consultas
Pongo los dos videos juntos , ya que así se explican todos los tipos de consultas que se pueden hacer, las cuales se explican en 
los dos videos. El primer tipo de consulta se explica en todo el primer video.

Existen varios tipos de formas de hacer consultas:
-Derivadas del nombre del método. Métodos que usan una nomencaltura concreta (findBy,readAll etc) más el atributo de entidad que quieren buscar. Ejemplo:
```
List<Persona>findByName()
```
Se puede complicar las consultas con ands, order by, distinct etc.

```
List<Persona>findByNameAndSurname();
List<Persona> findByNameContainsIgnoreCaseOrEmailContainsIgnoreCaseOrTelephoneContainsIgnoreCase(String name, String email, String telephone);
```
Declarando así los métodos, Spring Data se encarga de autogenerar la consulta correspondiente que actue sobre la base de datos.

-Consultas con el tag @Query. Con el tag query, que pondremos encima del método que ejecute la consulta, tendremos que poner entre 
parentesis la consulta que queramos que se ejecute. Una vez llamemos al método, ejecutará la consulta señalada con el tag @Query.
```
@Query("select * from Persona")
	List<Persona> findAllPersona(String input);

@Query("select p from Persona p where lower(p.name) like %?1% or lower(p.email) like %?1% or lower(p.telephone) like %?1%")
	List<Persona> findByNameMailOrPhone(String input);
```

-QueryDSL, framework que permite la construcción de consultas. Utiliza la interfaz _QuerydslPredicateExecutor_, la cual nos permite el uso
de predicados para hacer consultas.
-QueryByExample, se crea un objeto de ejemplo que se usa para buscar el resto de objetos que tengan los mismos campos que él, o que cumplan
ciertos patrones en el caso de las cadenas de texto.
```
Persona p = new Persona();
p.setTelephone("183745");
List<Persona> repositorio.findAll(p);
```
## 19/04/2020

#### Introducción al proyecto
Hoy voy a finalizar el curso de Spring Boot y Srping MVC, haciendo el proyecto final de la aplicación. El proyecto consistirá en una
aplicación sencilla de compras de segunda mano. Tendremos tres modelos de datos; producto , usuarios y compra.
Tendrá las funcionalidades tipicas de una tienda online, usuarios que se registren, carrito de compra etc. Se usará todo lo visto en este curso.


#### Creación de entidades 
Creamos las entidades producto , usuarios y compra, como hemos venido haciendo con el resto de entidades. Las entidades compra y usuario
tendrán una variable fecha con lo que usarán el tag @EnableListeners para que se rellenen automáticamente. Aprovechamos para hacer
la clase de configuración.
Entidad Usuario
```
@Entity
@EntityListeners(AuditingEntityListener.class)
public class Usuario {

    @Id
    @GeneratedValue
    private long id;
    private String nombre;
    private String apellidos;
    private String avatar;
    private String email;
    private String password;

    @CreatedDate
    @Temporal(TemporalType.TIMESTAMP)
    private Date date;
```

Entidad Compra
``` 
@Entity
@EntityListeners(AuditingEntityListener.class)
public class Compra {

    @Id
    @GeneratedValue
    private Long id;

    @CreatedDate
    @Temporal(TemporalType.TIMESTAMP)
    private Date date;

    @ManyToOne
    Usuario usuario;
```

Entidad Producto
``` 
@Entity
public class Producto {

    @Id
    @GeneratedValue
    private long id;
    private float precio;
    private String imagen;
    private String nombre;

    @ManyToOne
    private Usuario usuario;

    @ManyToOne
    private Compra compra;
```

Configuración
```
@Configuration
@EnableJpaAuditing
public class ConfigurationAuditory {
}
```

#### Creación de repositorios
Creamos los repositorios para manejar cada entidad.
En el caso de compra, tendremos una función para buscar por usuario.
```
public interface compraRespository extends JpaRepository<Compra,Long> {
    List<Compra> findAllByUsuario(Usuario usuario);
}
```

En el caso de usuario, tendremos una función para buscar por email.
```
public interface usarioRepository extends JpaRepository<Usuario,Long> {
    Usuario findFirstByEmail(String email);
}
```

En el caso de producto, tendremos varias funciones.
- Buscar por usuario.
- Buscar por compra.
- Buscar por compra si es nula.
- Buscar por una cadena de caractéres dentro de nombre.
- Buscar por una cadena de caractéres dentro de nombre y un usuario.
```
public interface productoRepository extends JpaRepository<Producto,Long>{

        List<Producto> findByUsuario(Usuario usuario);

        List<Producto> findByCompra(Compra compra);

        List<Producto> findByCompraIsNull();

        List<Producto> findByNombreContainsIgnoreCaseAndCompraIsNull(String nombre);

        List<Producto> findByNombreContainsIgnoreCaseAndPropietario(String nombre, Usuario usuario);
}
```

#### Aplicación de la seguridad.
A parte de la interfaz _WebSecurityConfigurerAdapter_ vista con anterioridad, usaremos otra interfaz para localizar a los usuarios
que están guardados en nuestra base de datos, llamada _UserDetailsService_. Craremos una clase que gestione la implementación de dicha
interfaz.
```
@Service("userDetailsService")
public class UserDetailsServiceImp implements UserDetailsService {
    @Autowired
    UsuarioRepository usuarioRepository;

    @Override
    public UserDetails loadUserByUsername(String s) throws UsernameNotFoundException {
        Usuario usuario = usuarioRepository.findFirstByEmail(s);

        User.UserBuilder builder = null;

        if (usuario != null) {
            builder = User.withUsername(s);
            builder.disabled(false);
            builder.password(usuario.getPassword());
            builder.authorities(new SimpleGrantedAuthority("ROLE_USER"));
        } else {
            throw new UsernameNotFoundException("Usuario no encontrado");
        }

        return builder.build();
    }
}
```
Esta clase se encargrá de utilizar un usuario creado en la base de datos (a través de un repositorio utilizando el email como nombre)
, para autencarnos en la aplicación. Si no encuentra al usuario, lanzará una excepción hacia arriba.
Ahora crearems la clase de seguridad tal y como hicimos anteriormente en el curso, poniendo la seguridad http habitual y quitando la 
seguridad _csrf_ . En la parte de auth , hacemos refencia al método de _UserDetails_ creado anteriormente. Por último, utilizaremos el
bean _BCryptPasswordEncoder_ proporcionado por Spring para encriptar la contraseña del usuario.
``` 
@Configuration
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {

    @Autowired
    UserDetailsService userDetailsService;

    @Bean
    public BCryptPasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder();
    }

    @Override
    protected void configure(AuthenticationManagerBuilder auth) throws Exception {
        auth.userDetailsService(userDetailsService).passwordEncoder(passwordEncoder());
    }

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http
                .authorizeRequests()
                .antMatchers("/", "/webjars/**", "/css/**", "/h2-console/**", "/public/**", "/auth/**", "/files/**").permitAll()
                .anyRequest().authenticated()
                .and()
                .formLogin()
                .loginPage("/auth/login")
                .defaultSuccessUrl("/public/index", true)
                .loginProcessingUrl("/auth/login-post")
                .permitAll()
                .and()
                .logout()
                .logoutUrl("/auth/logout")
                .logoutSuccessUrl("/public/index");

        http.csrf().disable();
        http.headers().frameOptions().disable();

    }
}
```

#### Creación de servicios
Crearemos un servicio para usuarios, otro para productos y otro para compras. Simplemente implementarán los métodos CRUD declarados en 
los repositorios.
Compra. Al insertar una compra, tendremos que insertar un usuario, puesto que las entidades están realcionadas.
```
@Service
public class CompraServicio {
	
	@Autowired
	CompraRepository repositorio;
	
	@Autowired
	ProductoServicio productoServicio;
	
	public Compra insertar(Compra c, Usuario u) {
		c.setUsuario(u);
		return repositorio.save(c);
	}
	
	public Compra insertar(Compra c) {
		return repositorio.save(c);
	}
	
	public Producto addProductoCompra(Producto p, Compra c) {
		p.setCompra(c);
		return productoServicio.editar(p);
	}
	
	public Compra buscarPorId(long id) {
		return repositorio.findById(id).orElse(null);
	}
	
	public List<Compra> todas() {
		return repositorio.findAll();
	}
	
	public List<Compra> porUsuario(Usuario u) {
		return repositorio.findByUsuario(u);
	}
	
}
```
Usuario. Añadimos también un password encoder, para cuando almacenemos un usuario en la base de datos, hay que encriptar la contraseña
para almacenarla.
```
@Service
public class UsuarioServicio {
	
	@Autowired
	UsuarioRepository repositorio;
	
	@Autowired
	BCryptPasswordEncoder passwordEncoder;
	
	
	public Usuario registrar(Usuario u) {
		u.setPassword(passwordEncoder.encode(u.getPassword()));
		return repositorio.save(u);
	}
	
	public Usuario findById(long id) {
		return repositorio.findById(id).orElse(null);
	}
	
	public Usuario buscarPorEmail(String email) {
		return repositorio.findFirstByEmail(email);
	}
}
```
Producto. Insertamos un metodó a mayores para buscar con id.
```
@Service
public class ProductoServicio {
	
	@Autowired
	ProductoRepository repositorio;
	
	@Autowired
	StorageService storageService;
	
	
	public Producto insertar(Producto p) {
		return repositorio.save(p);
	}
	
	public void borrar(long id) {
		repositorio.deleteById(id);
	}
	
	public void borrar(Producto p) {
		if (!p.getImagen().isEmpty())
			storageService.delete(p.getImagen());
		repositorio.delete(p);
	}
	
	public Producto editar(Producto p) {
		return repositorio.save(p);
	}
	
	public Producto findById(long id) {
		return repositorio.findById(id).orElse(null);
	}
	
	public List<Producto> findAll() {
		return repositorio.findAll();
	}
	
	public List<Producto> productosDeUnPropietario(Usuario u) {
		return repositorio.findByPropietario(u);
	}
	
	public List<Producto> productosDeUnaCompra(Compra c) {
		return repositorio.findByCompra(c);
	}
	
	public List<Producto> productosSinVender() {
		return repositorio.findByCompraIsNull();
	}
	
	public List<Producto> buscar(String query) {
		return repositorio.findByNombreContainsIgnoreCaseAndCompraIsNull(query);
	}
	
	public List<Producto> buscarMisProductos(String query, Usuario u) {
		return repositorio.findByNombreContainsIgnoreCaseAndUsuario(query,u);
	}
	
	public List<Producto> variosPorId(List<Long> ids) {
		return repositorio.findAllById(ids);
	}

}
```

#### Plantillas a utilizar
Usaremos las plantillas proporcionadas por OpenWebinars.

#### Login y registro
Crearemos un controlador que manejr el login de un usuario y el registro. Lo haremos a través del servicio de usuarios creado anteriormente
```
@Controller
public class LoginController {

    @Autowired
    UsuarioService usuarioServicio;

    @GetMapping("/")
    public String welcome() {
        return "redirect:/public/";
    }


    @GetMapping("/auth/login")
    public String login(Model model) {
        model.addAttribute("usuario", new Usuario());
        return "login";
    }


    @PostMapping("/auth/register")
    public String register(@ModelAttribute Usuario usuario, @RequestParam("file") MultipartFile file) {
        usuarioServicio.registrar(usuario);
        return "redirect:/auth/login";
    }

}
```
Como podemos ver , el servicio creado de usuarios es el que maneja la gestión del mismo, registrando nuevos usuarios. En caso de registro
nos redirige a la página de login.
Si alguien accede al raiz de nuestro sitio lo guiaremos al listado de productos que haremos posteriormente.

#### Listado de productos
Para ello creamos un nuevo controlador que liste los productos existentes en nuestra base de datos. Será como el controlador que maneje
la parte pública de la aplicación.
Para ello usaremos el servicio de productos creado anteriormente.
``` 
@Controller
@RequestMapping("/public")
public class ZonaPublicaController {
	
	@Autowired
	ProductoServicio productoServicio;
	
	
	@ModelAttribute("productos")
	public List<Producto> productosNoVendidos() {
		return productoServicio.productosSinVender();
	}
	
	
	@GetMapping({"/", "/index"})
	public String index(Model model, @RequestParam(name="q", required=false) String query) {
		if (query != null)
			model.addAttribute("productos", productoServicio.buscar(query));
		return "index";
	}
	
	@GetMapping("/producto/{id}")
	public String showProduct(Model model, @PathVariable Long id) {
		Producto result = productoServicio.findById(id); 
		if (result != null) {
			model.addAttribute("producto", result);
			return "producto";
		}
		return "redirect:/public";
	}
}
```
-_productosNoVendidos_ , carga cuando se inicie el controlador, los productos no vendidos.
-_index_ busca un producto por query, auqnue la query no es obligatoria. Si la query es nula, tira del listado de productos sin vender.
-_showProduct_ muestra un producto por id.


#### Comprar y carrito.
#### Finalización de la compra y factura
Junto los dos videos ya que se centran los dos en el mismo controlador.
Crearemos un controller que usara los servicios de los tres modelos, Usuario, Productoy Compra.
También manejaremos la sesión HTTP para evitar la pérdida de datos, con el bean autoinyectado _HTTPSession_ .





#### Gestión de productos
Nos centramos en crear el controller de productos.
```
@Controller
@RequestMapping("/app")
public class ProductosController {

    @Autowired
    ProductoService productoServicio;

    @Autowired
    UsuarioService usuarioServicio;

    private Usuario usuario;

    @ModelAttribute("misproductos")
    public List<Producto> misProductos() {
        String email = SecurityContextHolder.getContext().getAuthentication().getName();
        usuario = usuarioServicio.buscarPorEmail(email);
        return productoServicio.productosDeUnPropietario(usuario);
    }

    @GetMapping("/misproductos")
    public String list(Model model, @RequestParam(name = "q", required = false) String query) {
        if (query != null)
            model.addAttribute("misproductos", productoServicio.buscarMisProductos(query, usuario));

        return "app/producto/lista";
    }

    @GetMapping("/misproductos/{id}/eliminar")
    public String eliminar(@PathVariable Long id) {
        Producto p = productoServicio.findById(id);
        if (p.getCompra() == null)
            productoServicio.borrar(p);
        return "redirect:/app/misproductos";
    }

    @GetMapping("/producto/nuevo")
    public String nuevoProductoForm(Model model) {
        model.addAttribute("producto", new Producto());
        return "app/producto/form";
    }

    @PostMapping("/producto/nuevo/submit")
    public String nuevoProductoSubmit(@ModelAttribute Producto producto, @RequestParam("file") MultipartFile file) {
        producto.setUsuario(usuario);
        productoServicio.insertar(producto);
        return "redirect:/app/misproductos";
    }
}
```
Tendremos los siguientes métodos:
-_misProductos_ , carga cuando se inicie el controlador, los productos de un usuario.
-_list_ lista productos de un usuario. Puede hacerlo en base a una query, pero no es obligatorio.
-_eliminar_ elimina un producto de un usuario.
-_nuevoProductoForm_ Crea un nuevo producto desde el formulario de la aplicación.
-_nuevoProductoSubmit_  Añade un producto creado a la base de datos.

#### Subida de imágenes y gestión de almacenamiento.



