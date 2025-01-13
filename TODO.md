* Spring Boot 3
  - https://www.bilibili.com/video/BV1534y1F7co/?spm_id_from=333.337.search-card.all.click&vd_source=9f2611296548fd13ce88cc658d20413f
  - GraalVM 
* Spring Boot 自动配置类原理
   - SPI
   - Spring Boot 2.X: META-INF/spring.factories
   - Spring Boot 3.0: META-INF/spring/org.springframework.boot.autoconfigure.AutoConfiguration.imports file instead of META-INF/spring.factories
java code:
```java
package org.springframework.boot.autoconfigure;
  
@Import(AutoConfigurationImportSelector.class)
public @interface EnableAutoConfiguration {
    String ENABLED_OVERRIDE_PROPERTY = "spring.boot.enableautoconfiguration";
    Class<?>[] exclude() default {};
    String[] excludeName() default {};
}
```
```java
	protected List<String> getCandidateConfigurations(AnnotationMetadata metadata, AnnotationAttributes attributes) {
		ImportCandidates importCandidates = ImportCandidates.load(this.autoConfigurationAnnotation,
				getBeanClassLoader());
		List<String> configurations = importCandidates.getCandidates();
		Assert.notEmpty(configurations,
				"No auto configuration classes found in " + "META-INF/spring/"
						+ this.autoConfigurationAnnotation.getName() + ".imports. If you "
						+ "are using a custom packaging, make sure that file is correct.");
		return configurations;
	}
```
* Spring Meta data
  - https://springdoc.cn/spring-boot/configuration-metadata.html#appendix.configuration-metadata
* Spring Security
  - https://docs.spring.io/spring-security/reference/index.html
  - https://openid.net/specs/openid-connect-discovery-1_0.html
* @Configuration(proxyBeanMethods = false) 
  - https://juejin.cn/post/7181637005774684218
  - @Configuration注释中的proxyBeanMethods参数是springboot1.0，升级到springboot2.0之后新增的比较重要的内容，
    该参数是用来代理bean的
    1.理论
    首先引出两个概念：Full 全模式，Lite 轻量级模式
    Full(proxyBeanMethods = true) :proxyBeanMethods参数设置为true时即为：Full 全模式。 该模式下注入容器中的同一个组件无论被取出多少次都是同一个bean实例，即单实例对象，在该模式下SpringBoot每次启动都会判断检查容器中是否存在该组件
    Lite(proxyBeanMethods = false) :proxyBeanMethods参数设置为false时即为：Lite 轻量级模式。该模式下注入容器中的同一个组件无论被取出多少次都是不同的bean实例，即多实例对象，在该模式下SpringBoot每次启动会跳过检查容器中是否存在该组件
    什么时候用Full全模式，什么时候用Lite轻量级模式？
    当在你的同一个Configuration配置类中，注入到容器中的bean实例之间有依赖关系时，建议使用Full全模式
    当在你的同一个Configuration配置类中，注入到容器中的bean实例之间没有依赖关系时，建议使用Lite轻量级模式，以提高springboot的启动速度和性能
* Use spring-boot-starter-data-elasticsearch in SimpleMonitor project for the auto configuration
```yaml
spring:
  elasticsearch:
    uris: "https://search.example.com:9200"
    socket-timeout: "10s"
    username: "user"
    password: "secret"

```
* Fat jar
  - https://docs.spring.io/spring-boot/specification/executable-jar/nested-jars.html
* Actuator
  - add a custom web endpoint for easy oAuth health check
* Prometheus
  - https://prometheus.io/docs/prometheus/latest/getting_started/