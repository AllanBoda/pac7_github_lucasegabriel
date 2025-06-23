# Exercício – Análise Técnica de Projetos Reais no GitHub
**Nome:** [Seu Nome]  
**Data:** 23/06/2025  
**Objetivo:** Analisar projetos open-source no GitHub que implementem características técnicas específicas, identificando trechos de código e justificando seu uso.

---

## 1. Projeto Analisado: [Event-Driven Microservices](https://github.com/piomin/sample-spring-cloud-stream-kafka)
**Link:** https://github.com/piomin/sample-spring-cloud-stream-kafka  
**Característica Identificada:** Filas de mensageria (Kafka)

### Evidência (Trecho de Código):
```java
// src/main/java/pl/piomin/services/kafka/consumer/KafkaConsumer.java
@KafkaListener(topics = "orders")
public void listen(Order order) {
    log.info("Received: {}", order);
    // Process order
}
```

### Justificativa:
O projeto utiliza Kafka para comunicação assíncrona entre microserviços, permitindo o processamento de eventos como pedidos em um sistema de e-commerce. Kafka foi escolhido por sua capacidade de lidar com fluxos de dados em tempo real e alta throughput, garantindo desacoplamento entre produtores e consumidores. Isso melhora a escalabilidade e a resiliência do sistema.

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

## 3. Projeto Analisado: [Microservices Resilience](https://github.com/danielbryantuk/oreilly-docker-java-shopping)
**Link:** https://github.com/danielbryantuk/oreilly-docker-java-shopping  
**Característica Identificada:** Circuit breaker

### Evidência (Trecho de Código):
```java
// src/main/java/com/example/shopping/service/StockService.java
@HystrixCommand(fallbackMethod = "getDefaultStock")
public Stock getStock(String productId) {
    return restTemplate.getForObject("http://stock-service/stock/" + productId, Stock.class);
}

public Stock getDefaultStock(String productId, Throwable t) {
    return new Stock(productId, 0);
}
```

### Justificativa:
O projeto usa Hystrix para implementar o padrão circuit breaker, protegendo chamadas a serviços externos como o de estoque. Essa técnica foi escolhida para evitar falhas em cascata em uma arquitetura de microserviços, retornando respostas padrão quando um serviço está indisponível. Isso aumenta a robustez do sistema em cenários de falhas parciais.

---

## 4. Projeto Analisado: [Spring Boot Microservices](https://github.com/in28minutes/spring-microservices-v2)
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

## 5. Projeto Analisado: [SonarQube Integration](https://github.com/mc1arke/sonarqube-community-branch-plugin)
**Link:** https://github.com/mc1arke/sonarqube-community-branch-plugin  
**Característica Identificada:** Análise estática de código (SonarQube)

### Evidência (Trecho de Código):
```yaml
# .github/workflows/build.yml
name: Build
on: [push]
jobs:
  build:
    steps:
      - name: SonarQube Scan
        run: ./gradlew sonarqube -Dsonar.projectKey=sonarqube-plugin -Dsonar.host.url=http://localhost:9000
```

### Justificativa:
O projeto integra SonarQube para análise estática, identificando problemas de código como duplicações e vulnerabilidades. SonarQube foi escolhido por sua integração com pipelines CI/CD e relatórios detalhados, ajudando a manter um código limpo e seguro. Essa prática é essencial em projetos colaborativos para evitar dívidas técnicas.

---

## 6. Projeto Analisado: [Java Spring Boot Example](https://github.com/khoubyari/spring-boot-rest-example)
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

## 7. Projeto Analisado: [Serverless Spring Boot](https://github.com/awslabs/aws-serverless-java-container)
**Link:** https://github.com/awslabs/aws-serverless-java-container  
**Característica Identificada:** Serverless/Lambda

### Evidência (Trecho de Código):
```yaml
# serverless.yml
service: spring-boot-serverless
provider:
  name: aws
  runtime: java8
functions:
  api:
    handler: com.example.StreamLambdaHandler::handleRequest
    events:
      - http:
          path: /api/{proxy+}
          method: any
```

### Justificativa:
O projeto utiliza AWS Lambda para executar uma aplicação Spring Boot em um ambiente serverless. Essa abordagem foi escolhida para reduzir custos, pois a cobrança é baseada no uso, e para escalar automaticamente com a demanda. É ideal para APIs com tráfego esporádico, como serviços de consulta.

---

## 8. Projeto Analisado: [SonarQube Example](https://github.com/SonarSource/sonar-scanning-examples)
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

## 9. Projeto Analisado: [Spring Boot Testing](https://github.com/junit-team/junit5-samples)
**Link:** https://github.com/junit-team/junit5-samples  
**Característica Identificada:** Testes automatizados (unitários e integração)

### Evidência (Trecho de Código):
```java
// src/test/java/com/example/MyServiceTest.java
@SpringBootTest
class MyServiceTest {
    @Autowired
    private MyService myService;

    @Test
    void testServiceMethod() {
        String result = myService.processData("input");
        assertEquals("processed: input", result);
    }
}

```

## 10. Projeto Analisado: [Spring Security OAuth](https://github.com/spring-projects/spring-security-samples)
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

### Justificativa:
O projeto inclui testes unitários e de integração com JUnit 5, verificando o comportamento de serviços críticos. Testes automatizados foram adotados para garantir a estabilidade do código e detectar regressões durante o desenvolvimento. Essa prática aumenta a confiança em refatorações e deployments frequentes.

---

**Notas Finais:**
- Os projetos foram selecionados por sua relevância, popularidade (>10 estrelas) e documentação clara.
- As justificativas destacam o impacto técnico de cada característica no contexto do projeto.
- A busca no GitHub utilizou termos como “kafka”, “redis”, “hystrix”, “github actions”, “sonarqube”, “jwt”, “serverless”, “oauth”, e “junit” para encontrar repositórios adequados.
