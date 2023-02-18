# Corteza

![Corteza](https://github.com/zakery1369/pics/blob/master/Corteza.png?raw=true)

### Configure docker-compose.yml

```
version: '3.5'

services:
  server:
    image: cortezaproject/corteza:${VERSION}
    restart: always
    env_file: [ .env ]
    depends_on: [ db ]
    ports: [ "127.0.0.1:18080:80" ]

  db:
    
    image: postgres:13
    restart: always
    healthcheck: { test: ["CMD-SHELL", "pg_isready -U corteza"], interval: 10s, timeout: 5s, retries: 5 }
    volumes:
      - "dbdata:/var/lib/postgresql/data"
    environment:
      POSTGRES_USER:     corteza
      POSTGRES_PASSWORD: corteza
```

Create .env next to docker-compose.yml
### Configure .env

```
DOMAIN=localhost:18080
VERSION=2022.9

DB_DSN=postgres://corteza:corteza@db:5432/corteza?sslmode=disable

HTTP_WEBAPP_ENABLED=true

ACTIONLOG_ENABLED=false
```
Direct your browser to http://localhost:18080

---

### Expose Corteza to your internal network and the world
If you want to use Corteza in production and with other users,Change the following code in docker-compose.yml as below :

```
ports: [ "0.0.0.0:18080:80" ]
```
And Change the following code in .env as below :

```
DOMAIN=YOUR_PUBLIC_IP:18080
```

---

### Exposing external port
Edit .env to change the exposed port

```
DOMAIN=YOUR_PUBLIC or LOCAL_IP:8585
```
Direct your browser to http://localhost:8585
