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
