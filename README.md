# 📦 Carqo

Carqo is an open-source logistics data standard that brings clarity and simplicity to the global transport ecosystem. It defines a unified structure for shipments, parties, planning, and reporting, allowing carriers, shippers, platforms, and software vendors to speak the same language.

## Structure

```mermaid
classDiagram

class Shipment {
    <<schema>>
    string reference <<required>>
    Event[] events <<required>>
    Cargo cargo <<required>>
}

class Cargo {
    <<schema>>
    string id <<required>>
    Item[] items <<required>>
    string description
}

class Item {
    <<schema>>
    string id <<required>>
    int amount <<required>>
    string unit <<required>>
    Weight weight <<required>>
    string description
}

class Weight {
    <<schema>>
    int value <<required>>
    string unit <<required>>
}

class Event {
    <<schema>>
    int sequence <<required>>
    EventType type <<required>>
    Position position <<required>>
    Moment moment <<required>>
    string[] items <<required>>
}

class EventType {
    <<enumeration>>
    LOAD
    UNLOAD
    STOP
}

class Position {
    <<schema>>
    number latitude <<required>>
    number longitude <<required>>
    string name
    Address address
}

class Address {
    <<schema>>
    string addressLine1 <<required>>
    string zip <<required>>
    string city <<required>>
    string state
    string country <<required>>
}

class Moment {
    <<schema>>
    string start <<required>>
    string end
}

Shipment "1" --> "*" Event : contains
Shipment "1" --> Cargo : contains
Cargo "1" --> "*" Item : contains
Item --> Weight : contains
Event --> EventType : uses
Event --> Position : contains
Position "0..1" --> Address : optional
Event --> Moment : contains
Event ..> Item : "references ids"
```
