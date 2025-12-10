# 📦 Carqo

Carqo is an open-source logistics data standard that brings clarity and simplicity to the global transport ecosystem. It defines a unified structure for shipments, parties, planning, and reporting, allowing carriers, shippers, platforms, and software vendors to speak the same language.

## Model

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

## Example

As simple as possible:

```json
{
  "reference": "SHIPMENT-001",
  "events": [
    {
      "sequence": 1,
      "type": "LOAD",
      "position": {
        "latitude": 52.400334,
        "longitude": 6.61591
      },
      "moment": {
        "start": "2026-05-28T07:00:00Z"
      },
      "items": ["CARGO-001-ITEM-001"]
    },
    {
      "sequence": 2,
      "type": "UNLOAD",
      "position": {
        "latitude": 51.893867,
        "longitude": 4.520616
      },
      "moment": {
        "start": "2026-05-28T17:00:00Z"
      },
      "items": ["CARGO-001-ITEM-001"]
    }
  ],
  "cargo": {
    "id": "CARGO-001",
    "items": [
      {
        "id": "CARGO-001-ITEM-001",
        "amount": 5,
        "unit": "BOX",
        "weight": {
          "value": 250,
          "unit": "KG"
        }
      }
    ]
  }
}
```
