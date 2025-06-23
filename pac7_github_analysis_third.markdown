# Exercício – Análise Técnica de Projetos Reais no GitHub
**Nome:** [Seu Nome]  
**Data:** 23/06/2025  
**Objetivo:** Analisar projetos open-source no GitHub que implementem características técnicas específicas, identificando trechos de código e justificando seu uso.

---

## 1. Projeto Analisado: [Spring Cloud Stream RabbitMQ](https://github.com/spring-cloud-samples/spring-cloud-stream-samples)
**Link:** https://github.com/spring-cloud-samples/spring-cloud-stream-samples  
**Característica Identificada:** Filas de mensageria (RabbitMQ)

### Evidência (Trecho de Código):
```java
// src/main/java/sample/Consumer.java
@StreamListener(Sink.INPUT)
public void handleMessage(String message) {
    System.out.println("Received: " + message);
    // Process message
}
```

### Justificativa:
O projeto utiliza RabbitMQ com Spring Cloud Stream para processar mensagens assíncronas em uma aplicação distribuída. RabbitMQ foi adotado por sua confiabilidade em entrega de mensagens e suporte a padrões de roteamento flexíveis, permitindo desacoplamento entre serviços como notificações e processamento de eventos. Isso melhora a escalabilidade e tolerância a falhas.

---

## 2. Projeto Analisado: [Spring Boot Cache Redis](https://github.com/eugenp/tutorials/tree/master/spring-boot-modules/spring-boot-cache)
**Link:** https://github.com/eugenp/tutorials  
**Característica Identificada:** Uso de cache (Redis)

### Evidência (Trecho de Código):
```java
// src/main/java/com/baeldung/cache/config/CacheConfig.java
@Configuration
@EnableCaching
public class CacheConfig {
    @Bean
    public RedisCacheManager cacheManager(RedisConnectionFactory connectionFactory) {
        return RedisCacheManager.create(connectionFactory);
    }
}
```

### Justificativa:
O projeto utiliza Redis para caching de dados, como resultados de consultas frequentes a APIs. Redis foi escolhido por sua alta performance em memória e baixa latência, reduzindo a carga em bancos de dados relacionais. Essa abordagem melhora o desempenho de endpoints críticos, especialmente em aplicações com tráfego intenso.

---

## 3. Projeto Analisado: [Spring Boot Resilience4j](https://github.com/resilience4j/resilience4j-spring-boot2-demo)
**Link:** https://github.com/resilience4j/resilience4j-spring-boot2-demo  
**Característica Identificada:** Circuit breaker

### Evidência (Trecho de Código):
```java
// src/main/java/io/github/resilience4j/demo/service/BusinessService.java
@CircuitBreaker(name = "backendA", fallbackMethod = "fallback")
public String callBackendA() {
    return restTemplate.getForObject("http://backend-a/api", String.class);
}

public String fallback(Throwable t) {
    return "Fallback response due to: " + t.getMessage();
}
```

### Justificativa:
O projeto implementa o padrão circuit breaker com Resilience4j para proteger chamadas a serviços externos. Essa técnica foi adotada para prevenir falhas em cascata em arquiteturas de microserviços, retornando respostas de fallback quando um serviço está indisponível. Isso garante maior resiliência e estabilidade do sistema.

---

## 4. Projeto Analisado: [Spring Boot CI/CD Demo](https://github.com/piomin/sample-spring-boot-ci-cd)
**Link:** https://github.com/piomin/sample-spring-boot-ci-cd  
**Característica Identificada:** CI/CD com pipelines

### Evidência (Trecho de Código):
```yaml
# .github/workflows/build.yml
name: CI/CD Pipeline
on:
  push:
    branches: [ main ]
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Setup Java
        uses: actions/setup-java@v3
        with:
          java-version: '17'
      - name: Build and Test
        run: mvn clean package
```

### Justificativa:
O projeto utiliza GitHub Actions para um pipeline CI/CD que automatiza build, testes e deploy. Essa prática foi implementada para garantir entregas contínuas e detectar erros rapidamente, permitindo integração frequente de código. A execução de testes no pipeline assegura a qualidade antes de atingir produção.

---

## 5. Projeto Analisado: [SonarQube Java Example](https://github.com/SonarSource/sonar-java)
**Link:** https://github.com/SonarSource/sonar-java  
**Característica Identificada:** Análise estática de código (SonarQube)

### Evidência (Trecho de Código):
```yaml
# sonar-project.properties
sonar.projectKey=sonar-java
sonar.projectName=Sonar Java
sonar.sources=src/main/java
sonar.tests=src/test/java
sonar.java.binaries=target/classes
```

### Justificativa:
O projeto integra SonarQube para análise estática, identificando bugs, vulnerabilidades e code smells. SonarQube foi escolhido por sua capacidade de fornecer métricas detalhadas e integração com CI/CD, promovendo um código mais limpo e seguro. Essa prática é crucial para manter a qualidade em projetos colaborativos.

---

## 6. Projeto Analisado: [Spring Boot JWT Demo](https://github.com/szerhusenBC/jwt-spring-security-demo)
**Link:** https://github.com/szerhusenBC/jwt-spring-security-demo  
**Característica Identificada:** Autenticação com JWT

### Evidência (Trecho de Código):
```java
// src/main/java/org/zerhusen/security/JwtTokenProvider.java
public String generateToken(UserDetails userDetails) {
    Map<String, Object> claims = new HashMap<>();
    return Jwts.builder()
            .setClaims(claims)
            .setSubject(userDetails.getUsername())
            .setIssuedAt(new Date())
            .setExpiration(new Date(System.currentTimeMillis() + expiration * 1000))
            .signWith(SignatureAlgorithm.HS512, secret)
            .compact();
}
```

### Justificativa:
O projeto usa JWT para autenticação em uma API REST, permitindo acesso seguro sem sessões no servidor. JWT foi adotado por ser stateless, facilitando escalabilidade e integração com clientes diversos. A assinatura e expiração do token garantem segurança em endpoints protegidos.

---

## 7. Projeto Analisado: [Serverless AWS Java](https://github.com/serverless/examples/tree/master/aws-java-maven)
**Link:** https://github.com/serverless/examples  
**Característica Identificada:** Serverless/Lambda

### Evidência (Trecho de Código):
```yaml
# serverless.yml
service: aws-java-maven
provider:
  name: aws
  runtime: java11
functions:
  hello:
    handler: com.serverless.Handler
    events:
      - http:
          path: /hello
          method: get
```

### Justificativa:
O projeto utiliza AWS Lambda para executar funções serverless, eliminando a gestão de servidores. Essa abordagem foi escolhida para reduzir custos operacionais, com cobrança por execução, e para escalar automaticamente. É ideal para APIs com tráfego variável, como endpoints de consulta.

---

## 8. Projeto Analisado: [Spring OAuth2 Resource Server](https://github.com/spring-projects/spring-security-samples/tree/main/servlet/spring-boot/java/oauth2)
**Link:** https://github.com/spring-projects/spring-security-samples  
**Característica Identificada:** Autenticação com OAuth

### Evidência (Trecho de Código):
```java
// src/main/java/sample/SecurityConfig.java
@Configuration
@EnableWebSecurity
public class SecurityConfig {
    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        http
            .authorizeHttpRequests(authorize -> authorize.anyRequest().authenticated())
            .oauth2ResourceServer(OAuth2ResourceServerConfigurer::jwt);
        return http.build();
    }
}
```

### Justificativa:
O projeto implementa OAuth 2.0 como servidor de recursos, validando tokens JWT de provedores externos. OAuth foi escolhido por sua capacidade de delegar autenticação, reduzindo a complexidade de gerenciar credenciais. Essa abordagem é segura e amplamente usada em APIs modernas com integrações de terceiros.

---

## 9. Projeto Analisado: [Spring Boot Test Example](https://github.com/spring-projects/spring-boot-samples/tree/main/spring-boot-sample-test)
**Link:** https://github.com/spring-projects/spring-boot-samples  
**Característica Identificada:** Testes automatizados (unitários e integração)

### Evidência (Trecho de Código):
```java
// src/test/java/sample/ApplicationTests.java
@SpringBootTest
class ApplicationTests {
    @Autowired
    private SampleService service;

    @Test
    void testService() {
        String result = service.process("test");
        assertEquals("TEST", result);
    }
}
```

### Justificativa:
O projeto inclui testes unitários e de integração com Spring Boot Test e JUnit, verificando funcionalidades críticas. Testes automatizados foram adotados para garantir estabilidade e detectar regressões, permitindo alterações frequentes com confiança. Essa prática é essencial para manter a qualidade do software.

---

**Notas Finais:**
- Os projetos foram selecionados por sua popularidade (>10 estrelas) e clareza na documentação.
- As justificativas refletem o impacto técnico das características no contexto de cada projeto.
- A busca no GitHub utilizou termos como “rabbitmq”, “redis”, “resilience4j”, “github actions”, “sonarqube”, “jwt”, “serverless”, “oauth2”, e “spring test” para identificar repositórios relevantes.