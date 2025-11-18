# Nginx Single Endpoint Deployment

## nginx-pod.yaml

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: frontend-build-cfg-test
  namespace: autox-dev
  labels:
    app: frontend-build
    component: frontend-build-test
data:
  default.conf: |+


    server {
        listen 8080;
        server_name localhost;

        root /usr/share/nginx/dt;
        index index.html;

        # -------- AutoX Frontend ---------
        location /autox/assets/ {
            root /usr/share/nginx/dt;
            expires 1y;
            add_header Cache-Control "public";
        }

        # React app routing
        location / {
            root /usr/share/nginx/dt;
            try_files $uri /autox/index.html;
        }
        # -------- AutoX Frontend Ends---------


        # -------- Agent Builder ---------
        location /agentbuilder/assets/ {
            root /usr/share/nginx/dt;
            expires 1y;
            add_header Cache-Control "public";
        }

        # React app routing
        location /agentbuilder/ {
            root /usr/share/nginx/dt;
            try_files $uri /agentbuilder/index.html;
        }
    #    location = /agentbuilder/login {
    #        return 301 /agentbuilder/index.html;
    #    }
        # -------- Agent Builder Ends---------


    # -------- AgentBuilder Backend ---------
    location /agentbuilderapi/ {
        proxy_pass http://agent-builder-backend:7860/;
        proxy_http_version 1.1;

        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";

        proxy_read_timeout 3600s;
        proxy_send_timeout 3600s;
        proxy_buffering off;
        add_header Cache-Control no-cache;

        # ✅ CORS Headers
        add_header 'Access-Control-Allow-Origin' '*' always;
        add_header 'Access-Control-Allow-Methods' 'GET, POST, PUT, DELETE, PATCH, OPTIONS' always;
        add_header 'Access-Control-Allow-Headers' 'Authorization, Content-Type, X-Requested-With, Accept, Origin' always;
        add_header 'Access-Control-Allow-Credentials' 'true' always;

        # ✅ Handle OPTIONS preflight
        if ($request_method = OPTIONS) {
            add_header 'Access-Control-Allow-Origin' '*';
            add_header 'Access-Control-Allow-Methods' 'GET, POST, PUT, DELETE, PATCH, OPTIONS';
            add_header 'Access-Control-Allow-Headers' 'Authorization, Content-Type, X-Requested-With, Accept, Origin';
            add_header 'Access-Control-Max-Age' 3600;
            add_header 'Content-Length' 0;
            add_header 'Content-Type' 'text/plain charset=UTF-8';
            return 204;
        }
    }

    # -------- Gateway Backend ---------
    location /gateway/ {
        proxy_pass http://gateway:8585/;
        proxy_http_version 1.1;

        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";

        proxy_read_timeout 3600s;
        proxy_send_timeout 3600s;
        proxy_buffering off;
        add_header Cache-Control no-cache;

        # CORS not enabled here
    }

    # -------- NodeServer Backend ---------
    # This NodeServer block is having SSE configuration also.

    location /resource/ {
        proxy_pass http://node-server:8760/;
        proxy_http_version 1.1;

        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";

        proxy_read_timeout 3600s;
        proxy_send_timeout 3600s;
        proxy_buffering off;
        proxy_cache off;
        add_header Cache-Control no-cache;

        # CORS Headers
        add_header 'Access-Control-Allow-Origin' '*' always;
        add_header 'Access-Control-Allow-Methods' 'GET, POST, PUT, DELETE, PATCH, OPTIONS' always;
        add_header 'Access-Control-Allow-Headers' 'Authorization, Content-Type, X-Requested-With, Accept, Origin' always;
        add_header 'Access-Control-Allow-Credentials' 'true' always;

        if ($request_method = OPTIONS) {
            add_header 'Access-Control-Allow-Origin' '*';
            add_header 'Access-Control-Allow-Methods' 'GET, POST, PUT, DELETE, PATCH, OPTIONS';
            add_header 'Access-Control-Allow-Headers' 'Authorization, Content-Type, X-Requested-With, Accept, Origin';
            add_header 'Access-Control-Max-Age' 3600;
            add_header 'Content-Length' 0;
            add_header 'Content-Type' 'text/plain charset=UTF-8';
            return 204;
        }
    }

    # -------- NodeServer slack---------
    location /slack/ {
        proxy_pass http://node-server:8761/;
        proxy_http_version 1.1;

        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";

        proxy_read_timeout 3600s;
        proxy_send_timeout 3600s;
        proxy_buffering off;
        proxy_cache off;
        add_header Cache-Control no-cache;

        # CORS Headers
        add_header 'Access-Control-Allow-Origin' '*' always;
        add_header 'Access-Control-Allow-Methods' 'GET, POST, PUT, DELETE, PATCH, OPTIONS' always;
        add_header 'Access-Control-Allow-Headers' 'Authorization, Content-Type, X-Requested-With, Accept, Origin' always;
        add_header 'Access-Control-Allow-Credentials' 'true' always;

        if ($request_method = OPTIONS) {
            add_header 'Access-Control-Allow-Origin' '*';
            add_header 'Access-Control-Allow-Methods' 'GET, POST, PUT, DELETE, PATCH, OPTIONS';
            add_header 'Access-Control-Allow-Headers' 'Authorization, Content-Type, X-Requested-With, Accept, Origin';
            add_header 'Access-Control-Max-Age' 3600;
            add_header 'Content-Length' 0;
            add_header 'Content-Type' 'text/plain charset=UTF-8';
            return 204;
        }
    }

    ## Agent-observer block ##

    location /agentobserver {
        proxy_pass http://agent-observer:3000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
        proxy_read_timeout 3600;
     }

    }

---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: frontend-build-test
  namespace: autox-dev
spec:
  replicas: 1
  selector:
    matchLabels:
      app: frontend-build-test
  template:
    metadata:
      labels:
        app: frontend-build-test
    spec:
      containers:
        - name: frontend-build-test
          imagePullPolicy: Always
          image: emindsguardians/nginxpath:autox-1 
          command:
          - sh
          - -c
          - |
            cp /config/default.conf /etc/nginx/conf.d/default.conf
            nginx -g "daemon off;"
          ports:
            - containerPort: 8080
          volumeMounts:
            - name: frontend-build-cfg-volume-test
              mountPath: /config/default.conf
              subPath: default.conf
      volumes:
        - name: frontend-build-cfg-volume-test
          configMap:
            name: frontend-build-cfg-test

---
apiVersion: v1
kind: Service
metadata:
  name: frontend-build-test
  namespace: autox-dev
spec:
  type: NodePort
  ports:
    - name: svc
      port: 8080
      targetPort: 8080
      nodePort: 30085
  selector:
    app: frontend-build-test

```

---

# Overview

### Purpose of this Nginx Pod

* Expose one **single endpoint** (NodePort) to outside the Kubernetes cluster.
* Internally route different paths to different services using Nginx path-based routing.

### Types of Scenarios Handled

1. **Direct build file serving (CSR - Client-Side Rendering)**
2. **Proxy to internal backend services using paths**
   Example: `/gateway/`
3. **SSE (Server-Sent Events)**
   Implemented in `/resource/` block.
4. **SSR (Server-Side Rendering)**
   Implemented in `/agentobserver`
5. **Process exposure through `/slack/`**

### Frontend Details

* Build files from **AutoX Frontend** → `/usr/share/nginx/dt/autox`
* Build files from **Agent Builder Frontend** → `/usr/share/nginx/dt/agentbuilder`

### Important Note

> Whatever configuration we update in this Nginx config, we must also update the Nginx config in the respective services on the `10.0.0.99` machine if DNS routing is used.

---

# Dockerfile (Sample)

```dockerfile
FROM emindsguardians/base:node18 AS autoxfrontend
ADD autox-frontend /autox-frontend
WORKDIR autox-frontend
RUN npm install --force
RUN npm run build

#############################################################################################

FROM emindsguardians/base:python3.11 AS autoxagentbuilder

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update && apt-get install -y --no-install-recommends \
    make build-essential python3-dev curl wget git ca-certificates \
    libreadline-dev zlib1g-dev libssl-dev unzip \
    && rm -rf /var/lib/apt/lists/*

RUN wget https://www.sqlite.org/2023/sqlite-autoconf-3420000.tar.gz \
    && tar -xvzf sqlite-autoconf-3420000.tar.gz \
    && cd sqlite-autoconf-3420000 \
    && ./configure --prefix=/usr/local \
    && make -j$(nproc) && make install \
    && cd .. && rm -rf sqlite-autoconf-3420000* \
    && ldconfig

RUN sqlite3 --version

ENV NVM_DIR=/root/.nvm
ENV NODE_VERSION=18.20.8
RUN curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.3/install.sh | bash \
    && bash -c ". $NVM_DIR/nvm.sh && nvm install $NODE_VERSION && nvm alias default $NODE_VERSION"
ENV PATH=$NVM_DIR/versions/node/v$NODE_VERSION/bin:$PATH

RUN curl -LsSf https://astral.sh/uv/install.sh | sh
ENV PATH="/root/.local/bin:$PATH"

COPY autox-agent-builder /autox-agent-builder

WORKDIR /autox-agent-builder/src/frontend
RUN npm i && npm run build

EXPOSE 3000

############################################################################################

FROM emindsguardians/base:nginx
RUN apt update
RUN apt install curl nano -y
COPY --from=autoxfrontend /autox-frontend/dist /usr/share/nginx/dt/autox
COPY --from=autoxagentbuilder /autox-agent-builder/src/frontend/build /usr/share/nginx/dt/agentbuilder
```

---

