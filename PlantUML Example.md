### PlantUML Example
```plantuml
Bob -> Alice : hello
Alice -> Wonderland: hello
Wonderland -> next: hello
next -> Last: hello
Last -> next: hello
next -> Wonderland : hello
Wonderland -> Alice : hello
Alice -> Bob: hello
Bob --> Alice: response
Bob --> Alice: hahahahaha
```


### PlantUML Example
```plantuml
@startuml

abstract class KafkaMessageHandler {
  {abstract} public abstract void processKafkaMessage()
  protected <T extends BaseKafkaMessage> T getPayloadDetails()
}

KafkaMessageHandler *-down- VisaTokenizationAlert
KafkaMessageHandler *-down- CustomerAlertTopic1
KafkaMessageHandler *-down- CustomerAlertTopic2
KafkaMessageHandler *-down- CustomerAlertTopic3

@enduml
```


