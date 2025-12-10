# 📦 carqo/core

Carqo is an open-source logistics data standard that brings clarity and simplicity to the global transport ecosystem. It defines a unified structure for shipments, parties, planning, and reporting, allowing carriers, shippers, platforms, and software vendors to speak the same language.

## Structure

```mermaid
classDiagram

class Shipment {
    <<schema>>
    Event[] events <<required>>
}

class Event {
    <<schema>>
    int sequence <<required>>
    EventType type <<required>>
    Position position <<required>>
    Moment moment <<required>>
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

Shipment "1" --> "1..*" Event : contains
Event --> Position : contains
Position "0..1" --> Address : optional
Event --> Moment : contains
```
