# [Spring Boot 4.0 完整核心特性及实践指南 - 教程](https://www.cnblogs.com/gccbuaa/p/19469075)



#### 文章目录

- - [一、引言：Spring Boot 4.0 的战略意义](https://www.cnblogs.com/gccbuaa/p/19469075#Spring_Boot_40__2)
  - [二、架构革新：模块化重构的深度解析](https://www.cnblogs.com/gccbuaa/p/19469075#_16)
  - - [2.1 自动配置模块化的战略意义](https://www.cnblogs.com/gccbuaa/p/19469075#21__18)
    - [2.2 模块化对应用生命周期的影响](https://www.cnblogs.com/gccbuaa/p/19469075#22__53)
  - [三、空安全标准化：JSpecify 集成的实践指南](https://www.cnblogs.com/gccbuaa/p/19469075#JSpecify__82)
  - - [3.1 JSpecify 注解体系的战略价值](https://www.cnblogs.com/gccbuaa/p/19469075#31_JSpecify__84)
    - [3.2 与 Kotlin 空类型的无缝集成](https://www.cnblogs.com/gccbuaa/p/19469075#32__Kotlin__120)
    - [3.3 空安全的最佳实践策略](https://www.cnblogs.com/gccbuaa/p/19469075#33__154)
  - [四、Java 版本支持：拥抱现代 Java 特性](https://www.cnblogs.com/gccbuaa/p/19469075#Java__Java__211)
  - - [4.1 JDK 25 的一级支持](https://www.cnblogs.com/gccbuaa/p/19469075#41_JDK_25__213)
    - [4.2 Jakarta EE 11 对齐的技术意义](https://www.cnblogs.com/gccbuaa/p/19469075#42_Jakarta_EE_11__256)
  - [五、HTTP Service Clients：声明式 HTTP 调用新范式](https://www.cnblogs.com/gccbuaa/p/19469075#HTTP_Service_Clients_HTTP__316)
  - - [5.1 HTTP Service Clients 的架构设计](https://www.cnblogs.com/gccbuaa/p/19469075#51_HTTP_Service_Clients__318)
    - [5.2 API 版本管理的优雅实现](https://www.cnblogs.com/gccbuaa/p/19469075#52_API__372)
    - [5.3 HTTP Interface Groups 的高级用法](https://www.cnblogs.com/gccbuaa/p/19469075#53_HTTP_Interface_Groups__440)
  - [六、弹性与韧性：内置弹性功能详解](https://www.cnblogs.com/gccbuaa/p/19469075#_546)
  - - [6.1 Spring Retry 的集成增强](https://www.cnblogs.com/gccbuaa/p/19469075#61_Spring_Retry__548)
    - [6.2 并发控制的声明式管理](https://www.cnblogs.com/gccbuaa/p/19469075#62__636)
  - [七、JSON 处理升级：Jackson 3 集成指南](https://www.cnblogs.com/gccbuaa/p/19469075#JSON_Jackson_3__691)
  - - [7.1 Jackson 3 的包重定位影响](https://www.cnblogs.com/gccbuaa/p/19469075#71_Jackson_3__693)
    - [7.2 JSON 序列化的最佳实践](https://www.cnblogs.com/gccbuaa/p/19469075#72_JSON__757)
  - [八、测试增强：构建时查询编译与测试容器 2.0](https://www.cnblogs.com/gccbuaa/p/19469075#_20_843)
  - - [8.1 Spring Data 构建时查询编译](https://www.cnblogs.com/gccbuaa/p/19469075#81_Spring_Data__845)
    - [8.2 TestContainers 2.0 集成](https://www.cnblogs.com/gccbuaa/p/19469075#82_TestContainers_20__902)
  - [九、监控与可观察性：OpenTelemetry 集成](https://www.cnblogs.com/gccbuaa/p/19469075#OpenTelemetry__1001)
  - - [9.1 OpenTelemetry 集成增强](https://www.cnblogs.com/gccbuaa/p/19469075#91_OpenTelemetry__1003)
  - [十、迁移指南：从 Spring Boot 3.x 升级到 4.0](https://www.cnblogs.com/gccbuaa/p/19469075#_Spring_Boot_3x__40_1113)
  - - [10.1 核心变更点总结](https://www.cnblogs.com/gccbuaa/p/19469075#101__1115)
    - [10.2 分步迁移策略](https://www.cnblogs.com/gccbuaa/p/19469075#102__1167)
    - [10.3 迁移工具和脚本](https://www.cnblogs.com/gccbuaa/p/19469075#103__1281)
  - [十一、总结与展望](https://www.cnblogs.com/gccbuaa/p/19469075#_1332)
  - - [11.1 Spring Boot 4.0 的核心价值](https://www.cnblogs.com/gccbuaa/p/19469075#111_Spring_Boot_40__1334)
    - [11.2 采用建议和最佳实践](https://www.cnblogs.com/gccbuaa/p/19469075#112__1346)



### 一、引言：Spring Boot 4.0 的战略意义

Spring Boot 4.0.0 于 2025 年 11 月 20 日正式发布，标志着 Spring 生态系统的重大里程碑版本。作为 Spring Boot 4.x 系列的首个正式版本，它构建在 Spring Framework 7 之上，为未来多年的企业级应用开发奠定了坚实基础[0†]。

**版本定位与兼容性：**

- **JDK 要求**：最低支持 JDK 17（LTS 版本），推荐使用 JDK 25（2025 年 9 月发布版本）[4†]
- **Jakarta EE 对齐**：升级至 Jakarta EE 11 标准，包含 Jakarta Servlet 6.1、Jakarta WebSocket 2.2、Jakarta Validation 3.1 和 Jakarta Persistence 3.2
- **兼容性策略**：虽然拥抱 JDK 25 的新特性，但保持与 JDK 17 的完全兼容性，符合当前行业的 Java 17 基线共识

**架构升级的核心驱动力：**
Spring Boot 4.0 的设计理念围绕三大核心主题展开：模块化架构、空安全标准化和云原生就绪。通过重构代码库结构、引入现代 Java 特性、标准化开发实践，该版本为构建更加健壮、可维护和高效的企业级应用提供了系统性解决方案。

------

### 二、架构革新：模块化重构的深度解析

#### 2.1 自动配置模块化的战略意义

Spring Boot 4.0 最具颠覆性的改进是**完整的代码库模块化重构**。这一改变将原本的单体 `spring-boot-autoconfigure` 和 `spring-boot-test-autoconfigure` JAR 包拆分为多个更加聚焦和精简的模块[0†]。

**模块化架构的核心优势：**

1. **减少应用足迹**：仅引入所需功能的依赖，避免包含未使用的组件，显著减小最终 JAR 包体积
2. **提升 IDE 性能**：防止自动完成功能建议未使用的类或配置属性，改善开发体验
3. **优化构建效率**：更精确的依赖管理减少编译时间和类加载开销
4. **增强安全性和可维护性**：减少攻击面，简化依赖关系图

**核心技术实现：**

```java
// 模块化自动配置示例
// 传统方式：引入整个 spring-boot-autoconfigure
// 新方式：仅引入所需的具体模块
dependencies {
implementation 'org.springframework.boot:spring-boot-starter-webmvc'
implementation 'org.springframework.boot:spring-boot-starter-data-jpa'
// 而不是：implementation 'org.springframework.boot:spring-boot-autoconfigure'
}
```

**模块划分的技术细节：**

Spring Boot 4.0 将自动配置按技术栈进行精细化拆分，主要包括：

- `spring-boot-starter-webmvc`：Web MVC 相关自动配置
- `spring-boot-starter-webflux`：响应式 Web 自动配置
- `spring-boot-starter-data-jpa`：JPA 数据访问自动配置
- `spring-boot-starter-amqp`：AMQP 消息队列自动配置
- `spring-boot-starter-kafka`：Kafka 流处理自动配置

每个模块都独立管理其自动配置逻辑，实现了更加清晰的关注点分离。

#### 2.2 模块化对应用生命周期的影响

**启动阶段优化：**

- 条件评估更加精确，仅加载启用的模块自动配置
- Bean 定义注册过程更加高效，减少不必要的候选 Bean
- 应用上下文刷新速度提升，特别是在大型应用中效果显著

**运行时优势：**

- 类加载器层次结构更加清晰，降低内存占用
- 反射调用开销减少，提升方法执行效率
- 热部署场景下更加高效，仅重新加载相关模块

**开发和调试体验改善：**

```java
// 启用调试日志查看模块加载情况
@SpringBootApplication
public class Application {
public static void main(String[] args) {
// 添加 --debug 参数或设置系统属性
System.setProperty("debug", "true");
SpringApplication.run(Application.class, args);
}
}
```

通过 `ConditionEvaluationReport` 机制，开发者可以清晰地了解每个模块的自动配置决策过程，极大提升了问题排查效率。

------

### 三、空安全标准化：JSpecify 集成的实践指南

#### 3.1 JSpecify 注解体系的战略价值

Spring Boot 4.0 实现了**全栈空安全改进**，通过集成 JSpecify 注解体系来标准化整个 Spring Portfolio 的空安全处理[0†][7†]。

**JSpecify 联盟背景：**
JSpecify 是由 OpenJDK、Broadcom、Google、JetBrains 和 Sonar 等主要 Java 生态系统参与者组成的联合项目，旨在为 Java 空安全提供标准化的注解解决方案。

**核心技术改进：**

```java
// JSpecify 注解使用示例
import org.springframework.lang.NonNull;
import org.springframework.lang.Nullable;
@Service
public class UserService {
// 明确表示参数不能为 null
public User findById(@NonNull Long id) {
// 实现逻辑
}
// 表示返回值可能为 null
@Nullable
public User findByEmail(String email) {
// 实现逻辑
}
}
```

**空安全覆盖范围：**

- 标准类型和泛型类型
- 数组和可变参数元素
- 嵌套类型和复杂对象图
- 集合框架接口和实现

#### 3.2 与 Kotlin 空类型的无缝集成

**双向空安全映射：**
Spring Framework 7 完成了向标准化 JSpecify 注解的迁移，Kotlin 2 会自动将这些注解转换为 Kotlin 的空安全类型[7†]。

```kotlin
// Kotlin 中的空安全处理
@Service
class UserService {
// Kotlin 可空类型与 JSpecify 注解的对应
fun findById(@NonNull id: Long): User? {
// 对应 Java 中的 @Nullable 返回类型
}
}
```

**IDE 支持和编译时检查：**

IntelliJ IDEA 2025.3 提供了一级支持，包括：

- 完整的数据流分析
- 空安全违规的实时检测
- 自动修复建议和快速修复
- 复杂条件路径的精确分析

**构建时空安全验证：**

使用 NullAway 项目进行构建时检查，虽然当前要求至少 JDK 21，但社区正在推进 JDK 17 的支持[7†]。

```bash
# 使用 NullAway 进行空安全检查
mvn -Pnullaway verify
```

#### 3.3 空安全的最佳实践策略

**分层空安全设计：**

1. **API 层**：严格使用 `@NonNull` 和 `@Nullable` 注解明确契约
2. **服务层**：防御性编程，对可能为空的参数进行校验
3. **数据层**：使用 Optional 或空对象模式处理可能缺失的数据
4. **视图层**：安全的字符串处理和集合访问

**空安全异常处理策略：**

```java
@RestController
public class UserController {
@GetMapping("/users/{id}")
public ResponseEntity<?> getUser(@PathVariable @NonNull Long id) {
  try {
  User user = userService.findById(id);
  if (user == null) {
  return ResponseEntity.notFound().build();
  }
  return ResponseEntity.ok(user);
  } catch (NullPointerException e) {
  // 记录详细的空安全异常信息
  log.error("Null pointer exception for user id: {}", id, e);
  return ResponseEntity.internalServerError().build();
  }
  }
  }
```

**空安全测试策略：**

```java
@SpringBootTest
class UserServiceTest {
@Autowired
private UserService userService;
@Test
void whenValidId_thenReturnUser() {
// 正常情况测试
User user = userService.findById(1L);
assertNotNull(user);
}
@Test
void whenInvalidId_thenReturnNull() {
// 边界情况测试
User user = userService.findById(-1L);
assertNull(user);
}
}
```

------

### 四、Java 版本支持：拥抱现代 Java 特性

#### 4.1 JDK 25 的一级支持

Spring Boot 4.0 提供** JDK 25 的一级支持**，同时保持与 JDK 17 的完全兼容性[0†][4†]。

**JDK 25 新特性的核心价值：**

1. **虚拟线程（Virtual Threads）**：简化高并发编程模型，显著提升应用吞吐量
2. **记录模式（Record Patterns）**：增强模式匹配能力，使代码更加简洁和安全
3. **字符串模板（String Templates）**：提供更灵活和高效的字符串处理方式
4. **sealed 接口和类的增强**：进一步改善面向对象设计的表达能力

**虚拟线程的实践应用：**

```java
@Service
public class OrderProcessingService {
// 虚拟线程支持的高并发处理
public CompletableFuture<Order> processOrder(Order order) {
  // 使用虚拟线程处理阻塞 I/O 操作
  return CompletableFuture.supplyAsync(() -> {
  // 模拟数据库调用或外部服务调用
  try {
  return orderRepository.save(order);
  } catch (Exception e) {
  throw new OrderProcessingException(e);
  }
  }, VirtualThreadScheduler.get());
  }
  }
```

**版本兼容性管理：**

Spring Boot 4.0 的兼容性矩阵如下：

| 组件   | 最低版本 | 推荐版本 | 关键特性                       |
| ------ | -------- | -------- | ------------------------------ |
| JDK    | 17       | 25       | 虚拟线程、记录模式、字符串模板 |
| Maven  | 3.8.1    | 3.9.2    | 更好的依赖管理和构建性能       |
| Gradle | 8.5      | 9.0      | 增量的构建能力和插件生态       |
| Kotlin | 1.9.0    | 2.2.20   | 协程、空安全、脚本支持         |

#### 4.2 Jakarta EE 11 对齐的技术意义

**Jakarta EE 升级的战略考量：**

升级至 Jakarta EE 11 带来了关键的平台组件更新[7†]：

- **Jakarta Servlet 6.1**：提供更现代的 Web 开发 API，包括对服务器推送的更好支持
- **Jakarta WebSocket 2.2**：增强的 WebSocket 支持，改善实时通信能力
- **Jakarta Validation 3.1**：标准化的 Bean 验证 API，支持自定义约束注解
- **Jakarta Persistence 3.2**：JPA 规范的最新版本，提供更灵活的持久化能力

**兼容性注意事项：**

由于 Jakarta EE 11 的要求，Spring Boot 4.0 **暂不支持 Undertow** Web 服务器，因为其尚未兼容 Jakarta Servlet 6.1[7†]。

```xml
<!-- pom.xml 中的 Web 服务器配置示例 -->
  <properties>
    <!-- 使用 Tomcat 11.0 作为默认 Web 服务器 -->
    <tomcat.version>11.0.0</tomcat.version>
    </properties>
    <dependencies>
      <!-- 默认包含 Tomcat -->
        <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-tomcat</artifactId>
        </dependency>
        <!-- Undertow 暂不支持 Spring Boot 4.0 -->
          <!--
          <dependency>
          <groupId>org.springframework.boot</groupId>
          <artifactId>spring-boot-starter-undertow</artifactId>
          </dependency>
          -->
        </dependencies>
```

**迁移指南：**

对于从 Jakarta EE 9 升级的开发者，需要注意以下关键变化：

```java
// 包名变化示例
// 旧版本（Jakarta EE 9）
import javax.servlet.http.HttpServletRequest;
import javax.persistence.EntityManager;
import javax.validation.Constraint;
// 新版本（Jakarta EE 11）
import jakarta.servlet.http.HttpServletRequest;
import jakarta.persistence.EntityManager;
import jakarta.validation.Constraint;
// Spring Boot 4.0 自动处理这些映射，但第三方库需要显式升级
```

------

### 五、HTTP Service Clients：声明式 HTTP 调用新范式

#### 5.1 HTTP Service Clients 的架构设计

Spring Boot 4.0 引入了**HTTP Service Clients**，为 REST 应用提供了一流的 HTTP 客户端支持[0†][1†]。

**核心组件架构：**

1. **@HttpService 注解**：标记接口为 HTTP 服务客户端
2. **HttpServiceProxyFactory**：创建代理实例的核心工厂
3. **HttpServiceProxyRegistry**：管理多个 HTTP 服务客户端的注册表[11†]
4. **HttpServiceGroupConfigurer**：配置 HTTP 服务组的回调接口

**基础使用模式：**

```java
// 定义 HTTP 服务客户端接口
@HttpService
public interface UserServiceClient {
@GetExchange("/users/{id}")
User getUserById(@PathVariable Long id);
@PostExchange("/users")
User createUser(@RequestBody User user);
@PutExchange("/users/{id}")
User updateUser(@PathVariable Long id, @RequestBody User user);
@DeleteExchange("/users/{id}")
void deleteUser(@PathVariable Long id);
}
```

**服务注册与配置：**

```java
@Configuration
public class HttpServiceConfiguration {
@Bean
public UserServiceClient userServiceClient(RestClient.Builder builder) {
return builder
.baseUrl("https://api.example.com")
.defaultHeader("Authorization", "Bearer " + getAuthToken())
.build()
.proxy(UserServiceClient.class);
}
private String getAuthToken() {
// 获取认证令牌的逻辑
return "token-value";
}
}
```

#### 5.2 API 版本管理的优雅实现

Spring Boot 4.0 在 Spring MVC 和 Spring WebFlux 中引入了**一流的 REST API 版本管理**功能[11†]。

**支持的版本管理策略：**

1. **路径版本ing**：`/api/v1/users`
2. **头部版本ing**：通过自定义头部指定版本
3. **查询参数版本ing**：`?version=1`
4. **媒体类型版本ing**：通过 `Accept` 头部指定版本

**API 版本策略配置：**

```java
@Configuration
public class ApiVersionConfiguration {
@Bean
public ApiVersionStrategy apiVersionStrategy() {
// 组合多种版本策略
return new CompositeApiVersionStrategy(
// 路径版本策略
new PathApiVersionStrategy("v{version}"),
// 头部版本策略
new HeaderApiVersionStrategy("X-API-Version"),
// 查询参数版本策略
new QueryApiVersionStrategy("version")
);
}
}
```

**控制器中的版本声明：**

```java
@RestController
@RequestMapping("/api")
public class UserController {
@GetMapping(
value = "/users/{id}",
produces = "application/vnd.myapp.v1+json"
)
@ApiVersion("1.0")
public UserV1 getUserV1(@PathVariable Long id) {
// 版本 1 的实现逻辑
return userService.findByIdV1(id);
}
@GetMapping(
value = "/users/{id}",
produces = "application/vnd.myapp.v2+json"
)
@ApiVersion("2.0")
public UserV2 getUserV2(@PathVariable Long id) {
// 版本 2 的实现逻辑，包含更多字段
return userService.findByIdV2(id);
}
}
```

**版本化 API 的设计最佳实践：**

1. **URL 路径版本ing**：最直观和广泛支持的方式
2. **媒体类型版本ing**：适合内容协商的场景
3. **废弃策略**：使用 `@Deprecated` 注解和自定义响应头通知客户端
4. **文档化**：使用 OpenAPI/Swagger 自动生成版本化文档

#### 5.3 HTTP Interface Groups 的高级用法

**HTTP Interface Groups 的核心价值：**

允许应用程序一次性配置多个 HTTP Interface Clients，并让它们共享同一个 RestClient 实例[11†]。

**Group 配置示例：**

```java
// 定义多个相关接口
@HttpService
public interface UserServiceClient {
@GetExchange("/users/{id}")
User getUser(@PathVariable Long id);
}
@HttpService
public interface OrderServiceClient {
@GetExchange("/orders/{userId}")
List<Order> getOrdersByUser(@PathVariable Long userId);
  }
  @HttpService
  public interface PaymentServiceClient {
  @PostExchange("/payments")
  Payment processPayment(@RequestBody PaymentRequest request);
  }
  // Group 配置
  @Configuration
  public class ServiceGroupConfiguration {
  @Bean
  public HttpServiceGroupConfigurer userGroupConfigurer(RestClient.Builder builder) {
  return configurer -> configurer
  .baseUrl("https://api.example.com/user-service")
  .clientName("user-service-client")
  .configure(builder);
  }
  @Bean
  public HttpServiceGroupConfigurer orderGroupConfigurer(RestClient.Builder builder) {
  return configurer -> configurer
  .baseUrl("https://api.example.com/order-service")
  .clientName("order-service-client")
  .configure(builder);
  }
  @Bean
  public HttpServiceGroupConfigurer paymentGroupConfigurer(RestClient.Builder builder) {
  return configurer -> configurer
  .baseUrl("https://api.example.com/payment-service")
  .clientName("payment-service-client")
  .interceptors(new LoggingInterceptor())
  .configure(builder);
  }
  }
```

**共享 RestClient 的优势：**

1. **连接池管理**：共享底层 HTTP 客户端连接池
2. **配置一致性**：统一的超时、重试和拦截器配置
3. **性能优化**：减少连接开销和内存占用
4. **管理简化**：集中管理 HTTP 客户端生命周期

**高级特性：请求拦截器和响应处理：**

```java
@Component
public class AuthInterceptor implements ClientHttpRequestInterceptor {
@Override
public ClientHttpResponse intercept(
HttpRequest request,
byte[] body,
ClientHttpRequestExecution execution) throws IOException {
// 添加认证头部
request.getHeaders().add("Authorization", "Bearer " + getAuthToken());
// 添加请求 ID 用于追踪
String requestId = UUID.randomUUID().toString();
request.getHeaders().add("X-Request-ID", requestId);
// 记录请求日志
logRequest(request, body);
// 执行请求
ClientHttpResponse response = execution.execute(request, body);
// 记录响应日志
logResponse(response);
return response;
}
private String getAuthToken() {
// 获取认证令牌
return authService.getToken();
}
}
```

------

### 六、弹性与韧性：内置弹性功能详解

#### 6.1 Spring Retry 的集成增强

Spring Framework 7 将 **Spring Retry 的弹性功能**直接集成到框架中，无需额外依赖[11†]。

**@Retryable 注解的核心功能：**

```java
@Service
public class ExternalApiService {
@Retryable(
// 重试的异常类型
value = { IOException.class, ConnectException.class },
// 最大重试次数
maxAttempts = 3,
// 重试间隔（指数退避）
backoff = @Backoff(delay = 1000, multiplier = 2),
// 重试条件
condition = "!'result.success'"
)
public ApiResponse callExternalApi(Request request) {
// 可能失败的外部 API 调用
return restTemplate.postForObject(
"/api/external",
request,
ApiResponse.class
);
}
@Recover
public ApiResponse recover(IOException e, Request request) {
// 重试失败后的恢复逻辑
log.error("All retry attempts failed for request: {}", request, e);
return ApiResponse.error("Service unavailable, please try later");
}
}
```

**响应式方法的重试支持：**

```java
@Service
public class ReactiveExternalService {
@Retryable(
value = { WebClientResponseException.class },
maxAttempts = 3,
backoff = @Backoff(delay = 500, multiplier = 1.5)
)
public Mono<ApiResponse> callExternalApiReactively(Request request) {
  return webClient.post()
  .uri("/api/external")
  .bodyValue(request)
  .retrieve()
  .bodyToMono(ApiResponse.class)
  .doOnError(e -> log.error("API call failed", e));
  }
  }
```

**重试策略的高级配置：**

```java
@Configuration
public class RetryConfiguration {
@Bean
public RetryTemplate retryTemplate() {
RetryTemplate template = new RetryTemplate();
// 配置重试策略
template.setRetryPolicy(new SimpleRetryPolicy(3, new HashMap<Class<? extends Throwable>, Boolean>() {{
  put(IOException.class, true);
  put(ConnectException.class, true);
  put(SocketTimeoutException.class, true);
  }}));
  // 配置退避策略
  template.setBackOffPolicy(new ExponentialBackOffPolicy());
  ((ExponentialBackOffPolicy) template.getBackOffPolicy()).setInitialInterval(1000);
  ((ExponentialBackOffPolicy) template.getBackOffPolicy()).setMultiplier(2.0);
  ((ExponentialBackOffPolicy) template.getBackOffPolicy()).setMaxInterval(10000);
  return template;
  }
  }
```

#### 6.2 并发控制的声明式管理

**@ConcurrencyLimit 注解的应用场景：**

该注解提供了声明式的并发控制能力，特别与虚拟线程结合使用时价值显著[11†]。

```java
@Service
public class ConcurrencyControlledService {
@ConcurrencyLimit(
// 最大并发数
maxConcurrent = 10,
// 超时时间（毫秒）
timeout = 5000,
// 拒绝策略
rejectPolicy = ConcurrencyLimit.RejectPolicy.RUN
)
public Result processWithConcurrencyLimit(Input input) {
// 业务处理逻辑
return heavyComputation(input);
}
// 使用 Semaphore 进行更细粒度的控制
@Semaphore(value = 5, timeout = "10s")
public Result processWithSemaphore(Input input) {
// 仅允许 5 个并发执行
return process(input);
}
}
```

**虚拟线程与并发控制的协同：**

```java
@Service
public class VirtualThreadConcurrencyService {
@ConcurrencyLimit(maxConcurrent = 100, timeout = 30000)
public CompletableFuture<Result> processWithVirtualThreads(Input input) {
  // 使用虚拟线程处理大量并发请求
  return CompletableFuture.supplyAsync(() -> {
  // 模拟阻塞 I/O 操作
  try {
  return blockingOperation(input);
  } catch (Exception e) {
  throw new ProcessingException(e);
  }
  }, VirtualThreadScheduler.get());
  }
  }
```

------

### 七、JSON 处理升级：Jackson 3 集成指南

#### 7.1 Jackson 3 的包重定位影响

Spring Boot 4.0 将 JSON 处理库升级到 **Jackson 3**，带来了重要的包结构变化[11†]。

**包重定位的核心变化：**

```java
// Jackson 2（旧版本）
import com.fasterxml.jackson.databind.ObjectMapper;
import com.fasterxml.jackson.databind.SerializationFeature;
// Jackson 3（新版本）
import tools.jackson.databind.ObjectMapper;
import tools.jackson.databind.SerializationFeature;
import tools.jackson.databind.JsonMapper;
```

**JsonMapper 的推荐使用：**

Jackson 3 引入了不可变的 `JsonMapper`，提供了更好的类型安全性和性能[11†]。

```java
@Configuration
public class JsonConfiguration {
@Bean
public JsonMapper jsonMapper() {
return JsonMapper.builder()
// 配置序列化行为
.configure(SerializationFeature.WRITE_DATES_AS_TIMESTAMPS, false)
.configure(SerializationFeature.SORT_PROPERTIES_ALPHABETICALLY, true)
// 注册自定义模块
.addModule(new JavaTimeModule())
.addModule(new Jdk8Module())
.build();
}
}
```

**默认序列化行为的变化：**

```java
// 新的默认行为
@Configuration
public class DefaultJsonConfiguration {
@Bean
public ObjectMapper objectMapper() {
ObjectMapper mapper = new ObjectMapper();
// 1. 属性按字母顺序排序
mapper.configure(SerializationFeature.SORT_PROPERTIES_ALPHABETICALLY, true);
// 2. 日期序列化为 ISO-8601 字符串（而非时间戳）
mapper.configure(SerializationFeature.WRITE_DATES_AS_TIMESTAMPS, false);
// 3. 忽略未知属性
mapper.configure(DeserializationFeature.FAIL_ON_UNKNOWN_PROPERTIES, false);
return mapper;
}
}
```

#### 7.2 JSON 序列化的最佳实践

**日期时间处理：**

```java
@JsonFormat(shape = JsonFormat.Shape.STRING, pattern = "yyyy-MM-dd'T'HH:mm:ss.SSSZ")
private LocalDateTime createdAt;
// 或者使用 JavaTimeModule
@Configuration
public class DateTimeConfiguration {
@Bean
public Module javaTimeModule() {
JavaTimeModule module = new JavaTimeModule();
// 自定义日期格式
module.addSerializer(LocalDateTime.class, new LocalDateTimeSerializer(
DateTimeFormatter.ISO_LOCAL_DATE_TIME
));
module.addDeserializer(LocalDateTime.class, new LocalDateTimeDeserializer(
DateTimeFormatter.ISO_LOCAL_DATE_TIME
));
return module;
}
}
```

**自定义序列化和反序列化：**

```java
// 自定义注解
@Target({ElementType.FIELD})
@Retention(RetentionPolicy.RUNTIME)
public @interface SensitiveData {
String mask() default "***";
}
// 自定义序列化器
public class SensitiveDataSerializer extends JsonSerializer<String> {
  @Override
  public void serialize(String value, JsonGenerator gen, SerializerProvider serializers) throws IOException {
  if (value == null) {
  gen.writeNull();
  } else {
  // 掩码处理
  String masked = value.substring(0, 4) + "***" + value.substring(value.length() - 4);
  gen.writeString(masked);
  }
  }
  }
  // 在实体类中使用
  public class User {
  @SensitiveData(mask = "****-****-****-****")
  private String creditCardNumber;
  // getters and setters
  }
```

**性能优化策略：**

```java
@Configuration
public class JsonPerformanceConfiguration {
@Bean
public ObjectMapper optimizedObjectMapper() {
return JsonMapper.builder()
// 启用性能优化
.configure(JsonGenerator.Feature.WRITE_NUMBERS_AS_STRINGS, false)
.configure(JsonParser.Feature.ALLOW_SINGLE_QUOTES, true)
.configure(JsonParser.Feature.ALLOW_UNQUOTED_FIELD_NAMES, true)
// 使用-afterburner 字节码生成提升性能
.addModule(new AfterburnerModule())
.build();
}
}
```

------

### 八、测试增强：构建时查询编译与测试容器 2.0

#### 8.1 Spring Data 构建时查询编译

Spring Data 在 Spring Boot 4.0 中引入了**构建时查询编译**功能，显著提升应用启动速度[11†]。

**工作原理：**

Spring Data 在构建阶段（编译时）将符合条件的查询方法转换为字节码，避免运行时动态解析。

**启用方式：**

```java
// 在 Repository 接口上添加注解
public interface UserRepository extends JpaRepository<User, Long> {
  // 这些查询会在构建时编译
  User findByEmail(String email);
  @Query("SELECT u FROM User u WHERE u.name = :name")
  User findByName(@Param("name") String name);
  // 动态查询不支持构建时编译
  @Query("SELECT u FROM User u WHERE u.name = :name AND u.email = :email")
  User findByNameAndEmail(@Param("name") String name, @Param("email") String email);
  }
```

**性能提升效果：**

| 场景     | 传统方式（运行时编译） | 构建时编译 | 性能提升 |
| -------- | ---------------------- | ---------- | -------- |
| 简单查询 | 5-10 ms                | 1-2 ms     | 50-80%   |
| 复杂查询 | 20-50 ms               | 5-10 ms    | 60-75%   |
| 启动时间 | 2-5 秒                 | 1-2 秒     | 40-60%   |

**配置和限制：**

```java
@Configuration
@EnableJpaRepositories(
basePackages = "com.example.repository",
// 启用构建时查询编译
queryLookupStrategy = QueryLookupStrategy.Key.CREATE_IF_NOT_FOUND
)
public class DataConfiguration {
// 可以通过属性配置控制编译行为
@Bean
public JpaTransactionManager transactionManager(EntityManagerFactory emf) {
JpaTransactionManager transactionManager = new JpaTransactionManager();
transactionManager.setEntityManagerFactory(emf);
// 启用查询缓存
transactionManager.setNestedTransactionAllowed(true);
return transactionManager;
}
}
```

#### 8.2 TestContainers 2.0 集成

Spring Boot 4.0 集成了 **TestContainers 2.0**，提供了更加高效和功能丰富的集成测试能力[11†]。

**TestContainers 2.0 的核心改进：**

1. **启动速度提升**：镜像缓存和按需拉取
2. **并行测试支持**：更好的资源管理和隔离
3. **覆盖更多服务**：支持 Kafka、Pulsar、Elasticsearch 等
4. **生命周期管理**：更灵活的测试容器生命周期控制

**数据库集成测试示例：**

```java
@SpringBootTest
@Testcontainers
class DataAccessLayerTest {
@Container
static PostgreSQLContainer<?> postgres = new PostgreSQLContainer<>("postgres:15-alpine")
  .withDatabaseName("testdb")
  .withUsername("test")
  .withPassword("test");
  @Autowired
  private UserRepository userRepository;
  @Test
  void whenSaveUser_thenCanBeFound() {
  // Given
  User user = new User("John Doe", "john.doe@example.com");
  // When
  User savedUser = userRepository.save(user);
  // Then
  assertNotNull(savedUser.getId());
  assertEquals("John Doe", savedUser.getName());
  // 验证数据库中的数据
  User foundUser = userRepository.findById(savedUser.getId()).orElseThrow();
  assertEquals(savedUser, foundUser);
  }
  }
```

**多容器集成测试：**

```java
@SpringBootTest
@Testcontainers
class MicroserviceIntegrationTest {
@Container
static PostgreSQLContainer<?> postgres = new PostgreSQLContainer<>("postgres:15-alpine");
  @Container
  static GenericContainer<?> redis = new GenericContainer<>("redis:7-alpine");
    @Container
    static KafkaContainer kafka = new KafkaContainer();
    @Test
    void whenProcessingMessage_thenPersistsInDatabase() {
    // 测试跨服务的数据流
    // 1. 生产者发送消息到 Kafka
    // 2. 消费者处理消息并存储到 PostgreSQL
    // 3. 验证数据一致性
    }
    }
```

**自定义测试容器配置：**

```java
@TestConfiguration
public class TestContainerConfiguration {
@Bean
@Primary
public DataSource testDataSource() {
return new EmbeddedDatabaseBuilder()
.setType(EmbeddedDatabaseType.H2)
.setName("testdb")
.build();
}
@Bean
public Flyway flyway(DataSource dataSource) {
return Flyway.configure()
.dataSource(dataSource)
.locations("classpath:db/migration")
.load();
}
}
```

------

### 九、监控与可观察性：OpenTelemetry 集成

#### 9.1 OpenTelemetry 集成增强

Spring Boot 4.0 引入了新的 **spring-boot-starter-opentelemetry**，提供了完整的 OTLP（OpenTelemetry Protocol）指标和链路追踪导出能力[11†]。

**OpenTelemetry 模块依赖：**

```xml
<dependency>
<groupId>org.springframework.boot</groupId>
<artifactId>spring-boot-starter-opentelemetry</artifactId>
</dependency>
<!-- 可选：包含所有 OTLP 依赖 -->
  <dependency>
  <groupId>io.opentelemetry</groupId>
  <artifactId>opentelemetry-sdk-extension-otlp</artifactId>
  </dependency>
```

**配置示例：**

```yaml
# application.yml
spring:
opentelemetry:
# 启用 OpenTelemetry
enabled: true
# 导出器配置
exporter:
# OTLP 导出器
otlp:
# 端点配置
endpoint: "http://localhost:4317"
# 导出间隔
schedule-delay: 10s
# 批处理大小
batch-size: 512
# 采样配置
sampling:
# 概率采样
probability: 0.01
# 资源标签
resource:
service.name: "my-spring-boot-app"
service.version: "1.0.0"
```

**自定义指标和追踪：**

```java
@Service
public class BusinessService {
private final MeterRegistry meterRegistry;
private final Tracer tracer;
public BusinessService(MeterRegistry meterRegistry, Tracer tracer) {
this.meterRegistry = meterRegistry;
this.tracer = tracer;
}
public void processBusinessOperation(String input) {
// 创建自定义 span
Span span = tracer.spanBuilder("processBusinessOperation").startSpan();
try (Scope scope = span.makeCurrent()) {
// 记录自定义事件
span.addEvent("businessOperation.started");
// 记录自定义指标
meterRegistry.counter("business.operation.count").increment();
// 业务逻辑
doProcess(input);
span.addEvent("businessOperation.completed");
} catch (Exception e) {
span.recordException(e);
throw e;
} finally {
span.end();
}
}
}
```

**健康检查集成：**

```java
@Component
public class OpenTelemetryHealthIndicator implements HealthIndicator {
@Override
public Health health() {
// 检查 OpenTelemetry 导出器的健康状态
if (isExporterHealthy()) {
return Health.up().withDetail("exporter", "healthy").build();
} else {
return Health.down().withDetail("exporter", "unhealthy").build();
}
}
private boolean isExporterHealthy() {
// 自定义健康检查逻辑
return true;
}
}
```

------

### 十、迁移指南：从 Spring Boot 3.x 升级到 4.0

#### 10.1 核心变更点总结

**主要破坏性变更：**

1. **JDK 要求**：最低 JDK 17，推荐 JDK 25[4†]
2. **Jakarta EE 升级**：升级至 Jakarta EE 11[7†]
3. **Jackson 包名**：从 `com.fasterxml.jackson` 迁移至 `tools.jackson`
4. **自动配置模块化**：单体 JAR 拆分为多个模块
5. **注解包名**：`javax.*` 迁移至 `jakarta.*`
6. **Undertow 不支持**：暂不支持 Undertow Web 服务器[7†]

**依赖升级路径：**

```xml
<!-- pom.xml 示例：从 Spring Boot 3.5 升级到 4.0 -->
  <properties>
  <spring-boot.version>4.0.0</spring-boot.version>
  <maven.compiler.source>17</maven.compiler.source>
  <maven.compiler.target>17</maven.compiler.target>
  </properties>
  <dependencies>
    <!-- Spring Boot 核心依赖 -->
      <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter</artifactId>
      <version>${spring-boot.version}</version>
      </dependency>
      <!-- Web 开发 -->
        <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
        <version>${spring-boot.version}</version>
        </dependency>
        <!-- 数据访问 -->
          <dependency>
          <groupId>org.springframework.boot</groupId>
          <artifactId>spring-boot-starter-data-jpa</artifactId>
          <version>${spring-boot.version}</version>
          </dependency>
          <!-- OpenTelemetry（新增） -->
            <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-opentelemetry</artifactId>
            <version>${spring-boot.version}</version>
            </dependency>
          </dependencies>
```

#### 10.2 分步迁移策略

**第一阶段：环境准备**

1. **升级 JDK**：确保使用 JDK 17 或更高版本

```bash
# 检查 Java 版本
java -version
# 设置 JAVA_HOME
export JAVA_HOME=/path/to/jdk-17
```

1. **更新 IDE**：升级 IntelliJ IDEA、Eclipse 或 VS Code 到最新版本
2. **备份项目**：创建代码仓库备份和分支

**第二阶段：依赖升级**

1. **更新 Spring Boot 版本**：

```xml
<properties>
<spring.boot.version>4.0.0</spring.boot.version>
</properties>
<dependencyManagement>
  <dependencies>
    <dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-dependencies</artifactId>
    <version>${spring.boot.version}</version>
    <type>pom</type>
    <scope>import</scope>
    </dependency>
  </dependencies>
</dependencyManagement>
```

1. **更新第三方依赖**：

```xml
<!-- Jackson 迁移 -->
  <properties>
  <jackson.version>3.0.0</jackson.version>
  </properties>
  <dependencies>
    <dependency>
    <groupId>tools.jackson.core</groupId>
    <artifactId>jackson-databind</artifactId>
    <version>${jackson.version}</version>
    </dependency>
  </dependencies>
```

**第三阶段：代码修改**

1. **包名替换**：

```bash
# 使用 IDE 的重构功能进行批量替换
# javax.servlet.* -> jakarta.servlet.*
# javax.persistence.* -> jakarta.persistence.*
# javax.validation.* -> jakarta.validation.*
```

1. **注解更新**：

```java
// 旧版本
@RestController
public class UserController {
@GetMapping("/users")
public List<User> getUsers() {
  // 实现
  }
  }
  // 新版本（添加 API 版本支持）
  @RestController
  @RequestMapping("/api/v1")
  public class UserController {
  @GetMapping("/users")
  @ApiVersion("1.0")
  public List<User> getUsers() {
    // 实现
    }
    }
```

1. **配置文件更新**：

```yaml
# application.yml（新增 OpenTelemetry 配置）
spring:
opentelemetry:
enabled: true
exporter:
otlp:
endpoint: "${OTEL_EXPORTER_OTLP_ENDPOINT:http://localhost:4317}"
```

**第四阶段：测试验证**

1. **单元测试**：运行所有单元测试，确保功能正常

```bash
mvn test
```

1. **集成测试**：使用 TestContainers 进行集成测试

```bash
mvn verify
```

1. **性能测试**：验证应用性能没有回归

```bash
# 使用 JMeter 或 Gatling 进行性能测试
```

#### 10.3 迁移工具和脚本

**Spring Boot Properties Migrator：**

Spring Boot 提供了属性迁移工具，帮助自动处理配置属性的变更[7†]。

```xml
<dependency>
<groupId>org.springframework.boot</groupId>
<artifactId>spring-boot-properties-migrator</artifactId>
<scope>runtime</scope>
</dependency>
```

**迁移脚本示例：**

```bash
#!/bin/bash
# migrate-to-4.0.sh
echo "开始迁移到 Spring Boot 4.0..."
# 1. 备份当前代码
echo "备份代码..."
git branch backup-before-4.0
# 2. 更新 Spring Boot 版本
echo "更新 Spring Boot 版本..."
mvn versions:set -DnewVersion=4.0.0
# 3. 替换包名
echo "替换包名..."
find src -name "*.java" -type f -exec sed -i 's/javax\./jakarta./g' {} \;
# 4. 格式化代码
echo "格式化代码..."
mvn com.coveo:fmt-maven-plugin:format
# 5. 编译项目
echo "编译项目..."
mvn compile
# 6. 运行测试
echo "运行测试..."
mvn test
echo "迁移完成！请 review 变更并提交代码。"
```

------

### 十一、总结与展望

#### 11.1 Spring Boot 4.0 的核心价值

Spring Boot 4.0 的发布代表了 Java 企业级应用开发的新里程碑。通过模块化架构、空安全标准化、现代 Java 特性拥抱、声明式 HTTP 客户端和内置弹性功能等核心特性，该版本为构建更加健壮、高效和可维护的应用提供了系统性解决方案。

**关键优势总结：**

1. **开发效率提升**：模块化架构减少了不必要的依赖，IDE 性能和编译速度显著提升
2. **代码质量改善**：JSpecify 注解体系和空安全机制在编译期防止了大量的潜在 NullPointerException
3. **性能优化**：构建时查询编译、虚拟线程支持和优化的 JSON 处理提升了运行时性能
4. **云原生就绪**：OpenTelemetry 集成、HTTP Service Clients 和 API 版本管理简化了微服务架构
5. **现代化开发体验**：对最新 Java 特性的支持改善了开发体验和代码表达力

#### 11.2 采用建议和最佳实践

**新项目建议：**

对于新启动的项目，建议直接采用 Spring Boot 4.0，以充分利用其现代化特性和长期支持（LTS）。

**现有项目迁移路径：**

1. **评估影响**：使用 Spring Boot Migrator 评估迁移工作量
2. **分阶段迁移**：采用"绞杀者模式"逐步替换旧代码
3. **并行维护**：在迁移期间保持旧版本的稳定运行
4. **充分测试**：利用 TestContainers 2.0 进行全面的集成测试

**长期演进方向：**

Spring Boot 团队已经明确了未来发展方向：

- **持续性能优化**：虚拟线程、AOT 编译和 GraalVM 原生镜像支持
- **云原生能力增强**：Kubernetes 集成、服务发现和负载均衡
- **AI 和数据集成**：与 Spring AI 的深度集成，简化 AI 应用开发
- **开发者体验**：改进的热部署、调试和性能分析工具

Spring Boot 4.0 为未来的 Java 企业级应用开发奠定了坚实基础，是开发者拥抱现代开发理念和构建高质量应用的最佳选择。