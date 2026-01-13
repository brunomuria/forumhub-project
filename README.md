# forumhub-project
forumhub-project

*   groupId: `io.github.brunomuria`
*   Autor/README: “Bruno Muria  — GitHub: @brunomuria"
*   Docker Compose (MySQL + App)
*   Testes automatizados (JUnit/Mockito)

*   # Stack & Build

*   *   **Java 17**, **Spring Boot 3**
*   Maven (build pronto)
*   Spring Web, Spring Data JPA, Spring Validation
*   Spring Security + **JWT** (Auth0 `java-jwt`)
*   MySQL + **Flyway** (migrations versionadas)
*   Swagger/OpenAPI (**springdoc**)
*   **JUnit 5 + Mockito** (testes unitários)
*   **Dockerfile** (multi-stage) + **docker-compose.yml** (MySQL + App)

groupId e README

  `pom.xml`:
    ```xml
    <groupId>io.github.brunomuria</groupId>
    <artifactId>forumhub</artifactId>


    *   `README.md`:
    *   Autor:  Bruno Muria
    *   GitHub: @brunomuria
    *   Instruções Docker, Swagger, JWT, testes.

    Docker Compose (MySQL + App)

    Arquivos criados:

*   **Dockerfile** (multi-stage build: Maven → Temurin JRE)
*   **docker-compose.yml** (serviços `db` e `app`)

Principais configs:

```yaml
services:
  db:
    image: mysql:8.0
    environment:
      MYSQL_ROOT_PASSWORD: root
      MYSQL_DATABASE: forumhub
      MYSQL_USER: forumuser
      MYSQL_PASSWORD: forumpass
  app:
    build: .
    environment:
      SPRING_DATASOURCE_URL: jdbc:mysql://db:3306/forumhub?useSSL=false&serverTimezone=UTC
      SPRING_DATASOURCE_USERNAME: forumuser
      SPRING_DATASOURCE_PASSWORD: forumpass
      JWT_SECRET: change_me_super_secret
      JWT_EXPIRATION-HOURS: 2
    ports:
      - "8080:8080"
```
Testes automatizados (JUnit/Mockito)


*   **`TokenServiceTest`**: garante geração/validação do token JWT.
*   **`TopicoServiceTest`**: valida regra de **não duplicidade** (mock do repositório).
*   Perfil de teste usa **H2** (escopo de teste) e desabilita Flyway.

Comandos:

```bash
mvn test
```

***

## Estrutura principal

    forumhub/
     ├─ pom.xml                           # groupId io.github.brunomuria
     ├─ Dockerfile
     ├─ docker-compose.yml
     ├─ .dockerignore
     ├─ README.md                         # autor + link GitHub + docs
     ├─ postman/ForumHub.postman_collection.json
     └─ src/
        ├─ main/java/com/forumhub/
        │  ├─ ForumHubApplication.java
        │  ├─ SwaggerConfig.java
        │  ├─ controller/
        │  │  ├─ TopicoController.java
        │  │  ├─ UsuarioController.java
        │  │  └─ RespostaController.java
        │  ├─ dto/
        │  │  ├─ TopicoDTOs.java
        │  │  ├─ UsuarioDTOs.java
        │  │  └─ RespostaDTOs.java
        │  ├─ model/
        │  │  ├─ Topico.java
        │  │  └─ Resposta.java
        │  ├─ repository/
        │  │  ├─ TopicoRepository.java
        │  │  └─ RespostaRepository.java
        │  ├─ security/
        │  │  ├─ Usuario.java
        │  │  ├─ UsuarioRepository.java
        │  │  ├─ SecurityConfigurations.java
        │  │  ├─ TokenService.java
        │  │  ├─ JwtSecurityFilter.java
        │  │  ├─ AuthController.java
        │  │  └─ dto/AuthDTOs.java
        ├─ main/resources/
        │  ├─ application.properties      # com override por env
        │  └─ db/migration/
        │     ├─ V1__create_table_topico.sql
        │     ├─ V2__create_table_usuario.sql
        │     ├─ V3__create_table_resposta.sql
        │     └─ V4__seed_usuario_teste.sql
        └─ test/
           ├─ java/com/forumhub/security/TokenServiceTest.java
           ├─ java/com/forumhub/service/TopicoServiceTest.java
           └─ resources/application-test.properties




