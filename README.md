spring-retale
=============

Spring framework based projects

Spring Notes:
=============

* Dependency management
```
<dependencies>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-context</artifactId>
        <version>4.0.2.RELEASE</version>
        <scope>runtime</scope>
    </dependency>
</dependencies>
```
* Releases Repository
```
<repositories>
    <repository>
        <id>io.spring.repo.maven.release</id>
        <url>http://repo.spring.io/release/</url>
        <snapshots><enabled>false</enabled></snapshots>
    </repository>
</repositories>
```
* Milestone Repository
```
<repositories>
    <repository>
        <id>io.spring.repo.maven.milestone</id>
        <url>http://repo.spring.io/milestone/</url>
        <snapshots><enabled>false</enabled></snapshots>
    </repository>
</repositories>
```
* Snapshots Repository
```
<repositories>
    <repository>
        <id>io.spring.repo.maven.snapshot</id>
        <url>http://repo.spring.io/snapshot/</url>
        <snapshots><enabled>true</enabled></snapshots>
    </repository>
</repositories>
```
* Maven "Bill Of Materials" Dependency
  * Ensure that all spring dependencies (both direct and transitive) are at the same version.
  * Benefit of using the BOM is that you no longer need to specify the <version> attribute when depending on Spring 
```
<dependencies>
	<dependency>
	    <groupId>org.springframework</groupId>
	    <artifactId>spring-framework-bom</artifactId>
	    <version>4.0.2.RELEASE</version>
	    <type>pom</type>
	    <scope>import</scope>
	</dependency>
</dependencies>
```
* Exclude commons-logging and using SLF4J
```
<dependencies>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-core</artifactId>
        <version>4.0.2.RELEASE</version>
        <exclusions>
            <exclusion>
                <groupId>commons-logging</groupId>
                <artifactId>commons-logging</artifactId>
            </exclusion>
        </exclusions>
    </dependency>
    <dependency>
        <groupId>org.slf4j</groupId>
        <artifactId>jcl-over-slf4j</artifactId>
        <version>1.5.8</version>
    </dependency>
    <dependency>
        <groupId>org.slf4j</groupId>
        <artifactId>slf4j-api</artifactId>
        <version>1.5.8</version>
    </dependency>
    <dependency>
        <groupId>org.slf4j</groupId>
        <artifactId>slf4j-log4j12</artifactId>
        <version>1.5.8</version>
    </dependency>
    <dependency>
        <groupId>log4j</groupId>
        <artifactId>log4j</artifactId>
        <version>1.2.14</version>
    </dependency>
</dependencies>
```
* IOC container
  * Also know as DI dependency Injection
  * Mechanism like service locator pattern
  * In Spring, the objects that form the backbone of your application and that are managed by the Spring IoC container are called beans. A bean is an object that is instantiated, assembled, and otherwise managed by a Spring IoC container. Otherwise, a bean is simply one of many objects in your application. Beans, and the dependencies among them, are reflected in the configuration metadata used by a container.
  * ClassPathXmlApplicationContext or FileSystemXmlApplicationContext
```
ApplicationContext context =
    new ClassPathXmlApplicationContext(new String[] {"services.xml", "daos.xml"});

// retrieve configured instance
PetStoreService service = context.getBean("petStore", PetStoreService.class);
``` 
  * Application code should have no calls to the getBean() method at all, and thus no dependency on Spring APIs at all.
  * Metadata configuratiojn
    * XML based
    * Anotation based
    * Java based
  * XML based
```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd">

    <import resource="services.xml"/>
    <import resource="resources/messageSource.xml"/>
    <import resource="/resources/themeSource.xml"/>
    <import resource="file:C:/config/services.xml"/><!-- Avoid - through "${…}" placeholders that are resolved against JVM system properties at runtime -->
    
    <bean id="..." class="...">
        <!-- collaborators and configuration for this bean go here -->
    </bean>

    <bean id="..." class="...">
        <!-- collaborators and configuration for this bean go here -->
    </bean>

    <!-- more bean definitions go here -->

</beans>
```
  * Bean Definitions include - class, name, scope, constructor arguments, properties, autowiring mode, lazy-initialization mode, initialization method, destruction method
  * Aliasing a bean outside its definition
    * A bean in the same container which is named fromName, may also, after the use of this alias definition, be referred to as toName.
```
  <alias name="fromName" alias="toName"/>
```
  * Use of 'Class' property
    * Typically, to specify the bean class to be constructed in the case where the container itself directly creates the bean by calling its constructor reflectively, somewhat equivalent to Java code using the new operator.
    * To specify the actual class containing the static factory method that will be invoked to create the object, in the less common case where the container invokes a static, factory method on a class to create the bean. The object type returned from the invocation of the static factory method may be the same class or another class entirely.
  * Static nested class are accessed using binary name of inner class in bean definition. e.g. com.example.Foo$Bar
  * Instantiating static factory method
```
<bean id="clientService"
    class="examples.ClientService"
    factory-method="createInstance"/>
````
  * Instantiating instance factory method
```
<bean id="serviceLocator" class="examples.DefaultServiceLocator">
    <!-- inject any dependencies required by this locator bean -->
</bean>

<bean id="clientService"
    factory-bean="serviceLocator"
    factory-method="createClientServiceInstance"/>

<bean id="accountService"
    factory-bean="serviceLocator"
    factory-method="createAccountServiceInstance"/>
```
```
public class DefaultServiceLocator {

    private static ClientService clientService = new ClientServiceImpl();
    private static AccountService accountService = new AccountServiceImpl();

    private DefaultServiceLocator() {}

    public ClientService createClientServiceInstance() {
        return clientService;
    }

    public AccountService createAccountServiceInstance() {
        return accountService;
    }

}
```
* IOC
  * The process is fundamentally the inverse, hence the name Inversion of Control (IoC), i.e the bean itself controlling the instantiation or location of its dependencies on its own by using direct construction of classes, or the Service Locator pattern.
* DI exists in two major variants, Constructor-based dependency injection and Setter-based dependency injection.
* Constructor-based DI
```
<beans>
    <bean id="foo" class="x.y.Foo">
        <constructor-arg ref="bar"/>
        <constructor-arg ref="baz"/>
    </bean>

    <bean id="bar" class="x.y.Bar"/>

    <bean id="baz" class="x.y.Baz"/>
</beans>
```
```
<bean id="exampleBean" class="examples.ExampleBean">
    <constructor-arg type="int" value="7500000"/>
    <constructor-arg type="java.lang.String" value="42"/>
</bean>
```
```
<bean id="exampleBean" class="examples.ExampleBean">
    <constructor-arg index="0" value="7500000"/>
    <constructor-arg index="1" value="42"/>
</bean>
```
```
<bean id="exampleBean" class="examples.ExampleBean">
    <constructor-arg name="years" value="7500000"/>
    <constructor-arg name="ultimateanswer" value="42"/>
</bean>
```
* A good rule of thumb is to use constructor arguments for mandatory dependencies and setters for optional dependencies.
* By default Spring can convert a value supplied in string format to all built-in types, such as int, long, String, boolean, etc. 
* Beans that are singleton-scoped and set to be pre-instantiated (the default) are created when the container is created.
* detects configuration problems, such as references to non-existent beans and circular dependencies, at container load-time.
* Circular dependency
  * Class A requires an instance of class B through constructor injection, and class B requires an instance of class A through constructor injection. If you configure beans for classes A and B to be injected into each other, the Spring IoC container detects this circular reference at runtime, and throws a BeanCurrentlyInCreationException.
  * One possible solution is to edit the source code of some classes to be configured by setters rather than constructors. Alternatively, avoid constructor injection and use setter injection only. In other words, although it is not recommended, you can configure circular dependencies with setter injection.
* Setter-based DI
*
