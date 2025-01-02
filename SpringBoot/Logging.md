# Spring Doc
https://docs.spring.io/spring-boot/reference/features/logging.html#features.logging
https://www.slf4j.org/manual.html
https://logback.qos.ch/
## Default logging provider
LogBack

## Some fun features should highlight
1. $ java -jar myapp.jar --debug (can replace to --trace for more details). It also can be set in application.properties (for example: debug=true). If you use idea, you can set it in the run configuration: program arguments.
2. spring.output.ansi.enabled=ALWAYS (or DETECT) in application.properties to enable color output in console/termial.