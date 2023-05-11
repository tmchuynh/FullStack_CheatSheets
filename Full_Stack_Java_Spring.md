# Spring Boot App Cheat Sheet

## Start a new Spring Boot Project

1. Create an empty folder or directory wherever you want your Spring Boot projects to be located at.

2. Open STS and choose your workspace.

3. On the left-hand side in the Package Explorer, right click and hover over "New" and click on "Spring Starter Project."

4. In the 'New Spring Starter Project' window, we must fill out information about our project.

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

**Running the Application:**
> Right-click on your application -> click on `"Run As"` -> click on `Spring Boot App` -> go to "localhost:8080" to check your work

&nbsp;

## Dependencies & Configurations Reference

At the bottom, open the `pom.xml` file and replace everything in the `<dependencies>` tag with:

```xml
        <!-- DEPENDENCIES FOR STARTING SPRING PROJECTS-->
    <dependencies>
		<dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-jpa</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <scope>runtime</scope>
        </dependency>
        <dependency>
            <groupId>com.mysql</groupId>
            <artifactId>mysql-connector-j</artifactId>
            <scope>runtime</scope>
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
		<dependency>
			<groupId>jakarta.servlet.jsp.jstl</groupId>
			<artifactId>jakarta.servlet.jsp.jstl-api</artifactId>
		</dependency>
		<dependency>
			<groupId>org.glassfish.web</groupId>
			<artifactId>jakarta.servlet.jsp.jstl</artifactId>
		</dependency>
		<dependency>
			<groupId>org.apache.tomcat.embed</groupId>
			<artifactId>tomcat-embed-jasper</artifactId>
		</dependency>
        <dependency>
            <groupId>org.mindrot</groupId>
            <artifactId>jbcrypt</artifactId>
            <version>0.4</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-validation</artifactId>
        </dependency>
        
        <dependency>
	        <groupId>org.webjars</groupId>
	        <artifactId>webjars-locator</artifactId>
	        <version>0.46</version>
	    </dependency>
	    
	    <!-- BOOTSTRAP DEPENDENCIES -->
	    <dependency>
	        <groupId>org.webjars</groupId>
	        <artifactId>bootstrap</artifactId>
	        <version>5.2.3</version>
	    </dependency>
	</dependencies>
```

### application.properties file:

> Go to *`src/main/resources`* and open the `application.properties` file.

**/src/main/resources/application.properties:**

```xml
spring.mvc.view.prefix=/WEB-INF/

spring.datasource.url=jdbc:mysql://localhost:3306/<<YOUR_SCHEMA_NAME>>
spring.datasource.username=root
spring.datasource.password=root
spring.jpa.hibernate.ddl-auto=update
spring.mvc.hiddenmethod.filter.enabled=true
```

*NOTE: Don't forget to change your schema name!!!*

## Login and Registration
`User.java model`
Remember to make your  getters and setters
```java
@Entity
@Table(name = "users")
public class User {
	@Id
	@GeneratedValue(strategy = GenerationType.IDENTITY)
	private Long id;

	@NotEmpty(message = "Username is required!")
	@Size(min = 3, max = 30, message = "Username must be between 3 and 30 characters")
	private String userName;

	@NotEmpty(message = "Email is required!")
	@Email(message = "Please enter a valid email!")
	private String email;

	@NotEmpty(message = "Password is required!")
	@Size(min = 8, max = 128, message = "Password must be between 8 and 128 characters")
	private String password;

	@Transient
	@NotEmpty(message = "Confirm Password is required!")
	@Size(min = 8, max = 128, message = "Confirm Password must be between 8 and 128 characters")
	private String confirm;
  
  public User() {
	}
}
```

`LoginUser.java model`
```java
public class LoginUser {
	@NotEmpty(message="Email is required!")
    @Email(message="Please enter a valid email!")
    private String email;
    
    @NotEmpty(message="Password is required!")
    @Size(min=8, max=128, message="Password must be between 8 and 128 characters")
    private String password;
    
    public LoginUser() {}

	public String getEmail() {
		return email;
	}

	public void setEmail(String email) {
		this.email = email;
	}

	public String getPassword() {
		return password;
	}

	public void setPassword(String password) {
		this.password = password;
	}
    
}
```

`UserController.java`
```java
@Controller
public class UserController {
	@Autowired
	private UserService userServ;

	@GetMapping("/")
	public String index(Model model) {
		model.addAttribute("newUser", new User());
		model.addAttribute("newLogin", new LoginUser());
		return "index.jsp";
	}

	@GetMapping("/logout")
	public String logout(HttpSession session) {
		session.invalidate();
		return "redirect:/";
	}

	@PostMapping("/register")
	public String register(@Valid @ModelAttribute("newUser") User newUser, BindingResult result, Model model,
			HttpSession session) {
		User registeredUser = userServ.register(newUser, result);
		if (result.hasErrors()) {
			model.addAttribute("newLogin", new LoginUser());
			return "index.jsp";
		}
		session.setAttribute("userId", registeredUser.getId());
		return "redirect:/";
	}

	@PostMapping("/login")
	public String login(@Valid @ModelAttribute("newLogin") LoginUser newLogin, BindingResult result, Model model,
			HttpSession session) {
		User user = userServ.login(newLogin, result);
		if (result.hasErrors()) {
			model.addAttribute("newUser", new User());
			return "index.jsp";
		}
		session.setAttribute("username", user.getUserName());
		return "redirect:/welcome"; // this redirects to whatever handles the next --> usually is going to be your next Controller ie. /books 
	}
}
```

`UserRepository.java`
```java
@Repository
public interface UserRepository extends JpaRepository<User, Long> {
	User findByEmail(String email);
}
```

`UserService.java`
```java
@Service
public class UserService {
	@Autowired
    private UserRepository userRepo;

    public User register(User newUser, BindingResult result) {
        User existingUser = userRepo.findByEmail(newUser.getEmail());
        if (existingUser != null && existingUser.isPresent()) {
            result.rejectValue("email", "Unique", "This email is already in use!");
        }
        
        if (!newUser.getPassword().equals(newUser.getConfirm())) {
            result.rejectValue("confirm", "Match", "Passwords do not match!");
        }

        if (result.hasErrors()) {
            return null;
        }

        String hashed = BCrypt.hashpw(newUser.getPassword(), BCrypt.gensalt());
        newUser.setPassword(hashed);
        return userRepo.save(newUser);
    }

 	public User login(LoginUser newLoginObject, BindingResult result) {
 	    if (result.hasErrors()) {
 	        return null;
 	    }

 	    User existingUser = userRepo.findByEmail(newLoginObject.getEmail());
 	    if (existingUser == null) {
 	        result.rejectValue("email", "NotFound", "Email not found!");
 	        return null;
 	    }

 	    User user = existingUser;
 	    if (!BCrypt.checkpw(newLoginObject.getPassword(), user.getPassword())) {
 	        result.rejectValue("password", "Incorrect", "Incorrect Password!");
 	        return null;
 	    }
      
 	    return user;
 	}



 	public User getUserByEmail(String email) {
 	    return userRepo.findByEmail(email);
 	}


 	 public User getUserById(Long userId) {
         return userRepo.findById(userId)
                 .orElseThrow(() -> new IllegalArgumentException("Invalid user ID: " + userId));
     }

 	public User findById(Long id) {
 	    Optional<User> optionalUser = userRepo.findById(id);
 	    if (optionalUser.isPresent()) {
 	        return optionalUser.get();
 	    }
		return null;
 	}
}
```

`Login page`
```html
<%@ page language="java" contentType="text/html; charset=UTF-8"
	pageEncoding="UTF-8"%>
<%@ taglib uri="http://www.springframework.org/tags/form" prefix="form"%>
<!DOCTYPE html>
<html>

<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>

<body>
	<h1>Welcome!</h1>
	<h5>Join our growing community!</h5>

	<h2>Register</h2>
	<form:form modelAttribute="newUser" action="/register" method="post">
		<form:label path="userName">Name:</form:label>
		<form:input path="userName" />
		<br>
		<form:errors path="userName" cssClass="error" />
		<br>

		<form:label path="email">Email:</form:label>
		<form:input path="email" />
		<br>
		<form:errors path="email" cssClass="error" />
		<br>

		<form:label path="password">Password:</form:label>
		<form:password path="password" />
		<br>
		<form:errors path="password" cssClass="error" />
		<br>

		<form:label path="confirm">Confirm Password:</form:label>
		<form:password path="confirm" />
		<br>
		<form:errors path="confirm" cssClass="error" />
		<br>


		<input type="submit" value="Register" />
	</form:form>

	<h2>Login</h2>
	<form:form modelAttribute="newLogin" action="/login" method="post">
		<form:label path="email">Email:</form:label>
		<form:input path="email" />
		<br>
		<form:errors path="email" cssClass="error" />
		<br>

		<form:label path="password">Password:</form:label>
		<form:password path="password" />
		<br>
		<form:errors path="password" cssClass="error" />
		<br>

		<input type="submit" value="Login" />
	</form:form>
</body>

</html>
```
