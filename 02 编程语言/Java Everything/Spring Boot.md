# 概述

Spring Boot是简化Spring开发的框架、整合spring技术栈，J2EE开发解决方案

Spring Boot 的核心特性

1. 创建独立的 Spring 应用程序。
2. 内嵌 Tomcat、Jetty 或 Undertow （无需部署 war 文件)。
3. 提供 starter 简化配置。
4. 尽量进行 Spring 和第三方库的自动化配置。
5. 提供生产就绪功能，例如指标、健康检查和外部化配置。
6. 完全无需代码生成，无需 XML 配置。



# 注解

## 元注解

@Documented：将会在被此注解注解的元素的javadoc文档中列出注解，一般都打上这个注解没坏处

@Target：注解能被应用的目标元素，比如类、方法、属性、参数等等，需要仔细思考

@Retention：仅在源码保留，还是保留到编译后的字节码，还是到运行时也去加载，超过90%的应用会在运行时去解析注解进行额外的处理，所以大部分情况我们都会设置配置为RetentionPolicy.RUNTIME

@Inherited：如果子类没有定义注解的话，能自动从父类获取定义了继承属性的注解，比如Spring的@Service是没有继承特性的，但是@Transactional是有继承特性的，在OO继承体系中使用Spring注解的时候请特别注意这点，理所当然认为注解是能被子类继承的话可能会引起不必要的Bug，需要仔细斟酌是否开启继承

@Repeatable：Java 8引入的特性，通过关联注解容器定义可重复注解，小小语法糖提高了代码可读性，对于元素有多个重复注解其实是很常见的事情，比如某方法可以是A角色可以访问也可以是B角色可以访问，某方法需要定时任务执行，要在A条件执行也需要在B条件执行

@Native：是否在.h头文件中生成被标记的字段，除非原生程序需要和Java程序交互，否则很少会用到这个元注解

## 基本注解

@Service: 注解在类上，表示这是一个业务层bean

@Controller：注解在类上，表示这是一个控制层bean

@Repository: 注解在类上，表示这是一个数据访问层bean

@Component： 注解在类上，表示通用bean ，value不写默认就是类名首字母小写

@Autowired：按类型注入.默认属性required= true

@Resource: 按名称装配。

## 启动注解

@SpringBootApplication：包含了@ComponentScan、@Configuration和@EnableAutoConfiguration注解。其中

@ComponentScan：让spring Boot扫描到Configuration类并把它加入到程序上下文。

@SpringBootConfiguration ：等同于spring的XML配置文件；使用Java代码可以检查类型安全。

@EnableAutoConfiguration ：自动配置。

## HTTP注解

@RequestBody：HTTP请求获取请求体（处理复杂数据，比如JSON）

@RequestHeader：HTTP请求获取请求头

@CookieValue：HTTP请求获取cookie

@SessionAttribute：HTTP请求获取会话

@RequestAttribute：HTTP请求获取请求的Attribute中（比如过滤器和拦截器手动设置的一些临时数据），

@RequestParam：HTTP请求获取请求参数（处理简单数据，键值对），

@PathVariable：HTTP请求获取路径片段，

@MatrixAttribute：HTTP请求获取矩阵变量允许我们采用特殊的规则在URL路径后加参数（分号区分不同参数，逗号为参数增加多个值）

## 其他注解

@Transient：表示该属性并非一个到数据库表的字段的映射,ORM框架将忽略该属性。

@ConfigurationProperties：给对象赋值，将注解转换成对象。

@RequestMapping：和请求报文是做对应的

@EnableCaching：注解驱动的缓存管理功能

@GeneratedValue：用于标注主键的生成策略，通过 strategy 属性指定

@JsonIgnore：作用是json序列化时将Java bean中的一些属性忽略掉,序列化和反序列化都受影响。

@JoinColumn（name=”loginId”）:一对一：本表中指向另一个表的外键。一对多：另一个表指向本表的外键。
