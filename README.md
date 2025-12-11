![banner](/assets/banner.jpg)

# The open standard for simple, structured logistics data

Carqo is an open-source data standard for logistics that brings transparency and simplicity to the global transport ecosystem. The standard provides a uniform and accessible structure for shipments and all involved parties, enabling organizations to exchange information consistently and collaborate far more efficiently.

## Model

```mermaid
classDiagram

class Message {
    <<schema>>
    string: id <<required>>
    Shipment[] shipments <<required>>
    Actor[] actors <<required>>
    string createdAt <<required>>
    string updatedAt <<required>>
}

class Actor {
    <<schema>>
    string id <<required>>
    Role[] roles <<required>>
    oneOf:
    + string directoryId
    + Relation relation
}

class Role {
    <<enumeration>>
    PRINCIPAL
    SENDER
    RECEIVER
    CARRIER
}

class Relation {
    <<schema>>
    string name <<required>>
    string vat <<required>>
    Address address
}

class Shipment {
    <<schema>>
    string reference <<required>>
    Event[] events <<required>>
    Cargo cargo <<required>>
}

class Cargo {
    <<schema>>
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
    Type type <<required>>
    Position position <<required>>
    Moment moment <<required>>
    string[] items <<required>>
}

class Type {
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

Message "1" --> "*" Actor : contains
Actor "1" --> "*" Role : uses
Actor "0..1" ..> Relation : optional
Relation "0..1" ..> Address : optional
Message "1" --> "*" Shipment : contains
Shipment "1" --> "*" Event : contains
Shipment "1" --> Cargo : contains
Cargo "1" --> "*" Item : contains
Item --> Weight : contains
Event --> Type : uses
Event --> Position : contains
Position "0..1" ..> Address : optional
Event --> Moment : contains
Event ..> Item : "references ids"
```

## Example

As simple as possible:

```json
{
  "id": "467491f1-d1b1-4fe8-b9d4-cda8623c8403",
  "shipments": [
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
          "items": ["ITEM-001"]
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
          "items": ["ITEM-001"]
        }
      ],
      "cargo": {
        "items": [
          {
            "id": "ITEM-001",
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
  ],
  "actors": [
    {
      "id": "ACTOR-001",
      "roles": ["PRINCIPAL", "SENDER"],
      "directoryId": "org:carqo:1234567890"
    },
    {
      "id": "ACTOR-002",
      "roles": ["RECEIVER"],
      "directoryId": "org:carqo:2468101214"
    },
    {
      "id": "ACTOR-003",
      "roles": ["CARRIER"],
      "directoryId": "org:carqo:0987654321"
    }
  ],
  "createdAt": "2026-05-28T07:00:00Z",
  "updatedAt": "2026-05-28T17:00:00Z"
}
```
