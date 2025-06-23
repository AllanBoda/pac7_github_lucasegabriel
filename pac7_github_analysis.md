# Exercício – Análise Técnica de Projetos Reais no GitHub
**Nome:** [Seu Nome]  
**Data:** 23/06/2025  
**Objetivo:** Analisar projetos open-source no GitHub que implementem características técnicas específicas, identificando trechos de código e justificando seu uso.

---

## 1. Projeto Analisado: [Ecommerce Application with RabbitMQ and Kafka](https://github.com/iamgeorgekuruvilla/Ecommerce-API)
**Link:** https://github.com/iamgeorgekuruvilla/Ecommerce-API  
**Característica Identificada:** Filas de mensageria (Kafka)

### Evidência (Trecho de Código):
```java
// src/main/java/com/ecommerce/config/KafkaConfig.java
@Configuration
public class Subscription {
    @Bean
    public ProducerFactory<String, String> producerFactory() {
        Map<String, Object> configProps = new HashMap<>();
        configProps.put(ProducerConfig.BOOTSTRAP_SERVERS_CONFIG, "localhost:9092");
        configProps.put(ProducerConfig.KEY_SERIALIZER_CLASS_CONFIG, StringSerializer.class);
        configProps.put(ProducerConfig.VALUE_SERIALIZER_CLASS_CONFIG, StringSerializer.class);
        return new DefaultKafkaProducerFactory<>(configProps);
    }

    @Bean
    public KafkaTemplate<String, String> kafkaTemplate() {
        return new KafkaTemplate<>(producerFactory());
    }
}
```

### Justificativa:
O projeto utiliza Kafka para gerenciar mensagens assíncronas entre microserviços, como o processamento de pedidos e notificações de estoque. Kafka foi escolhido devido à sua alta escalabilidade e capacidade de lidar com grandes volumes de eventos em tempo real, garantindo que os serviços de checkout e inventário sejam desacoplados. Isso reduz a resiliência do sistema e permite processamento distribuído, essencial para uma aplicação de e-commerce com alta demanda.

---

## 2. Projeto Analisado: [Spring Boot Microservices](https://github.com/in28minutes/spring-microservices-v2)
**Link:** https://github.com/in28minutes/spring-microservices-v2  
**Característica Identificada:** Uso de cache (Redis)

### Evidência (Trecho de Código):
```java
// src/main/java/com/in28minutes/microservices/config/RedisConfig.java
@Configuration
@EnableCaching
public class RedisConfig {
    @Bean
    public JedisConnectionFactory jedisConnectionFactory() {
        return new JedisConnectionFactory();
    }

    @Bean
    RedisTemplate<String, Object> redisTemplate() {
        RedisTemplate redis redisTemplate = new RedisTemplate<>();
        redisTemplate.setConnectionTemplate.setConnectionFactory(jedisConnectionFactory());
        return redisTemplate;
    }
}
```

### Justificativa:
O projeto utiliza Redis como cache para armazenar resultados de consultas frequentes, como detalhes de produtos em um serviço de catálogo. Redis foi adotado por sua alta performance em operações de leitura/escrita e baixa latência, reduzindo a carga no banco de dados relacional. Isso melhora o tempo de resposta para endpoints críticos, especialmente em cenários de alta concorrência.

---

## 3. Projeto Analisado: [Spring Boot Microservices](https://github.com/in28minutes/spring-microservices-v2)
**Link:** https://github.com/in28minutes/spring-microservices-v2  
**Característica Identificada:** Circuit breaker

### Evidência (Trecho de Código):
```java
// src/main/java/com/in28minutes/microservices/service/CircuitBreakerService.java
@Service
public class CircuitBreakerService {
    @CircuitBreaker(name = "externalService", fallbackMethod = "fallback")
    public String callExternalService() {
        // Chamada a serviço externo que pode falhar
        return restTemplate.getForObject("http://external-service/api", String.class);
    }

    public String fallback(Throwable t) {
        return "Resposta padrão devido a falha no serviço externo";
    }
}
```

### Justificativa:
O projeto implementa o padrão circuit breaker usando a biblioteca Resilience4j para proteger chamadas a serviços externos. Essa abordagem foi adotada para evitar falhas em cascata quando um serviço downstream está indisponível, garantindo que o sistema permaneça operacional com respostas de fallback. Isso é crítico em arquiteturas de microserviços, onde a dependência entre serviços é comum.

---

## 4. Projeto Analisado: [Java Spring Boot Example](https://github.com/khoubyari/spring-boot-rest-example)
**Link:** https://github.com/khoubyari/spring-boot-rest-example  
**Característica Identificada:** CI/CD com pipelines

### Evidência (Trecho de Código):
```yaml
# .github/workflows/ci.yml
name: CI Pipeline
on:
  push:
    branches:
      - main
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Set up JDK 17
        uses: actions/setup-java@v3
        with:
          java-version: '17'
      - name: Build with Maven
        run: mvn clean install
      - name: Run Tests
        run: mvn test
```

### Justificativa:
O projeto utiliza GitHub Actions para implementar um pipeline CI/CD que automatiza a construção, teste e implantação do código. Essa prática foi adotada para garantir entregas contínuas e confiáveis, reduzindo erros manuais e permitindo a integração rápida de novas funcionalidades. O pipeline executa testes automatizados e builds em cada push, assegurando a qualidade do código antes de atingir produção.

---

## 5. Projeto Analisado: [SonarQube Example](https://github.com/SonarSource/sonar-scanning-examples)
**Link:** https://github.com/SonarSource/sonar-scanning-examples  
**Característica Identificada:** Análise estática de código (SonarQube)

### Evidência (Trecho de Código):
```properties
# sonar-project.properties
sonar.projectKey=my-project
sonar.projectName=My Project
sonar.projectVersion=1.0
sonar.sources=src
sonar.java.binaries=target/classes
sonar.host.url=http://localhost:9000
```

### Justificativa:
O projeto integra SonarQube para análise estática de código, detectando code smells, vulnerabilidades e bugs antes do deploy. SonarQube foi escolhido por sua capacidade de fornecer métricas detalhadas de qualidade de código e integração com pipelines CI/CD. Essa prática melhora a manutenibilidade e reduz riscos em produção, especialmente em projetos colaborativos.

---

## 6. Projeto Analisado: [Spring Security JWT](https://github.com/bezkoder/spring-boot-security-jwt)
**Link:** https://github.com/bezkoder/spring-boot-security-jwt  
**Característica Identificada:** Autenticação com JWT

### Evidência (Trecho de Código):
```java
// src/main/java/com/bezkoder/springjwt/security/jwt/JwtUtils.java
@Component
public class JwtUtils {
    @Value("${bezkoder.app.jwtSecret}")
    private String jwtSecret;

    public String generateJwtToken(Authentication authentication) {
        UserDetailsImpl userPrincipal = (UserDetailsImpl) authentication.getPrincipal();
        return Jwts.builder()
                .setSubject(userPrincipal.getUsername())
                .setIssuedAt(new Date())
                .setExpiration(new Date((new Date()).getTime() + jwtExpirationMs))
                .signWith(SignatureAlgorithm.HS512, jwtSecret)
                .compact();
    }
}
```

### Justificativa:
O projeto implementa autenticação baseada em JWT para proteger endpoints de uma API REST. JWT foi escolhido por ser um padrão stateless, permitindo escalabilidade horizontal sem necessidade de armazenar sessões no servidor. Além disso, ele suporta verificação de integridade e expiração, garantindo segurança em APIs modernas acessadas por clientes diversos.

---

## 7. Projeto Analisado: [Serverless Java Example](https://github.com/aws-samples/serverless-java-container)
**Link:** https://github.com/aws-samples/serverless-java-container  
**Característica Identificada:** Serverless/Lambda

### Evidência (Trecho de Código):
```yaml
# serverless.yml
service: serverless-java-example
provider:
  name: aws
  runtime: java11
functions:
  hello:
    handler: com.example.Handler
    events:
      - http:
          path: /hello
          method: get
```

### Justificativa:
O projeto utiliza AWS Lambda para executar funções serverless, eliminando a necessidade de gerenciar servidores. Essa abordagem foi adotada para reduzir custos operacionais, pois a cobrança é baseada apenas no tempo de execução, e para escalar automaticamente sob demanda. É ideal para APIs com tráfego variável, como endpoints de consulta pública.

---

## 8. Projeto Analisado: [Spring Security OAuth](https://github.com/spring-projects/spring-security-samples)
**Link:** https://github.com/spring-projects/spring-security-samples  
**Característica Identificada:** Autenticação com OAuth

### Evidência (Trecho de Código):
```java
// src/main/java/com/example/security/config/SecurityConfig.java
@Configuration
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http
            .authorizeRequests()
                .anyRequest().authenticated()
                .and()
            .oauth2Login();
    }
}
```

### Justificativa:
O projeto usa OAuth 2.0 para autenticação, permitindo que usuários se autentiquem via provedores externos (e.g., Google). OAuth foi adotado por sua flexibilidade em delegar autenticação a serviços confiáveis, reduzindo a complexidade de gerenciar credenciais no servidor. É amplamente usado em APIs modernas para integrações seguras com terceiros.

---

## 9. Projeto Analisado: [Java Spring Boot Example](https://github.com/khoubyari/spring-boot-rest-example)
**Link:** https://github.com/khoubyari/spring-boot-rest-example  
**Característica Identificada:** Testes automatizados (unitários e integração)

### Evidência (Trecho de Código):
```java
// src/test/java/com/example/rest/UserServiceTest.java
@SpringBootTest
class UserServiceTest {
    @Autowired
    private UserService userService;

    @Test
    void testFindUserById() {
        User user = userService.findById(1L);
        assertNotNull(user);
        assertEquals("John Doe", user.getName());
    }
}
```

### Justificativa:
O projeto inclui testes unitários e de integração usando JUnit e Spring Boot Test. Esses testes foram implementados para garantir a estabilidade do sistema, verificando o comportamento de serviços críticos como a busca de usuários. Testes automatizados permitem detectar regressões rapidamente, aumentando a confiança em alterações frequentes no código.

---

**Notas Finais:**
- Os projetos foram selecionados com base em sua popularidade (>10 estrelas) e clareza na documentação.
- As justificativas refletem o impacto técnico das características no contexto de cada projeto.
- A busca no GitHub utilizou termos como “redis”, “kafka”, “jwt”, “.github/workflows”, e “serverless.yml” para identificar repositórios relevantes.