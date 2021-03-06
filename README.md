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

```
@Controller
@RequestMapping("/app")
public class CompraController {
	@Autowired
	CompraServicio compraServicio;
	@Autowired
	ProductoServicio productoServicio;
	@Autowired
	UsuarioServicio usuarioServicio;
	@Autowired
	HttpSession session;
	
	private Usuario usuario;
	
	@ModelAttribute("carrito")
	public List<Producto> productosCarrito() {
		List<Long> contenido = (List<Long>) session.getAttribute("carrito");
		return (contenido == null) ? null : productoServicio.variosPorId(contenido);
	}
	
	@ModelAttribute("total_carrito")
	public Double totalCarrito() {
		List<Producto> productosCarrito = productosCarrito();
		if (productosCarrito != null)
			return productosCarrito.stream()
				.mapToDouble(p -> p.getPrecio())
				.sum();
		return 0.0;
	}
	
	@ModelAttribute("mis_compras")
	public List<Compra> misCompras() {
		String email = SecurityContextHolder.getContext().getAuthentication().getName();
		usuario = usuarioServicio.buscarPorEmail(email);
		return compraServicio.porPropietario(usuario);
	}
	
	
	@GetMapping("/carrito")
	public String verCarrito(Model model) {
		return "app/compra/carrito";
	}
	
	@GetMapping("/carrito/add/{id}")
	public String addCarrito(Model model, @PathVariable Long id) {
		List<Long> contenido = (List<Long>) session.getAttribute("carrito");
		if (contenido == null)
			contenido = new ArrayList<>();
		if (!contenido.contains(id))
			contenido.add(id);
		session.setAttribute("carrito", contenido);
		return "redirect:/app/carrito";
	}
	
	@GetMapping("/carrito/eliminar/{id}")
	public String borrarDeCarrito(Model model, @PathVariable Long id) {
		List<Long> contenido = (List<Long>) session.getAttribute("carrito");
		if (contenido == null)
			return "redirect:/public";
		contenido.remove(id);
		if (contenido.isEmpty())
			session.removeAttribute("carrito");
		else
			session.setAttribute("carrito", contenido);
		return "redirect:/app/carrito";
		
	}
	
	@GetMapping("/carrito/finalizar")
	public String checkout() {
		List<Long> contenido = (List<Long>) session.getAttribute("carrito");
		if (contenido == null)
			return "redirect:/public";
		
		List<Producto> productos = productosCarrito();
		
		Compra c = compraServicio.insertar(new Compra(), usuario);
		
		productos.forEach(p -> compraServicio.addProductoCompra(p, c));
		session.removeAttribute("carrito");
		
		return "redirect:/app/compra/factura/"+c.getId();
		
	}
	
	@GetMapping("/miscompras")
	public String verMisCompras(Model model) {
		return "/app/compra/listado";
	}
	
	@GetMapping("/compra/factura/{id}")
	public String factura(Model model, @PathVariable Long id) {
		Compra c = compraServicio.buscarPorId(id);
		List<Producto> productos = productoServicio.productosDeUnaCompra(c);
		model.addAttribute("productos", productos);
		model.addAttribute("compra", c);
		model.addAttribute("total_compra", productos.stream().mapToDouble(p -> p.getPrecio()).sum());
		return "/app/compra/factura";
	}
```
Tenemos las siguientes peticiones:
- _verCarrito_ , nos permite ver el carrito actual .
- _addCarrito_ , añade un producto al carrito.
- _borrarDeCarrito_ , borra un producto del carrito. Si llega a cero, nos avisa de que no tenemos ninguna compra hecha.
- _checkout_ , nos muestra en la vista factura, como sería la factura actual si realizásemos la compra.
- _verMisCompras_ ,  .
- _factura_ , nos crea una factura con los productos comprados y el precion en la vista factura.


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
La subida de imágenes, se harán con el servicio de almacenamiento de ficheros visto anteriormente. El servicio será idéntico al mostrado
anteriormente, llamado _StorageService_.
Como el único modelo que utiliza imágenes es el _Producto_ , lo que hay que gestionar es en _ProductoService_, el borrado de datos, ya que 
la imágen se inserta automáticamente desde el controller, que también debemos modificar. Así que dentro de el servicio de productos añadimos el servicio de almacenamiento
e insertamos un método para borrar.

```
@Autowired
	StorageService storageService;
	
	public void borrar(Producto p) {
		if (!p.getImagen().isEmpty())
			storageService.delete(p.getImagen());
		repositorio.delete(p);
	}
```
Por último en el controlador de productos, en el método de creación añadimos el código para que cree una imagen.
```
@PostMapping("/producto/nuevo/submit")
	public String nuevoProductoSubmit(@ModelAttribute Producto producto, @RequestParam("file") MultipartFile file) {		
		if (!file.isEmpty()) {
			String imagen = storageService.store(file);
			producto.setImagen(MvcUriComponentsBuilder
					.fromMethodName(FilesController.class, "serveFile", imagen).build().toUriString());
		}
		producto.setPropietario(usuario);
		productoServicio.insertar(producto);
		return "redirect:/app/misproductos";
	}
```

## 23/04/2020

Hoy empecé el tercer curso de la carrera _Curso de desarrollo de una API REST con Spring Boot_ .

#### Presentación del curso
Nos presenta el curso y los conocimientos mínimos que debemos aprender. Además, nos presenta lo que vamos a aprender.

#### ¿Qué es un servicio REST?
Nos explica que es un servicio Rest, como se usa el protocolo HTTP dentro de REST, de donde surgió la tecnología REST y sus ventajas frente
al protocolo RCP.
Estos conceptos estan explicados en su mayoría en la asignatura de Acceso a Datos.
Una de las principales ventajas de REST es su uso en multiplataforma, solo necesitamos un lenguaje que nos permita usar una librería REST 
para crear nuestro propio servidor y que esté a la escucha de peticiones HTTP.

#### Protocolo HTTP
#### Algunos elementos de HTTP
En estos dos videos se nos explica el funcionamiento y elementos de algunos elementos HTTP.
Derivado de  _Hipertext Transfer Transimsion Protocol_, es un protocolo sin estados con un esquema petición respuesta.
El formato del mensaje en el protocolo HTTP es el mismo para peticiones como para respuesta, y cuenta de una cabecera y un curepo.
Los métodos mas conocidos son:
- _GET_, recupera un recurso del servidor.
- _POST_, envía un recurso al servidor para que sea procesado.
- _PUT_, actualiza un recurso del servidor. (Ojo solo actualizar, si deseamos crear datos necesitamos usar POST).
- _DELETE_, elimina un recurso especificado en el servidor.

También existe otros recursos menos utilizados como _HEAD_ , _OPTIONS_ o _PATH_

Algunos de los códigos de respuesta son.
- _2XX_ peticiones con éxito
- _4XX_ error en el lado del cliente
- _5XX_ error en el aldo del servidor

#### Nuestro entorno de desarrollo
Nos explica el entorno de desarrollo que vamos a usar en este curso. En el curso van a utilizar el IDE _Spring Tool Suite_ . Yo
voy a usar el IDE _Intell IJ_ puesto que es el que uso en el trabajo, y además, a la hora de  crear APIs REST, las creamos como 
un proyecto Maven, a traves del cual, importamos las librerías necesarias para gestionar Spring.
También usaran Postman para testear las APIs y hacer peticions. Yo utilizaré Rester.

#### Uso de librería Lombok
Lombok es una librería de generación de código (Getters setters etc), que usaremos para ahorrar trabajo de escritura de código.
La implataré con su dependencia maven.
```
<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <version>1.18.10</version>
    <scope>provided</scope>
</dependency>
```
La principal anotación Lombok es _@Data_, que si la ponemos al principio de una clase, nos autogenera getters, setters, equals, hashCode,
toString y un constructor vacio.

#### Soporte de Spring Boot para servicios REST

A través de desarrolar REST con Spring Boot, tendremos todas las ventajas de Spring aplicada a las APIs REST, como puede ser la inyección
de dependecias o seguridad. También incorpora tags orientados a REST como @RestController. 
Spring Boot incormpora algunas herramientas como Jackson-2 , que se encarga del mapeo de objetos a JSON.
También nos permite consumir e interactuar con otras apis con RestTemplate.

## 24/04/2020

Hoy he empezado con la parte de _Mi primer servicio REST con Spring Boot_ , en la que haremos un servicio REST básico para ver las 
funcionalidades de Spring Boot en REST.

#### Mi primer servicio REST

Montamos la estructura básica del proyecto (Método main, librerias maven, modelos y controladores).
En el caso del controlador usamos _@RestController_ en vez de _@Controller_.
```
@RestControllerpublic class GreetingController { 
	//Resto del código
	@GetMapping("/greeting")public Greeting greeting( @RequestParam(value="name", defaultValue="World") String name) {
		return new Greeting(counter.incrementAndGet(),String.format(template, name));     
	}
} 
```
En el caso de el método, la repuesta, pasa por un filtro llamado _HttpMessageConverter_ que lo transforma en JSON.

#### Puesta en marcha de la aplicación.

La aplicación se debe lanzar desde IntellIj con la configuración de _Aplication_ y especificando la clase principal. También hay que tener 
cuidado con si estamos especificando el puerto donde se lanza.
También se puede lanzar desde el terminal del proyecto usando comandos maven.
```
mvn clean
mvn install
mvn spring-boot:run
```
Obviamente hay que tener instalado maven en local. El comando _clean_ actuliza y carga los cambios del pryecto. El comando _install_ ,
instala la aplicación en local. Por último _spring-boot:run_ lanza la aplicación.

#### Estructura de las rutas
Aquí se nos explica la estructura del proyecto.
Manejaremos un modelo llamado _producto_ que tendrá un id, un nombre y un precio.
Se creará también un repositorio e insertaremos datos de ejemplo.
En cuanto a la estructura de las rutas
- /producto GET. Obtendrá todos los productos
- /producto/{id} GET. Obtendrá un producto por su id
- /producto POST . Creará un nuevo prducto.
- /producto/{id} PUT. Actualizará un producto por su id
- /producto/{id} DELETE. Borrará un producto por su id

El modelo _producto_.
```
@Data 
@Entity
public class Producto {

	@Id @GeneratedValue
	private Long id;
	
	private String nombre;
	
	private float precio;
	
}
```
 El controlador.
 ```
@RestController
public class ProductoController {

	private final ProductoRepositorio productoRepositorio;

	@GetMapping("/producto")
	public List<Producto> obtenerTodos() {
		return productoRepositorio.findAll();
	}

	@GetMapping("/producto/{id}")
	public Producto obtenerUno(@PathVariable Long id) {
		return productoRepositorio.findById(id).orElse(null);
	}

	@PostMapping("/producto")
	public Producto nuevoProducto(@RequestBody Producto nuevo) {
		return productoRepositorio.save(nuevo);
	}

	@PutMapping("/producto/{id}")
	public Producto editarProducto(@RequestBody Producto editar, @PathVariable Long id) {
		if (productoRepositorio.existsById(id)) {
			editar.setId(id);
			return productoRepositorio.save(editar);
		} else {
			return null;
		}
	}
	
	@DeleteMapping("/producto/{id}")
	public Producto borrarProducto(@PathVariable Long id) {
		if (productoRepositorio.existsById(id)) {
			Producto result = productoRepositorio.findById(id).get();
			productoRepositorio.deleteById(id);
			return result;
		} else
			return null;
	}

}
 ```
 
 Como podemos ver no construimos la repuesta con un response (Para gestionar los códigos HTTP), sino que devolvemos el objeto sobre el
 que interactuamos. Por lo tanto, la respuesta HTTP siempre va a ser OK.

## 25/04/2020
#### Clase y anotaciones de Srpring Rest
- _ResponseEntity_ , usada para manejar las repuestas de la API Rest, hereda de _HttpEntity_. La respuesta se compone de un cuerpo y una 
cabecera. Vamos a aplicarlo al servicio anterior.

```
@RestController
public class ProductoController {

	private final ProductoRepositorio productoRepositorio;

	@GetMapping("/producto")
	public ResponseEntity<?> obtenerTodos() {
		List<Producto> result = productoRepositorio.findAll();

		if (result.isEmpty()) {
			return ResponseEntity.notFound().build();
		} else {
			return ResponseEntity.ok(result);
		}

	}

	@GetMapping("/producto/{id}")
	public ResponseEntity<?> obtenerUno(@PathVariable Long id) {
		Producto result = productoRepositorio.findById(id).orElse(null);
		if (result == null)
			return ResponseEntity.notFound().build();
		else
			return ResponseEntity.ok(result);
	}
	
	@PostMapping("/producto")
	public ResponseEntity<?> nuevoProducto(@RequestBody Producto nuevo) {
		Producto saved = productoRepositorio.save(nuevo);
		return ResponseEntity.status(HttpStatus.CREATED).body(saved);
	}

	@PutMapping("/producto/{id}")
	public ResponseEntity<?> editarProducto(@RequestBody Producto editar, @PathVariable Long id) {

		return productoRepositorio.findById(id).map(p -> {
			p.setNombre(editar.getNombre());
			p.setPrecio(editar.getPrecio());
			return ResponseEntity.ok(productoRepositorio.save(p));
		}).orElseGet(() -> {
			return ResponseEntity.notFound().build();
		});
	}

	@DeleteMapping("/producto/{id}")
	public ResponseEntity<?> borrarProducto(@PathVariable Long id) {
		productoRepositorio.deleteById(id);
		return ResponseEntity.noContent().build();
	}

}
```
Podemos observar como con las responses, mandamos diferentes informaciones del resultado de su petición que el usuario final leera 
como un código HTTP. Por ejemplo, si hace la operación POST para guardar un producto, recibirán un código 201 (Created).
Como podemos ver, se puede construir el cuerpo de una respuesta pasandole como argumento el objeto que queremos devolver, el cual se
transformará automáticamente a json. Podemos ver como en los GET se hace ese proces.
Todavía no tenemos gestión de excepciones, como petición erronea , se darán más adelante en el curso.

#### Uso de Data Transfer Object (DTO)
Las DTO son objetos POJO, son clases  que son intermediarias entre la aplicacióny una entidad de base de datos. No puede tener nada
de lógica de negocio, y debe de ser serializable.
Imaginemos una entidad Producto(nombre , id,categoría), y una entidad Categoría (Nombre,id, descripcion).
Para relacionar las dos entidades poedmos crear un DTO para productos con nombre, id y id de categoría, y nombre de categoría.
Los DTO dependerán de como querramos usar nuestros datos en la aplicación.

#### Implementando DTO con Model Mapper
_ModelMapper_ facilita la creación de DTO de forma más dinámica. Tenemos que implementar su dependencia y un bean que decuelva el model
mapper que realice las transformaciones.
```
<dependency>
	<groupId>org.modelmapper</groupId>
        <artifactId>modelmapper</artifactId>
        <version>2.3.5</version>
</dependency>
```
```
public ModelMapper modelMapper(){
	return new ModelMapper();
}
```
Vamos a crear un DTO de un producto realcionado con el nombre de una categoría.
```
public class ProductoDTO {
	private long id;
	private String nombre;
	private String categoriaNombre;
}
```
Para transformar productos en _ProductosDTO_ , usaremos como dijimos _ModelMapper_. Crearemos un componente para ello.
```
@Component
public class ProductoDTOConverter {
	
	private final ModelMapper modelMapper;
	public ProductoDTO convertToDto(Producto producto) {
		return modelMapper.map(producto, ProductoDTO.class);
		
	}

}
```
Como podemos ver, es una clase muy sencilla y con my poco código y fácil de leer.
Ahora implementaremos esta funcionalidad en el método GET del controlador.
```
@GetMapping("/producto")
	public ResponseEntity<?> obtenerTodos() {
		List<Producto> result = productoRepositorio.findAll();

		if (result.isEmpty()) {
			return ResponseEntity.notFound().build();
		} else {

			List<ProductoDTO> dtoList = result.stream().map(productoDTOConverter::convertToDto)
					.collect(Collectors.toList());

			return ResponseEntity.ok(dtoList);
		}
	}
```

## 26/04/2020

Hoy empiezo con la parte de manejo de errores en una API REST.

##### Manejo básico de errores.

Podemos crear excepciones y ligarlas a un código de respuesta con el tag _ResponseStatus_ .
```
@ResponseStatus(HttpStatus.NOT_FOUND)
public class ProductNotFoundException(){...}
```` 
Ahora cuando salte esa excepción , el controlador responderá con un código 404(Not found). En el controlador podremos manejar las 
de la manera que mas consideremos necesaria, lo importante, es que sepamos que respuesta debe recibir cada error.

#### Modelo de respuesta de un error.

EL modelo de error lo fabrica Spring por defecto con la clase _DefaultErrorAtributes_ . Cuenta con los atributos de fecha, tipo de error,
mensaje, estado y el _path_ del error(en que ruta se produjo) . 
También podremos crear modelos de error para añadir o quitar atributos. Haremos una clase POJO que tenga los atributos que querramos.
Como ejemplo haremos un modelo básico con estado, fecha y mensaje. 
```
@Setter
@Getter
public class ApiError {

	
	private HttpStatus estado;
	@JsonFormat(shape = Shape.STRING, pattern = "dd/MM/yyyy hh:mm:ss")
	private LocalDateTime fecha;
	private String mensaje;
	
}
```
Los tags de getter y setter  son implementados por la librería Lombak, y, lo que hace, es crear automáticamente los métodos getter y 
setter de todos los atributos de la clase.

#### Manejo de excepciones con @ExceptionHandler
 El tag _@ExceptionHandler_ controla que se produzca una excepción especificada que se produzca en cualquier método de un controlador
 y responde al cliente con el modelo de la excepción correspondiente.
 Vamos a hacer un manejador para _ProductNotFoundException_ .
 ```
 @ExceptionHandler(ProductoNotFoundException.class)
	public ResponseEntity<ApiError> handleProductoNoEncontrado(ProductoNotFoundException ex) {
		ApiError apiError = new ApiError();
		apiError.setEstado(HttpStatus.NOT_FOUND);
		apiError.setFecha(LocalDateTime.now());
		apiError.setMensaje(ex.getMessage());
		return ResponseEntity.status(HttpStatus.NOT_FOUND).body(apiError);
	}
 ```
 Si este método lo añadimos a un controlador, todos los errores que produzcan un _ProductNotFoundException_ serán tratados por este método
, y será el mismo el que gestione la respuesta del servidor.

#### Manejo de errores con @ControllerAdvice I
#### Manejo de errores con @ControllerAdvice II
El tag _@ControllerAdvice_ sive para manejar todas las excepciones que se produzcan a nivel aplicación. También se le puede limitar a 
que se asplique a un o varios controladores específicos.
Dentro de un _@ControllerAdvice_ se manejan todos los _@ExceptionHandler_ de toda la aplicación , o , en caso de que se hayan especificado,
de los controladores especificados.
Usaremos _@RestControllerAdvice_, que lo que hace es unificar _@ControllerAdvice_ y _@ResponseBody_ .


```
@RestControllerAdvice
public class GlobalControllerAdvice extends ResponseEntityExceptionHandler {
	
	@ExceptionHandler(ProductoNotFoundException.class)
	public ResponseEntity<ApiError> handleProductoNoEncontrado(ProductoNotFoundException ex) {
		ApiError apiError = new ApiError(HttpStatus.NOT_FOUND, ex.getMessage());
		return ResponseEntity.status(HttpStatus.NOT_FOUND).body(apiError);
	}
}
```

#### Novedades en Spring 5 : ResponseStatusException
La excepción _ResponseStatusException_ , es una excepción propia de Spring genérica con una serie de atributos específicos y unida a
_@ResponseBody_ , para poder devolver una respuesta estructurada desde servidor.
Es una novedad poco usada y que es da un paso hacia atras , ya que nos quita toda la globalidad ganada con _@ControllerAdvice_ 

## 27/04/2020
Hoy empiezo la parte de manejo de CORS, subida de ficheros y documentación.

#### ¿Que es CORS y por qué me va a dar problemas?
CORS( Cross Origin Resource Sharing) es una política de seguridad a nivel de navegadores web. Permite proteger a nuestra API de ataques 
maliciosos. Por razones de seguridad los navegadores prohiben llamadas AJAX a recursos que residen fuera del origen actual. Esto evita
por ejemplo, que tengamos dos webs distintas , y que una pudiera hacer solicitudes a la otra.
CORS utiliza cabeceras HTTP para perimitir al cliente (Navegador),acceder a un servidor de origen diferente al servidor actual.

#### ¿Cómo habilitar a nivel de método?
Con el tag _@CrossOrigin_, Spring nos permite modificar la seguridad especificando diferentes atributos.
- _origins_, lista de orígenes permitidos.
- _methods_, lista de  métodos soportados (GET,POST etc).
- _maxAge_, duración máxima en segundos de la duración de la caché.
También tiene otros atributos como _allowCredentials_, _allowHeaders_ o _exposeHeaders_ .
El tag puede ser tanto a nivel de cláse como a nivel de método. En el siguiente método vamos a poner que solo lo pueda ejecutar
un determinado servidor.

```
@CrossOrigin(origins = "http://localhost:9001")
	@GetMapping("/producto")
	public ResponseEntity<?> obtenerTodos() {
		List<Producto> result = productoRepositorio.findAll();

		if (result.isEmpty()) {
			throw new ResponseStatusException(HttpStatus.NOT_FOUND, "No hay productos registrados");
		} else {

			List<ProductoDTO> dtoList = result.stream().map(productoDTOConverter::convertToDto)
					.collect(Collectors.toList());

			return ResponseEntity.ok(dtoList);
		}

	}
```

#### Configuración CORS global
Para evitar poner el tag CrossMapping en cada clase o método, Spring permite crear una configuración Global declarando un bean
WebMvcConfigurer. En dicho bean especificamos los atributos de seguridad para todo el proyecto. 
```
@Bean
	public WebMvcConfigurer corsConfigurer() {
		return new WebMvcConfigurer() {

			@Override
			public void addCorsMappings(CorsRegistry registry) {
				registry.addMapping("/producto/**")
					.allowedOrigins("http://localhost:9001")
					.allowedMethods("GET", "POST", "PUT", "DELETE")
					.maxAge(3600);
			}
			
		};
	}
```

#### Subida de ficheros
Esta parte contiene tres vídeos, _Servicio de subida de ficheros_ , _Implementación del servicio de subida de ficheros_ y _Uso del 
servicio de subida de ficheros_ .
Estos vídeos nos explican de la misma manera, forma y código que en el curso de _SpringWebMVC_ , como usar la clase _Multipart_ para 
trabjar con varios ficheros y que además puedanser de distinto tipo. El código es exactamente el mismo que el el curso anterior y ademas 
la estructura de trabajo también (Creación de la interfaz _StorageService_, para luego hacer un servicio que lo implemente etc.).
Si se quiere bucar el trabajo de subida de ficheros, está hecho el día 14/04/2020.

Ahora seguimos con la suiguiente parte, _Documentando nuestra API REST con Swagger

#### ¿Que es Swagger?
Swagger es un framework que nos va a permitir dos cosas.
-Documentar todo nuestro proyecto, todas las peticiones que hacen, que argumentos tienen y de que tipo, ejemplos, modelos con los que 
trabaja la API que requisitos tienen que cumplir etc.
-Nos provee de una interfaz grafica facil e intuitiva de usar (Además de bonita), que nos permitirá probar nuestras peticiones de una
forma mucho más fácil y cómoda.
En mi trabajo es la forma que tenemos de documentar nuestros servicios, aunque lo combinamos con otra librería llamada _SpringDoc_, que
añade un par de cambios a nivel documentación.

#### ¿Cómo introducir Swagger en nuestra API?
Lo primero es importar las dependencias de Swagger en maven.
```
<dependency>
            <groupId>io.springfox</groupId>
            <artifactId>springfox-swagger2</artifactId>
            <version>3.0.0-SNAPSHOT</version>
        </dependency>
        <dependency>
            <groupId>io.springfox</groupId>
            <artifactId>springfox-data-rest</artifactId>
            <version>3.0.0-SNAPSHOT</version>
        </dependency>
        <dependency>
            <groupId>io.springfox</groupId>
            <artifactId>springfox-swagger-ui</artifactId>
            <version>3.0.0-SNAPSHOT</version>
```
Se puede comentar a nivel método, especificando que hace, que devuelve, que códigos HTTP puedes recibir a nivel de respuesta, como deben
ser los argumentos etc. Se usan los tags.
- _@ApiOperation_ , define que hace una petición de la API.
- _@ApiResponse_ , define responde una petición de la API.
- _@ApiParam_, define que parametros rebibe una petición de la API y de que tipo son.

```
@ApiOperation(value="Obtener un producto por su ID", notes="Provee un mecanismo para obtener todos los datos de un producto por su ID")
	@ApiResponses(value= {
			@ApiResponse(code=200, message="OK", response=Producto.class),
			@ApiResponse(code=404, message="Not Found", response=ApiError.class),
			@ApiResponse(code=500, message="Internal Server Error", response=ApiError.class)
	})
	@GetMapping("/producto/{id}")
	public Producto obtenerUno(@ApiParam(value="ID del producto", required=true, type = "long") @PathVariable Long id) {
		try {
			return productoRepositorio.findById(id)
					.orElseThrow(() -> new ProductoNotFoundException(id));
		} catch (ProductoNotFoundException ex) {
			throw new ResponseStatusException(HttpStatus.NOT_FOUND, ex.getMessage());
		}
				
	}
```

También podemos documentar un modelo con _@ApiModelProperty_ . Este tag nos permite explicar las propiedades de un modelo, poniendo 
ejemplos de sis valores, que tipo de valores reciben, algún requerimiento que puedan tener, además, obviamente de su descripción.

```
@Data 
@Entity
public class Producto {

	@ApiModelProperty(value="ID del Producto", dataType="long",  example="1", position=1)
	@Id @GeneratedValue
	private Long id;
	
	@ApiModelProperty(value="Nombre del producto", dataType="String", example="Jamón ibérico de bellota", position=2)
	private String nombre;
	
	@ApiModelProperty(value="Precio del producto", dataType = "float", example="253.27", position=3)
	private float precio;
	
	@ApiModelProperty(value="Imagen del producto", dataType = "String", example="http://www.midominio.com/files/12345-imagen.jpg", position=4)
	private String imagen;
	
	
	@ApiModelProperty(value="Categoría del producto", dataType="Categoria", position=5)
	@ManyToOne
	@JoinColumn(name="categoria_id")
	private Categoria categoria;
	
}
```

## 28/04/2020
Hoy empezaré el _Curso de elementos avanzados en tu API REST con Spring Boot_

#### Presentación
Nos presenta el curso y los conocimientos mínimos que debemos aprender. Presenta el curos como una extensión del anterior, que completará
el conocimiento en lo que se refiere al desarrollo de APIs en Spring.

#### Proyecto de ejemplo
Como utilizamos la base del curso anterior, utilizaremos el proyecto del curso anterior. Este vídeo es un recordatiorio de como está 
estructurado el proyecto y que funciones tiene.

#### Paginación de resultados.
La paginación es la división de un conjunto de datos en subconjutos más peuqeños. Esto hace mas fácil de visualizar los datos además de
agilizar las peticiones , ya que , tenemos menos cantidad de datos que pidiendo el conjunto de datos entero.
Para hacer paginación d elos datos de una consulta usaremos la interfaz de Spring _PagingAndSortingRepository_ , que hereda de 
_JpaRepository_.
El resultado de una paginación es una página, _Page<T>_ , que es un conjunto de objetos sobre los que se pueden hacer diferentes operaciones , como
por ejemplo, ordenar. También nos permite ver el numero del resto de páginas, el total de elementos, el tamñao por página etc. 
Vamos a buscar productos en nuestro servicio por paginación. Nuestro respositorio de productos ya extiende de _JpaRepository_ por lo tanto 
implementa _PagingAndSortingRepository_.
Debemos pasar el producto como un objeto _Pageable_ . Con el tag _@PageableDefault_ , donde le podemos poner valores por defecto del
valores como el tamaño de cada página y el número de página que queremos saber .
El método _findAll_ del repositorio encontrará todos los productos pero solo recuperará solo los que estén en la página indicada.

```
@GetMapping("/producto")
	public ResponseEntity<?> obtenerTodos(@PageableDefault(size = 10, page = 0) Pageable pageable) {
		Page<Producto> result = productoServicio.findAll(pageable);

		if (result.isEmpty()) {
			throw new ProductoNotFoundException();
		} else {
			Page<ProductoDTO> dtoList = result.map(productoDTOConverter::convertToDto);
		}
	}
```


#### Manejo de parámetros query I
#### Manejo de parámetros query II
En esta parte complementaremos la paginación con seleccionar datos con consultas de selección mas complejas. Utilizando el tag
_@RequestParam_ , equivalente a _@QueryParam_ , seleccionaremos un producto por diferetes argumentos como son el precio o el
nombre.
_@RequestParam_ puede tener varios atributos: 
- _name_ o _value_ : nombre del parámetro que vamos a recibir
- _required_ : Indica si el parámetro es o no obligatorio.
- _defaultValue_ : proporciona un valor por defecto en el caso de no recibir ninguno.
Con esto podemos jugar con valores por defecto en las consultas, y, asegurarnos de que siempre son consultas válidas (Que no
generan errores). En caso de no especificar, el valor _reuquired_ es _false_.
Para poder mantener las ventajas de required, vamos a crear un objeto de tipo _Optional_ .
Con esto, podemos hacer una consulta por precio y nombre sin que alguno de estos dos tenga algún valor.
```
@GetMapping(value = "/producto")
	public ResponseEntity<?> buscarProductosPorVarios(
			@RequestParam("nombre") Optional<String> txt,
			@RequestParam("precio") Optional<Float> precio,
			Pageable pageable, HttpServletRequest request) {
		
		Page<Producto> result = productoServicio.findByArgs(txt, precio, pageable);
	
		if (result.isEmpty()) {
			throw new SearchProductoNoResultException();
		} else {
			Page<ProductoDTO> dtoList = result.map(productoDTOConverter::convertToDto);
		}
	}	
```

#### Soporte para XML
Para soportar XML , no solo como respuesta sino tambien como contenido, basta con incorporar la librería _Jackson Data Media XML_ en el
archivo _pomp.xml_ .
```
<dependency>
    <groupId>com.fasterxml.jackson.dataformat</groupId>
    <artifactId>jackson-dataformat-xml</artifactId>
</dependency>
```
Después de esto Jackson se encargará de tratar como XML todos los recuros que entren o salgan de la aplicación señalados como tipo
``` application/xml ``` , tanto en _AcceptType_, como en _ContentType_ .

## 01/05/2020
Hoy voy con la parte de modelos de datos más complejos.

#### Uso del patrón DTO en peticiones y respuestas.
En JPA tenemos la anotación _@Entity_ para seleccionar un objeto, como una entidad de base de datos , en las que las propiedades del objeto
serían las columnas de la entidad. No es bueno que se usen las entidades en la lógica de  negocio, par ello usamos _DTO_.
_DataTranferObject_ es un objeto que puede tener datos de:
- Una entidad.
- Varias entidades distintas.
- Varias entidades relacionadas.
- Agrupar datos de todas las entidades.
El _DTO_ será el objeto que se usará para gestionar los datos de una entidad para usarlos en la lógica de negocio de nuestra aplicación.
Mientras que las clases _Entity_, serán usadas unicamente para recuperar datos de la base de datos.
```
@GetMapping("/producto")
	public ResponseEntity<?> obtenerTodos() {
		List<Producto> result = productoRepositorio.findAll();

		if (result.isEmpty()) {
			return ResponseEntity.notFound().build();
		} else {

			List<ProductoDTO> dtoList = result.stream().map(productoDTOConverter::convertToDto)
					.collect(Collectors.toList());

			return ResponseEntity.ok(dtoList);
		}
	}
```

Como podemos observar en el método viso anteriormente, en  la variable _result_, se devuelven una lista de entidades Producto, que luego
mapeamos a DTO, que devolvemos como respuesta al cliente.

#### Ajustando nuestras clases con JSONView
_JSONView_ es una herramienta de la librería _Jackson2_ que nos permite elegir que campos de un objeto serán transformados a JSON.
Se puede usar tanto con DTOs como con Entidades.
Para crearlas, creamos un objeto que tendrá varias inerfaces, las cuales serán las vistas a utilizar.
```
public class ProductoViews {
	
	public interface Dto { }
	public interface DtoConPrecio extends Dto { }

}
```
Despúes en un objeto DTO o Entity, le indicamos con el tag _@JsonView_ , las propiedades que queremos que se usen en las vistas y en
que vistas se utilizarán. Las vistas que hereden de otras tendrán los elementos de la clase padre.
```
public class ProductoDTO {
	
	@JsonView(ProductoViews.Dto.class)
	private long id;
	@JsonView(ProductoViews.Dto.class)
	private String nombre;
	@JsonView(ProductoViews.Dto.class)
	private String imagen;
	@JsonView(ProductoViews.DtoConPrecio.class)
	private float precio;
	@JsonView(ProductoViews.Dto.class)
	private String categoria;
}
```

Por último, hay que indicar en el conttrolador a nivél de método, que vista queremos utilizar con el tag _@JsonView_ .

```
@JsonView(ProductoViews.DtoConPrecio.class)
	@GetMapping(value = "/producto")
	public ResponseEntity<?> buscarProductosPorVarios(@RequestParam("nombre") Optional<String> txt,
			@RequestParam("precio") Optional<Float> precio, Pageable pageable, HttpServletRequest request) {

		Page<Producto> result = productoServicio.findByArgs(txt, precio, pageable);

		if (result.isEmpty()) {
			throw new SearchProductoNoResultException();
		} else {

			Page<ProductoDTO> dtoList = result.map(productoDTOConverter::convertToDto);
			UriComponentsBuilder uriBuilder = UriComponentsBuilder.fromHttpUrl(request.getRequestURL().toString());

			return ResponseEntity.ok().header("link", paginationLinksUtils.createLinkHeader(dtoList, uriBuilder))
					.body(dtoList);

		}

	}
```

#### Asociaciones Many To One
Según el tipo de relaciones entre entidades tratar el tipo de modelo puede ser mas o menos comlicado. Empezamos a ver las relaciones
Many to One (N -> 1 en bases de datos).
Se suele variar la respuesta de las peticiones en el caso del tipo de relación. En el controlador de Productos, como podemos ver
a lo largo del proyecto la respuesta de los diferentes CRUD ha sido:
- GET de todos los productos -> DTO.
- GET por id -> Objeto completo.
- POST -> DTO del objeto creado.
- PUT -> DTO del objeto modificado.
- DELETE -> Sin cuerpo de resupuesta puesto que , con el 204, nos confirma que el objeto ha sido eliminado.
Implementamos esto en productos, que tiene una asiciacion many to one con categoría.

#### Asociaciones One to Many
Nos se recomienda hacer este tipo de asociaciones de forma unidireccioal, puesto que suelen ser complementarios de una relación many to one
en sentido contrario. En el ejemplo anterior tenemos Producto y Categoría. Si gestionamos la relación desde una categoría (One to many ), 
tendríamos que crear tres tablas, una de Categorías, otra de Procuctos y un join entre ambas. Por lo tanto se recomienda que la 
relación sea bidireccional.
Para la entidad categoría tendríamos que poner el el join el tipo de relación y la columna a partir de la cual se van a relacionar, y ,
además , usar el tag de _Jackson2_ _@JsonBackReference_ , desde el otro lado (Entidad producto), debemos indicar que estamos manejando
un nuevo json con _@JsonManagedReference_
Esto hace que se eviten referencias circulares a la hora de crear el Json.
```
public class Categoria {

	@Id @GeneratedValue
	private Long id;
	private String nombre;
	
	@JsonBackReference
	@Join(name = "productoID")
	private Set<Producto> producto;
	
}
```
Producto.
```
public class Producto {

	@Id @GeneratedValue
	private Long id;
	
	private String nombre;
	
	private float precio;
	
	private String imagen;
	
	
	@ManyToOne
	@JsonManagedReference
	@JoinColumn(name="categoria_id")
	private Categoria categoria;
}
 ```
A nivel controlador se recomienda
- GET de todos los productos -> Dos DTO, uno pr cada entidad
- GET por id -> Dos DTO, uno pr cada entidad
- POST -> DTO del objeto a crear , que incluya también los datos de la otra entidad relacionada.
- PUT -> Usaremos el de la petición POST, aunque depende del programa , se puede variar, dependiendo de que atributos querramos modificar.
- DELETE -> Sin cuerpo de resupuesta puesto que , con el 204, nos confirma que el objeto ha sido eliminado.

#### Relaciones many to many
Como en las relaciones one to many , se recomienda tratarlas de forma bidireccional, para evitar recursión infinita.
La forma de implementación será identica.
Añadiremos a parte el tag _@JsonIdentity_, el cual añade un id a un JSON,ya sea un id artificial o un id sacado de una entidad.
Crearemos un objeto _Lote_ , el cual tendrá una relación many to many con productos.

```
@JsonIdentityInfo(generator = ObjectIdGenerators.PropertyGenerator.class, property = "id", scope = Lote.class)
public class Lote {
	
	@Id @GeneratedValue
	private Long id;
	
	private String nombre;
	
	@JsonManagedReference
	@ManyToMany(fetch = FetchType.EAGER)
	@ManyToMany
	@JoinTable(
			joinColumns = @JoinColumn(name="lote_id"),
			inverseJoinColumns = @JoinColumn(name="producto_id")
	)
	@Builder.Default
	private Set<Producto> productos = new HashSet<>();
```
Producto.
```
@JsonIdentityInfo(generator = ObjectIdGenerators.PropertyGenerator.class, property = "id", scope = Producto.class )
public class Producto {

	@Id @GeneratedValue
	private Long id;
	
	private String nombre;
	
	private float precio;
	
	private String imagen;
	
	
	@ManyToOne
	@JoinColumn(name="categoria_id")
	private Categoria categoria;
	
	@JsonBackReference
	@EqualsAndHashCode.Exclude
	@ToString.Exclude
	@ManyToMany(mappedBy="productos")
	@Builder.Default
	private Set<Lote> lotes = new HashSet<>();

}
```

## 02/05/2020

#### ¿Que es HATEOAS y HALL?
HATEOAS son las siglas de Hypermedia as the engine of application state. Esto no es un concepto que sea nuevo en REST puesto que el
protocolo HTTP se basa en Hypermedia. Esto significa que  el cliente pueda moverse por la aplicación web siguiendo las URI en formato 
hipermedia, en vez de tenr que buscar dato a dato cada en cada JSON .
Aqui tenemos una respuesta en modo JSON normal.
```
{
  "id" : 323423,
  "nombre" : "Game of Thrones",
  "categoria" : "Culebrones"
}
```
Y aquí en modo HATEOAS
```
{
  "self" : "/libros/culebrones/323423",
  "nombre" : "Game of Thrones",
  "categoria" : "/libros/culebrones",
  "autor" : {
       "nombre" : "George R. R. Martin",
       "self" : "/autores/george-r-r-martin"
  }
}
```
#### Spring Data Rest
_Spring Data Rest_ se basa en los repositorios de _Spring Data_ y expone los recursos HTTP como hipermedia (HATEOAS). Además implementa 
todas las funcionalidades de _Spring Data_ . Su versión actual (3.2.0) soporta Mogo DB ,  Neo4j,  Solr, Cassandra y Gemfire.

#### Primer ejemplo con Spring Data Rest
Haremos un proyecto nuevo, que nos informará sobre ciudades y paises.
Hay que incorporar las dependencias necesarias para que se trabaje con _Spring Data Rest_. QUe serían
- Spring Web
- Spring Data JPA
- H2
- Lombok
- REST repositories
- REST HAL repositories
Con el tag _@RepositoryRestResource(path = "ciudades", collectionResourceRel = "ciudades")_ especificamos como sería la pluralidad debido,
a que _Spring Data Rest_ hace las pluralidades en inglés y no en español.

Modelo Ciudad 
```
@Entity
public class Ciudad {

	@Id @GeneratedValue
	private Long id;
	
	private String nombre;
	
	@ManyToOne
	@JoinColumn(name = "pais_id")
	private Pais pais;
	
}
```

Modelo Pais
```
@Entity
public class Pais {

	@Id @GeneratedValue
	private Long id;
	
	private String nombre;
	
}
```

Repositorio Pais
```
public interface PaisRepositorio extends JpaRepository<Pais, Long>{}
```

Repositorio Ciudad
```
@RepositoryRestResource(path = "ciudades", collectionResourceRel = "ciudades")
public interface CiudadRepositorio extends JpaRepository<Ciudad, Long>{
```
La dependencia, _REST HAL repositories_ , tranforma automaticamente las repuestas de los modelos en hipermedia.

#### Configuración de algunos parámetros
Se pueden configurar aspectos como la Uri base, formato de salida  o detección de repositorios. 
La uri base se puede cambiar en el archivo .properties mediante la siguiente propiedad ``` spring.data.rest.base-path=/api  ```, y
Spring Data Rest automáticamente traslada el path de la api a la especificada en la property.
En cuanto a la detecciónd de repositorios, por defecto con todos, peritiene en cuenta el tag @RestResource o @RepositoryRestResource, con
la propiedad _exported_ a false. Si esta porpiedad está a false, no detectará el repositorio.
También tendremos:
- ALL , exporta todos los repositosrios.
- ANOTATION , solo detecta los repositorios con _exported_ a true.
- VISIBILITY , solo los repositorios público anotados.

Para configurarlo hay que implementar el método _configureRepositoryRestConfiguration_, de la interfaz _RepositorioRestConfigurer_ .
```
@Component
public class RestConfiguration implements RepositoryRestConfigurer {

	@Override
	public void configureRepositoryRestConfiguration(RepositoryRestConfiguration config) {
		config.setRepositoryDetectionStrategy(RepositoryDetectionStrategies.ALL);
	}
}
```
Se pueden cambiar otros parámetros mediante properties predefinidas.
```
spring.data.rest.default-page-size=2
spring.data.rest.max-page-size=100
spring.data.rest.return-body-on-create=false
```
Como podemos observar hemos cambiado el maximo de elementos en una página, solo queremos ver ds páginas, y ,cuando se crean elementos,
no se devuelve ningún cuerpo.

#### Paginación y búsqueda
Para paginar, como lo hicimos anteriormente, hay que implementar _JpaRepository_. Una gran característica de _Spring Data Rest_ , es que
integra en las uri por defecto las propiedades _sort_ , _page_ y _size_ , con lo que no tendremos que declarar los parámetros de paginación
como hicimos anteriormente. 
Por defecto, _Spring Data Rest_ oredena de manera descendente, pagina todos los resultados y además el tamaño de página por defecto es 20.
Lo único que hay que hacer entonces para paginar es implementar _JpaRepository_ y declara un objeto pageable como _Pageable_ en el
método que queramos que pagine sus resultados.

```
@RepositoryRestResource(path = "ciudades", collectionResourceRel = "ciudades")
public interface CiudadRepositorio extends JpaRepository<Ciudad, Long>{
	@RestResource(path = "nombreComienzaPor", rel = "nombreComienzaPor")
	public Page<Ciudad> findByNombreStartsWith(@Param("nombre") String nombre, Pageable p);

}
```

#### Proyecciones
Las proyecciones son algo parecido a los dto. Son propiedades de los objetos que queremos que se devuelvan en una respuesta , en vez
de que se decuelvan enteras. Ejemplo, una ciudad sin pais, solo con el nombre.
Se definen a través de una interfaz, que implementa el tag _@Proyection_, especificando que clase contiene la entidad
, y declaramos los métodos getter de las propiedades que queramos saber de una entidad.
```
@Projection(name = "ciudadSinUbicacion", types = { Ciudad.class })
public interface CiudadProj {

	String getNombre();
	
}
```
Para consumir la proyección debemos especificar en la uri de una ciudad concreta , la query _proyection=ciudadSinUbicacion_ .

#### Excrepts : Composición de modelos
Los excrepts son sustitutos de los modelos , hechos a través de sus proyecciones. Se implenta en el repositorio del modelo que queramos
modificar , con el tag @RepositoryRestResource y la propiedad _excerptProjection_ con la clase que contenga la proyección que queremos
usar en vez del modelo.

```
@RepositoryRestResource(path = "ciudades", collectionResourceRel = "ciudades", excerptProjection = CiudadProj.class)
public interface CiudadRepositorio extends JpaRepository<Ciudad, Long>{
	
	@RestResource(path = "nombreComienzaPor", rel = "nombreComienzaPor")
	public Page<Ciudad> findByNombreStartsWith(@Param("nombre") String nombre, Pageable p);

}
```


## 03/05/2020

#### Añadir una base de datos PostDegree Sql


#### Creación de un nuevo perfil de configuración


#### Monitorización con actuator


#### Dockerización de nuestra aplicación


## 09/05/2020
Hoy empiezo el último curso de la carrera de _Desarrollador de Spring_; _Curso de seguridad en tu API REST_

#### Presentación
Nos presenta el curso y los requisitos mínimos de conocimiento para realizar el mismo. Basicamente el curso trata sobre la implementación 
de seguridad en las APIs REST de distintas maneras.

#### Introducción a Spring Security
_Spring Security_ es un proyecto paragüas que incorpora varios proyectos de seguridad, que responde a dos preguntas básicas, ¿Quién eres?
(Autenticación), y ¿Qué puedes hacer?(Autorización).
La implementación de _Spring Security_ es inmediata y rápida. Solo hay que implemetar su dependencia maven en el pom.xml de la aplicación.
```
<dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-security</artifactId>
</dependency>
```
Al implmentar la dependencia, _Spring Security_ se integran en el contenedor de sevlets utlizando _Java Filters_ . Un filtro es una 
funcionalidad que se cloca entre el cliente y el servidor, que deja pasar o no algunas peticiones.
La implementación hace varias cosas por defecto:
- Implementa login y logout.
- Soporta protección contra ataques CSRF.
- Crea por defecto un usuario y contraseña.
- Habilita la configuración por defecto a través de un filtro llamado _springSecurityFilterChain_ , y se registra para todas las peticiones.
- Protege el almacenamiento de la contraseña con _BCrypt_.

#### Autenticación vs atorización
Como dijimos antes estos dos conceptos responde a dos preguntas.
- Autenticación, ¿Quién eres?. Se implenta a través de una interfaz _AutenticationManager_ , y su método _autenticate_, que devuelve tres 
cosas; null, _autenticationException_, o en caso de éxito, un objeto _Autentication_ con la propiedad _auteticated = true_ . El método
recibe un objeto _Autentication_, con el cual procederemos a autenticarnos.
Esta interfaz se implementa en _ProviderManager_, que a mayores de _AutenticationManager_, nos añade un método que nos indica si la 
instancia soporta determinados tipos de auntenticación.

- Autorización, ¿Qué puedes hacer?. Se hace hace a trevés de _AccesDecisionManager_. Hay tres implementaciones de esta interfaz pero todas
delegan en un objeto _ActionDecisionVoter_ , que es un objeto compuesto de una autenticación y un objeto seguro que es decorado a través de 
una colección de _ConfigAttributes_ . Los _ConfigAttributes_ son un conjunto de metadatos que determinan el nivel de acceso del usuario.
Se puede cnfigurar la autenticación de dos maneras:
- A nivel de método con los tags _@PreAuthorize_ , _@PostAuthorize_
- Extendiendo de la clase _WebSecurityConfigurerAdapter_

#### Clases e interfaces de Spring Security
Nos centraremos en las clases más comunes de Srpring Security además de las que hemos visto en el anterior vídeo.
- _WebSecurityConfigurerAdapter_ , clase base para una configuración básica web de aunteticación y autorización. Se usa el tag
_@EnableWebSecurity_ , para "encender" o "apagar" , el uso de la seguridad.
- _Autentication_ , extiende a _Principal_ . Un _Principal_ es  es una entidad (persona, grupo etc) que puede ser autenticado en un sistema
informático o red. Representa un token para realizar la autenticación, o para un principal una vez autenticado.
- _AutenticationManagerBuilder_ , builder para contrir objetos tipo _AutenticationManager_ . Puede guardar la configuración de usuarios 
en memoria o usando JDBC, o LDAP, o _UserDatails_.
- _UserDatails_ representa la información de un usuario, que irá dentro de un objeto _Autentication_.
- _User_ objeto modelo que incluye información de usuario obtenida de un _UserDatails_, del que pede extender directamente.
- _GrantedAutority_, interfaz que representa un privilegio individual.
- _SimpleGrantedAutority_, clase que implementa  _GrantedAutority_, almacena un string de una autority de un objeto _Autentication_.
- _UserDatailsService_ , solamenta implementa un método que carga la información de un usuario. Se utiliza como DAO(Data Acces Object).


#### Posibilidad de implementarla seguridad de una api REST
En el caso de las API REST, no hay interfaz de usuario, por lo tanto no podemos acceder mediante un login. Vamos a ver tres formas, 
aunque hay bastantes más.
- Básica (Basic).
Más elemental a través del protocolo HTTP. Simple pero poco fiable. N es muy elegante pero cumple su función. Cunado se hace una 
solicitud se pide la contraseña, que si es correcta nos da acceso.

- Json Web Tokens. No es un estandar de autenticación , sino un estarndar de creación de tokens de acceso que permiten propagar la 
identidad y privilegios. La información puede ser verificada porque está firmada digitalmente.
El proceso de autenticación es el siguiente; el usuario se loguea, se crea el token JWT y se le envía al cliente, el cliente pide la
autorización con el token como encabezado, se comprueba el token ,y , si es correcto, se devuelve el recurso.

- OAuth 2.0. Permite compartir información entre sitios sin compartir identidad, implementando diferentes fujos de autenticación 
(_autentication code flow_, _resource owner password  credential flow_ etc).  Se definen varios roles, dueño del recurso, cliente, servidor
de recursos protegidos, servidor de autorización.
El proceso es el siguiente, peticion de autorizacion del cliente , el servidor se la concede, el cliete pide el token de seguridad de un
recurso protegido, el servidor se lo da, y, con ese recurso se consigue el recurso protegido.

#### Modelo de usuario y rol
Haremos una api con el modelo Usuario; representación de la persona que utilice la api, con datos básicos, nombre , contraseña, avatar y 
roles (Será el otro modelo). Implementará la interfaz _UserDetails_.  Tendermos diferentes clase para usar nuestro modelo usuario  UserEntity, User, CreateUserDto y GetUserDto.
Vamos a proceder a crear el modelo Usuario
```
@Entity
@EntityListeners(AuditingEntityListener.class)
@Data
@Builder
public class UserEntity implements UserDetails {

	private static final long serialVersionUID = 6189678452627071360L;

	@Id
	@GeneratedValue
	private Long id;

	@Column(unique = true)
	private String username;

	private String password;

	private String avatar;

	@ElementCollection(fetch = FetchType.EAGER)
	@Enumerated(EnumType.STRING)
	private Set<UserRole> roles;

	@CreatedDate
	private LocalDateTime createdAt;

	@Builder.Default
	private LocalDateTime lastPasswordChangeAt = LocalDateTime.now();

	@Override
	public Collection<? extends GrantedAuthority> getAuthorities() {
		return roles.stream().map(ur -> new SimpleGrantedAuthority("ROLE_" + ur.name())).collect(Collectors.toList());
	}

	@Override
	public boolean isAccountNonExpired() {
		return true;
	}

	@Override
	public boolean isAccountNonLocked() {
		return true;
	}

	@Override
	public boolean isCredentialsNonExpired() {
		return true;
	}
	
	@Override
	public boolean isEnabled() {
		return true;
	}
}
```
De momento vamos a implementar los métodos de _UserDeatils_, pero sin gestionarlos. En las comprobaciones de cuenta o credenciales
devolveremos true para evitar problemas.
Y la los roles lo gestionaremos mediante un ENUM.
```
public enum UserRole {
	USER, ADMIN

}
```


#### Repositorio y servicio
Vamos a crear un repositorio para _UserEntity_. En el que buscaremos un usuario por nombre.
```
public interface UserEntityRepository extends JpaRepository<UserEntity, Long> {
	
	Optional<UserEntity> findByUsername(String username);

}
```
Crearemos un servicio base, para envolver el repositorio con el resto de operaciones CRUD. Se puede hacer los métodos CRUD directamente
pero en profesor de el curso lo ha hecho de  esta manera.

```
public abstract class BaseService<T, ID, R extends JpaRepository<T, ID>> {

	@Autowired
	protected R repositorio;
	
	public T save(T t) {
		return repositorio.save(t);
	}
	
	public Optional<T> findById(ID id) {
		return repositorio.findById(id);
	}
	
	public List<T> findAll() {
		return repositorio.findAll();
	}
	
	public T edit(T t) {
		return repositorio.save(t);
	}
	
	public void delete(T t) {
		repositorio.delete(t);
	}
	
	public void deleteById(ID id) {
		repositorio.deleteById(id);
	}	
}
```
Este servicio está hecho para cualquier entidad. Ahora tendremos que crear un servicio destinando a la gestión de usuarios que extienda
de _BaseService_
```
@Service
public class UserEntityService extends BaseService<UserEntity, Long, UserEntityRepository>{

	public Optional<UserEntity> findUserByUsername(String username) {
		return this.repositorio.findByUsername(username);
	}
}
```

#### Servicio UserDetails
Este servicio será tratado como un DAO para la gestión de usuarios, es decir, no va aautenticar ni autorizar, simplemente, va a recoger 
información de usuario. Esta información más adelante, sí que la usaremos para autenticar al usuario.

```
@Service("userDetailsService")
@RequiredArgsConstructor
public class CustomUserDetailsService implements UserDetailsService{

	private final UserEntityService userEntityService;
	
	@Override
	public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
		return userEntityService.findUserByUsername(username)
					.orElseThrow(()-> new UsernameNotFoundException(username + " no encontrado"));
	}
}
```
Vamos a implementar la configuración, pero que de momento no tenga seguridad, que ya añadiremos más adelante. El método _configure_ , 
ignorará la seguridad para las peticiones que le digamo, en este caso, todas.

```
@Configuration
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {

	@Override
	public void configure(WebSecurity web) {
		web.ignoring().anyRequest();
	}	
}
```

#### Controlador de registro
Vamos a hacer un controlador para usuarios. Su rol será basico y la contraseña será guardada en la base de datos encriptada.
Vamos a configurar un bean para encriptar la contraseña en la clase _SecurityConfig_
```
@Configuration
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {

	@Override
	public void configure(WebSecurity web) {
		web.ignoring().anyRequest();
	}

	@Bean
	public PasswordEncoder passwordEncoder() {
		return new BCryptPasswordEncoder();
	}	
}
```
Nos situamos en _serEntityService_y añadimos el bean, para usarlo en la creación de usuarios.
```
@Service
@RequiredArgsConstructor
public class UserEntityService extends BaseService<UserEntity, Long, UserEntityRepository>{
	
	@Autowired
	private final PasswordEncoder passwordEncoder;

	public Optional<UserEntity> findUserByUsername(String username) {
		return this.repositorio.findByUsername(username);
	}
	
	public UserEntity nuevoUsuario(UserEntity userEntity) {
		userEntity.setPassword(passwordEncoder.encode(userEntity.getPassword()));
		userEntity.setRoles(Stream.of(UserRole.USER).collect(Collectors.toSet()));
		return save(userEntity);
	}
}
```
Ahora que guarda la contraseña cifrada cada vez que cree un usuario, procedemos a hacer el cntrolador.
```
@RestController
@RequestMapping("/user")
public class UserController {
	@Autowired
	private final UserEntityService userEntityService;

	@PostMapping("/")
	public UserEntity nuevoUsuario(@RequestBody UserEntity newUser) {
		try {
			return userEntityService.nuevoUsuario(newUser);
		} catch (DataIntegrityViolationException ex) {
			throw new ResponseStatusException(HttpStatus.BAD_REQUEST, ex.getMessage());
		}
	}
}
```

#### Refractorización para usar DTO
Crearemos un DTO para getionar la creaciónde usuarrios, ya que nuestro modelo no es un buen candidato tanto para entrada como salida
de datos. Podríamos pedir contraseña por duplicado, asignar el rol internamente y , en la salida, devolver solo el nombre de usuario,
roles y avatar
Vamos a crear dos DTOs, uno para entrada y otro para salida, como especificamos en la presentación del modelo usuario.
CreateUser
```
@Getter @Setter
@NoArgsConstructor @AllArgsConstructor @Builder
public class CreateUserDto {
	
	private String username;
	private String avatar;
	private String password;
	private String password2;
}
```

GetUserDTO
``` 
@Getter @Setter
@uilder
public class GetUserDto {
	
	private String username;
	private String avatar;
	private Set<String> roles;
}
```

Para convertir un _UserEntity_ en un _GetUSerDto_ , vamos a crear una tercera clase para mapear los DTOs.
```
@Component
public class UserDtoConverter {
	
	public GetUserDto convertUserEntityToGetUserDto(UserEntity user) {
		return GetUserDto.builder()
				.username(user.getUsername())
				.avatar(user.getAvatar())
				.roles(user.getRoles().stream()
							.map(UserRole::name)
							.collect(Collectors.toSet())
				).build();
	}
}
```

Vamos acrear diferentes excepciones con las que controlaremos los errores que se puedan producir en la autenticación.
```
public class NewUserWithDifferentPasswordsException extends RuntimeException {

	private static final long serialVersionUID = -7978601526802035152L;

	public NewUserWithDifferentPasswordsException() {
		super("Las contraseñas no coinciden");
	}
}
```

Las excepciones la gestionaremos en un _ControllerAdvice_.
```
@RestControllerAdvice
public class GlobalControllerAdvice extends ResponseEntityExceptionHandler {
	
	@ExceptionHandler(NewUserWithDifferentPasswordsException.class)
	public ResponseEntity<ApiError> handleNewUserErrors(Exception ex) {
		return buildErrorResponseEntity(HttpStatus.BAD_REQUEST, ex.getMessage());
	}
	
	@Override
	protected ResponseEntity<Object> handleExceptionInternal(Exception ex, Object body, HttpHeaders headers,
			HttpStatus status, WebRequest request) {
		ApiError  apiError = new ApiError(status, ex.getMessage());
		return ResponseEntity.status(status).headers(headers).body(apiError);
	}
	
	private ResponseEntity<ApiError> buildErrorResponseEntity(HttpStatus status, String message) {
		return ResponseEntity.status(status)
					.body(ApiError.builder()
							.estado(status)
							.mensaje(message)
							.build());
					
	}
}
```

Ahora vamos a modificar el método _nuevoUsuario_ de el servicio _UserService_ para que al crear un usuario, compruebe las contraseñas 
y le asigne internamente un rol.
```
public UserEntity nuevoUsuario(CreateUserDto newUser) {

		if (newUser.getPassword().contentEquals(newUser.getPassword2())) {
			UserEntity userEntity = UserEntity.builder().username(newUser.getUsername())
					.password(passwordEncoder.encode(newUser.getPassword())).avatar(newUser.getAvatar())
					.roles(Stream.of(UserRole.USER).collect(Collectors.toSet())).build();
			try {
				return save(userEntity);
			} catch (DataIntegrityViolationException ex) {
				throw new ResponseStatusException(HttpStatus.BAD_REQUEST, "El nombre de usuario ya existe");
			}
		} else {
			throw new NewUserWithDifferentPasswordsException();
		}
	}
```
Por último solo hay que modificar el post del controller para modificar la creación de usuario. Ahora recibiríamos un _CreateUserDTO_ y
reponderíamos con un _GetUserDTO_.
```
@PostMapping("/")
	public GetUserDto nuevoUsuario(@RequestBody CreateUserDto newUser) {
			return userDtoConverter.convertUserEntityToGetUserDto(userEntityService.nuevoUsuario(newUser));
	}
```

#### ¿En qué consiste la seguridad básica?
Es una seguridad no pensada para canales públicos, no usa cookies y cifra los datos en base64 . Si trabajamos en REST, cualquiera los 
podría descifrar. El proceso de autenticación se hace con cotraseña, y si es correcta se recibe el recurso protegido. Si la contraseña
es incorrecta nos devuelve un codigo 401, no autorizado.


#### Implementación de la seguridad básica?
Vamos a modificar nuestro mecanismo de autenticación. Nos basaremos en el proyecto anterior, y lo añadiremos al proyecto de compra de 
productos visto en los anteriores cursos.
Vamos a sacar el password encoder de la clase de configuración para evitar dependencias circulares. Crearemos una clase nueva llamada
_PasswordEncoder_.
```
@Configuration
public class PasswordEncoderConfig {
	
	@Bean
	public PasswordEncoder passwordEncoder() {
		return new BCryptPasswordEncoder();
	}
}
```
Vamos a crear nuestro propio _BasicAutenticationEntryPoint_, que es un objeto de _Spring Security_ que se devuelve cuando hay errores en
autenticación. Es un response para errores or así decirlo, que si extendemos de él , podremos modificar su contenido.
```
@Component
@RequiredArgsConstructor
public class CustomBasicAuthenticationEntryPoint extends BasicAuthenticationEntryPoint {

	private final ObjectMapper mapper;
	
	@Override
	public void commence(HttpServletRequest request, HttpServletResponse response,
			AuthenticationException authException) throws IOException, ServletException {
		response.addHeader("WWW-Authenticate", "Basic realm='" + getRealmName() + "'");
		response.setContentType("application/json");
		response.setStatus(HttpStatus.UNAUTHORIZED.value());		
		
		ApiError apiError = new ApiError(HttpStatus.UNAUTHORIZED, authException.getMessage());
		String strApiError = mapper.writeValueAsString(apiError);
		
		PrintWriter writer = response.getWriter();
		writer.println(strApiError);
		
		
	}

	@PostConstruct
	public void initRealname() {
		setRealmName("openwebinars.net");
	}
}
```

Por último, modificamos nuestra calse de seguridad.
```

@Configuration
@EnableWebSecurity
@RequiredArgsConstructor
public class SecurityConfig extends WebSecurityConfigurerAdapter {

	private final CustomBasicAuthenticationEntryPoint customBasicAuthenticationEntryPoint;
	private final UserDetailsService userDetailsService;
	private final PasswordEncoder passwordEncoder;
	
	@Override
	protected void configure(AuthenticationManagerBuilder auth) throws Exception {
		auth.userDetailsService(userDetailsService).passwordEncoder(passwordEncoder);
	}

	@Override
	protected void configure(HttpSecurity http) throws Exception {
		http
			.httpBasic()
			.authenticationEntryPoint(customBasicAuthenticationEntryPoint)
			.and()
			.authorizeRequests()
				.antMatchers(HttpMethod.GET, "/producto/**", "/lote/**").hasRole("USER")
				.antMatchers(HttpMethod.POST, "/producto/**", "/lote/**").hasRole("ADMIN")
				.antMatchers(HttpMethod.PUT, "/producto/**").hasRole("ADMIN")
				.antMatchers(HttpMethod.DELETE, "/producto/**").hasRole("ADMIN")
				.antMatchers(HttpMethod.POST, "/pedido/**").hasAnyRole("USER","ADMIN")
				.anyRequest().authenticated()
			.and()
				.csrf().disable();				
	}
}
```
En el metodo configure que usa un _AuthenticationManagerBuilder_ hacemos la autenticación con contraseña. En es metodo configure con
_HttpSecurity_, autorizamos por roles a los usuarios a poder realizar diferentes métodos.
- Rol USER , puede hacer el método getProducto y post para hacer pedidos.
- Rol ADMIN puede hacer todos los métodos salvo getProducto.
Para terminar , hemos desabilitado la seguridad CSRF.

#### Refractorización del controlador
#### Despliegue y pruebas
Vamos a hacer un endpoint para gestionar los usuarios y vamos a gestionar los métodos del controlador para que se le apliquen los roles de usuario.
Lo primero es crear un controlador para los usuarios, va crear ususarios y además vamos a devolver la información del usuario autenticado.
```
@RestController
@RequestMapping("/user")
public class UserController { 
	
	private final UserEntityService userEntityService;
	private final UserDtoConverter userDtoConverter;
	
	@PostMapping("/")
	public GetUserDto nuevoUsuario(@RequestBody CreateUserDto newUser) {
			return userDtoConverter.convertUserEntityToGetUserDto(userEntityService.nuevoUsuario(newUser));

	}
	
	@GetMapping("/me")
	public GetUserDto yo(@AuthenticationPrincipal UserEntity user) {
		return userDtoConverter.convertUserEntityToGetUserDto(user);
	}
}
```

Ahora vamos a modificar el controlador de pedidos para que tenga en cuenta los roles de usuario. Solo modificaremos el controlador de 
pedido, el resto no.
```
@GetMapping("/")
	public ResponseEntity<?> pedidos(Pageable pageable, HttpServletRequest request,
			@AuthenticationPrincipal UserEntity user) throws PedidoNotFoundException {
		Page<Pedido> result = null;
		if (user.getRoles().contains(UserRole.ADMIN)) {
			result = pedidoServicio.findAll(pageable);
		} else {
			result = pedidoServicio.findAllByUser(user, pageable);
		}

		if (result.isEmpty()) {
			throw new PedidoNotFoundException();
		} else {
			UriComponentsBuilder uriBuilder = UriComponentsBuilder.fromHttpUrl(request.getRequestURL().toString());
			Page<GetPedidoDto> dtoPage = result.map(pedidoDtoConverter::convertPedidoToGetPedidoDto);

			return ResponseEntity.ok().header("link", paginationLinksUtils.createLinkHeader(result, uriBuilder))
					.body(dtoPage);
		}
	}
```
Como podemos ver, si es _ADMIN_, podrá consultar todos los pedidos. Si no lo es, solo podrá consultar los pedidos propios.

#### ¿En que consiste JWT?
Como dijimos antes , no es un estandar de autenticación , sino un estarndar de creación de tokens de acceso que permiten propagar la 
identidad y privilegios. La información puede ser verificada porque está firmada digitalmente.
Un token tiene tres partes codificadas en base 64. las partes están separadas por un punto.
Un ejemplo es:
```
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJPbmxpbmUgSldUIEJ1aWxkZXIiLCJpYXQiOjE1ODkwNDcyOTAsImV4cCI6MTYyMDU4MzI5MCwiYXVkIjoid3d3LmV4YW1wbGUuY29tIiwic3ViIjoianJvY2tldEBleGFtcGxlLmNvbSIsIkdpdmVuTmFtZSI6IkpvaG5ueSIsIlN1cm5hbWUiOiJSb2NrZXQiLCJFbWFpbCI6Impyb2NrZXRAZXhhbXBsZS5jb20iLCJSb2xlIjpbIk1hbmFnZXIiLCJQcm9qZWN0IEFkbWluaXN0cmF0b3IiXX0.A2dBTdSxP3V_ZZvt9yneEfbUAfkwjH6Por5tEmtnNEA
```

La última parte de la cadena se conoce como _secreto_ o firma, la cual se utiliza para la seguridad de autenticación. Es la parte que se
compara para certificar que un usuario está  entrando en un lugar en el que está identificado, ya que usuario y servidor comparten el 
token.
Se invita a usar HTTPS para que la información sea encriptada.
El proceso de autenticación es el siguiente; el usuario se loguea, se crea el token JWT y se le envía al cliente, el cliente pide la
autorización con el token como encabezado, se comprueba el token ,y , si es correcto, se devuelve el recurso.

#### Librerías necesarias
_Spring Security_ , implementa _JWT_ de forma nativa, pero ligado a _OAuth2_. Si queremos usar _JWT_ de forma independiente en nuestro proyecto debemos declarar las sigueinetes librerñias en nuestro pomp.xml.
```
<dependency>
    <groupId>io.jsonwebtoken</groupId>
    <artifactId>jjwt-impl</artifactId>
    <version>0.10.7</version>
    <scope>runtime</scope>
</dependency>

<dependency>
    <groupId>io.jsonwebtoken</groupId>
    <artifactId>jjwt-api</artifactId>
    <version>0.10.7</version>
</dependency>

<dependency>
    <groupId>io.jsonwebtoken</groupId>
    <artifactId>jjwt-jackson</artifactId>
    <version>0.10.7</version>
    <scope>runtime</scope>
</dependency>
```

#### Implementación de seguridad con JWT.
Al trabajar con JWT de manera independiente , necesitamos crear nuestro controlador de autenticación, que recogerá usuario y contraseña
y , si es válido, constrirá un token. También haremos un filtro de autorización, que recogerá un token, y, si es válido permitirá 
realizar la petición. Haremos también la seguridad a nivel de método y modificaremos el entry point personalizado.
Vamos a modificar nuestra clase _SecurityConfig_
```
@Configuration
@EnableWebSecurity
@EnableGlobalMethodSecurity(prePostEnabled = true)
public class SecurityConfig extends WebSecurityConfigurerAdapter {

	private final UserDetailsService userDetailsService;
	private final PasswordEncoder passwordEncoder;
	
	@Bean
	@Override
	public AuthenticationManager authenticationManagerBean() throws Exception {
		return super.authenticationManagerBean();
	}

	@Override
	protected void configure(AuthenticationManagerBuilder auth) throws Exception {
		auth.userDetailsService(userDetailsService).passwordEncoder(passwordEncoder);
	}

	@Override
	protected void configure(HttpSecurity http) throws Exception {
		
		http
			.csrf()
				.disable()
			.exceptionHandling()
				.authenticationEntryPoint(null) // Lo modificamos más adelante
			.and()
			.sessionManagement()
				.sessionCreationPolicy(SessionCreationPolicy.STATELESS)
			.and()
			.authorizeRequests()
				.antMatchers(HttpMethod.GET, "/producto/**", "/lote/**").hasRole("USER")
				.antMatchers(HttpMethod.POST, "/producto/**", "/lote/**").hasRole("ADMIN")
				.antMatchers(HttpMethod.PUT, "/producto/**").hasRole("ADMIN")
				.antMatchers(HttpMethod.DELETE, "/producto/**").hasRole("ADMIN")
				.antMatchers(HttpMethod.POST, "/pedido/**").hasAnyRole("USER","ADMIN")
				.anyRequest().authenticated();
		http.addFilterBefore(null, UsernamePasswordAuthenticationFilter.class);
	}
}
```
Hemos añadido el bean _AuthenticationManager_, que luego podamos tomar en el filtro. Además, en el método _configure(HttpSecurity http)_
añadimos al final del mismo, el filtro, que declararemos más adelante. Por lo demás la clase de seguridad queda exactamente igual.

#### Customización del AutenticationEntryPoint.
Vamos a moficar el _AutenticationEntryPoint_, que devolverá en los errores 401.

```
@Component
@RequiredArgsConstructor
public class CustomBasicAuthenticationEntryPoint extends BasicAuthenticationEntryPoint {

	private final ObjectMapper mapper;
	
	@Override
	public void commence(HttpServletRequest request, HttpServletResponse response,
			AuthenticationException authException) throws IOException, ServletException {
		response.addHeader("WWW-Authenticate", "Basic realm='" + getRealmName() + "'");
		response.setContentType("application/json");
		response.setStatus(HttpStatus.UNAUTHORIZED.value());		
		
		ApiError apiError = new ApiError(HttpStatus.UNAUTHORIZED, authException.getMessage());
		String strApiError = mapper.writeValueAsString(apiError);
		
		PrintWriter writer = response.getWriter();
		writer.println(strApiError);
	}

	@PostConstruct
	public void initRealname() {
		setRealmName("openwebinars.net");
	}
}
```

El esquema es similar. En este caso la diferencia es que hereda de _BasicAuthenticationEntryPoint_ . Habría que implementarlo también
en nuestra configuración de seguridad, para que devuelva ese entry point cuando un usuario no esté autorizado.
```
@Configuration
@EnableWebSecurity
@RequiredArgsConstructor
public class SecurityConfig extends WebSecurityConfigurerAdapter {

	
	private final CustomBasicAuthenticationEntryPoint customBasicAuthenticationEntryPoint;
	private final UserDetailsService userDetailsService;
	private final PasswordEncoder passwordEncoder;
	private final AccessDeniedHandler accessDeniedHandler;

	@Override
	protected void configure(AuthenticationManagerBuilder auth) throws Exception {
		auth.userDetailsService(userDetailsService).passwordEncoder(passwordEncoder);
	}

	@Override
	protected void configure(HttpSecurity http) throws Exception {
		http
			.httpBasic()
			.authenticationEntryPoint(customBasicAuthenticationEntryPoint)
			.and()
			.authorizeRequests()
				.antMatchers(HttpMethod.GET, "/producto/**", "/lote/**").hasRole("USER")
				.antMatchers(HttpMethod.POST, "/producto/**", "/lote/**").hasRole("ADMIN")
				.antMatchers(HttpMethod.PUT, "/producto/**").hasRole("ADMIN")
				.antMatchers(HttpMethod.DELETE, "/producto/**").hasRole("ADMIN")
				.antMatchers(HttpMethod.POST, "/pedido/**").hasAnyRole("USER","ADMIN")
				.anyRequest().authenticated()
			.and()
				.csrf().disable();
		
		http.exceptionHandling()
			.accessDeniedHandler(accessDeniedHandler);
		
	}
}
```

## 10/05/2020
Hoy vamos a finalizar el _Curso de seguridad con en tu API REST con Spring Boot_ ,y ,con el curso, la carrera de _Curso de desarrollador de 
Spring_.

#### Modelo de usuario y UsersDetailService
En cuanto al modelo de usuarios nos sirve el del proyecto base. Más adelante puede que necesitemos un DTO a la hora de hacer un login.
Nuestro filtro necesitará buscar usuarios por id, y para ello modificaremos la implemetación de _UsersDeatilService_, que hemos hecho
con _CustomUsersDetailService_ .
```
@Service("userDetailsService")
@RequiredArgsConstructor
public class CustomUserDetailsService implements UserDetailsService{

	private final UserEntityService userEntityService;
	
	@Override
	public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
		return userEntityService.findUserByUsername(username)
					.orElseThrow(()-> new UsernameNotFoundException(username + " no encontrado"));
	}
	
	public UserDetails loadUserById(Long id) throws UsernameNotFoundException {
		return userEntityService.findById(id)
				.orElseThrow(()-> new UsernameNotFoundException("Usuario con ID: " + id + " no encontrado"));
	}
}
```

#### Manejo del token JWT
Vamos a crear un _JWTProvider_ que se encargará de:
- Generar un token usando _Autentication_.
- Obtener el ID de usuario a partir del payload de un token.
- Verificar si el token si es válido.
Utilizaremos la librería _JWTBuilder_

```
@Component
public class JwtTokenProvider {

	public static final String TOKEN_HEADER = "Authorization";
	public static final String TOKEN_PREFIX = "Bearer ";
	public static final String TOKEN_TYPE = "JWT";

	@Value("${jwt.secret:EnUnLugarDeLaManchaDeCuyoNombreNoQuieroAcordarmeNoHaMuchoTiempoQueViviaUnHidalgo}")
	private String jwtSecreto;

	@Value("${jwt.token-expiration:86400}")
	private int jwtDuracionTokenEnSegundos;

	public String generateToken(Authentication authentication) {
		
		UserEntity user = (UserEntity) authentication.getPrincipal();
		
		Date tokenExpirationDate = new Date(System.currentTimeMillis() + (jwtDuracionTokenEnSegundos * 1000));
		
		return Jwts.builder()
				.signWith(Keys.hmacShaKeyFor(jwtSecreto.getBytes()), SignatureAlgorithm.HS512)
				.setHeaderParam("typ", TOKEN_TYPE)
				.setSubject(Long.toString(user.getId()))
				.setIssuedAt(new Date())
				.setExpiration(tokenExpirationDate)
				.claim("fullname", user.getFullName())
				.claim("roles", user.getRoles().stream()
								.map(UserRole::name)
								.collect(Collectors.joining(", "))
						)
				.compact();
	}
	
	
	public Long getUserIdFromJWT(String token) {
		Claims claims = Jwts.parser()
							.setSigningKey(Keys.hmacShaKeyFor(jwtSecreto.getBytes()))
							.parseClaimsJws(token)
							.getBody();
		
		return Long.parseLong(claims.getSubject());
	}
	
	public boolean validateToken(String authToken) {
		
		try {
			Jwts.parser().setSigningKey(jwtSecreto.getBytes()).parseClaimsJws(authToken);
			return true;
		} catch (SignatureException ex) {
			log.info("Error en la firma del token JWT: " + ex.getMessage());
		} catch (MalformedJwtException ex) {
			log.info("Token malformado: " + ex.getMessage());
		} catch (ExpiredJwtException ex) {
			log.info("El token ha expirado: " + ex.getMessage());
		} catch (UnsupportedJwtException ex) {
			log.info("Token JWT no soportado: " + ex.getMessage());
		} catch (IllegalArgumentException ex) {
			log.info("JWT claims vacío");
		}
        return false;
		
		
		
	}
}
```
Como podemos ver tenemos tres métodos para trabajar con tokens:
- _generateToken_ , construye el token con los valores especificados en las propiedades de la clase, y, a través del autentication recibido
como parámetro.
- _getUserIdFromJWT_ , obtiene una ID a través de un token.
- _validateToken_ , valida un token. Puede dar excepciones propias de JWT, como _SignatureException_ , _MalformedJwtException_ o _UnsupportedJwtException_.

#### JWT AuthenticationFilter
Utilizaremos varias clases:
- _OncePerRequestFilter_ , clase para que se procese una petición por filtro.
- _UsernamePasswordAuthenticationToken_ , una representación simple de _Authentication_ solo con usuario y contraseña.
El algoritmo del  filtro es sencillo :
1- Extraemos el toke de la petición
2- Obtenemos el ide de usuario, con ello, el ususario, y, construimos el _Autehntication_.
3- Establecemos el  _Autehntication_ como contexto de seguridad.
Si en alguno de los puntos anteriores se produce algún error, el token no pasaría el filtro.
Creamos la clase _JwtAuthorizationFilter_ , que hereda de _OncePerRequestFilter_ . 
```
@Component
@RequiredArgsConstructor
public class JwtAuthorizationFilter extends OncePerRequestFilter {

	private final JwtTokenProvider tokenProvider;
	private final CustomUserDetailsService userDetailsService;

	@Override
	protected void doFilterInternal(HttpServletRequest request, HttpServletResponse response, FilterChain filterChain)
			throws ServletException, IOException {

		try {
			String token = getJwtFromRequest(request);

			if (StringUtils.hasText(token) && tokenProvider.validateToken(token)) {
				Long userId = tokenProvider.getUserIdFromJWT(token);

				UserEntity user = (UserEntity) userDetailsService.loadUserById(userId);
				UsernamePasswordAuthenticationToken authentication = new UsernamePasswordAuthenticationToken(user,
						user.getRoles(), user.getAuthorities());
				authentication.setDetails(new WebAuthenticationDetails(request));

				SecurityContextHolder.getContext().setAuthentication(authentication);

			}
		} catch (Exception ex) {
			log.info("No se ha podido establecer la autenticación de usuario en el contexto de seguridad");
		}

		filterChain.doFilter(request, response);
	}

	private String getJwtFromRequest(HttpServletRequest request) {
		String bearerToken = request.getHeader(JwtTokenProvider.TOKEN_HEADER);
		if (StringUtils.hasText(bearerToken) && bearerToken.startsWith(JwtTokenProvider.TOKEN_PREFIX)) {
			return bearerToken.substring(JwtTokenProvider.TOKEN_PREFIX.length(), bearerToken.length());
		}
		return null;
	}
}
```
Haremos también un método auxiliar _getJwtFromRequest_ , el cual se encarga de extraer un token de una petición HTTP, lo valida, y
lo devuelve para que podamos trabajar con el.
Como podemos ver, el algoritmo que especificamos se implementa en el método _doFilterInternal_ ,el cual, como explicamos, usa
_UsernamePasswordAuthenticationToken_ , para trabajar con una autenticación simple con usuario y contraseña.
Anteriormente habíamos definido en _SecurityConfig_ , una instrucción para que las peticiones HTTP pasasen por un filtro. Ahora que 
ya definimos el filtro, hay que especificarlo.
Creamos la propiedad.
```
@Autowired
private JwtAutorizationFilter jwtAutorizationFilter;
```
Implementamos su uso
```
http.addFilterBefore(jwtAutorizationFilter, UsernamePasswordAuthenticationFilter.class);
```


#### Modelo para el login y respuesta.
#### Despliegue y prubas con JWT
Implementaremos el modelo de login y respuesta que luego usaremos en un controlador.
Vamos a extender de la clase ya creada _GetUserDTO_ , para crear un usuario añadiendole el token, que será nuestra respuesta.
```
@Getter
@Setter
public class JwtUserResponse extends GetUserDto {
	
	private String token;
	
	@Builder(builderMethodName="jwtUserResponseBuilder")
	public JwtUserResponse(String username, String avatar, String fullName, String email, Set<String> roles, String token) {
		super(username, avatar, fullName, email, roles);
		this.token = token;
	}
}
```

Para recoger un objeto de petición de login, crearemos un objeto simple con usuario y contraseña, pero que los tenga que implementar si o si.
```
@Getter
@NoArgsConstructor
public class LoginRequest {
	@NotBlank
	private String username;
	@NotBlank
	private String password;

}
```

#### Refractorización del controlador
Vamos a crear un controlador para hacert el login.
```
@RestController
@RequiredArgsConstructor
public class AuthenticationController {
	
	private final AuthenticationManager authenticationManager;
	private final JwtTokenProvider tokenProvider;
	private final UserDtoConverter converter;
	
	@PostMapping("/auth/login")
	public JwtUserResponse login(@Valid @RequestBody LoginRequest loginRequest) {
		Authentication authentication = 
				authenticationManager.authenticate(
					new UsernamePasswordAuthenticationToken(
							loginRequest.getUsername(),
							loginRequest.getPassword()
							
						)							
				);
		
		SecurityContextHolder.getContext().setAuthentication(authentication);
		UserEntity user = (UserEntity) authentication.getPrincipal();
		String jwtToken = tokenProvider.generateToken(authentication);
		
		return convertUserEntityAndTokenToJwtUserResponse(user, jwtToken);		
	}
	
	@PreAuthorize("isAuthenticated()")
	@GetMapping("/user/me")
	public GetUserDto me(@AuthenticationPrincipal UserEntity user) {
		return converter.convertUserEntityToGetUserDto(user);
	}
	
	private JwtUserResponse convertUserEntityAndTokenToJwtUserResponse(UserEntity user, String jwtToken) {
		return JwtUserResponse
				.jwtUserResponseBuilder()
				.fullName(user.getFullName())
				.email(user.getEmail())
				.username(user.getUsername())
				.avatar(user.getAvatar())
				.roles(user.getRoles().stream().map(UserRole::name).collect(Collectors.toSet()))
				.token(jwtToken)
				.build();
	}
}
```
Como podemos ver es un controlador con dos peticiones.
- _login_ recibe un _loginRequest_ y a través del _AutenticationManager_, crea un _Authentication_ simple a través del usuario y
contraseña del  _loginRequest_. A través del _Authentication_ creamos un _UserEntity_ y le asignamos un token, el cual usarermos
para devolver un _JwtUserResponse_.
- me, le pasamos un _UserEntity_ , el cual ns dice si está autenticado o no.

#### ¿En qué consiste OAuth2?
Como dijimos anteriormente _OAuth2_ rermite compartir información entre sitios sin compartir identidad, implementando diferentes fujos de 
autenticación (_autentication code flow_, _resource owner password  credential flow_ etc).  Se definen varios roles, dueño del recurso, 
cliente, servidor de recursos protegidos, servidor de autorización. El proceso es el siguiente, peticion de autorizacion del cliente , el 
servidor se la concede, el cliete pide el token de seguridad de un recurso protegido, el servidor se lo da, y, con ese recurso se consigue 
el recurso protegido.
Surge de la necesidad de paliar el envio de credenciales entre cliente y servidor. El usuario delega la capacidad de realizar ciertas 
operaciones en su nombre, y, no tenemos ala necesidad de almacenar usuario y contraseña.
Cabe espeficicar que OAuth2 es un controlador de acceso , no de autenticación.
Usada por Twitter o Google cuando usamos aplicaciones de terceros.

#### Roles
Se definen 4 roles:
- Dueño del recurso, dueño de la información que se va a manejar.
- Cliente, la aplicación que desea acceder a la cuenta de ususario, siendo autorizado.
- Servidor de autorización, responsable de las peticiones de autorización. Autentica mediante tokens de acceso (No son tokens JWT).
- Servidor de recursos, aloja los recursos protegidos.

#### Flujo abstracto del protocolo
Es abstracto ya que no hay un único flujo en el protocolo. Normalmente se hacen 7 pasos.
1- Petición de autenticación.
2- El servidor de autenticación, nos identifica (login u otros métodos). El usuario debe dar el consentimiento (permisos) de lo que la
aplicación quiere hacer.
3- Se devuelve un código de autorización. (Incluye consentimientos).
4- Con ese código, solicitamos un token al servidor de autorización.
5- Se devuelve un token de acceso.
6- Solicitud de recurso protegido utilizando un token.
7- Respuesta del servidor con los recursos solicitados.
- _Scopes_ o ámbitos, consentimientos que el usuario concede a la aplicación.

#### Grant Types
Los _Grant Types_ son las diferentes formas de obtener un token.
Dos tipos;
- Clientes confidenciales. Capaces de guardar contraseñas sin que sean expuestas.
- Clientes públicos. No soncapaces de guardar contraseñas y mantenerlas a salvo.

Los _Grant Types_ que vamos a utilizar son:
- Authorization code, mismo flujo que el fujo abstrcto visto anteriormente. Es el más completo. Utilizado en clientes confidenciales. El
token se obtiene a partir de un código(repsuesta) pasandole como parametros un id, una url de redirección y los scopes.
- Implicit, usado en clientes públicos. Es lo mismo que el anterior pero recibe un token en vez de un código.
- Password, usado cuendo no haya otra alternativa de flujo. Se hace en aplicaciones que tienen la confianza de sus clientes. Solo se le
pasa usuario y contraseña y se recibe un token.

#### Implementando el servidor de autorización
Para implementarlo hay que primero incorporar su dependencia.
```
<dependency>
    <groupId>org.springframework.security.oauth.boot</groupId>    <artifactId>spring-security-oauth2-autoconfigure</artifactId>
    <version>2.X.Y.RELEASE</version>
</dependency>
```
Vamos a ahacer la primera parte del proceso. Petición de autorizaciones y concesiones de tokens (Pasos 1-5).
El servidor de autorización extenderá de _AuthorizationServerConfigurerAdapter_, y vamoa a configurar:
- Clientes y sus características.
- Seguridad del token.
- Conexión con el modelo de autorización.
El proyecto base será el mismo proyecto base que hemos utilizado para implemntar los otros dos proyectos de las ditintas formas de 
seguridad del curso (Básica y JWT).
Procedemos a crear el servidor de autorización.
```@Configuration
@EnableAuthorizationServer
@RequiredArgsConstructor
public class OAuth2AuthorizationServer extends AuthorizationServerConfigurerAdapter{

	private final PasswordEncoder passwordEncoder;
	private final AuthenticationManager authenticationManager;
	private final UserDetailsService userDetailsService;
	
	@Value("${oauth2.client-id}")
	private String clientId;
	
	@Value("${oauth2.client-secret}")
	private String clientSecret;
	
	@Value("${oauth2.redirect-uri}")
	private String redirectUri;
	
	@Value("${oauth2.access-token-validity-seconds}")
	private int accessTokenValiditySeconds;
	
	@Value("${oauth2.access-token-validity-seconds}")
	private int refreshTokenValiditySeconds;

	private static final String CODE_GRANT_TYPE = "authorization_code";
	private static final String IMPLICIT_GRANT_TYPE = "implicit";
	private static final String PASS_GRANT_TYPE = "password";
	private static final String REFRESH_TOKEN_GRANT_TYPE = "refresh_token";

	@Override
	public void configure(AuthorizationServerSecurityConfigurer security) throws Exception {
		security
			.tokenKeyAccess("permitAll()")
			.checkTokenAccess("isAuthenticated()")
			.allowFormAuthenticationForClients();
	}

	@Override
	public void configure(ClientDetailsServiceConfigurer clients) throws Exception {
		clients
			.inMemory()
			.withClient(clientId)
			.secret(passwordEncoder.encode(clientSecret))
			.authorizedGrantTypes(PASS_GRANT_TYPE, REFRESH_TOKEN_GRANT_TYPE, IMPLICIT_GRANT_TYPE, CODE_GRANT_TYPE)
			.authorities("READ_ONLY_CLIENT")
			.scopes("read")
			.resourceIds("oauth2-resource")
			.redirectUris(redirectUri)
			.accessTokenValiditySeconds(accessTokenValiditySeconds)
			.refreshTokenValiditySeconds(refreshTokenValiditySeconds);
			
	
	}

	@Override
	public void configure(AuthorizationServerEndpointsConfigurer endpoints) throws Exception {
		endpoints.authenticationManager(authenticationManager).userDetailsService(userDetailsService);
	}	
}
```
Hemos configurado las propiedades(desde el fichero properties), _clientId_, _clientSecret_, _redirectUri_, _accessTokenValiditySeconds_ y
_refreshTokenValiditySeconds_, que usaremos para constuir y validar el token. Las propiedades las definimos en _application.properties_.
```
oauth2.client-id=cliente
oauth2.client-secret=123456
oauth2.redirect-uri=http://localhost:8081/login
# Un día
oauth2.access-token-validity-seconds=86400
# Un día y medio
oauth2.refresh-token-validity-seconds=129600
```
Tenemos difierentes métodos _configure_
- _ClientDetailsServiceConfigurer_ , para registrar clientes, con sus grant types, secreto y scopes.
- _AuthorizationServerSecurityConfigurer_ , da permisos a todas las peticiones una vez que estén autenticadas.
- _AuthorizationServerEndpointsConfigurer_ , crea los endpoints para para acceder al servidor de autorización.

Después de crear el servidor de autorización, tendremos que hacer los siguientes cambios en _SpringSecurity_.

```
@Configuration
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {

	private final PasswordEncoder passwordEncoder;
	private final UserDetailsService userDetailsService;
	
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http.requestMatchers()
          .antMatchers("/login", "/oauth/authorize")
          .and()
          .authorizeRequests()
          .anyRequest().authenticated()
          .and()
          .formLogin().permitAll();
    }
 
    @Override
    protected void configure(AuthenticationManagerBuilder auth) throws Exception {    	
    	auth.userDetailsService(userDetailsService).passwordEncoder(passwordEncoder);
    }

    @Bean(BeanIds.AUTHENTICATION_MANAGER)
    @Override
	public AuthenticationManager authenticationManagerBean() throws Exception {
		return super.authenticationManagerBean();
	}
}
```

En _configure(HttpSecurity http)_ ponemos el endpoint del servidor de autorización que por defecto, la librería OAuth2 lo crea en 
_/oauth/authorize_.

#### Implementando el servidor de recursos
Se centrará en enviar el token y recibir el recurso.
```
@Configuration
@EnableResourceServer
public class OAuth2ResourceServer extends ResourceServerConfigurerAdapter {

	@Override
	public void configure(ResourceServerSecurityConfigurer resources) throws Exception {
		resources.resourceId("oauth2-resource");
	}

	@Override
	public void configure(HttpSecurity http) throws Exception {

		http
		.csrf()
			.disable()
		.sessionManagement()
			.sessionCreationPolicy(SessionCreationPolicy.STATELESS)
		.and()
		.authorizeRequests()
			//.antMatchers(HttpMethod.POST, "/oauth/token").permitAll()
			.antMatchers(HttpMethod.GET, "/producto/**", "/lote/**").hasRole("USER")
			.antMatchers(HttpMethod.POST, "/producto/**", "/lote/**").hasRole("ADMIN")
			.antMatchers(HttpMethod.PUT, "/producto/**").hasRole("ADMIN")
			.antMatchers(HttpMethod.DELETE, "/producto/**").hasRole("ADMIN")
			.antMatchers(HttpMethod.POST, "/pedido/**").hasAnyRole("USER","ADMIN")
			.anyRequest().authenticated();

	
	}
}
```
En _configure(HttpSecurity http)_ configuramos el servidor, donde le espicificamos que no cree sesiones y deshabilite CSRF.
Después ñe indicamos, como en los dos proyectos anteriores, a los endpoints que puede acceder como _USER_ y como _ADMIN_.

#### Reconfigurando CORS Despliegue y prueba de ejecución
Vamos a eliminar la configuración heredada. Crearemos un filtro y le daremos prioridad.
Se creará un clase _SimpleCORSFilter_  y le damoa la prioridad más alta con el tag _@Order_.

```
@Component
@Order(Ordered.HIGHEST_PRECEDENCE)
public class SimpleCorsFilter implements Filter {

    @Override
    public void doFilter(ServletRequest req, ServletResponse res, FilterChain chain) throws IOException, ServletException {
        final HttpServletResponse response = (HttpServletResponse) res;
        response.setHeader("Access-Control-Allow-Origin", "*");
        response.setHeader("Access-Control-Allow-Methods", "POST, PUT, GET, OPTIONS, DELETE");
        response.setHeader("Access-Control-Allow-Headers", "Authorization, Content-Type");
        response.setHeader("Access-Control-Max-Age", "3600");
        if (HttpMethod.OPTIONS.name().equalsIgnoreCase(((HttpServletRequest) req).getMethod())) {
            response.setStatus(HttpServletResponse.SC_OK);
        } else {
            chain.doFilter(req, res);
        }
    }

    @Override
    public void destroy() {
    }

    @Override
    public void init(FilterConfig config) throws ServletException {
    }
}
```
Definimos.
- Acceso a todos los endpoints
- Acceso a alos métodos POST, PUT, GET, OPTIONS, DELETE
- Cabeceras con Content-Type
- Máxima duración de 3600 segundos por pétición
Por último en la clase _SecurityConfig_ , donde deshabilitamos CSRF, habilitamos CORS.
```
http.cors().and().csrf().disable()
```

#### Tokens en base de datos
Hasta ahora se han almacenado los tokens en memoria. La propia Srping Secutity OAuth2, en su pagina nos provee con el siguiente esquema
que podemos copiar y pegar tal cual, ya que es el que usaremos.
```
-- used in tests that use HSQL
create table oauth_client_details (
  client_id VARCHAR(256) PRIMARY KEY,
  resource_ids VARCHAR(256),
  client_secret VARCHAR(256),
  scope VARCHAR(256),
  authorized_grant_types VARCHAR(256),
  web_server_redirect_uri VARCHAR(256),
  authorities VARCHAR(256),
  access_token_validity INTEGER,
  refresh_token_validity INTEGER,
  additional_information VARCHAR(4096),
  autoapprove VARCHAR(256)
);

create table oauth_client_token (
  token_id VARCHAR(256),
  token LONGVARBINARY,
  authentication_id VARCHAR(256) PRIMARY KEY,
  user_name VARCHAR(256),
  client_id VARCHAR(256)
);

create table oauth_access_token (
  token_id VARCHAR(256),
  token LONGVARBINARY,
  authentication_id VARCHAR(256) PRIMARY KEY,
  user_name VARCHAR(256),
  client_id VARCHAR(256),
  authentication LONGVARBINARY,
  refresh_token VARCHAR(256)
);

create table oauth_refresh_token (
  token_id VARCHAR(256),
  token LONGVARBINARY,
  authentication LONGVARBINARY
);

create table oauth_code (
  code VARCHAR(256), authentication LONGVARBINARY
);

create table oauth_approvals (
    userId VARCHAR(256),
    clientId VARCHAR(256),
    scope VARCHAR(256),
    status VARCHAR(10),
    expiresAt TIMESTAMP,
    lastModifiedAt TIMESTAMP
);


-- customized oauth_client_details table
create table ClientDetails (
  appId VARCHAR(256) PRIMARY KEY,
  resourceIds VARCHAR(256),
  appSecret VARCHAR(256),
  scope VARCHAR(256),
  grantTypes VARCHAR(256),
  redirectUrl VARCHAR(256),
  authorities VARCHAR(256),
  access_token_validity INTEGER,
  refresh_token_validity INTEGER,
  additionalInformation VARCHAR(4096),
  autoApproveScopes VARCHAR(256)
);
```
Para poder trabajar con bases de datos, vamos a habilitar H2 , utlizado anteriormente con _Spring Data_.
Para configurar la base de datos, añadimos las properties propias de hibernate.
```
# Subida de ficheros
upload.root-location=upload-dir
spring.jackson.mapper.default-view-inclusion=true
spring.jpa.show-sql=true
spring.datasource.url=jdbc:h2:./db/basededatos
spring.datasource.username=sa
spring.datasource.password=
spring.jpa.hibernate.ddl-auto=create-drop
spring.h2.console.enabled=true
```
En el servidor de autenticación creamos un data source para almacenar los tokens de los clientes en bases de datos mediante JDBC. Y
creamos un bean para poder manejar los tokens almacenados.
```
@Configuration
@EnableAuthorizationServer
@RequiredArgsConstructor
public class OAuth2AuthorizationServer extends AuthorizationServerConfigurerAdapter{

	private final PasswordEncoder passwordEncoder;
	private final AuthenticationManager authenticationManager;
	private final UserDetailsService userDetailsService;
	private final DataSource dataSource;
	
	@Value("${oauth2.client-id}")
	private String clientId;
	
	@Value("${oauth2.client-secret}")
	private String clientSecret;
	
	@Value("${oauth2.redirect-uri}")
	private String redirectUri;
	
	@Value("${oauth2.access-token-validity-seconds}")
	private int accessTokenValiditySeconds;
	
	@Value("${oauth2.access-token-validity-seconds}")
	private int refreshTokenValiditySeconds;
	
	

	
	private static final String CODE_GRANT_TYPE = "authorization_code";
	private static final String IMPLICIT_GRANT_TYPE = "implicit";
	private static final String PASS_GRANT_TYPE = "password";
	private static final String REFRESH_TOKEN_GRANT_TYPE = "refresh_token";
	
	@Override
	public void configure(AuthorizationServerSecurityConfigurer security) throws Exception {
		security
			.tokenKeyAccess("permitAll()")
			.checkTokenAccess("isAuthenticated()")
			.allowFormAuthenticationForClients();

	}

	@Override
	public void configure(ClientDetailsServiceConfigurer clients) throws Exception {
		clients
			//.inMemory()
			.jdbc(dataSource)
			.withClient(clientId)
			.secret(passwordEncoder.encode(clientSecret))
			.authorizedGrantTypes(PASS_GRANT_TYPE, REFRESH_TOKEN_GRANT_TYPE, IMPLICIT_GRANT_TYPE, CODE_GRANT_TYPE)
			.authorities("READ_ONLY_CLIENT")
			.scopes("read")
			.resourceIds("oauth2-resource")
			.redirectUris(redirectUri)
			.accessTokenValiditySeconds(accessTokenValiditySeconds)
			.refreshTokenValiditySeconds(refreshTokenValiditySeconds);
	}

	@Override
	public void configure(AuthorizationServerEndpointsConfigurer endpoints) throws Exception {
		endpoints
			.authenticationManager(authenticationManager)
			.userDetailsService(userDetailsService)
			.tokenStore(tokenStore());
	}
	
	@Bean
	public TokenStore tokenStore() {
		return new JdbcTokenStore(dataSource);
	}
}
```
Con est el servidor guardará los tokens en base de datos , en las tablas especificadas en el esquema. 
Podemos cambiar el esquema proporcionado por Spring para adaptarlo a nuestra aplicación, pero no es recomendable. Para ello , es preferible
hacer nuestro propio esquema



#### Integrando OAuth2 y JWT
Vamos a cambiar los tokens de acceso por tokens tipo JWT. 
Lo haremos mediante la interfaz _JWTAccessTokensConverter_, que transforma un token de accesso en JWT.
Lo único que hay que hacer es implementar dicha interfaz en el servidor de autorización.
```
@Configuration
@EnableAuthorizationServer
@RequiredArgsConstructor
public class OAuth2AuthorizationServer extends AuthorizationServerConfigurerAdapter{

	private final PasswordEncoder passwordEncoder;
	private final AuthenticationManager authenticationManager;
	private final UserDetailsService userDetailsService;
	private final DataSource dataSource;
	
	@Value("${oauth2.client-id}")
	private String clientId;
	
	@Value("${oauth2.client-secret}")
	private String clientSecret;
	
	@Value("${oauth2.redirect-uri}")
	private String redirectUri;
	
	@Value("${oauth2.access-token-validity-seconds}")
	private int accessTokenValiditySeconds;
	
	@Value("${oauth2.access-token-validity-seconds}")
	private int refreshTokenValiditySeconds;
	
	

	
	private static final String CODE_GRANT_TYPE = "authorization_code";
	private static final String IMPLICIT_GRANT_TYPE = "implicit";
	private static final String PASS_GRANT_TYPE = "password";
	private static final String REFRESH_TOKEN_GRANT_TYPE = "refresh_token";
	
	

	@Override
	public void configure(AuthorizationServerSecurityConfigurer security) throws Exception {
		security
			.tokenKeyAccess("permitAll()")
			.checkTokenAccess("isAuthenticated()")
			.allowFormAuthenticationForClients();

	}

	@Override
	public void configure(ClientDetailsServiceConfigurer clients) throws Exception {
		clients
			.jdbc(dataSource)
			.withClient(clientId)
			.secret(passwordEncoder.encode(clientSecret))
			.authorizedGrantTypes(PASS_GRANT_TYPE, REFRESH_TOKEN_GRANT_TYPE, IMPLICIT_GRANT_TYPE, CODE_GRANT_TYPE)
			.authorities("READ_ONLY_CLIENT")
			.scopes("read")
			.resourceIds("oauth2-resource")
			.redirectUris(redirectUri)
			.accessTokenValiditySeconds(accessTokenValiditySeconds)
			.refreshTokenValiditySeconds(refreshTokenValiditySeconds);
			
	
	}

	@Override
	public void configure(AuthorizationServerEndpointsConfigurer endpoints) throws Exception {
		endpoints
			.authenticationManager(authenticationManager)
			.userDetailsService(userDetailsService)
			.tokenStore(tokenStore())
			.accessTokenConverter(accessTokenConverter());
	}
	
	@Bean
	public TokenStore tokenStore() {
		return new JdbcTokenStore(dataSource);
	}
	
	@Bean
	public AccessTokenConverter accessTokenConverter() {
		return new JwtAccessTokenConverter();
	}
}
```
Por último, señalamos el convertidor de tokens en el método _configure(AuthorizationServerEndpointsConfigurer endpoints)_ ,  y, cada 
vez que usemos un token, el servidor nos lo pasará automáticamente como token de acceso.

Con esto finaliza el _Curso de seguridad con en tu API REST con Spring Boot_, la carrera de _Curso de desarrollador de Spring_.






