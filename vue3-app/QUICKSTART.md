# 🚀 Quick Start - Vue 3 App

## Para Desarrollo

```bash
# 1. Entrar al directorio
cd vue3-app

# 2. Configurar variables de entorno (si aún no lo hiciste)
cp .env.example .env
# Edita .env y ajusta UID/GID si es necesario

# 3. Crear la red Docker (solo primera vez)
docker network create dokerizado_vue3-development

# 4. Levantar contenedores
docker compose -f compose.dev.yaml up -d

# 5. Acceder al workspace e instalar dependencias
docker compose -f compose.dev.yaml exec workspace-vue bash
npm install
npm run dev

# 6. Acceder a la aplicación
# http://localhost:5173
```

## Para Producción

```bash
# 1. Entrar al directorio
cd vue3-app

# 2. Construir y levantar contenedores
docker compose -f compose.prod.yaml up -d --build

# 3. Acceder a la aplicación
# http://localhost:8080
```

## Detener Servicios

```bash
# Desarrollo
docker compose -f compose.dev.yaml down

# Producción
docker compose -f compose.prod.yaml down
```

## Estructura de Archivos Creados

```
vue3-app/
├── compose.dev.yaml              # Docker Compose para desarrollo
├── compose.prod.yaml             # Docker Compose para producción
├── package.json                  # Dependencias del proyecto
├── vite.config.js               # Configuración de Vite
├── index.html                   # Punto de entrada HTML
├── .env                         # Variables de entorno
├── .env.example                 # Ejemplo de variables de entorno
├── .gitignore                   # Archivos ignorados por Git
├── .dockerignore                # Archivos ignorados por Docker
├── README.md                    # Documentación completa
├── src/
│   ├── main.js                  # Punto de entrada de Vue
│   ├── App.vue                  # Componente principal
│   └── style.css                # Estilos globales
├── public/
│   └── vite.svg                 # Logo de Vite
└── docker/
    ├── development/
    │   └── Dockerfile           # Dockerfile para desarrollo
    └── production/
        ├── Dockerfile           # Dockerfile para build
        └── nginx/
            ├── Dockerfile       # Dockerfile para Nginx
            └── nginx.conf       # Configuración de Nginx
```

¡Todo listo! 🎉
