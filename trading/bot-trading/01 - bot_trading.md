# Orden de arranque

1. Estructura del monorepo + git
2. MariaDB en Docker + esquema
3. Esqueleto de Express con capas
4. `/webhook` probado con Postman
5. React

# Estructura

```
bot-trading/
├── backend/
├── frontend/
├── infra/
└── .gitignore
```

# Infra

```
infra/
├── docker-compose.yml      # orquesta todos los servicios
├── .env                    # gitignored
├── mariadb/
│   └── init/
│       └── 01_esquema.sql
└── nginx/                  # cuando llegue
    └── nginx.conf

```

## MariaDB

Para levantar la imagen de MariaDB

```bash
docker compose up -d
docker compose logs mariadb | grep -i "01_esquema"
```

Para comprobar que funciona

```bash
docker exec -it bot_mariadb mariadb -u bot -p1234 bot_trading -e "SHOW TABLES;"
```


# Cluodflared

`C:\Users\tu_usuario\.cloudflared\` ya viviría en local de windows. De esta forma hay menos partes configurar para que se comunique bien con `localhost:3000`.