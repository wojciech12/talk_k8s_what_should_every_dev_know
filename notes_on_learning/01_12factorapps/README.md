## 12factor apps

Good to know what [12factorapp](https://12factor.net) is. 12factor is not new, so you might find points obvious.

Few that are very relevant for Kubernetes:

1. III. [Config](https://12factor.net/config) - environment variable, injected by environment
2. VIII. [Concurrency by process](https://12factor.net/concurrency)
3. IX. [Disposability](https://12factor.net/disposability)
4. X. [Dev/Prod parity](https://12factor.net/dev-prod-parity) including time, tooling, and backing services
5. XI. [Logs](https://12factor.net/logs) - push your logs to stdout and let log routers to take care of them
