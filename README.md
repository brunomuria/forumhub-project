
# 📚 FórumHub API

API REST em **Spring Boot 3** para gerenciamento de tópicos de fórum. Inclui **CRUD completo**, **MySQL + Flyway**, **JWT** (Auth0) e **Swagger UI** (springdoc). 

**Autor:** Bruno Daniel Muria de Farias · GitHub: [@brunomuria](https://github.com/brunomuria)

## ✅ Funcionalidades
- Autenticação JWT (`POST /login`)
- Tópicos: criar, listar (pagina/ordenar/filtrar), detalhar, atualizar, excluir
- Usuários: criar e listar (senha com BCrypt)
- Respostas: criar e listar por tópico
- Documentação: Swagger UI

## 🛠 Stack
Java 17 · Spring Boot 3 · JPA · Security · Auth0 JWT · MySQL · Flyway · Springdoc OpenAPI · Maven · Docker Compose · JUnit/Mockito

## ⚙️ Configuração local (sem Docker)
1. MySQL: crie o banco
```sql
CREATE DATABASE forumhub CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
```
2. `src/main/resources/application.properties` (ajuste usuário/senha):
```properties
spring.datasource.url=jdbc:mysql://localhost:3306/forumhub?useSSL=false&serverTimezone=UTC
spring.datasource.username=root
spring.datasource.password=senha
jwt.secret=uma_chave_super_secreta_e_longa_para_jwt
jwt.expiration-hours=2
```
3. Rode a aplicação:
```bash
mvn spring-boot:run
```
Flyway executará as migrations e criará um usuário de teste (user@test.com / 123456).

## 🐳 Executar com Docker Compose
Pré-requisitos: Docker e Docker Compose.
```bash
docker compose up --build -d
```
A API ficará em `http://localhost:8080` e o MySQL em `localhost:3306`.

> Variáveis de ambiente (ajustáveis em `docker-compose.yml`): `SPRING_DATASOURCE_URL`, `SPRING_DATASOURCE_USERNAME`, `SPRING_DATASOURCE_PASSWORD`, `JWT_SECRET`, `JWT_EXPIRATION-HOURS`.

## 🔐 Autenticação
- **Login**: `POST /login`
```json
{ "email": "user@test.com", "senha": "123456" }
```
Resposta:
```json
{ "token": "<JWT>", "tipo": "Bearer" }
```
Use o token nas requisições:
```
Authorization: Bearer <JWT>
```

## 📌 Endpoints de Tópicos
- **POST /topicos** – cria
- **GET /topicos** – lista (filtros: `?curso=...&ano=...`) com paginação
- **GET /topicos/{id}** – detalha
- **PUT /topicos/{id}** – atualiza (valida duplicidade `titulo+mensagem`)
- **DELETE /topicos/{id}** – exclui

## 📌 Usuários e Respostas
- **POST /usuario**, **GET /usuario**
- **POST /respostas**, **GET /respostas?topicoId=1**

## 🧪 Testes automatizados (JUnit/Mockito)
Rodar testes:
```bash
mvn test
```
Inclui testes de unidade para **TokenService** e **TopicoService**.

## 📖 Swagger UI
Acesse: `http://localhost:8080/swagger-ui.html`
OpenAPI: `http://localhost:8080/v3/api-docs`

## 📂 Estrutura
```
src/main/java/com/forumhub
 ├─ ForumHubApplication.java
 ├─ SwaggerConfig.java
 ├─ controller/ (TopicoController, UsuarioController, RespostaController)
 ├─ dto/ (Topico*, Resposta*, Usuario*)
 ├─ model/ (Topico, Resposta)
 ├─ repository/ (TopicoRepository, RespostaRepository)
 ├─ security/ (Usuario, UsuarioRepository, SecurityConfigurations, TokenService, JwtSecurityFilter, AuthController)
src/main/resources
 ├─ application.properties
 └─ db/migration (V1..V4)
```

## 👤 Autor
**Bruno Daniel Muria de Farias**  
GitHub: https://github.com/brunomuria

## 🚀 Upload no GitHub
```bash
git init
git add .
git commit -m "FórumHub API: CRUD + JWT + Swagger + Flyway + Docker Compose + Tests"
git branch -M main
git remote add origin https://github.com/brunomuria/forumhub.git
git push -u origin main
```
