# Carqo

The open standard for simple, structured logistics data.

## Core

```mermaid
graph TD
    Position -->|contains| latitude
    Position -->|contains| longitude
    Position -->|may reference| Address

    Address -->|contains| addressLine1
    Address -->|contains| postalCode
    Address -->|contains| city
    Address -->|contains| country
```
