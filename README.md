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



## 30/03/2020

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
