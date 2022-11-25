# Spring Boot 3.0 Goes GA <!-- omit in toc -->

[Spring Blog](https://spring.io/blog/2022/11/24/spring-boot-3-0-goes-ga) announced that **Spring Boot 3.0** is now generally available and 3.0.0 can be found in Maven Central on November 24, 2022.

## 1. Highlights of the new release include:

- A **Java 17** baseline
- Support for generating **native images with GraalVM**, superseding the experimental Spring Native project
- **Improved observability** with Micrometer and Micrometer Tracing
- Support for **Jakarta EE 10 with an EE 9 baseline**

## 2. Migrate to Spring Boot 3.0

We could follow the official [Migration Guide](https://github.com/spring-projects/spring-boot/wiki/Spring-Boot-3.0-Migration-Guide).

Please read this guide carefully, especially for the Spring features that are used in your applications.

## 3. What is New?

### 3.1. Java 17

Java 17 brings lots of new language features and JVM features. You refer [my blog](./2022-11-15-java-17-vs-java-8.md)

## 4. Jakarta EE 9

The most important change might be the jump from Java EE to Jakarta EE9, where the package namespace changed from _javax.\*_ to _jakarta.\*_. As a result, we need to adjust all imports in our code whenever we use classes from Java EE directly.

For example, when we access the _HttpServletRequest_ object within our Spring MVC Controller, we need to replace:

```java
import javax.servlet.http.HttpServletRequest;
```

with

```java
import jakarta.servlet.http.HttpServletRequest;
```

## 5. Native Executables

Building native executables and deploying them to **GraalVM** gets a higher priority. So the Spring Native initiative is moving into Spring proper.

For AOT generation, there's no need to include separate plugins, we can just use a new goal of the spring-boot-maven-plugin:

```sh
$ mvn spring-boot:aot-generate
```

The native application can be built as follows:

```sh
$ mvn spring-boot:build-image

# or
$ gradle bootBuildImage
```
