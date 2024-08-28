# Mock Json Server

## Ejecutar json-server
Ejecuta el servidor json-server apuntando a tu archivo `db.json`:

```bash
json-server --watch src/db.json --port 5005
```

Esto levantará un servidor en el puerto 3000 (o el puerto que elijas) con una API RESTful basada en el contenido del archivo db.json.

## Acceder a la API RESTful

Una vez que el servidor esté en marcha, podrás acceder a los datos mediante peticiones HTTP. Por ejemplo:

- Obtener la lista de micro frontends:
```bash
GET http://localhost:3000/microFrontends
```
Obtener la configuración de los componentes:
```bash

GET http://localhost:3000/componentsConfig
```
- Agregar o modificar configuraciones:

- Puedes hacer peticiones POST, PUT, o PATCH para agregar o modificar los datos. Por ejemplo, para actualizar la configuración del header:
```bash
curl -X PUT -H "Content-Type: application/json" -d '{"backgroundColor": "#000", "textColor": "#fff"}' http://localhost:3000/componentsConfig/header
```

## Integrar `json-server` con tu aplicación

Ahora puedes configurar tu aplicación para hacer peticiones a este servidor y cargar dinámicamente las configuraciones de micro frontends y componentes desde el servidor `json-server`

## Ejemplo de integración en React

Aquí tienes un ejemplo básico de cómo podrías integrar la carga de configuraciones desde `json-server` en una aplicación React:

```jsx
import React, { useEffect, useState } from 'react';

const App = () => {
const [config, setConfig] = useState(null);

useEffect(() => {
// Cargar configuración desde el json-server
fetch('http://localhost:3000/componentsConfig')
.then(response => response.json())
.then(data => setConfig(data));
}, []);

if (!config) {
return <div>Loading...</div>;
}

return (
<div>
<header style={{ backgroundColor: config.header.backgroundColor, color: config.header.textColor }}>
Header Component
</header>
<footer style={{ backgroundColor: config.footer.backgroundColor, color: config.footer.textColor }}>
Footer Component
</footer>
</div>
);
};

export default App;
```

## Produccion

Se preocede a montar un BaaS (Backend as a Service) y desplegarlo en un contenedor, hay varias opciones populares que se puede utilizar, como Firebase, Supabase, Hasura, o Parse. Se elige Supabase, que es una opción de código abierto que puedes desplegar en un contenedor Docker.

### ¿Qué es Supabase?
Supabase es una alternativa de código abierto a Firebase, que ofrece una base de datos PostgreSQL con API RESTful y en tiempo real, autenticación, almacenamiento, y más. Todo esto se puede desplegar en tu propia infraestructura utilizando Docker.

### Pasos para montar y desplegar Supabase en un contenedor Docker:
1. Instalación de Docker y Docker Compose

Si aún no tienes Docker y Docker Compose instalados, instala ambos:

- Docker: Instalación de Docker
- Docker Compose: Instalación de Docker Compose

2. Configurar un archivo Docker Compose para Supabase

Crea un archivo `docker-compose.yml` que contendrá la configuración para desplegar `Supabase`. Este archivo describirá los contenedores para el backend, la base de datos PostgreSQL y otros servicios necesarios.

Este `docker-compose.yml` define varios servicios:

- db: El servicio PostgreSQL que actúa como la base de datos para Supabase.
- api: El servicio de autenticación (GoTrue) que maneja la autenticación de usuarios.
- realtime: El servicio de tiempo real que permite suscribirse a los cambios en la base de datos.
- storage: El servicio de almacenamiento de archivos.
- postgrest: El servicio de API REST que convierte la base de datos PostgreSQL en una API RESTful automáticamente.

3. Ejecutar Docker Compose

Una vez que tengas el archivo docker-compose.yml, puedes levantar los servicios de Supabase ejecutando:

```bash
docker-compose up -d
```

Esto descargará las imágenes de Docker necesarias y levantará los servicios en contenedores.

4. Acceso a los Servicios

- Base de Datos PostgreSQL: Puedes acceder a la base de datos PostgreSQL en localhost:5432.
- Autenticación: El servicio de autenticación estará disponible en http://localhost:9999.
- PostgREST: El servicio que expone la base de datos como una API REST estará disponible en http://localhost:3000.
- Realtime: El servicio de tiempo real estará disponible en http://localhost:4000.
- Storage: El servicio de almacenamiento de archivos estará disponible en http://localhost:5000.

5. Configurar Supabase

Puedes acceder a la base de datos `PostgreSQL` y crear tus tablas utilizando herramientas como `pgAdmin` o conectarte directamente con `psql`. Después, puedes configurar las reglas de acceso, JWT, y políticas directamente desde la base de datos.