# Worker cluster

| ID | P | Needs | Claim | Setup | Steps | Expected | Result |
|---|---|---|---|---|---|---|---|
| W1 | P1 | net | GET is public usage | none | `curl https://imprint-api.coey.dev/` | 200 JSON with grep/paths | pass |
| W2 | P1 | net | POST without bearer is 401 | none | POST question | 401 unauthorized | pass |
| W3 | P1 | net | POST bad bearer is 401 | none | POST with Bearer not-a-real-token | 401 unauthorized | pass |
| W4 | P2 | token | Authenticated ask returns selectedFiles | ASK token | POST valid body | 200 + files | blocked |
| W5 | P2 | token | Grep slice on Worker | ASK token | POST grep | only matching files | blocked |
| W6 | P3 | net | GET names version 0.0.1 | curl | read name/version | imprint 0.0.1 | pass |
