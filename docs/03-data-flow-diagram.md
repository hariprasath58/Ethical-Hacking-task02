# Data-Flow Diagram

```mermaid
flowchart LR
    subgraph OUTSIDE["OUT OF SCOPE: physical host, home network, internet"]
        X["Real network and personal data (never tested)"]
    end
    subgraph VM["Trust boundary 4: Kali VM (tester workstation)"]
        B["External entity: Browser / tester"]
        subgraph CT["Trust boundary 3: Docker container keen_neumann, 127.0.0.1:3000"]
            W(["Process: Juice Shop web app and REST API"])
            D[("Data store: SQLite database")]
        end
    end
    B -->|"1. HTTP requests: login, search, orders (TB1)"| W
    W -->|"2. HTML, JSON, session token"| B
    W -->|"3. Database queries (TB2)"| D
    D -->|"4. Query results"| W
```

## Data flows

| ID | From | To | Data | Trust boundary crossed |
|---|---|---|---|---|
| 1 | Browser | Web app | Credentials, search terms, orders, uploads | TB1 (untrusted input enters app) |
| 2 | Web app | Browser | Pages, JSON, session token | TB1 |
| 3 | Web app | Database | Queries built from user input | TB2 |
| 4 | Database | Web app | User, product, order records | TB2 |

## Notes
- The lab is bound to 127.0.0.1, so no flow exists between the lab and the outside network.
- The red-flag point is flow 1 and flow 3: user input reaches the database.
