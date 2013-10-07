!SLIDE subsection
# STOMP in<br>Spring Framework 4

!SLIDE smaller bullets incremental
# New module: `spring-messaging`

* Support for STOMP 
* Message-handling annotations
* Key abstractions moved from Spring Integration<br>(`Message`, `MessageChannel`, `MesageHandler`, ...)
* Foundation for WebSocket messaging architecture

!SLIDE smaller
# Annotated Message Handling
<br>
    @@@ java

    @Controller
    public class GreetingController {


      // Receive client messages to destination "/greeting"


      @MessageMapping("/greeting")

      public void greet(String greeting) {

      }

    }

!SLIDE smaller
# Annotated Message Handling
<br>
    @@@ java

    @Controller
    public class GreetingController {


      // Receive client messages to destination "/greeting"
      // Process + send to subscribers of "/topic/greetings"

      @MessageMapping("/greeting")
      @SendTo("/topic/greetings")
      public String greet(String greeting) {
          return "[" + getTimestamp() + "]: " + greeting;
      }

    }


!SLIDE smaller bullets incremental
# What Handles `"/topic/greetings"` Subscriptions?
<br><br>
* Simple, built-in message broker, or
* Full-featured, external STOMP broker-<br>RabbitMQ, ActiveMQ, etc

!SLIDE smaller
# Configuration

    @@@ java

    @Configuration
    @EnableWebSocketMessageBroker
    public class Config implements WebSocketMessageBrokerConfigurer {


      @Override
      public void registerStompEndpoints(StompEndpointRegistry r) {
        r.addEndpoint("/stomp"); // WebSocket URL prefix
      }

      @Override
      public void configureMessageBroker(MessageBrokerConfigurer c) {
        c.enableSimpleBroker("/topic/"); // destination prefix
        c.setAnnotationMethodDestinationPrefixes("/app");
      }

    }

!SLIDE small center
# Message Flow
<br>
![Diagram](architecture.png)

!SLIDE smaller bullets incremental
# Send Messages From Anywhere

    @@@ java

    @Controller
    public class GreetingController {

      @Autowired
      private SimpMessagingTemplate template;


      @RequestMapping(value="/greeting", method=POST)
      public void greet(String greeting) {
        String text = "[" + getTimeStamp() + "]:" + greeting;
        this.template.convertAndSend("/topic/greeting", text);
      }

    }

!SLIDE smaller
# Responding to a Subscription
## _(Request-Reply Pattern)_

    @@@ java

    @Controller
    public class PortfolioController {


      @SubscribeEvent("/positions")
      public List<PortfolioPosition> getPositions(Principal user) {
        Portfolio portfolio = ...
        return portfolio.getPositions();
      }

    }


