# 📦 carqo/core

Carqo is an open-source logistics data standard that brings clarity and simplicity to the global transport ecosystem. It defines a unified structure for shipments, parties, planning, and reporting, allowing carriers, shippers, platforms, and software vendors to speak the same language.

## Structure

```mermaid
classDiagram

    class Event {
        sequence
        Type
        Position
        Moment
    }

    class Position {
        latitude
        longitude
        _name_
        _Address_
    }

    class Address {
        addressLine1
        zip
        city
        _state_
        country
    }

    class Moment {
        start
        _end_
    }

    Event --> Position : has a
    Position --> Address : may have
    Event --> Moment   : has a
```
