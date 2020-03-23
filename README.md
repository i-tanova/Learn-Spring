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

* Spring Dependency Injections
* Create project sfg-di. Use Spring Initializr. Do not put dependencies and import it. 
Click on pom file and select "Add as maven project". Run Maven task Lifecycle -> Verify
* Spring Context - can give us a reference to a controller we've created
 - Create package controllers and a controller inside: MyController. Annotate with @Controller
 - Inside Application main method take that controller:
ApplicationContext ctx = SpringApplication.run(Application.class, args);
		MyController controller = (MyController) ctx.getBean("myController");
  
  - Injection by:
     - class property - dont
      -by setters - there are debats
      - by constructor - most prefered
      
      - injecting Interfaces is preferable(SOLID)
      
      - Inversion of control -a.k.a IoC - inject implementations at runtime - runtime control
      - DI - composition of a class vs IoC - runtime environment
      - Best practices: Use constructor injections, use final properties, interfaces (if practical)
      
      class Controller{
        public Service property;
        private Service setter;
        get(), set()
        private Service constructorPropl
        
        Controller(constructorProp: Service){
                constructorProp = constructorProp  // preffered way
        }
      }

 
  
