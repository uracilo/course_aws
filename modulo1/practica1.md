# Práctica 1: Explorar la consola de AWS

## Objetivo

Familiarizarse con la consola de AWS explorando los servicios fundamentales, configurando recursos básicos y lanzando tu primera página web estática.

---

## Secciones

### 1. Navegar por la Consola de AWS

En esta sección aprenderás a:

- Iniciar sesión en la consola de AWS.
- Identificar y navegar entre los diferentes servicios disponibles.
- Personalizar la consola usando la barra de favoritos.

#### Pasos:

1. **Inicia sesión**: Accede a [Consola de AWS](https://aws.amazon.com/console/).
2. **Explora servicios**: Ubica servicios como EC2, S3 e IAM en el buscador o en la categoría correspondiente.
3. **Configura favoritos**: Haz clic en el ícono de estrella para agregar servicios de uso frecuente a la barra de navegación.

### 2. Guía Práctica: EC2, S3 e IAM

#### **a. EC2: Crear una Instancia**

- **Objetivo**: Lanzar una instancia EC2 básica.
- **Pasos**:
  1. Ve a EC2 y selecciona "Launch Instance".
  2. Elige la AMI (Amazon Machine Image) "Amazon Linux 2".
  3. Selecciona una instancia de tipo **t2.micro** (incluida en el nivel gratuito).
  4. Configura el almacenamiento predeterminado y crea un par de claves para conectarte.
  5. Lanza la instancia y observa su estado en el panel.

#### **b. S3: Crear un Bucket**

- **Objetivo**: Configurar un bucket de S3 para almacenar archivos.
- **Pasos**:
  1. Ve a S3 y selecciona "Create Bucket".
  2. Ingresa un nombre único para tu bucket y selecciona una región (por ejemplo, **us-east-1**).
  3. Desactiva el bloqueo de acceso público si planeas usar el bucket para contenido público.
  4. Sube un archivo (por ejemplo, un archivo HTML o imagen).
  5. Habilita el acceso público al archivo para poder visualizarlo externamente.

#### **c. IAM: Configurar un Usuario y Rol**

- **Objetivo**: Crear un usuario con permisos restringidos.
- **Pasos**:
  1. Ve a IAM y selecciona "Add Users".
  2. Asigna un nombre al usuario y habilita el acceso programático (access key).
  3. Asocia el usuario a un grupo con permisos administradores (o crea uno nuevo).
  4. Descarga las credenciales de acceso.

### 3. Crear tu Primera Página Web

#### **Subir una Página Web a S3**

- **Objetivo**: Publicar una página HTML estática en S3.
- **Pasos**:
  1. Diseña una página web básica (`index.html`). Puedes usar este ejemplo:
     ```html
     <!DOCTYPE html>
     <html>
     <head>
         <title>Mi Primera Página Web</title>
     </head>
     <body>
         <h1>¡Hola, AWS!</h1>
         <p>Mi primera página web en Amazon S3.</p>
     </body>
     </html>
     ```
  2. Ve a tu bucket de S3 y sube el archivo `index.html`.
  3. Activa las propiedades del bucket para **Static Website Hosting**.
     - Ingresa `index.html` como el documento de inicio.
  4. Copia el enlace de tu dame Recursos Adicionales

* [Documentación oficial de AWS](https://docs.aws.amazon.com/)
* [Navegar por la consola de AWS](https://aws.amazon.com/getting-started/)
* [Curso introductorio gratuito de AWS](https://aws.amazon.com/training/intro-to-cloud/)

---

¡Felicitaciones por completar tu primera práctica en AWS! Ahora tienes las bases para explorar más servicios y expandir tus conocimientos.

