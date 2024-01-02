# Spring IOC和AOP
AOP是面向切面编程，它是对某一类事情的集中处理，它是一种思想，而Spring AOP是对这个思想的具体实现。

最具体的实现就是登录验证，除了登录和注册等几个功能不需要做用户登录验证之外，其他几乎所有页面调用的前端控制器(Controller)都需要先验证用户登录的状态。

IOC控制反转是Spring框架的一个核心特性，它通过将对象之间的依赖关系交由Spring容器来管理，实现了系统的松耦合和可扩展性。

# Spring事务的隔离级别
即包含以下5种隔离级别：
1. Isolation.DEFAULT:以连接的数据库的事务隔离级别为主。
2. Isolation.READ_UNCOMMITTED:读未提交，可以读取到未提交的事务，存在脏读。
3. Isolation.READ_COMMITTED:读已提交，只能读取到已经提交的事务，解决了脏读，存在不可重复读。（脏读：两个事务对同一个数据操作时，事务A更改了数据，事务B读到了这个数据，而事务A又回滚了，于是事务B读到的数据是无效的，就叫脏读）
4. Isolation.REPEATABLE_READ:可重复读，解决了不可重复读，但存在幻读（MySQL默认级别）
5. Isolation.SERIALIZABLE:串行化，可以解决所有并发问题，但性能太低。

# Spring、Spring Boot、Spring MVC
Spring是包含了众多工具方法的IOC容器

Spring Boot是为了简化Spring开发而产生的脚手架。

Spring MVC是一个基于MVC设计模式和Servlet API实现的Web项目，同时Spring MVC又是Spring框架中的一个Web模块，它是随着Spring的诞生而存在的一个框架。

# SpringBoot核心设计思想
1、约定大于配置：通过一些默认约定来减少配置的复杂性，使得开发者可以更快的搭建一个基于Spring的应用程序。

2、开箱即用：提供一组默认的配置和依赖，使得开发者可以快速地创建一个可用的应用程序，而不必进行大量的手动配置。

3、自动化配置：自动配置Spring框架中的各种组件，如数据源、事务管理、Web应用程序等，开发者可以通过简单的配置文件来进行自定义。

4、模块化设计：将应用程序分为多个模块，每个模块都有独立的功能和职责，使得应用程序更加灵活和易用。

5、注解驱动：通过注解来驱动应用程序的行为，简化开发过程，提高开发效率。

# SpringBoot的自动装配
1、自动装配：SpringBoot提供了大量的自动配置功能，可以快速地构建和部署应用程序。

2、条件装配：根据条件自动装配和配置应用程序中的各种组件和功能，使用@Conditional注解。

3、自动扫描：通过自动扫描，自动装配和配置应用程序中的各种组件和功能，使用@Controller、@Service、@Repository等注解。

4、属性配置：通过配置文件(application.properties或者application.yml)定义属性，自动装配和配置应用程序中的各种组件和功能。

# SpringBoot的常用注解
@Controller:标注控制层

@Service:标注服务层

@Mapper:标注数据持久层

@Configuration:标注配置层

@ResponseBody:表示返回的为非静态页面数据

@RestController:相当于@Controller+@ResponseBody

@RequestMapping:用来注册接口的路由映射

@Autowired:标注属性或构造函数，自动装配Spring容器中的Bean

@Value:标注属性，注入配置文件中的值。