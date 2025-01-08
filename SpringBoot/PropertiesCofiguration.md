# Spring Doc
https://docs.spring.io/spring-boot/reference/features/external-config.html#features.external-config.files.property-placeholders

* Canonical form (kebab-case using only lowercase letters).

For example, ${demo.item-price} will pick up demo.item-price and demo.itemPrice forms from the application.properties file, as well as DEMO_ITEMPRICE from the system environment. If you used ${demo.itemPrice} instead, demo.item-price and DEMO_ITEMPRICE would not be considered.

* Random Values
```properties
demo.secret="${random.value}"
demo.number="${random.int}"
demo.bignumber="${random.long}"
demo.uuid="${random.uuid}"
demo.number.less.than.ten="${random.int(10)}"
demo.number.in.range="${random.int[1024,65536]}"
```

* Properties binding
  * application.properties
```properties
my.service.enabled=true
my.service.remote-address=http://localhost:8080
my.service.security.username=user
my.service.security.password=secret
my.service.security.roles=USER,ADMIN
```
  * JavaBean properties binding
```java
import jakarta.validation.constraints.NotNull;
import lombok.Data;
import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.stereotype.Component;
import org.springframework.validation.annotation.Validated;

import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

@Component
@Data
@ConfigurationProperties("my.service")
@Validated
public class JavaBeanPropertiesBinding {
  private boolean enabled;
  @NotNull
  private String remoteAddress;
  private final Security security = new Security();

  @Data
  public static class Security {
    private String username;
    private String password;
    private List<String> roles = new ArrayList<>(Collections.singleton("USER"));
  }
}
```
```gradle
implementation 'jakarta.validation:jakarta.validation-api'
```
  * Constructor binding
```java
import lombok.Data;
import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.boot.context.properties.EnableConfigurationProperties;
import org.springframework.boot.context.properties.bind.ConstructorBinding;
import org.springframework.boot.context.properties.bind.DefaultValue;

import java.util.List;

@EnableConfigurationProperties
@Data
@ConfigurationProperties("my.service")
public class ConstructorPropertiesBinding {
    private boolean enabled;
    private String remoteAddress;
    private final Security security;

    @ConstructorBinding
    public ConstructorPropertiesBinding(boolean enabled, String remoteAddress, Security security) {
        this.enabled = enabled;
        this.remoteAddress = remoteAddress;
        this.security = security;
    }

    @Data
    public static class Security {
        private String username;
        private String password;
        private List<String> roles;

        @ConstructorBinding
        public Security(String username, String password, @DefaultValue("USER") List<String> roles) {
            this.username = username;
            this.password = password;
            this.roles = roles;
        }
    }
}

```