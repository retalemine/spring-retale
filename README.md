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

