!SLIDE subsection
# Plugging In External<br> Message Broker

!SLIDE smaller bullets incremental
# Reasons for Full-featured Broker

* Simple built-in broker supports subset of STOMP<br>(`SUBSCRIBE`, `UNSUBSCRIBE`, `MESSAGE`)
* No acks, receipts, transactions
* Relies on simple message sending loop
* Not suitable for clustering

!SLIDE smaller center
# Message Flow: External Message Broker
<br>
![Diagram](architecture-stomp-relay.png)

!SLIDE smaller
# Configuration

    @@@ java

    @Configuration
    @EnableWebSocketMessageBroker
    public class Config implements WebSocketMessageBrokerConfigurer {


      @Override
      public void configureMessageBroker(MessageBrokerConfigurer c) {
        c.enableStompBrokerRelay("/queue/", "/topic/");
        c.setAnnotationMethodDestinationPrefixes("/app");
      }

      // ...

    }

!SLIDE smaller bullets incremental
# Using Full-featured Broker

* Check broker STOMP documentation page
* Understand destination semantics
* Any additional features
* Application is now backed by message broker



