![banner](/assets/banner.jpg)

# The open standard of simple logistics data

Carqo is an open-source standard that makes logistics data simple. By focusing only on the essential questions of who is involved, what is shipped, where it is loaded and unloaded, and when, it removes unnecessary complexity and allows organizations to exchange information clearly, consistently, and efficiently.

## As simple as possible

This object provides a minimal yet complete representation of a shipment by addressing the four key questions: **who** is involved, **what** is being shipped, **where** it is loaded and unloaded, and **when** the operations are scheduled.

```json
{
  "what": {
    "amount": 33,
    "unit": "PALLET"
  },
  "where": {
    "load": {
      "latitude": 52.400334,
      "longitude": 6.61591
    },
    "unload": {
      "latitude": 51.893867,
      "longitude": 4.520616
    }
  },
  "when": {
    "load": {
      "start": "2026-05-28T08:00:00Z",
      "stop": "2026-05-28T09:00:00Z"
    },
    "unload": {
      "start": "2026-05-28T16:00:00Z",
      "stop": "2026-05-28T17:00:00Z"
    }
  }
}
```

## Structure

This class diagram represents the fundamental structure of Carqo shipments, highlighting the four key questions: **who**, **what**, **where** and **when**, keeping logistics data simple and clear.

```mermaid
classDiagram

class Shipment {
    <<schema>>
    Who: who <<required>>
    What: what <<required>>
    Where: where <<required>>
    When: when <<required>>
}

class Who {
    <<schema>>
    string principal <<required>>
    string sender <<required>>
    string receiver <<required>>
    string carrier <<required>>
}

class What {
    <<schema>>
    int amount <<required>>
    string unit <<required>>
}

class Where {
    <<schema>>
    Position: load <<required>>
    Position: unload <<required>>
}

class Position {
    <<schema>>
    number latitude <<required>>
    number longitude <<required>>
}

class When {
    <<schema>>
    Timeslot: load <<required>>
    Timeslot: unload <<required>>
}

class Timeslot {
    <<schema>>
    string start <<required>>
    string stop <<required>>
}

Shipment -->  Who: contains
Shipment -->  What: contains
Shipment -->  Where: contains
Shipment -->  When: contains

Where --> Position: contains
When --> Timeslot: contains
```
