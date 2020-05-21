# Learn Spring
Spring Framework 5 Begginer to guru Udemy course
Spring Framework 5: Spring Framework 5, Spring Boot 2, Spring MVC, Spring Data JPA, Spring Data MongoDB, Hibernate

- Install: Java, Maven, Gradle, IntelliJ Ultimate IDE

### Spring initializr

https://github.com/spring-io/initializr
Generate a backbone of a project here: start.spring.io

*Spring boot starters - those are dependencies
https://github.com/spring-projects/spring-boot/tree/master/spring-boot-project/spring-boot-starters

*Choose Maven, Java, Don't choose snapchot version of Spring boot, pack as jar, Java 8,

* Documentation: https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#boot-features-developing-web-applications

### Simple Web App

  * Choose as dependencie: Spring Web, Spring data JPA, H2 database\
    - File looks like: pom.xml (mvn), src, package, Application class, resources (stataic -web), test
    
  * Open in IntellJ - File -> New -> Project from existing sources -> Select the pom.xml file
  - click pom.xml - spring boot dependencies and see how many they are
  - click Maven intelij idea tool (tab) and look at dependecies
  - Help.md - all links to help pages
  - gitignore
  
  * You have Initializr inside IntelliJ Ultimate - File-> New -> Project -> Spring Initializr. Choose java version, name/
     - Web -> Spring Web
     - SQL -> Spring Data JPA
     - SQL -> H2 DB
     
     Tip: to reinit a project from pom.xml got to pom.xml right click and select add as maven project
     
   * Git tips
   Fork the original repo and clone it from IntellJ IDE, 
   In the same folder execute command: git remote add originalRepo [link to original repo]
   Execute fetch
   Now you can compare from both sources: forked and original one
   
   * JPA Entities
     - Java Persistence Api - Hibernate. Very broad theme.
   
      Tip: JDL-Studio for UML diagrams
   
     - Author* -> Book * (Many to many relationship)   *_*
     - Put entities classes at package "domain"
     - Create Author and Book classes with getter and setters. JPA requires an empty constructor. Annotate the class with @Entity, provide Long field id with geter and setter and annotate it with @Id and @GeneratedValue(strategy= GenerationType.Auto) - this will auto increment the ids
     - Set up many to many relationship: Books class has Set<Author> authors, Author object has Set<Book> books. Above books add annotation @ManyToMany(mappedBy = "authors") - this means that relationship will be described with Book.authors set annotation.
    Above Book.authors add @ManyToMany and @JoinTable(name="author_book", joinColumns = @JoinColumn(name="book_id"), inverseJoinColumns = @JoinColumn(name="author_id"))
  - Hibernate wants us to override equals (and hashCode) methods based on id. Use IntellJ generate hashCode and equals nad select only id field. Generate toString also.
 
 * Spring Data Repository
 - Create package repositories
 - Create two interfaces: BookRepository, AuthorRepository that inherit CrudRepository<Book, Long> (Author for author)
 
 * Initializing data
 - Create package bootstrap and class inside BootstrapData that implements CommandLineRunner and have constructor
  public BootstrapData(AuthorRepository authorRepository, BookRepository bookRepository) {
        this.authorRepository = authorRepository;
        this.bookRepository = bookRepository;
    }
  - Add annotation @Component 
  - Save data as 
  @Override
    public void run(String... args) throws Exception {
   authorRepository.save(oneAuthor);
   }
   
   * One to Many relationship
   - Create class Publisher. A book has one publisher, but a publisher has many books
   - In Publisher add annotation:
    @OneToMany
    @JoinColumn(name="publisher_id")
    
    - In Book add
      @ManyToOne
    private Publisher publisher;
    
    * h2 database
    - add to application.properties

spring.h2.console.enabled=true
spring.datasource.url=jdbc:h2:mem:testdb
spring.datasource.driverClassName=org.h2.Driver
spring.datasource.username=sa
spring.datasource.password=password
spring.jpa.database-platform=org.hibernate.dialect.H2Dialect

Then go to http://localhost:8080/h2-console and enter as JDBC driver url jdbc:h2:mem:testdb and user su/password

* MVC architecture
Controllers get receive requests from the user
* Create controller
add folder controller and a class BookController
annotate with @Controller, add method getBooks() and annotate it with @RequestMapping("/books")
- Add constructor that receives Repository
- Add parameter module Module to getBooks method and inside add module.setAttribute("books",repository.findAll)
* Create View
- Check for Spring boot tymeleaf dependency
- Create folder/file: resources/templates/book/list.html
- Add namespace to <html tag xmlns:th="http://www.thymeleaf.org"
- Use thimeleaf th:each="book : ${books} and th:text="${books.id}"

* Spring Pet Clinick application
https://github.com/i-tanova/spring-petclinic
git clone https://github.com/spring-projects/spring-petclinic.git
cd spring-petclinic
./mvnw package
java -jar target/*.jar

* Start new project
- Create git repo
- Create InteliJ IDA Sprng project using Core> Lambog, Dev tools, Web>Web
Template> Thymelief SQL> JPA,H2, mySQL Ops>Actuator

* SOLID prnciples
 - Single responsibility - Small classes, every class - one responsibility, Avoid God objects, Split to smaller classes
 - Open/Closed principle - open for extension, closed for modificaion, Use abstract base classes
 - Liskov substitude principle - looks like a duck, sounds like a duck but needs batttery - Wrong abstraction.
 Do not introduce new behaviour extending a class, you should be able to change substitude with the parent class and the program should work.
 - Interface segregation principle - Do not use one giant interface, use small and specific.
 - Dependency inversion principle - Don't solder a lamp directly in the wall, use a plug. Avoid details in the abstraction. Details should depend on the abstraction. 

* Spring Dependency Injections
* Create project sfg-di. Use Spring Initializr. Do not put dependencies and import it. 
Click on pom file and select "Add as maven project". Run Maven task Lifecycle -> Verify

* Spring Context - can give us a reference to a controller we've created
 - Create package controllers and a controller inside: MyController. Annotate with @Controller
 - Inside Application main method take that controller:
ApplicationContext ctx = SpringApplication.run(Application.class, args);
		MyController controller = (MyController) ctx.getBean("myController");
  
  - Injection by:
      - by property - don't
      - by setters - there are debats
      - by constructor - most prefered
      
      - injecting Interfaces is preferable(SOLID)
      
      - Inversion of control -a.k.a IoC - inject implementations at runtime - runtime control
      - DI - composition of a class vs IoC - runtime environment
      - Best practices: Use constructor injections, use final properties, interfaces (if practical)
      
      class Controller{
        public Service property;  // -Injections: class property - dont
        private Service setter; // Injected as setter
        get(), set()
        private Service constructorPropl
        
        Controller(constructorProp: Service){  // Constructor
                constructorProp = constructorProp  // preffered way
        }
      }
      
      - If you have more than one Service implementation use annotation @Qualifier("setterInjectedService") (See SetterInjectedController)
      - If you want one implementation to be default annotate it with @Primary (See project sfg-di - PrimaryGreetingService)

- Spring profile
     
     - Add service I18nEnglishService, I18nSpanishService and in I18nController 
     - Annotate both services with @Service("i18nService")
     - Annotate both services with @Service(@Service("i18nService")
     - In the controller say Qualifier("i18nService")
     - Now spring doesn't know wich one to take
     ConflictingBeanDefinitionException: Annotation-specified bean name 'i18nService' for bean class conflicts
     - Add annotation @Profile("EN") and @Profile("ES")
     - Go to resources/application.properties and add: spring.profiles.active="EN"
     
- Default profile
     
     - Add annotation @Profile({"EN", "default"}, remove resources/application.properties default profile
     Now Intelij says: "No active profile set, falling back to default profiles: default"
     
     
- Bean Lyfe Cycle
    
       - Two interfaces you can implement: InitializingBean -> afterPropertiesSet, DisposableBean -> destroy()
       - Lyfe Cycle annotations: @PostConstruct, @PreDestroy
       - Bean post processors - implement BeanPostProcessor -> postProcessBeforeInitialization, postProcessAfterInitialization
       - Aware interfaces (Rarely used). ApplicationEventPublisher, ApplicationContextAware
       
-Open-Cosed principle
	For good application design and the code writing part, you should avoid change in the existing code when requirements change. Open for extension. Closed for modifications. Use abstract classes and interfaces.
- Interface Segregation Principle
 	Your interface should not be bloated with methods that implementing classes don’t require.
	The Interface Segregation Principle advocates segregating a “fat interface” into smaller and highly cohesive interfaces, known as “role interfaces”.
- Dependency Inversion Principle
	avoid tightly coupled code. High-level modules should not depend on low-level modules.
	
- Interface naming convention
   - Don't start it with I. When there is one implementation name it InterfaceImpl but when there are few - name them by the difference.
 
 - Interface naming - Don't use I in front of it. Use Impl for implementations if they are few.
 
 - Pet Clinic POJO models: Person(firstName, lastName), Vet, Owner, PetType(name)
 When you comment a commit in GitHub and add "Closes #2" - this links your code to the GitHub issue.
 
 - Multimodule Maven project
 We want to separete data package to another module
  
     - Click on Project and select New -> Module -> Maven
     - Create two modules pet-clinic-data and pet-clinic-web
     - Move *Applicatin class to web module -> java folder
     - From pom files move
                  - spring-boot-starter-actuator, thymeleaf, starter-web, dev tools, starter test to web module
		  - mysql, jpa and lamboc to data module, h2
		  - move models - Person, Vet etc.. to data module
		  - add dependency for web to data module like that:
		  
		  <dependency>
                       <artifactId>pet-clinick-data</artifactId>
                       <groupId>com.tanovait</groupId>
                       <version>0.0.1-SNAPSHOT</version>
                 </dependency>
		 
		 - copy dependency start-test from web to data
		 - We want data module to be compiled as jar that doesn not contain its dependencies (this is default behaviour) 
		 
		  1. Add this property to data module pom.xml
		       <properties>
			 <spring-boot.repackage.skip>true</spring-boot.repackage.skip>
		      </properties>
			
		  2. On root module run Maven task "clean"
		  3. Run task "package"
		  4. Move Root module "resources" folder content to web module "resources" folder
		  5. Move root module test content to test folder in web
		  6. Delete root module "src" directory
		  
  #### Issue 1 Closed
		  
- Maven Release Plugin
            http://maven.apache.org/maven-release/maven-release-plugin/
	    
	http://maven.apache.org/maven-release/maven-release-plugin/examples/prepare-release.html
	    
	To prepare a release: 
	    - Check for uncommited changes
	    - Check that there are no snapshot dependencies
	    - Increase the version in pom.xml
	    - Transform the SCM information in the POM to include the final destination of the tag
	    *SCM - Source control management
	      ....
	      
	 -See section Perform a release
	 
	 Note: See Guru DevOps course for Deployment!!!

Spring Certification: https://store.education.pivotal.io/confirm-course?courseid=EDU-1202
https://medium.com/@raphaelrodrigues_74842/how-i-became-a-pivotal-spring-professional-certified-5-0-c6348da5f80b

https://tanzu.vmware.com/training/courses/core-spring-training

  In the root module add this dependency to build plugin.
  <build>
	<plugins>
		.....
		
		<plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-release-plugin</artifactId>
                <configuration>
                    <goals>
                        install
                    </goals>
                </configuration>
            </plugin>
  - See version of the snapshot <version>0.0.1-SNAPSHOT</version>
  - Add 
                  <scm>
        		<developerConnection>scm:git:https://github.com/i-tanova/spring-pet-clinic.git</developerConnection>
    		  </scm>
  - Run $ mvn release:prepare
  
  - Add auto versioning like:
                    <goals>
                        install
                    </goals>
                    <autoVersionSubmodules>true</autoVersionSubmodules>
  - Run mvn release:prepare
  Building spring-pet-clinic 0.0.2-SNAPSHOT
  
  - Now in GitHub - Branch - extendible menu - there is a tag: release.0.0.1
  
       #### Issue 3 Closed
       
 - Create Services interfaces
     - Create package services inside data module
     - Add interfaces: OwnerService (methods findById, findByName, findByLastName, getAll, save), VetService, PetService -same methods
     
     #### Issue 5 Closed
     
 - Implement Base Entity
           Base entity is JPA concept
	  - Add BaseEntity class in model package.
	  - Add Long id property
	  - extend Serializable
	  - Make Person to extends BaseEntity 
	  - Make PetType to extends BaseEntity
	  - Make Pet extends BaseEntity
	  * All classes must extend BaseEntity
	  
#### Issue 10 Closed

## Build Spring Boot Jokes App
- Create it from Spring Initializr with only Web and Thimelife dependencies
- Add external jar: Spring guru Chuk Norris as dependency
- Create Service class to return joke from Chuk Norris dependency lib
- Create Spring MVC Controller 
- Map context root / to Jokes view
- Add model Joke with "joke" property
- Return view name of "chucknorris" 
- Create Thymeleaf view "chucknorris"
- Display Joke string
- Run app
- Refresh for more jokes

* You can change Spring banner at the terminal when starting the application

## Spring Pet Clinic

 - Refactor Services to common interfaces
     - Create GitHub issue #10
     - Create interface CrudService (watch from CrudRepository)
     - add typed parameters <T, ID>
     - add methods: save, getAll - returns Set<T>, findById(), delete, deleteById

#### Issue 10 Closed

 - Issue 6. Implement Map based services
     - Create abstract class AbstractMapService<T, ID>
     - create inside map<ID, T>
     - add methods - findAll(), findById, save, delete, delteById
     - create classes OwnerMapService, Vet.., Pet.. that extend AbstractMapService and implement CrudService
     calling super methods
### Issue 6 closed

- Issue 8. Add pet clinic index page and controller
           - inside web module add resources/templates - index.html with thymeleaf text "Index"
	   - create package controllers
	   - Add Index Controller with method @RequestMap({"", "/", "index", "index.html"}) that returns "index"
### Issue 8 closed

- Issue 7. Create vet and owner index page and controller.
        - Crete folder templates/vets/index.html
	- Create controller VetsController with RequestMap({"vets", "vets/index", "vets/index.html"})
	- Same for owner
	- You can put 
	       @Controller
               @RequestMapping("/owners") and for the method RequestMap({"", "/"...})
	
### Issue 7 closed	

### Section 5. Spring Framework Configuration
- How we define Spring Beans

     - Spring Configuration Options
     	- XML base configuration - legacy but still supported, widly used
	- Annotation based configuration - Since Spring 3, class based annotations @Controller, @Service..
	- Java based configruation - Spring 3, Java classes for configuration @Configuration and @Bean annotations - Preffered
	- Groovy Bean Defenition DSL - Spring 4, beans in Groovy, borrowed from Grails - used from Groovy community
	
     - Spring stereotypes
           - @Component -> @Controller, @Repository, @Service
	   - @Controller -> @RestController ( @Controller and @RestBody)
	   
	   - @Repository adds error handling
	   
     - Spring commponent scan
           - How to tell Spring which directories to scan, because in a large project scanning all will take a lot of time
	   - @ComponentScan annotation overrides default Spring scanning behaviour. By default Spring scanns for packages under Application package
	   
     - Java coniguration example
           - We will modify Jokes application to use java configuration
	   - Modify JokeServiceImpl to accept ChuckNorrisQuotes object in the constructor
	   - Add package "config" and class ChuckConfigruation
	   - Add annotation @Configuration and a method that returns ChuckNorrisQuotest object with annotation @Bean
	   
     - Spring xml configiration example
            - Click on resources - New -> XML configuration file -> Spring Configuration
	    - add chuck-config class
	    - add <bean name="chuckNorrisQuotes" class="guru.springframework.norris.chuck.ChuckNorrisQuotes" />
	    - add annotation to Application class @ImportResource("classpath:chuck-config.xml")
	    
     - Using Spring Factory Beans
            - For more complex configirations
	    - Create Bean Factory inside @Configuration class
	    - GreetingServiceFactory with method getService(String language)
	    - config package with GreetingServiceConfig @Configuration
	    - method getServiceFactory that returns GreetingServiceFactory and has parameter (repository) and @Bean annotation.
	    
     - Spring Boot Configuration
             - Dependency management - Maven or Gradle (Spring boot version for every Spring Framework v)
	     - Maven - whenever possible don't specify versions in your pom.xml, because Spring takes care of this
	     - Auto configuration will bring a lot of jars to the project. You can exclude with:
	     @EnableAutoConfiguration(exclude={})
	     
    - Spring Bean Scope
            - Singleton (default)
	    - Prototype - new instance at each bean request
	    - Request - single instance for each http request
	    - Session - single instance for http session
	    - Global- Session - not used
	    - Application - bean is binded with the lifecycle of ServletContext
	    - WebSocket - valid through a life of a web socket.
	    - Custom - you can defind your own scope
	    
	    * Most used: Singleton, Prototype
	    
	    - Declaring scope: 
	        - For singleton - declaration is not needed
		
    - Load Bootstrap data on startup
            - In the web module create package "bootstrap"
	    - Create class DataLoader that implements CommandLineRunner
	    - Add constructor that initializes OwnerService and PetService with Map implementations
	    - in run method add owners and vets and call service.save()
     
     - Issue 21. Implement Spring Configuration for Services
           - Add @Service annotation to VetMapService and OwnerMapService
	   - In DataLoader add both service as constructor parameters
	   * Note in Spring 5 youdon't need @Autowired for constructor params
	   Issue 21 Closed
	   
     - Issue 14. List owners
           - Modify OwnerController to have private final field OwnerService
	   - Add OwnerService as constructor parameter
	   - Add to method listAll parameter Model model
	   - Call model.setArgumetn("owners", service.listAll)
	   - Inside owners/index.html use thymeleaf th:forEach to list owners
	   
     - Autogenerate ids
            - Add method getNextId() to AbstractMapService
	    - Collections.max(map.keySet()) + 1
	    - Check if map is not empty
	    - Make T extends BaseEntity and ID to extend Long
	    - Remove paramter id to add method
      
     - External properties
             - Create resource/datasource.properties file
	     - Fill some properties
	      - Create class config/PropertiesConfig
	      - Add @Configuration annotation
	      - Add @PropertySource("classpath:datasource.properties") - this can be a list of values
	      - add method properties() that returns new instace of
	      PropertySourcesPlaceholderConfigurer
	      - add @Bean annotation to method
	      - add fields for all properties with annotation  @Value("${propname}")
	      - Get class as ctx.getBean(FakeDataSource.class
     
     - Environment properties
                - add property from Android studio -> project configuration
		- add @Configuration class
		- add field Environment env; with annotation @AutoWired
		- env.getProperty("USERNAME")
    
     - Spring boot application properties
                - Default way
		 - appplication.properties
		 
     - YAML - yet another markup language
              - spaces are important
	      - don't use tabs
	      - file with extension yml
	      
     - resoureces - new - application.yml
          guru:
	     jms:
	        username: JMS username
		password: passsword
     
     - Externalized Configuration
            - Read here:https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#boot-features-external-config
	    - properties files can override each other
	    - there is yierarchy in which application.yml is first then apppication-profile.properties and then application.properties
	    - yml files can contain two files separated with --- delimiter

### Section 7. Web development with Spring MVC

  - Start recipe project, Http protocol, Developer tools, Bootstrap, Jquery, JPA model for Recipes project
  
  - Introduction to Thymeleaf
           - Java template engine
	   - Replacement for JSP
	   - It is not tied to web environment, it is not a web framework,  it produces pure HTML 5
	   - JSP is not pure html
	   
 - Add index page
          - add controllers/IndexController
	  - Annotate with @Controller and @RequestMapping({"/", "", "/index"}) getIndex method that returns "index"
	  - Create templates/index.html
	  - add namespace xmlns:th="http://www.thymeleaf.org"
	  * If you use snapchot of spring boot you need to import snapshot repository
	  
 - HTTP 
          - Hypertext transfer protoclol
	       - Tim Berners Lee 1989 CERN
	        - telnet google.com 80
		- HTTP/0.9
		 -HTTP 1.0 until 1995
		 - HTTP/1.1 1997, updated 1999 and 2014 - keep alive connections, transfer encode, chunked encoding transfers, bite range requests,request pipeline
		 added coockies, encoding, charset
		 - HTTP 2.0 2015 - improved performance

- HTTP request methods
            - GET - resource, visit page
	    - HEAD - like GET but asks for meta info, no body
	    - POST - create
	    - PUT - create or update
	    - DELETE
	    - TRACE
	    - OPTIONS
	    - CONNECT
	    - PATCH
	    - Safe methods: GET, HEAD, OPTIONS, TRACE
	    - IDEPONTENT methods(It is safe to repeat(retry)):  PUT, DELETE + Safe methods
	    - Non idepontent methods - POST
	    - Methods with request body - POST, PUT
	    - NO BODY - GET, DELETE, HEAD
	    - HTTP Status codes: 
	         - 100 - information
		 - 200 - success (200 - ok, 201 - created, 204 - accepted)
		 - 300 - redirections (301 moved permanently)
		 - 400 - client error (400 bad request, 401 not authorized, 404 not found)
		 - 500 - server error (503 service unavaliable server is overloaded, temporary condition)

- Developer tools
              - Chrome - Developer Tools - Networking Tab
	      - Firefox developer edition
	      - Safari Web Inspector
	      - Axis TCP Monitorе Intelij Plugin
	            - Set port to 8081
		    - Reload app changing the port to localhost:8081
	      - Spring Boot dev tools:
	            - Spring boot dev tools maven artifact
		    - triggers restart when class is changed
		    - Live reload - reload the browser when code is changed
		    - includes Live reload server
		    - Browser plugins are available at livereload.com
	     - Configure Spring boot dev tools
	            - Inside Intelij find action "Registry.."
		    - Enable "compiler.automake.allow.when.app.running"
		    - Choose Project->Compiler-> Build project automatically
	    
- Copy all static resources form Pet Clinic
                   - Downoad Spring Pet Clinic project as zip
		   - Create folder in web module - resources/static/resources
		   - Paste folders "fonts" and "images" from the old project
		   - Tomcat search for folder static
		   - Git doesn't commit empty directories

- Copy master template
                   - Old pet clinick welcome.html file has this :
		   th:replace="~{fragments/layout :: layout (~{::body},'home')}
		   It means - replace body with templates/fragments/layout.html
		   - Into layout.html we see th: include={template} <-- merge code here
		   - Copy the whole "fragments" directory into templates directory
		   
- Web resource optimizer for Java
                   - src/main/less directory
		   - Web resource optimizer docs: https://github.com/wro4j/wro4j
		   - Add this to pom.xml
  <!-- webjars -->
    <dependency>
      <groupId>org.webjars</groupId>
      <artifactId>webjars-locator-core</artifactId>
    </dependency>
    <dependency>
      <groupId>org.webjars</groupId>
      <artifactId>jquery</artifactId>
      <version>${webjars-jquery.version}</version>
    </dependency>
    <dependency>
      <groupId>org.webjars</groupId>
      <artifactId>jquery-ui</artifactId>
      <version>${webjars-jquery-ui.version}</version>
    </dependency>
    <dependency>
      <groupId>org.webjars</groupId>
      <artifactId>bootstrap</artifactId>
      <version>${webjars-bootstrap.version}</version>
    </dependency>
    <!-- end of webjars -->
    
    - Add web dependencies inside <properties> xml tag
    <!-- Web dependencies -->
    <webjars-bootstrap.version>3.3.6</webjars-bootstrap.version>
    <webjars-jquery-ui.version>1.11.4</webjars-jquery-ui.version>
    <webjars-jquery.version>2.2.4</webjars-jquery.version>
    <wro4j.version>1.8.0</wro4j.version>
	
   - Add <build><plugins>
	and
	 <plugin>
        <groupId>ro.isdc.wro4j</groupId>
        <artifactId>wro4j-maven-plugin</artifactId>
        <version>${wro4j.version}</version>
        <executions>
          <execution>
            <phase>generate-resources</phase>
            <goals>
              <goal>run</goal>
            </goals>
          </execution>
        </executions>
        <configuration>
          <wroManagerFactory>ro.isdc.wro.maven.plugin.manager.factory.ConfigurableWroManagerFactory</wroManagerFactory>
          <cssDestinationFolder>${project.build.directory}/classes/static/resources/css</cssDestinationFolder>
          <wroFile>${basedir}/src/main/wro/wro.xml</wroFile>
          <extraConfigFile>${basedir}/src/main/wro/wro.properties</extraConfigFile>
          <contextFolder>${basedir}/src/main/less</contextFolder>
        </configuration>
        <dependencies>
          <dependency>
            <groupId>org.webjars</groupId>
            <artifactId>bootstrap</artifactId>
            <version>${webjars-bootstrap.version}</version>
          </dependency>
          <dependency>
            <groupId>org.mockito</groupId>
            <artifactId>mockito-core</artifactId>
            <version>${mockito.version}</version>
          </dependency>
        </dependencies>
      </plugin>
	
	- Copy src/main/wro folder
	- Copy less folder
	 - Go to maven tool window -> root project -> Lyfecycle. Execute clean and package tasks
		    
- Apply Master Layout To Index Page
         - copy form wellcome.html 
	 - th:replace="~{fragments/layout :: layout (~{::body},'home')}"
	 -<h2 th:text="#{welcome}">Welcome</h2>

<div class="row">
    <div class="col-md-12">
        <img class="img-responsive" src="../static/resources/images/pets.png" th:src="@{/resources/images/pets.png}"/>
    </div>
</div>


- Add internationalization
       - Copy resources/messages folder in the same folder
       - Add in application.properties:
         spring.messages.basename=messages/messages

- Apply master layout on ownres page
         - go to the old project templates/owners/ownersList.html
	 - copy th:replace="~{fragments/layout :: layout (~{::body},'owners')}"
	 - add it to templates/owners/index.thml
	 - add class="table table-striped" to table html element
	 - copy style to "th" element
	 - go to http://localhost:8080/owners
	 
- Apply master layout to vet page
          - go to old project templates/vets/vetsList
	  - copy th:replace="~{fragments/layout :: layout (~{::body},'vets')}"
          - copy table style
	  - go to http://localhost:8080/vets
	  
	  !! When you have problem with the style do maven task: clean and then package
	  
- Create Visit, Pet Speciality Entities
         - Go to data module - Owner and add Set<Pet>
	 - Add getter and setter
	 - Add Visit model that extends BaseEntity (LocalDate, description, Pet)
	 - Add model Speciality(description)
	 - Add Set of Specialties as field to model Vet

- Create contact info
            - Add properties address, city, phonenumber to Owner
	    
- Create pet type map service
            - Add interface PetTypeService to services package
	    - Extend CrudService<PetType, Long> 
	    - Add map based implementation in package service.map - PetTypeMapService
	    - Extend AbstractMapService<PetType, Long>
	    - Implement methods calling super
	    - Add @Service annotation
	    
- Add Pet Types in with Bootstrap (startup) #31
           - In web module find com.tanovait.springpetclinic.bootstrap
	   - DataLoader add lines
	         PetType dog = new PetType()
		 dog.setName("dog")
	   - Add service PetTypeService as constructor parameters
	   - Call petTypeService.save(dog) and with returned instance
	   - Do the same for cat
	   
- Enhance Owners with Contact info and Pets on startup
           - Add reference to services PetService and PetTypeService to OwnerMapService
	   - Whenever owner is saved check if it has pets. Check if pets have id. If the dont - call petService.save and set the id. 
	   
	      if(pet.getId() == null){
                       Pet savedPet =  petService.save(pet);
                       pet.setId(savedPet.getId());
                    }
		    
	
	Check if those have pet type  - throw exception if they dont, check pet type - id, if they dont have it call petTypeService.save()
	   Check if they have pet type. Check if pet type has id. If it doesn't - save them.
	   - At DataLoader - create Pet for onwner 1 nad 2 and 
	                 - Set the pet's owner  - pet.setOwner(owner)
			 - Set owner's pet      - owner.setPet(pet)
			 - Save the owner to the service - service.saveOwner() - here we will check for pets, petype, pet
			 We will throw  throw new RuntimeException("Pet type is required"); if pet type is missing from Pet
	  
	 
- Create Specialty Map Service #30
        - Add interface SpecialtyService that extends CrudService<Specialty
	- Add Map based implementation
	- Don't forget @Service annotation
	* Use Intelij IDEA split horizontally windows
	
- Create Specialty, add to Vets at Startup(Bootstrap) #33
        - Use SpecialtyService inside DataLoader
        - Create specialty - surgery, dentistry, radiology
	- Save them in the SpecialtyService
	- Add saved instances (they have id) to the vet and vet1 instances before saving the Vet
	- add private method loadData containing all the logic
	- add check
	
	if(petTypeService.getAll().size() == 0) {
            System.out.println("Load data");
            loadData();
        }
	
	- In the VetMapService when saving a Vet check if there are Specialty
	- If they are - forEach and check if each has Id
	- If id is missing - save Specialty to SpecialtyService and set returned Id to the specialty 
	    
- Menu Links Are Broken #44
        - Find Owners and Error page are not implemented yet so:
        - Add html template for notimplemented page
	- At OwnersController add: 
	 @RequestMapping("find") 
	 method that maps this page
	 - At IndexController add: RequestMapping ("oups) that returns same
	 - List of vets link is broken. It is vets.html
	 - Go to VetsController and att "/vets.html" to the request method mapping for method listVets
	 
### JPA Data Modeling with Spring & Hibernate
  - Relationships one to many, many to many, spring data jpa repositories, database initialization, display list of recipes on index page, 
            
	   - JPA Entity Relationship
	           - @OneToOne
		   - @OneToMany - one has a  List, Set.. of the other
		   - @ManyToOne
		   - @ManyToMany - each has List of the other, join table relationship
		   - Unidirectional - one way, one side desn't know about the other
		   - Bidirectional - both sides know about each other, Recommended!
		   - Owning side - holds foreign key in DB
		   - mappedBy - field that owns the reference of the relationship
		   
	   - Fetch type
	         - Lazy - data is not queued until referenced
		 - Eager - data is queued upfront
		 - Hibernate 5 supports the JPA 2.1 Fetch Type Defaults
		           - One to Many -Lazy
			   - Many to one - Eager
			   - Many to Many - Lazy
			   - One to One - Eager
	   - JPA Cascade types
	        - State change cascades from parent to child object
		- Persist - save operations will cascade to related entities - 
		- Merge - related entities are merged when the owned entity is merged
		- Refresh - related entities are refreshed when the owned is refreshed
		- Remove - If you delete parent, childs are deleted too
		- Detach - detach related entities
		- All - apply all above
		By default - NO OPERATION IS CASCADED
		
	   - Embeddable Types
	        - common set of properties like Address (Objects inside persisted JPA Entity)
		
	   - Inheritance
	         - MappedSuperclass - DB table is not created for the super class
		 - Single table - one table for all subclasses - Default!!
		 - Joined table - base and subclasses has their own table
		 - Table per class - each subclass its own table
		 
	   - Create and update timestamps
	        - for audit purpose
		 - JPA supports @PrePersist and @PreUpdate
		 - Hibernate has @CreationTimestamp and @UpdateTimestamp 
		 
- Recipes Data Model
           - Recipe (*-* Category, 1-*I ngredient, 1-1 Note, 1-1 Difficulty, description, prepTime, directions, image)
	   - Category - name
	   - Ingredient - description, amount
	   - Note - notes
	   - Difficulty - enum: EASY, HARD
	   
- One to one (Recipe - Note)
     - One recipe, one note 1-1
     - Put entities in package domain
     - Recipe(Integer prepTime, String decription, Byte[] image)
     - Notes (Recipe recipe, String notes)
     
     - Mark both as @Entity
     - Add Long field id
     - Mark it with @Id,  @GeneratedValue(strategy = GenerationType.IDENTITY)
     - At Recipe add annotation above Notes declaration @OneToOne, (cascade = CascadeType.ALL) - delete notes when recipe is deleted
     - At Notes add annotation above Recipe @OneToOne
     - @Lob - this annotation tells to store an object as Blob - big data
     - Add it above notes String and image Byte array
     - Generate getters and setters for all fields
     
- One to many (Recipe - Ingredients)
     - Recipe - Ingredients has 1-* and Ingredients- Recipe has 1-* relationship
     - Create class Ingredient(Recipe recipe, description String, id Long)
     - Annotate as @Entity, add @Id Generated
     - In Recipe add property Set<Ingredient> ingredients
     - Annotate it as @OneToMany and add cascade type = All, and mappedBy="recipe" - this is the field in Ingredient class
	@OneToMany(cascade = CascadeType.ALL, mappedBy = "recipe")
     - Go to Ingredient and annotate recipe field with @OneToMany
	
- Assignment - create one to one relationship between UnitOfMeasure and Ingredient table
         - Create UnitOfMeasure Entity
	 - Create Unidirectional relationship from Ingredient to Unit of measure
	 - Do not cascade events
	 
	 @OneToOne(fetch = FetchType.EAGER)
         private UnitOfMeasure unitOfMeasure;
	 
- JPA Enumerations
       - in package domain add new -> enum Difficulty
       - add it as field in Recipe
       - add annotation @Enumerated
       - add value = EnumType.ORDINAL  - Ordinal is default value. This is how it will be persisted.
       - Ordinal means it will be persisted as numbers 1, 2, 3 - This means that if you add new value, all data will mess
       - Use String!!
        @Enumerated(value = EnumType.STRING)
	
- Many to Many JPA Relationships
      - Recipe and Category has many to many relationships *_*
      - Add Category with Set<Recipe> 
	- In Recipe add Set<Category>
      - Add @ManyToMany annotation both to Recipe and Category
	- If you leave it that way it will create two tables - recipe_categories and categories_recipes
      - In Recipe add @JoinTable(name="recipe_category", joinColumns="@JoinColumn)
      - In Category add
	@ManyToMany(mappedBy = "categories")
       - Now we have only one table:
	      - Recipe Category
	- Open h2 console here:
	     http://localhost:8080/h2-console
	
- Creating Spring data repositories 30.04
	- Cascade relationsips - Notes and Ingredients, update the one will updte the other
	- We dont need to Set up repositories for Notes and Ingedients because Recipe is the main object (They are parts of it)
	They are not maintained independently
	- Recipe, Category, Unit of measures will have Spring data repositories
	- Create package repositories
	 - Add interface RecipeRepository extending CrudRepository (Spring class)
	 - Add interfaces CategoryRepository, UnitOfMeasuer repository

- Database initialization  30.04
         - Hibernate DDL Auto (Data Definition Language)
	 - Hibernate property is set by the Spring
	    spring.jpa.hibernate.ddl-auto
	 - Options: none, validate, update, create, create-drop. Use validate on production
	 - Spring boot uses create-drop for embedded db as h2, derby. Create drop drops the table at shutdow
         - Initialize with Hibernate - load data from import.sql
	 - Spring DataIntializer via Spring Boot loads from shema and data from the root of the classpath
	 - can load and from platform specific files
	 - Add file data.sql into resources folder
	 insert into category(description) values ('american')
insert into category(description) values ('mexican')
insert into unit_of_measure(abbreviation) values ('kg')
insert into unit_of_measure(abbreviation) values ('g')
	 - Set dialect - MySql dialect
	 - Run the application - data is entered
	 - UnitOfMeasure will be unit_of_measure 

- Spring Data JPA Query methods
     - just add method     Optional<Category> findByDescription(String description);
	inside CategoryRepository interface
	
     - Inject category repositry inside RecipeController
     - Call inside home() method
       Category category = categoryRepository.findByDescription("american").get();
        System.out.println("Category is: " + category.getId());
	
- Assignment - List recipes - recreate from SimpleRecipes.com recipes for: perfect guacamole, spicy grilled chicken takos
  - add directions
  - add unito of measures
  - list recipes
  - Use Bootstrap class to add the two recipes
  - Create service to return recipe list to controller
  - Pass recipes list to Tymeleaf index page
  
  - Create RecipeService
  - add method getAll
  - Implemement using repository injection
  - Add package bootstrap
  - add class DataLoader that extends CommandLineRunner
  - Create Recipe 
         - add descriptiom
	 - add directions
	 - add difficulty
	 - add prep time
	 - Create Note
	 - add note to recipe
	 - Create Ingredients
	 - Add to Recipe

- Pro tips using setters for JPA
       - When we set Note object to Recipe we need to set Recipe object for Note
       - Same for Ingredient object
       
     - Enhancement: When we do setNotes on Recipe object we can add code to set Recipe object back to Note object
                
    public void setNotes(Notes notes) {
        this.notes = notes;
        notes.setRecipe(this);
    }
  
   
   Same for set Ingredient
        
   -     public void setIngredientSet(Set<Ingredient> ingredientSet) {
        this.ingredientSet = ingredientSet;
        ingredientSet.forEach(ingredient -> {ingredient.setRecipe(this);});
    }
        
	   
- Issue #34. Spring Pet Clinic - Create Base Entity 
       - Classes that extend BaseEntity will inherit methods: getId, setId, isNew
       - Go to data module - model package. Add BaseEntity and annotate it with @MappedSuperclass
       - Makes it extend Serializable
       - Annotate field "id" with @Id and GeneratedValue - strategy Identity
       - Other strategies are Table, Sequence, Auto
       - Table - based on the table count
       - Sequence - next number in a sequence
       - Identity - primary key
       - Auto - autogenerate, can give unpredictable results

-  Issue #35. Convert Owner to JPA Entity
      - Go to class Person
      - Add @MappedSuperClass
      - Add fields annotation @Column and snake case naming for columns
      - @Column(name="first_name")
      
      - Annotate Owner as @Entity
      - add @Table(name="owners")
      - add @Column to fields except for pets
      - For pets add:
       @ManyToMany(cascade = CascadeType.ALL, mappedBy = "owner")
       
      - Annotate Pet as @Entity
       - add @Table
       - add @Column
       - For Owner add @ManyToOne and @JoinColumn(name = "owner_id")
       - For PetType add @ManyToOne and @JoinColumn name type_id
       
      - Annotate PetType
       @@Entity
       @Table(name = "types")
       @Column(name = "name")
       
- Issue 36. Convert Vets to JPAEntity
    - Vet extends Person, and Person extends BaseEntity. One Vet can have many specialties and one Specialty many vets
    - Add @Entity, @Table
    - Add @ManyToMany for speciality set
    -  @ManyToMany(fetch = FetchType.EAGER)
    @JoinTable(name = "vet_specialties", joinColumns = @JoinColumn(name = "vet_id"), inverseJoinColumns = @JoinColumn(name = "specialty_id"))
       
       
    - Go to Specialty and add annotations @Entity, @Table, @Column
    
 - Issue 37. Create Visit Entity
      - Go to Visit
      - Add @Entity, @Table
      - Add @Colum to fields date and description
      - Go to Pet object and add a property Set<Visit> visits
	- Note: Initialize sets with emtpy set
      - Add following annotation to property visits
	   @OneToMany(cascade = CascadeType.ALL, mappedBy = "pet")
      - Go to Visits and on the Pet property add:
	    @ManyToOne()
            @JoinColumn(name = "pet_id")

- Issue 38. Create Repository
       - Create package repositories
       - There a 3 major repository patterns - Crud, JPA, Pager..we will use Crud
       - Add interface OwnerRepository that extends CrudRepository<Owner, Long>
       - Add interfaces for all model objects
       - Spring will generate all needed code for the repo
       
- Issue 39. Create JPA Service
       - Create package springdatajpa
       - Create class OwnerJpaService that extends OwnerService
       - Add annotation @Service
       - Add OwnerRepository instance inside constructor
       - Go to OwnerRepository and create method findByLastName
       -  Owner findByLastName(String lastName);
       - Spring will create the implementation
       - Add implementation of findById as:
        Optional<Owner> optional = ownerRepisotory.findById(aLong);
        return optional.orElse(null);
	- find all:
	 Set<Owner> allOwners = new HashSet<>();
        ownerRepisotory.findAll().forEach(allOwners::add);
        return allOwners;
	
- Issue 40. Create Service for Vets
          - Same as Owner
- Issue 41. Create Service for PetType
- Issue 42. Create Service for Pet
- Issue 43. Create Service for Specialty.

- Issue 45. Create Visit Service interface extends from CrudService
            - Create Visit Map Servce extends AbstractMapService implements VisitService
            - Create Visit Jpa Service - extend VisitService and add annotation @Service
	    - Import private final VisitRepository as Repository
            - Add checks when adding a visit - check if it has a Pet, check if this pet has owner and Id
- Issue 47. Load Visit on Startup in Bootstrap
                - Add VisitService inside LoadData constructor
                - Just create visit object and save it
- Use Spring profiles to switch from Map based Services to JPA
      - If we don't specify profile - default wil be used
      - Add annotatin to Map based Service - @Profile({"default", "map"})
      - When we start the application it says "No active profile set, falling back to default profiles: default"
      - Go to web module -> application.properties and add:
            spring.profiles.active="springdatajpa"
	 Now the application says: The following profiles are active: "springdatajpa"

## Project Lombok
   -  Simplify code 
   -  Features: val, var, @NonNull, @Cleanup, @Getter, @Setter, @ToString, @EqualsAndHashcode, @AllArgsConstructor
   @Data - generates getters, setters, constructor,@Value, @Builder, @Getter(lazy=true)
   @SLF4j - logging
   -  For Intelij idea verify that you have enabled the "annotation" processing under compiler settings
   
   - Add dependency for project Lambok
        - Go to Recipes app
        - Add this to pom.xml:
	  <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
        </dependency>
	- Import the changes (Idea will notify)
	- Install Idea Lombok plugin
	- Go to domain/Notes and select form Idea menu -> Refactor > Lombok > Default data
	It will put this @Data annotation and removes constructor. Generates getter, setter, cosntructor, to String, equals...
                @Data
		@Entity
		public class Notes {
		    @Id
		    @GeneratedValue(strategy = GenerationType.IDENTITY)
		    private Long id;
		    @Lob
		    private String recipeNotes;
		    @OneToOne
		    private Recipe recipe;
		}
	 - You can revert with Delambok
	 - Go to Category and refactor to Lambok
	 - Go to Recipe and refactor to Lambok but MANUALY because we need to override some of these properties.
	      - put @Data annotation
	      - Leave setNotes and setIngrdientsSet
	      - Remove rest of the getters and setters - Lambok will generate them
	 - Go to services.RecipeServiceImpl and add SLF4J Lambok annotation
	   This will inject a logger. Now write inside getRecipes - 
	        > log.debug("Logging")
		
	 - Important: Invalidate cashe and restart Intellij IDEA in order Lambok to work
	 ***** Note: When something bed happen: Invalidate cashe and restart.

- Assignment: Refactor Recipe project to use Lambok
            - Refactor all entities to use @Data
	    - Use @slf4j on controller and bootstrap classes
	    - Add some debug logging
	    - If you run it like that it will crash with:
	    Caused by: org.hibernate.LazyInitializationException: failed to lazily initialize a collection of role: com.ivana.tanova.recipesspringapp.domain.Category.recipeSet, could not initialize proxy - no Session
	at org.hibernate.collection.internal.AbstractPersistentCollection.throwLazyInitializationException(AbstractPersistentCollection.ja
	at com.ivana.tanova.recipesspringapp.domain.Category.hashCode(Category.java:8) ~[classes/:na]
	Lambok can't generate equals and hash code
               - Go to Category and add 
	       @EqualsAndHashCode(exclude = {"recipeSet"})
	       - Go to Ingredients and Note and add:
	       @EqualsAndHashCode(exclude = {"recipe"})
               - If you see Lazy initialization exception add @Transactional annotation
	      
- Issue 50. Refactor Spring pet clinic to use Lambok
          - Don't use @Data on Entity use:
	  @Getter, @Setter, @NoargsConstructor, @AllArgsConstructor, @Builder
	  - Don't put @Builder annotation for Person as it is super class not meant to be initialized
	  - Instead for Owner do constructor with all fieer
Save to playlistlds and add @Builder annotaion in front
	  - Call Person.super(id, firstName, lastName) and BaseEntity.super(id)
	  - Remove @AllArgsConstructor annotation from Owner
	  - Now you can use builder patterm
	   Owner.builder().setFirstName("name").build()
	   
	   
## Testing Spring Framework Applications
- Get Bootstrap 
      - Go to Recipe app and:
      - Copy Bootstrap CDN links inside head:
           bootstrap.min.js
	   bootstrap.min.css
      - Explore Bootstrap
             <div class="container">
	     
## CRUD opearations
- Using Web Jars with Spring Boot - web jars of popular
WebJars are client-side web libraries (e.g. jQuery & Bootstrap) packaged into JAR (Java Archive) files.
https://www.webjars.org/
         - Grab Bootstrap, Jquery
	 	
<dependency>
    <groupId>org.webjars</groupId>
    <artifactId>bootstrap</artifactId>
    <version>4.5.0</version>
</dependency>
          - Go to Recipes app and put it there. Select "import changes" form Idea pop up
	  - Go to templates/index.html and include jquery and bootstrap from web jar
	  <script src="/webjars/bootstrap/js/bootstrap.min.js"></script>
          <script src="/webjars/jquery/jquery.min.js"></script>
	  
- Display Recipe by id
    - Here is how to display table in Timeleaf:
       - https://www.baeldung.com/thymeleaf-list
    - Here is how to style table in Bootstrap:
    https://getbootstrap.com/docs/4.4/content/tables/
    - <table class="table table-striped table-bordered table-hover">
    - grid system with row and col.
    -  <th scope="col"> ID</th>
  

   
		  
		  
		 
 
 
     
 
  
