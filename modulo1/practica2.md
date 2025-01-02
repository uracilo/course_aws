# Práctica 1: Exploración de AWS con Terraform

## Objetivo
Familiarizarse con el uso de Terraform para aprovisionar recursos básicos en AWS y desplegar tu primera página web estática.

---

## Secciones

### 1. Configurar Terraform

En esta sección aprenderás a:
- Instalar Terraform en tu sistema.
- Configurar las credenciales de AWS para Terraform.

#### Pasos:

1. **Instalación de Terraform**:
   - Descarga la versión adecuada para tu sistema operativo desde [Terraform](https://www.terraform.io/downloads).
   - Sigue las instrucciones de instalación.

2. **Configurar credenciales de AWS**:
   - Asegúrate de tener un usuario IAM con permisos necesarios (como `AdministratorAccess`).
   - Configura tus credenciales ejecutando `aws configure` en tu terminal.

3. **Inicializar Terraform**:
   - Crea un directorio de proyecto.
   - Ejecuta `terraform init` dentro de este directorio para inicializar el backend.

---

### 2. Guía Práctica: EC2, S3 e IAM con Terraform

#### **a. EC2: Crear una Instancia**
- **Objetivo**: Provisione una instancia EC2 utilizando un archivo de configuración de Terraform.
- **Archivo de configuración (`main.tf`)**:
  ```hcl
  provider "aws" {
    region = "us-east-1"
  }

  resource "aws_instance" "example" {
    ami           = "ami-0c02fb55956c7d316" # Amazon Linux 2
    instance_type = "t2.micro"

    tags = {
      Name = "MiPrimeraInstancia"
    }
  }
  ```
- **Pasos**:
  1. Guarda el archivo como `main.tf`.
  2. Ejecuta:
     - `terraform plan`: Verifica los cambios que se aplicarán.
     - `terraform apply`: Aprovisiona la instancia EC2.

#### **b. S3: Crear un Bucket**
- **Objetivo**: Configurar un bucket de S3 para almacenamiento de archivos.
- **Archivo de configuración (`main.tf`)**:
  ```hcl
  resource "aws_s3_bucket" "example" {
    bucket = "mi-primer-bucket-terraform"
    acl    = "public-read"

    website {
      index_document = "index.html"
    }
  }
  ```
- **Pasos**:
  1. Agrega este bloque a tu archivo `main.tf`.
  2. Sube un archivo estático al bucket.
  3. Habilita la configuración de hosting estático.

#### **c. IAM: Crear un Usuario y Rol**
- **Objetivo**: Crear un usuario IAM con permisos específicos.
- **Archivo de configuración (`main.tf`)**:
  ```hcl
  resource "aws_iam_user" "example" {
    name = "mi-usuario-terraform"
  }

  resource "aws_iam_policy_attachment" "example" {
    name       = "UsuarioAdminAttachment"
    users      = [aws_iam_user.example.name]
    policy_arn = "arn:aws:iam::aws:policy/AdministratorAccess"
  }
  ```
- **Pasos**:
  1. Agrega este bloque al archivo `main.tf`.
  2. Ejecuta `terraform apply` para crear el usuario.
  3. Verifica las credenciales en la consola de AWS.

---

### 3. Crear tu Primera Página Web

#### **Subir una Página Web a S3**
- **Objetivo**: Publicar una página HTML estática en S3 usando Terraform.
- **Pasos**:
  1. Diseña una página web básica (`index.html`). Puedes usar este ejemplo:
     ```html
     <!DOCTYPE html>
     <html>
     <head>
         <title>Mi Primera Página Web</title>
     </head>
     <body>
         <h1>¡Hola, Terraform!</h1>
         <p>Mi primera página web desplegada con Terraform.</p>
     </body>
     </html>
     ```
  2. Sube el archivo al bucket de S3 configurado previamente.
  3. Accede al enlace generado para el bucket de hosting estático.

---

## Recursos Adicionales
- [Documentación oficial de Terraform](https://www.terraform.io/docs/)
- [Documentación de AWS Provider para Terraform](https://registry.terraform.io/providers/hashicorp/aws/latest/docs)
- [Guía para empezar con Terraform](https://learn.hashicorp.com/terraform)

---

¡Felicitaciones por completar tu primera práctica con Terraform en AWS! Ahora tienes las bases para automatizar el despliegue de recursos y optimizar tus flujos de trabajo.

