# Learn-Spring
Spring Framework 5 Udemy course

- Install: Java, Maven, Gradle, IntelliJ Ultimate IDE

### Spring initializr

https://github.com/spring-io/initializr
Generate a backbone of a project here: start.spring.io

*Spring boot starters - those are dependencies
https://github.com/spring-projects/spring-boot/tree/master/spring-boot-project/spring-boot-starters

*Choose Maven, Java, Don't choose snapchot version of Spring boot, pack as jar, Java 8,

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
	   
	
  

   
		  
		  
		 
 
 
     
 
  
