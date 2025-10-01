# Vue 3 App - Dockerizada

Aplicación Vue 3 de ejemplo dockerizada con configuraciones separadas para desarrollo y producción.

## 📁 Estructura del Proyecto

```
vue3-app/
├── src/                    # Código fuente de la aplicación
│   ├── App.vue            # Componente principal
│   ├── main.js            # Punto de entrada
│   └── style.css          # Estilos globales
├── public/                 # Archivos estáticos
├── docker/                 # Configuraciones Docker
│   ├── development/       # Dockerfile para desarrollo
│   └── production/        # Dockerfiles para producción (nginx + build)
├── compose.dev.yaml       # Docker Compose para desarrollo
├── compose.prod.yaml      # Docker Compose para producción
├── vite.config.js         # Configuración de Vite
├── package.json           # Dependencias del proyecto
└── .env.example           # Variables de entorno de ejemplo
```

## 🚀 Inicio Rápido

### Prerrequisitos

- Docker y Docker Compose instalados
- Archivo `.env` configurado (copiar desde `.env.example`)

Puedes verificar las versiones ejecutando:

```bash
docker --version
docker compose version
```

### 1. Configurar Variables de Entorno

```bash
cp .env.example .env
```

**Importante**: Ajusta las variables `UID` y `GID` en el archivo `.env` para que coincidan con tu ID de usuario y grupo. Puedes encontrarlos ejecutando:

```bash
id -u  # UID
id -g  # GID
```

Ejemplo de `.env`:

```env
UID=1000
GID=1000
VITE_PORT=5173
NODE_VERSION=22.0.0
```

### 2. Crear la red Docker (solo la primera vez)

```bash
docker network create dokerizado_vue3-development
```

### 3. Iniciar los servicios Docker Compose

```bash
docker compose -f compose.dev.yaml up -d
```

### 4. Instalar dependencias de Vue 3

Accede al contenedor workspace y ejecuta los comandos necesarios:

```bash
docker compose -f compose.dev.yaml exec workspace-vue bash
npm install
npm run dev
```

La aplicación estará disponible en: http://localhost:5173

### Comandos Útiles

#### Acceder al contenedor Workspace

El contenedor workspace incluye Node.js (vía NVM), NPM y otras herramientas necesarias para el desarrollo de Vue 3:

```bash
docker compose -f compose.dev.yaml exec workspace-vue bash
```

#### Instalar una nueva dependencia

```bash
docker compose -f compose.dev.yaml exec workspace-vue npm install axios
```

#### Reconstruir contenedores

```bash
docker compose -f compose.dev.yaml up -d --build
```

#### Detener el entorno

```bash
docker compose -f compose.dev.yaml down
```

#### Ver logs

```bash
docker compose -f compose.dev.yaml logs -f
```

Para servicios específicos:

```bash
docker compose -f compose.dev.yaml logs -f workspace-vue
```

### 3. Producción

#### Levantar el entorno de producción

```bash
docker compose -f compose.prod.yaml up -d --build
```

La aplicación estará disponible en: http://localhost:8080 (o el puerto configurado en NGINX_PORT)

#### Ver logs

```bash
docker compose -f compose.prod.yaml logs -f
```

#### Detener el entorno

```bash
docker compose -f compose.prod.yaml down
```

## 🛠️ Características

### Desarrollo

- ✅ Hot Module Replacement (HMR) con Vite
- ✅ Montaje de volúmenes para desarrollo en tiempo real
- ✅ Puerto configurable (default: 5173)
- ✅ Usuario no-root para seguridad

### Producción

- ✅ Build optimizado con Vite
- ✅ Servido por Nginx para máximo rendimiento
- ✅ Compresión Gzip habilitada
- ✅ Headers de seguridad configurados
- ✅ Caché de assets estáticos
- ✅ Soporte para SPA routing
- ✅ Health check endpoint

## 📝 Scripts Disponibles

```bash
# Desarrollo
npm run dev        # Inicia el servidor de desarrollo

# Producción
npm run build      # Construye la aplicación para producción
npm run preview    # Previsualiza el build de producción
```

## 🔧 Personalización

### Modificar la configuración de Vite

Edita `vite.config.js` para cambiar puertos, plugins u otras configuraciones.

### Modificar la configuración de Nginx

Edita `docker/production/nginx/nginx.conf` para ajustar la configuración del servidor web.

## 🐛 Troubleshooting

### El servidor de desarrollo no se actualiza automáticamente

Asegúrate de que `usePolling` esté habilitado en `vite.config.js` (ya está configurado por defecto).

### Errores de permisos

Verifica que las variables `UID` y `GID` en el archivo `.env` coincidan con tu usuario:

```bash
echo "UID=$(id -u)" >> .env
echo "GID=$(id -g)" >> .env
```

### El puerto ya está en uso

Cambia el valor de `VITE_PORT` o `NGINX_PORT` en el archivo `.env`.

## 📚 Recursos

- [Vue 3 Documentation](https://vuejs.org/)
- [Vite Documentation](https://vitejs.dev/)
- [Docker Documentation](https://docs.docker.com/)

## 📄 Licencia

Este proyecto es un ejemplo de configuración dockerizada para Vue 3.
