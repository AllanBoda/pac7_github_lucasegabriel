Exercício – Análise Técnica de Projetos Reais no GitHub
Nome: [Seu Nome]Data: 23/06/2025Objetivo: Analisar projetos open-source no GitHub que implementem características técnicas específicas, identificando trechos de código e justificando seu uso.

1. Projeto Analisado: Event-Driven Microservices
Link: https://github.com/piomin/sample-spring-cloud-stream-kafkaCaracterística Identificada: Filas de mensageria (Kafka)
Evidência (Trecho de Código):
// src/main/java/pl/piomin/services/kafka/consumer/KafkaConsumer.java
@KafkaListener(topics = "orders")
public void listen(Order order) {
    log.info("Received: {}", order);
    // Process order
}

Justificativa:
O projeto utiliza Kafka para comunicação assíncrona entre microserviços, permitindo o processamento de eventos como pedidos em um sistema de e-commerce. Kafka foi escolhido por sua capacidade de lidar com fluxos de dados em tempo real e alta throughput, garantindo desacoplamento entre produtores e consumidores. Isso melhora a escalabilidade e a resiliência do sistema.

2. Projeto Analisado: Spring Boot Redis Example
Link: https://github.com/spring-guides/gs-spring-data-redisCaracterística Identificada: Uso de cache (Redis)
Evidência (Trecho de Código):
// src/main/java/hello/Application.java
@Bean
RedisTemplate<String, Object> redisTemplate(RedisConnectionFactory connectionFactory) {
    RedisTemplate<String, Object> template = new RedisTemplate<>();
    template.setConnectionFactory(connectionFactory);
    return template;
}

Justificativa:
O projeto utiliza Redis para caching de dados frequentemente acessados, como resultados de consultas a uma API. Redis foi adotado por sua performance em memória, permitindo respostas rápidas e reduzindo a carga no banco de dados. Essa abordagem é ideal para aplicações com endpoints de alta demanda, melhorando a experiência do usuário.

3. Projeto Analisado: Microservices Resilience
Link: https://github.com/danielbryantuk/oreilly-docker-java-shoppingCaracterística Identificada: Circuit breaker
Evidência (Trecho de Código):
// src/main/java/com/example/shopping/service/StockService.java
@HystrixCommand(fallbackMethod = "getDefaultStock")
public Stock getStock(String productId) {
    return restTemplate.getForObject("http://stock-service/stock/" + productId, Stock.class);
}

public Stock getDefaultStock(String productId, Throwable t) {
    return new Stock(productId, 0);
}

Justificativa:
O projeto usa Hystrix para implementar o padrão circuit breaker, protegendo chamadas a serviços externos como o de estoque. Essa técnica foi escolhida para evitar falhas em cascata em uma arquitetura de microserviços, retornando respostas padrão quando um serviço está indisponível. Isso aumenta a robustez do sistema em cenários de falhas parciais.

4. Projeto Analisado: Spring Boot CI/CD Example
Link: https://github.com/callicoder/spring-boot-github-actionsCaracterística Identificada: CI/CD com pipelines
Evidência (Trecho de Código):
# .github/workflows/ci.yml
name: Spring Boot CI
on:
  push:
    branches: [ main ]
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Set up JDK 11
        uses: actions/setup-java@v3
        with:
          java-version: '11'
      - name: Build and Test
        run: ./mvnw clean verify

Justificativa:
O projeto utiliza GitHub Actions para automatizar a construção, teste e validação do código em um pipeline CI/CD. Essa prática foi adotada para garantir entregas contínuas e detectar erros cedo, permitindo deploy rápido e confiável. A execução de testes no pipeline assegura a qualidade do software antes de atingir produção.

5. Projeto Analisado: SonarQube Integration
Link: https://github.com/mc1arke/sonarqube-community-branch-pluginCaracterística Identificada: Análise estática de código (SonarQube)
Evidência (Trecho de Código):
# .github/workflows/build.yml
name: Build
on: [push]
jobs:
  build:
    steps:
      - name: SonarQube Scan
        run: ./gradlew sonarqube -Dsonar.projectKey=sonarqube-plugin -Dsonar.host.url=http://localhost:9000

Justificativa:
O projeto integra SonarQube para análise estática, identificando problemas de código como duplicações e vulnerabilidades. SonarQube foi escolhido por sua integração com pipelines CI/CD e relatórios detalhados, ajudando a manter um código limpo e seguro. Essa prática é essencial em projetos colaborativos para evitar dívidas técnicas.

6. Projeto Analisado: Spring Boot JWT Authentication
Link: https://github.com/callicoder/spring-security-jwt-authenticationCaracterística Identificada: Autenticação com JWT
Evidência (Trecho de Código):
// src/main/java/com/example/security/JwtTokenProvider.java
public String generateToken(Authentication authentication) {
    UserPrincipal userPrincipal = (UserPrincipal) authentication.getPrincipal();
    return Jwts.builder()
            .setSubject(Long.toString(userPrincipal.getId()))
            .setIssuedAt(new Date())
            .setExpiration(new Date((new Date()).getTime() + jwtExpirationInMs))
            .signWith(SignatureAlgorithm.HS512, jwtSecret)
            .compact();
}

Justificativa:
O projeto utiliza JWT para autenticação em uma API REST, permitindo acesso seguro sem armazenamento de sessões no servidor. JWT foi adotado por sua natureza stateless, facilitando escalabilidade e integração com clientes diversos. A verificação de assinatura e expiração garante segurança em endpoints protegidos.

7. Projeto Analisado: Serverless Spring Boot
Link: https://github.com/awslabs/aws-serverless-java-containerCaracterística Identificada: Serverless/Lambda
Evidência (Trecho de Código):
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

Justificativa:
O projeto utiliza AWS Lambda para executar uma aplicação Spring Boot em um ambiente serverless. Essa abordagem foi escolhida para reduzir custos, pois a cobrança é baseada no uso, e para escalar automaticamente com a demanda. É ideal para APIs com tráfego esporádico, como serviços de consulta.

8. Projeto Analisado: Spring OAuth2 Example
Link: https://github.com/spring-guides/gs-oauth2-clientCaracterística Identificada: Autenticação com OAuth
Evidência (Trecho de Código):
// src/main/java/hello/SecurityConfig.java
@Configuration
public class SecurityConfig extends WebSecurityConfigurerAdapter {
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http
            .authorizeRequests()
                .anyRequest().authenticated()
                .and()
            .oauth2Client()
                .and()
            .oauth2Login();
    }
}

Justificativa:
O projeto implementa OAuth 2.0 para autenticação via provedores externos, como GitHub ou Google. OAuth foi escolhido por delegar a autenticação a serviços confiáveis, reduzindo a necessidade de gerenciar credenciais locais. Essa abordagem é segura e amplamente usada em aplicações modernas com integração de terceiros.

9. Projeto Analisado: Spring Boot Testing
Link: https://github.com/junit-team/junit5-samplesCaracterística Identificada: Testes automatizados (unitários e integração)
Evidência (Trecho de Código):
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

Justificativa:
O projeto inclui testes unitários e de integração com JUnit 5, verificando o comportamento de serviços críticos. Testes automatizados foram adotados para garantir a estabilidade do código e detectar regressões durante o desenvolvimento. Essa prática aumenta a confiança em refatorações e deployments frequentes.

Notas Finais:

Os projetos foram selecionados por sua relevância, popularidade (>10 estrelas) e documentação clara.
As justificativas destacam o impacto técnico de cada característica no contexto do projeto.
A busca no GitHub utilizou termos como “kafka”, “redis”, “hystrix”, “github actions”, “sonarqube”, “jwt”, “serverless”, “oauth”, e “junit” para encontrar repositórios adequados.
