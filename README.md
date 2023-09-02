## Overview
Changing the Logging Level at the Runtime for a Spring Boot Application

# Springboot actuator
We're going to start by using the /loggers Actuator endpoint to display and change our logging level. The /loggers endpoint is available at actuator/loggers and we can access a specific logger by appending its name as part of the path.

For example, we can access the root logger with the URL http://localhost:8080/actuator/loggers/root.

# /logger endpoint
Comment or remove logback.xml file under resource folder.
this will enable Springboot default logging, by default info, warn and Error will be logged.

Start the application and run the following command.

Then run the endpoint api
curl http://localhost:8080/log

# Output:
2019-09-02 09:51:53.498  INFO 12208 --- [nio-8080-exec-1] c.b.s.b.m.logging.LoggingController      : This is an INFO level message
2019-09-02 09:51:53.498  WARN 12208 --- [nio-8080-exec-1] c.b.s.b.m.logging.LoggingController      : This is a WARN level message
2019-09-02 09:51:53.498 ERROR 12208 --- [nio-8080-exec-1] c.b.s.b.m.logging.LoggingController      : This is an ERROR level message

# Change logging levels
curl http://localhost:8080/actuator/loggers/uk.co.ksb.LoggingDemo.controller
{"configuredLevel":INFO,"effectiveLevel":"INFO"}

To change the logging level, we can issue a POST request to the /loggers endpoint:

curl -i -X POST -H 'Content-Type: application/json' -d '{"configuredLevel": "TRACE"}' http://localhost:8080/actuator/loggers/uk.co.ksb.LoggingDemo.controller
HTTP/1.1 204
Date: Sat, 02 Sep 2023 19:34:23 GMT

If we check the logging level again, we should see it set to TRACE:

curl http://localhost:8080/actuator/loggers/com.baeldung.spring.boot.management.logging
{"configuredLevel":"TRACE","effectiveLevel":"TRACE"}

Finally, we can re-run our log API and see our changes in action:

curl http://localhost:8080/log

20:37:02.642 [http-nio-8080-exec-2] TRACE u.c.k.L.controller.LoggingController - This is a TRACE level message
20:37:02.650 [http-nio-8080-exec-2] DEBUG u.c.k.L.controller.LoggingController - This is a DEBUG level message
20:37:02.650 [http-nio-8080-exec-2] INFO  u.c.k.L.controller.LoggingController - This is an INFO level message
20:37:02.650 [http-nio-8080-exec-2] WARN  u.c.k.L.controller.LoggingController - This is a WARN level message
20:37:02.650 [http-nio-8080-exec-2] ERROR u.c.k.L.controller.LoggingController - This is an ERROR level message

# Reference
https://www.baeldung.com/spring-boot-changing-log-level-at-runtime

