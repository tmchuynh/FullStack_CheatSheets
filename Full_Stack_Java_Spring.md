# Spring Boot App Cheat Sheet

- [Spring Boot App Cheat Sheet](#spring-boot-app-cheat-sheet)
  - [Start a new Spring Boot Project](#start-a-new-spring-boot-project)
  - [Dependencies & Configurations Reference](#dependencies--configurations-reference)
    - [application.properties file:](#applicationproperties-file)
    - [JSP tag library imports, settings, external styling](#jsp-tag-library-imports-settings-external-styling)
  - [Spring Boot Templating Boilerplates](#spring-boot-templating-boilerplates)
    - [JSP Template Inheritance](#jsp-template-inheritance)
      - [PutType](#puttype)
      - [Layout path prefix/suffix](#layout-path-prefixsuffix)
      - [Example](#example)
      - [Known problems](#known-problems)
  - [Spring MVC](#spring-mvc)
    - [Validation Annotations in Domain Models](#validation-annotations-in-domain-models)
    - [Repositories](#repositories)
    - [Services](#services)
  - [Adding Views (No More API/Postman)](#adding-views-no-more-apipostman)
    - [@Controller Boilerplate with full CRUD](#controller-boilerplate-with-full-crud)
    - [View All Table JSP Boilerplate](#view-all-table-jsp-boilerplate)
    - [View One JSP Boilerplate](#view-one-jsp-boilerplate)
    - [Create Form JSP Boilerplate](#create-form-jsp-boilerplate)
    - [Update Form/Delete JSP Boilerplate](#update-formdelete-jsp-boilerplate)

## Start a new Spring Boot Project

1. Create an empty folder or directory wherever you want your Spring Boot projects to be located at.

2. Open STS and choose your workspace.

3. On the left-hand side in the Package Explorer, right click and hover over "New" and click on "Spring Starter Project."

   ![start new project](2022-02-11-16-14-55.png)

4. In the 'New Spring Starter Project' window, we must fill out information about our project.

   ![new project info](2022-02-11-16-18-01.png)

The information that you provide here is crucial for sharing your project.
Here are some conventions:

- Name Field: `yourprojectname`. This will be your project name all lowercased.
- Group Field: `com.company.yourprojectname`. This will be a combination of your company and your project. For now, you can put your name in its place.
- Artifact Field: `yourprojectname`
- Description Field: Short description about your project
- Package Field: Same as the group field.

1. After clicking next, we will see a window to install our dependencies. Select the follow:
   - `Web` or `Spring Web`
   - `DevTools` or `Spring Boot DevTools`
   - `JPA` or `Spring Data JPA`
   - `MySQL`

   ![new project dependencies](2022-02-11-16-19-57.png)

**Running the Application:**
> Right-click on your application -> click on `"Run As"` -> click on `Spring Boot App` -> go to "localhost:8080" to check your work

&nbsp;

## Dependencies & Configurations Reference

At the bottom, open the `pom.xml` file and replace everything in the `<dependencies>` tag with:

```xml
        <!-- DEPENDENCIES FOR STARTING SPRING PROJECTS-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <scope>runtime</scope>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-tomcat</artifactId>
            <scope>provided</scope>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
        <!-- DEPENDENCIES FOR DISPLAYING JSPS AND USING JSTL TAGS -->
        <dependency>
            <groupId>org.apache.tomcat.embed</groupId>
            <artifactId>tomcat-embed-jasper</artifactId>
        </dependency>
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>jstl</artifactId>
        </dependency>
        <!-- DEPENDENCIES FOR INTEGRATING SQL DATABASE AND USING JPA -->
        <!-- Note: Project will not run until a schema has been created and the 
            proper settings in application properties are present for 
            connecting to a database. -->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <scope>runtime</scope>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-jpa</artifactId>
        </dependency>
        <!-- DEPENDENCY FOR USING VALIDATION ANNOTATIONS -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-validation</artifactId>
        </dependency>
        <!-- DEPENDENCY FOR USING BCRYPT  -->
        <dependency>
            <groupId>org.mindrot</groupId>
            <artifactId>jbcrypt</artifactId>
            <version>0.4</version>
        </dependency>
        <!-- DEPENDENCIES FOR BOOTSTRAP -->
        <dependency>
            <groupId>org.webjars</groupId>
            <artifactId>webjars-locator</artifactId>
            <version>0.42</version>
        </dependency>
        <dependency>
            <groupId>org.webjars</groupId>
            <artifactId>bootstrap</artifactId>
            <version>5.1.3</version>
        </dependency>
        <dependency>
            <groupId>org.webjars</groupId>
            <artifactId>jquery</artifactId>
            <version>3.6.0</version>
        </dependency>
        <!-- DEPENDENCY FOR JSP TEMPLATE INHERITANCE -->
        <dependency>
            <groupId>kr.pe.kwonnam.jsp</groupId>
            <artifactId>jsp-template-inheritance</artifactId>
            <version>0.3.RELEASE</version>
        </dependency>
```

### application.properties file:

> Go to *`src/main/resources`* and open the `application.properties` file.

**/src/main/resources/application.properties:**

```xml
# Where are jsp files? HERE!
spring.mvc.view.prefix=/WEB-INF/
# Data Persistence
spring.datasource.url=jdbc:mysql://localhost:3306/<<YOUR_SCHEMA_NAME>>
spring.datasource.username=root
spring.datasource.password=root
spring.datasource.driver-class-name=com.mysql.jdbc.Driver
spring.jpa.hibernate.ddl-auto=update
spring.jpa.open-in-view=false
# For Update and Delete method hidden inputs
spring.mvc.hiddenmethod.filter.enabled=true
```

*NOTE: Don't forget to change your schema name!!!*

### JSP tag library imports, settings, external styling

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<!-- c:out ; c:forEach etc. --> 
<%@taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c"%>
<!-- Formatting (dates) --> 
<%@taglib uri="http://java.sun.com/jsp/jstl/fmt" prefix="fmt"%>
<!-- form:form -->
<%@ taglib prefix="form" uri="http://www.springframework.org/tags/form"%>
<!-- for rendering errors on PUT routes -->
<%@ page isErrorPage="true" %>
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>Tacos</title>
    <link rel="stylesheet" href="/webjars/bootstrap/css/bootstrap.min.css">
    <link rel="stylesheet" href="/css/main.css"> <!-- change to match your file/naming structure -->
    <script src="/webjars/jquery/jquery.min.js"></script>
    <script src="/webjars/bootstrap/js/bootstrap.min.js"></script>
</head>
<body>
   
</body>
</html>
```

## Spring Boot Templating Boilerplates

> Go to `src` -> `main` -> right-click `webapp` and create a new folder named `WEB-INF`. Inside the `WEB-INF` folder, create your `.jsp` file.

**index.jsp:**

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8" 
pageEncoding="UTF-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert Title Here</title>
<!-- for Bootstrap CSS -->
<link rel="stylesheet" href="/webjars/bootstrap/css/bootstrap.min.css" />
<!-- YOUR own local CSS -->
<link rel="stylesheet" type="text/css" href="/css/style.css">
<!-- For any Bootstrap that uses JS or jQuery-->
<script src="/webjars/jquery/jquery.min.js"></script>
<script src="/webjars/bootstrap/js/bootstrap.min.js"></script>
<!-- YOUR own local JS -->
<script type="text/javascript" src="/js/app.js"></script>
</head>
<body>
 <div class="container mx-auto p-5">
  <!-- Enter body here -->
    <!-- c:out -->
    <c:out value = "${attribute_key}"/>
    <!-- c:forEach -->
    <c:forEach var="number" begin="1" end="10">
        <c:out value="${number}" />
    </c:forEach>
 </div>
</body>
</html>
```

&nbsp;

---

### JSP Template Inheritance

*NOTE: The dependency for this has already been added to the list of dependencies above.*

> Create a `web.xml` file in the `WEB-INF` folder

**/WEB-INF/web.xml:**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app version="2.5" xmlns="http://java.sun.com/xml/ns/javaee"
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd">
    <session-config>  <!--  10 minutes -->
        <session-timeout>10</session-timeout>
    </session-config>
    
    <context-param>
        <param-name>jsp-inheritance-prefix</param-name>
        <param-value>/WEB-INF/layouts/</param-value>
    </context-param>

    <context-param>
        <param-name>jsp-inheritance-suffix</param-name>
        <param-value>.jsp</param-value>
    </context-param>

    <welcome-file-list>
        <welcome-file>index.html</welcome-file>
        <welcome-file>index.htm</welcome-file>
        <welcome-file>index.jsp</welcome-file>
        <welcome-file>default.html</welcome-file>
        <welcome-file>default.htm</welcome-file>
        <welcome-file>default.jsp</welcome-file>
    </welcome-file-list>

</web-app>
```

**/WEB-INF/layouts/base.jsp : layout**

```jsp
<%@page contentType="text/html; charset=UTF-8" %>
<%@ taglib uri="http://kwonnam.pe.kr/jsp/template-inheritance" prefix="layout"%>
<!DOCTYPE html>
<html lang="en">
    <head>
        <title>JSP Template Inheritance</title>
    </head>

<h1>Head</h1>
<div>
    <layout:block name="header">
        header
    </layout:block>
</div>

<h1>Contents</h1>
<div>
    <p>
    <layout:block name="contents">
        <h2>Contents will be placed under this h2</h2>
    </layout:block>
    </p>
</div>

<div class="footer">
    <hr />
    <a href="https://github.com/kwon37xi/jsp-template-inheritance">jsp template inheritance example</a>
</div>
</html>
```

**/WEB-INF/layouts/view.jsp : contents**

```jsp
<%@page contentType="text/html; charset=UTF-8" %>
<%@ taglib uri="http://kwonnam.pe.kr/jsp/template-inheritance" prefix="layout"%>
<layout:extends name="base">
    <layout:put block="header" type="REPLACE">
        <h2>This is an example about layout management with JSP Template Inheritance</h2>
    </layout:put>
    <layout:put block="contents">
        Lorem ipsum dolor sit amet, consectetur adipiscing elit. Proin porta,
        augue ut ornare sagittis, diam libero facilisis augue, quis accumsan enim velit a mauris. Ut eleifend elit ante, sit amet suscipit leo lobortis eu. Quisque vitae lorem feugiat, lacinia nulla eu, interdum eros. Ut dignissim tincidunt nisl ac iaculis. Praesent consectetur arcu vitae tellus scelerisque venenatis. Morbi vel leo eros. In id libero ultricies, laoreet enim et, tempor magna. Vestibulum lorem velit, accumsan id purus at, lobortis fermentum diam. Aenean nec placerat elit. Aenean vel sem arcu.
    </layout:put>
</layout:extends>
```

#### PutType

1. APPEND : The put contents will be appended after block's contents. default.
2. PREPEND : The put contents will be prepended before block's contents.
3. REPLACE : The put contents will replace block's contents. The block's contents will be removed.

PutType values are case insensitive.

#### [Layout path prefix/suffix](https://github.com/kwon37xi/jsp-template-inheritance#layout-path-prefixsuffix)

In web.xml you can set `jsp-inheritance-prefix` and `jsp-inheritance-suffix` context parameters. The layout name will be prefixed and suffixed with these parameters.

When you set `jsp-inheritance-prefix` as `/WEB-INF/layouts/`, `jsp-ineheritance-suffix` as `.jsp` and call `<layout:extends name='base'>` the layout file URL will be `/WEB-INF/layouts/base.jsp`.

The prefix and suffix can be omitted.

#### [Example](https://github.com/kwon37xi/jsp-template-inheritance#example)

`example` module is web application layout example. Run the module and browse <http://localhost:8080/index.jsp>. Refer to `index.jsp`, `WEB-INF/layouts/base.jsp`, `WEB-INF/layouts/secondLayout.jsp`.

#### [Known problems](https://github.com/kwon37xi/jsp-template-inheritance#known-problems)

1. If you want to pass a variable from a child page to it's layout page, the variable must be added to **REQUEST** scope.

&nbsp;

---

## Spring MVC

### Validation Annotations in Domain Models

*NOTE: The dependency for this has already been added to the list of dependencies above.*

> Within the main package `src/main/java/com/<<your_name>>/<<project_name>>`, create a new package called `models` and inside the models package, create a new class with private attributes, an empty constructor, and getters and setters.

**src/main/java/com/<<your_name>>/<<project_name>>/models/Class.java:**

```java
package com.<<your_name>>.<<project_name>>.models;

import java.util.Date;

import javax.persistence.Column;
import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;
import javax.persistence.PrePersist;
import javax.persistence.PreUpdate;
import javax.persistence.Table;
import javax.validation.constraints.Min;
import javax.validation.constraints.NotNull;
import javax.validation.constraints.Size;

import org.springframework.format.annotation.DateTimeFormat;

// These two annotations tell Spring the following:
@Entity // specifies that this class is an entity and is related to the database
@Table(name = "books") // there is a table named "books" in the database
public class Book {
    @Id // primary key is going to be auto-generated with "@GeneratedValue"
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    // Validations
    @NotNull
    @Size(min = 5, max = 200, message = "Enter error messages here")
    private String title;
    @NotNull
    @Size(min = 5, max = 200, message = "Enter error messages here")
    private String description;
    @NotNull
    @Size(min = 3, max = 40, message = "Enter error messages here")
    private String language;
    @NotNull
    @Min(value = 100, message = "Enter error messages here")
    private Integer numberOfPages;
    // This will not allow the createdAt column to be updated after creation
    @Column(updatable = false)
    @DateTimeFormat(pattern = "yyyy-MM-dd")
    private Date createdAt;
    @DateTimeFormat(pattern = "yyyy-MM-dd")
    private Date updatedAt;

    // Constructors
    public Book() {
    }
    public Book(String title, String desc, String lang, int pages) {
        this.title = title;
        this.description = desc;
        this.language = lang;
        this.numberOfPages = pages;
    }
    
    // Getters & Setters
    /**
      * @return the id
      */
    public Long getId() {
      return id;
    }
    /**
      * @param id the id to set
      */
    public void setId(Long id) {
      this.id = id;
    }
    /**
      * @return the title
      */
    public String getTitle() {
      return title;
    }
    /**
      * @param title the title to set
      */
    public void setTitle(String title) {
      this.title = title;
    }
    /**
      * @return the description
      */
    public String getDescription() {
      return description;
    }
    /**
      * @param description the description to set
      */
    public void setDescription(String description) {
      this.description = description;
    }
    /**
      * @return the language
      */
    public String getLanguage() {
      return language;
    }
    /**
      * @param language the language to set
      */
    public void setLanguage(String language) {
      this.language = language;
    }
    /**
      * @return the numberOfPages
      */
    public Integer getNumberOfPages() {
      return numberOfPages;
    }
    /**
      * @param numberOfPages the numberOfPages to set
      */
    public void setNumberOfPages(Integer numberOfPages) {
      this.numberOfPages = numberOfPages;
    }
    /**
      * @return the createdAt
      */
    public Date getCreatedAt() {
      return createdAt;
    }
    /**
      * @param createdAt the createdAt to set
      */
    public void setCreatedAt(Date createdAt) {
      this.createdAt = createdAt;
    }
    /**
      * @return the updatedAt
      */
    public Date getUpdatedAt() {
      return updatedAt;
    }
    /**
      * @param updatedAt the updatedAt to set
      */
    public void setUpdatedAt(Date updatedAt) {
      this.updatedAt = updatedAt;
    }
    
    @PrePersist // runs the method right before the object is created
        protected void onCreate(){
            this.createdAt = new Date();
    }
    @PreUpdate // runs a method when the object is modified
    protected void onUpdate(){
        this.updatedAt = new Date();
    }
}

```

&nbsp;

---

### [Repositories](https://docs.spring.io/spring-data/commons/docs/current/api/org/springframework/data/repository/CrudRepository.html)

> Create another package within the main package and call it `repositories`. Create the class repository *interface* and *extend* the `CrudRepository`.

**src/main/java/com/<<your_name>>/<<project_name>>/repositories/BookRepository.java:**

```java
package com.<<your_name>>.<<project_name>>.models;

import java.util.List;

import org.springframework.data.repository.CrudRepository;
import org.springframework.stereotype.Repository;

import com.<<your_name>>.<<project_name>>.models.Book;

@Repository
public interface BookRepository extends CrudRepository<Book, Long> {
    // CrudRepository holds all the CRUD operations
    // "Long" is the datatype of the primary key
    // How JPA works to create custom queries following a certain format
    // depending on the method signature
    List<Book> findAll(); // this method retrieves all the books from the database

    List<Book> findByDescriptionContaining(String search); // this method finds books with descriptions containing the search string

    Long countByTitleContaining(String search); // this method counts how many titles contain a certain string

    Long deleteByTitleStartingWith(String search); // this method deletes a book that starts with a specific title
}
```

**QUERY LINKS:**

- [Query Creation Examples](https://docs.spring.io/spring-data/jpa/docs/current/reference/html/#jpa.query-methods.query-creation)
- [Supported Query Keywords](https://docs.spring.io/spring-data/jpa/docs/current/reference/html/#repository-query-keywords)
- [Baeldung Examples of Derived Queries](https://www.baeldung.com/spring-data-derived-queries)

&nbsp;

---

### Services

> Create a new package within the main package and call it `services`. In that package, create a class and declare it as a service.

**src/main/java/com/<<your_name>>/<<project_name>>/services/BookService.java:**

```java
package com.<<your_name>>.<<project_name>>.services;

import java.util.List;
import java.util.Optional;

import org.springframework.stereotype.Service;

import com.<<your_name>>.booksapi.models.Book;
import com.<<your_name>>.booksapi.repositories.BookRepository;

@Service
public class BookService {
    private final BookRepository bookRepository;
    // Constructor
    public BookService(BookRepository bookRepository) {
      this.bookRepository = bookRepository;
    }

    // retrieves all the books
    public List<Book> allBooks() {
        return bookRepository.findAll();
    }
    // creates a book
    public Book createBook(Book b) {
        return bookRepository.save(b);
    }
    // retrieves a book if it exists (optional)
    public Book findBook(Long id) {
        Optional<Book> optionalBook = bookRepository.findById(id);
        if (optionalBook.isPresent()) {
            return optionalBook.get();
        } else {
            return null;
        }
    }
    // update a book
    public Book updateBook(Book book, Long id) {
        Optional<Book> optionalBook = bookRepository.findById(id);
        if (optionalBook.isPresent()) {
        Book book1 = optionalBook.get();
        book1.setTitle(book.getTitle());
        book1.setDescription(book.getDescription());
        book1.setLanguage(book.getLanguage());
        book1.setNumberOfPages(book.getNumberOfPages());
            return bookRepository.save(book1);
        } else {
            return null;
        } 
    }
    // delete a book
    public String delete(Long id) {
        Optional<Book> optionalBook = bookRepository.findById(id);
        if (optionalBook.isPresent()) {
            bookRepository.deleteById(id);
            return "Deleted";
        } else {
            return "No book to delete";
        }
    }
}
```

**src/main/java/com/<<your_name>>/<<project_name>>/controllers/BooksApi.java:**

```java
package com.<<your_name>>.<<project_name>>.controllers;

import java.util.List;

import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.RestController;

import com.<<your_name>>.booksapi.models.Book;
import com.<<your_name>>.booksapi.services.BookService;

@RestController
public class BooksApi {
    private final BookService bookService; // tells us that we are going to be using a bookService and that it will not be changing
    // Constructor
    public BooksApi(BookService bookService){
        this.bookService = bookService;
    }
    // Get all
    @RequestMapping("/api/books")
    public List<Book> index() {
        return bookService.allBooks();
    }
    // Create
    @RequestMapping(value="/api/books", method=RequestMethod.POST)
    public Book create(
        @RequestParam(value="title") String title, 
        @RequestParam(value="description") String desc, 
        @RequestParam(value="language") String lang, 
        @RequestParam(value="pages") Integer numOfPages) {
        Book book = new Book(title, desc, lang, numOfPages);
        return bookService.createBook(book);
    }
    // View One
    @RequestMapping("/api/books/{id}")
    public Book show(@PathVariable("id") Long id) {
        Book book = bookService.findBook(id);
        return book;
    }
    // Update
    @RequestMapping(value="/api/books/{id}", method=RequestMethod.PUT)
    public Book update(
        @PathVariable("id") Long id, 
        @RequestParam(value="title") String title, 
        @RequestParam(value="description") String desc, 
        @RequestParam(value="language") String lang,
        @RequestParam(value="pages") Integer numOfPages) {
        Book book = bookService.updateBook(id, title, desc, lang, numOfPages);
        return book;
    }
    // Delete
    @RequestMapping(value="/api/books/{id}", method=RequestMethod.DELETE)
    public void destroy(@PathVariable("id") Long id) {
        bookService.deleteBook(id);
    }

}
```

> Open up `Postman` and test your CRUD functionality.

&nbsp;

---

## Adding Views (No More API/Postman)

### @Controller Boilerplate with full CRUD

> Create a new controller in the controller package.

**src/main/java/com/<<your_name>>/<<project_name>>/controllers/BooksController.java:**

```java
package com.<<your_name>>.<<project_name>>.controllers;

import java.util.List;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;

import com.<<your_name>>.<<project_name>>.models.Book;
import com.<<your_name>>.<<project_name>>.services.BookService;

@Controller
public class BookController {
//  dependency injection to access the information in service:
//  private final BookService bookService;
//    
//    public BooksController(BookService bookService) {
//        this.bookService = bookService;
//    }
    // OR use @Autowired annotation to inject bean from service file 
    // to this file in place of doing the steps above
    @Autowired
    BookService bookService;
    // View All Books
    @GetMapping("/books")
    public String index(Model model) {
          List<Book> books = bookService.allBooks();
          model.addAttribute("books", books);
          return "index.jsp";
    }
    // View One Book
    @GetMapping("/books/{bookId}")
    public String show(Model model, @PathVariable("bookId") Long bookId) {
      //  System.out.println(bookId);
        Book book = bookService.findBook(bookId);
      //  System.out.println(book);
        model.addAttribute("book", book);
        return "show.jsp";
      }
        // Route to form (without data binding)
        @GetMapping("/books/new")
        public String newBook() {
          return "new.jsp"; // create new jsp file to render form
        }
        // Route to form (with data binding using @ModelAttribute annotation)
        @GetMapping("/books/new")
            public String newBook(@ModelAttribute("book") Book book) {
            return "new.jsp";
            }
        // Route to form (with data binding using Model dependency injection)
        @GetMapping("/books/new")
            public String newBook(Model model) {
                model.addAttribute("books", new Book());
            return "new.jsp";
            }
        // Submit new (without data binding)
        @PostMapping("/books/submit")
        public String create(
            @RequestParam("title") String title,
            @RequestParam("description") String description,
            @RequestParam("language") String language,
            @RequestParam("pages") Integer pages) 
        {
            // CODE TO MAKE A NEW BOOK AND SAVE TO THE DB
            Book book = new Book(title, description, language, pages);
            bookService.createBook(book);
            return "redirect:/books";
        }
        // Submit new book (with data binding and validation with @Valid annotation)
        @PostMapping("/books/submit")
        public String create(@Valid @ModelAttribute("book") Book book,
            BindingResult result) 
        {
            // CODE TO MAKE A NEW BOOK AND SAVE TO THE DB
            bookService.createBook(book);
            return "redirect:/books";
        }
        // Edit book page
        @GetMapping("/books/{bookId}/edit")
        public String edit(@PathVariable("bookId") Long bookId, Model model) {
            Book book = bookService.findBook(bookId);
            model.addAttribute("book", book);
            return "edit.jsp";
        }
        // Submit updates
        @PutMapping("/books/{bookId}")
        public String update(
                @PathVariable("bookId") Long bookId,
                @Valid @ModelAttribute("book") Book book, BindingResult result) {
            if (result.hasErrors()) {
                return "edit.jsp";
            } else {
                System.out.println();
                bookService.updateBook(book, bookId);
                return "redirect:/books";
            }
        }
        // Delete a book
        @DeleteMapping("/books/{bookId}")
        public String destroy(@PathVariable("bookId") Long bookId) {
            bookService.delete(bookId);
            return "redirect:/books";
        }
}
```

### View All Table JSP Boilerplate

**(View all with forEach to render data in a table dynamically) index.jsp:**

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
 pageEncoding="UTF-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Books</title>
<!-- for Bootstrap CSS -->
<link rel="stylesheet" href="/webjars/bootstrap/css/bootstrap.min.css" />
<!-- YOUR own local CSS -->
<link rel="stylesheet" type="text/css" href="/css/style.css">
<!-- For any Bootstrap that uses JS or jQuery-->
<script src="/webjars/jquery/jquery.min.js"></script>
<script src="/webjars/bootstrap/js/bootstrap.min.js"></script>
<!-- YOUR own local JS -->
<script type="text/javascript" src="/js/app.js"></script>
</head>
<body>
  <div class="container mx-auto p-5">
    <h4 class="heading mb-4">All Books</h4>
    <table class="table">
      <thead>
        <tr>
          <th scope="col">ID</th>
          <th scope="col">Title</th>
          <th scope="col">Language</th>
          <th scope="col"># Pages</th>
        </tr>
      </thead>
      <tbody>
        <c:forEach var="book" items="${books}">
          <tr>
            <td><c:out value="${book.id}" /></td>
            <td><a href="/books/${book.id}"><c:out value="${book.title}" /></a></td>
            <td><c:out value="${book.language}" /></td>
            <td><c:out value="${book.numberOfPages}" /></td>
          </tr>
        </c:forEach>
      </tbody>
    </table>
  </div>
</body>
</html>
```

### View One JSP Boilerplate

**(View one to show book details) show.jsp:**

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
  pageEncoding="UTF-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>View Book Details</title>
<!-- for Bootstrap CSS -->
<link rel="stylesheet" href="/webjars/bootstrap/css/bootstrap.min.css" />
<!-- YOUR own local CSS -->
<link rel="stylesheet" type="text/css" href="/css/style.css">
<!-- For any Bootstrap that uses JS or jQuery-->
<script src="/webjars/jquery/jquery.min.js"></script>
<script src="/webjars/bootstrap/js/bootstrap.min.js"></script>
<!-- YOUR own local JS -->
<script type="text/javascript" src="/js/app.js"></script>
</head>
<body>
  <div class="container mx-auto p-5">
    <div class="card mx-auto p-5">
      <h3 class="mb-4">${book.title}</h3>
      <p><strong>Description:</strong> ${book.description}</p>
      <p><strong>Language:</strong> ${book.language}</p>
      <p><strong>Number of Pages:</strong> ${book.numberOfPages}</p>
    <form action="/books/${book.id}" method="post">
        <input type="hidden" name="_method" value="delete">
        <input type="submit" value="Delete">
    </form>
    </div>
    <div class="d-flex m-4 justify-content-center">
      <a href="/books">Back to Books List</a>
    </div>
  </div>
</body>
</html>
```

### Create Form JSP Boilerplate

> Add form prefix import, put `form:` in front of all label and input tags (including closing tags except for the submit input) and change all `name` attributes to `path`. *All path values must match the name of that member variable in the Class.*

**(Form page with form prefix import and error validations) new.jsp:**

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>
<%@ taglib prefix="form" uri="http://www.springframework.org/tags/form" %>
<%@ page isErrorPage="true" %> 
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Add New Book</title>
<!-- for Bootstrap CSS -->
<link rel="stylesheet" href="/webjars/bootstrap/css/bootstrap.min.css" />
<!-- YOUR own local CSS -->
<link rel="stylesheet" type="text/css" href="/css/style.css">
<!-- For any Bootstrap that uses JS or jQuery-->
<script src="/webjars/jquery/jquery.min.js"></script>
<script src="/webjars/bootstrap/js/bootstrap.min.js"></script>
<!-- YOUR own local JS -->
<script type="text/javascript" src="/js/app.js"></script>
</head>
<body>
    <div class="container mx-auto p-5">
        <h2 class="text-center mb-4">Add New Book</h2>
        <div class="card mx-auto p-5">
            <form:form action="/books" method="post" modelAttribute="book">
                <p>
                    <form:label path="title">Title</form:label>
                    <form:errors path="title"/>
                    <form:input path="title"/>
                </p>
                <p>
                    <form:label path="description">Description</form:label>
                    <form:errors path="description"/>
                    <form:textarea path="description"/>
                </p>
                <p>
                    <form:label path="language">Language</form:label>
                    <form:errors path="language"/>
                    <form:input path="language"/>
                </p>
                <p>
                    <form:label path="numberOfPages">Pages</form:label>
                    <form:errors path="numberOfPages"/>     
                    <form:input type="number" path="numberOfPages"/>
                </p>    
                <input type="submit" value="Submit"/>
            </form:form>  
        </div>
    </div>
</body>
</html>
```

### Update Form/Delete JSP Boilerplate

> Add input type hidden with name of `_method` and value of `put`.

**src/main/webapp/WEB-INF/books/edit.jsp:**

```jsp
<!-- additional tags to add -->
<%@ page isErrorPage="true" %>    
<%@ taglib prefix="form" uri="http://www.springframework.org/tags/form"%>     
<h1>Edit Book</h1>
<form:form action="/books/${book.id}" method="post" modelAttribute="book">
    <input type="hidden" name="_method" value="put">
    <p>
        <form:label path="title">Title</form:label>
        <form:errors path="title"/>
        <form:input path="title"/>
    </p>
    <p>
        <form:label path="description">Description</form:label>
        <form:errors path="description"/>
        <form:textarea path="description"/>
    </p>
    <p>
        <form:label path="language">Language</form:label>
        <form:errors path="language"/>
        <form:input path="language"/>
    </p>
    <p>
        <form:label path="numberOfPages">Pages</form:label>
        <form:errors path="numberOfPages"/>     
        <form:input type="number" path="numberOfPages"/>
    </p>    
    <input type="submit" value="Submit"/>
    <form action="/books/${book.id}" method="post">
        <input type="hidden" name="_method" value="delete">
        <input type="submit" value="Delete">
    </form>
</form:form>
```
