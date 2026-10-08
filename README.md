# 🌳 DevTree – Backend

API REST de **DevTree**, una aplicación tipo *link in bio* donde cada usuario tiene un perfil público con todas sus redes sociales en un solo enlace (por ejemplo, `devtree.com/tu-usuario`).

Este repositorio contiene el servidor, construido con **Node.js**, **Express** y **TypeScript**, con **MongoDB** como base de datos, autenticación con **JWT** y subida de imágenes a **Cloudinary**.

> 🔗 El frontend de este proyecto está en [devtree_frontend](https://github.com/SuemyDzib/devtree_frontend).

## ✨ Características

- **Registro de usuarios** con nombre, email, *handle* (nombre de usuario) y contraseña.
- **Inicio de sesión** que devuelve un token JWT con vigencia de 180 días.
- **Contraseñas cifradas** con bcrypt.
- **Rutas protegidas** mediante un middleware que valida el token enviado como `Bearer`.
- **Actualización de perfil**: handle, descripción y enlaces a redes sociales.
- **Subida de imagen de perfil** a Cloudinary.
- **Perfil público** consultable por handle, sin exponer email ni contraseña.
- **Búsqueda de disponibilidad** de un handle antes de registrarse.
- **Handles normalizados** con `slug` (sin espacios ni caracteres especiales) y únicos.
- **Validación de datos** con express-validator.
- **CORS configurado** para aceptar solo peticiones del frontend.

## 🛠️ Tecnologías

- [Node.js](https://nodejs.org/) y [Express](https://expressjs.com/)
- [TypeScript](https://www.typescriptlang.org/)
- [MongoDB](https://www.mongodb.com/) con [Mongoose](https://mongoosejs.com/)
- [JSON Web Token](https://github.com/auth0/node-jsonwebtoken) para la autenticación
- [bcrypt](https://github.com/kelektiv/node.bcrypt.js) para cifrar contraseñas
- [Cloudinary](https://cloudinary.com/) y [formidable](https://github.com/node-formidable/formidable) para la subida de imágenes
- [express-validator](https://express-validator.github.io/) para validaciones
- [nodemon](https://nodemon.io/) y [ts-node](https://typestrong.org/ts-node/) para desarrollo

## 📋 Requisitos

- [Node.js](https://nodejs.org/) 18 o superior
- Una base de datos MongoDB, local o en [MongoDB Atlas](https://www.mongodb.com/atlas)
- Una cuenta de [Cloudinary](https://cloudinary.com/) para las imágenes de perfil

## 🚀 Instalación y uso

1. Clona el repositorio:

   ```bash
   git clone https://github.com/SuemyDzib/devtree_backend.git
   cd devtree_backend
   ```

2. Instala las dependencias:

   ```bash
   npm install
   ```

3. Crea un archivo `.env` en la raíz a partir del ejemplo y completa tus datos:

   ```bash
   cp .env.example .env
   ```

4. Inicia el servidor en modo desarrollo:

   ```bash
   npm run dev
   ```

   El servidor quedará disponible en `http://localhost:4000`.

### Variables de entorno

| Variable                | Descripción                                                   |
| ----------------------- | ------------------------------------------------------------- |
| `PORT`                  | Puerto del servidor (opcional, por defecto `4000`)            |
| `FRONTEND_URL`          | URL del frontend permitida por CORS, ej. `http://localhost:5173` |
| `MONGO_URI`             | Cadena de conexión a MongoDB                                  |
| `JWT_SECRET`            | Clave secreta para firmar los tokens JWT                      |
| `CLOUDINARY_NAME`       | *Cloud name* de tu cuenta de Cloudinary                       |
| `CLOUDINARY_API_KEY`    | API key de Cloudinary                                         |
| `CLOUDINARY_API_SECRET` | API secret de Cloudinary                                      |

### Scripts

| Comando           | Descripción                                                                 |
| ----------------- | --------------------------------------------------------------------------- |
| `npm run dev`     | Inicia el servidor con recarga automática                                   |
| `npm run dev:api` | Igual que `dev`, pero permite peticiones sin origen (útil para Postman o Thunder Client) |
| `npm run build`   | Compila TypeScript a JavaScript en la carpeta `dist/`                       |
| `npm start`       | Ejecuta la versión compilada                                                |

## 🔌 Endpoints

| Método  | Endpoint         | Auth | Descripción                                        |
| ------- | ---------------- | :--: | -------------------------------------------------- |
| `POST`  | `/auth/register` |      | Registra un usuario nuevo                          |
| `POST`  | `/auth/login`    |      | Inicia sesión y devuelve un token JWT              |
| `GET`   | `/user`          |  ✅  | Obtiene los datos del usuario autenticado          |
| `PATCH` | `/user`          |  ✅  | Actualiza handle, descripción y enlaces            |
| `POST`  | `/user/image`    |  ✅  | Sube la imagen de perfil a Cloudinary              |
| `POST`  | `/search`        |      | Verifica si un handle está disponible              |
| `GET`   | `/:handle`       |      | Obtiene el perfil público de un usuario            |

Las rutas marcadas con ✅ requieren el encabezado:

```
Authorization: Bearer <token>
```

### Ejemplo: registro

```http
POST /auth/register
Content-Type: application/json

{
  "name": "Juan Pérez",
  "email": "juan@correo.com",
  "handle": "juanperez",
  "password": "password123"
}
```

## 🗃️ Modelo de usuario

| Campo         | Tipo   | Notas                                              |
| ------------- | ------ | -------------------------------------------------- |
| `handle`      | String | Único, en minúsculas                               |
| `name`        | String | Obligatorio                                        |
| `email`       | String | Único, en minúsculas                               |
| `password`    | String | Cifrado con bcrypt                                 |
| `description` | String | Opcional                                           |
| `image`       | String | URL de la imagen en Cloudinary                     |
| `links`       | String | Arreglo de redes sociales guardado como JSON       |

## 👤 Autor

Desarrollado por **Suemy Dzib** – [@SuemyDzib](https://github.com/SuemyDzib) a través del curso de Udemy impartido por Juan de la Torre.