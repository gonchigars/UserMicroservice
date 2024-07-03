1. Add dependencies to `pom.xml` in the project root:

```xml
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-boot-starter</artifactId>
    <version>3.0.0</version>
</dependency>
```

2. Create `SwaggerConfig.java` in `src/main/java/com/example/blogapp/config/`:

```java
package com.example.blogapp.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import springfox.documentation.builders.PathSelectors;
import springfox.documentation.builders.RequestHandlerSelectors;
import springfox.documentation.spi.DocumentationType;
import springfox.documentation.spring.web.plugins.Docket;

@Configuration
public class SwaggerConfig {
    @Bean
    public Docket api() {
        return new Docket(DocumentationType.SWAGGER_2)
            .select()
            .apis(RequestHandlerSelectors.basePackage("com.example.blogapp.controllers"))
            .paths(PathSelectors.any())
            .build();
    }
}
```

3. Update `UserController.java` in `src/main/java/com/example/blogapp/controllers/`:

```java
package com.example.blogapp.controllers;

import com.example.blogapp.models.User;
import com.example.blogapp.services.UserService;
import io.swagger.annotations.Api;
import io.swagger.annotations.ApiOperation;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.*;

@RestController
@RequestMapping("/api/users")
@Api(tags = "User Management")
public class UserController {
    @Autowired
    private UserService userService;

    @ApiOperation("Create a new user")
    @PostMapping
    public User createUser(@RequestBody User user) {
        return userService.createUser(user);
    }

    @ApiOperation("Get all users")
    @GetMapping
    public List<User> getAllUsers() {
        return userService.getAllUsers();
    }

    @ApiOperation("Get a user by ID")
    @GetMapping("/{userId}")
    public User getUserById(@PathVariable int userId) {
        return userService.getUserById(userId);
    }

    @ApiOperation("Update a user")
    @PutMapping("/{userId}")
    public User updateUser(@PathVariable int userId, @RequestBody User userDetails) {
        return userService.updateUser(userId, userDetails);
    }

    @ApiOperation("Delete a user")
    @DeleteMapping("/{userId}")
    public void deleteUser(@PathVariable int userId) {
        userService.deleteUser(userId);
    }
}
```

4. Update `application.properties` in `src/main/resources/`:

```properties
spring.mvc.pathmatch.matching-strategy=ant_path_matcher
```

5. Run your application and access Swagger UI at:

   - http://localhost:8080/swagger-ui/

6. The JSON specification will be available at:
   - http://localhost:8080/v2/api-docs

To present this in your interview:

1. Show the `pom.xml` and explain the added dependency.
2. Present the `SwaggerConfig.java` and describe its purpose.
3. Demonstrate the annotations added to `UserController.java`.
4. Show the `application.properties` update.
5. If possible, give a live demo of the Swagger UI and the generated JSON specification.

Emphasize that this setup:

- Automatically generates API documentation
- Provides an interactive UI for API testing
- Improves the developer experience for API consumers
- Follows best practices for API documentation in a microservices architecture
