# ubuntu
# docker
* install
    ```
    sudo apt-get remove docker docker-engine docker.io containerd runc
    ```
    ```
    sudo apt-get update
    ```
    ```
    sudo apt-get install \
        ca-certificates \
        curl \
        gnupg \
        lsb-release
    ```
    ```
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
    ```
    ```
    echo \
      "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
      $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
    ```
    ```
    sudo apt-get update
    ```
    ```
    sudo apt-get install docker-ce docker-ce-cli containerd.io
    ```
    ```
    sudo groupadd docker
    ```
    ```
    sudo usermod -aG docker $USER
    ```
    ```
    newgrp docker 
    ```
    ```
    sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
    ```
    ```
    sudo chmod +x /usr/local/bin/docker-compose
    ```
* docker-compose for postgres, pgadmin, redis, mongo
    ```
    code docker-compose.yml
    ```
    ```
    version: '3.1'

    services:

      mongodb:
        image: mongo
        restart: always
        ports:
          - 27017:27017

      redis:
        image: redis:6
        restart: always
        ports:
          - 6379:6379

      postgres:
        container_name: postgres
        image: postgres
        environment:
          POSTGRES_USER: ${POSTGRES_USER:-postgres}
          POSTGRES_PASSWORD: ${POSTGRES_PASSWORD:-postgres}
          PGDATA: /data/postgres
        volumes:
           - postgres:/data/postgres
        ports:
          - "5432:5432"
        networks:
          - postgres
        restart: unless-stopped

      pgadmin:
        container_name: pgadmin
        image: dpage/pgadmin4
        environment:
          PGADMIN_DEFAULT_EMAIL: ${PGADMIN_DEFAULT_EMAIL:-pgadmin@gmail.com}
          PGADMIN_DEFAULT_PASSWORD: ${PGADMIN_DEFAULT_PASSWORD:-postgres}
          PGADMIN_CONFIG_SERVER_MODE: 'False'
        volumes:
           - pgadmin:/root/.pgadmin

        ports:
          - "${PGADMIN_PORT:-5050}:80"
        networks:
          - postgres
        restart: unless-stopped

    networks:
      postgres:
        driver: bridge

    volumes:
        postgres:
        pgadmin:
    ```
