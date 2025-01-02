# Práctica 2: Creación de un Plugin Personalizado con Terraform

## Objetivo
Aprender a crear un plugin personalizado sencillo en Terraform que permita crear un bucket S3 y subir código HTML a él.

---

## Introducción
Terraform permite la extensión de sus capacidades mediante plugins personalizados. Estos plugins se construyen generalmente en Go y pueden interactuar con APIs de terceros o implementar lógicas específicas.

En esta práctica, crearás un plugin que creará un bucket en Amazon S3 y subirá un archivo HTML al bucket.

---

## Requisitos Previos
- Tener instalado:
  - [Terraform](https://www.terraform.io/downloads)
  - [Go](https://golang.org/dl/)
  - Credenciales de AWS configuradas en tu sistema.
- Conocimiento básico de Terraform y Go.

---

## Pasos

### 1. Configurar el Entorno de Desarrollo

1. **Crear un directorio de proyecto:**
   ```bash
   mkdir terraform-provider-s3html
   cd terraform-provider-s3html
   ```

2. **Inicializar un módulo Go:**
   ```bash
   go mod init terraform-provider-s3html
   ```

3. **Configurar la estructura de directorios:**
   ```bash
   mkdir -p s3html
   touch main.go s3html/resource_s3html.go
   ```

---

### 2. Implementar el Plugin

#### **Archivo `main.go`**

Este archivo registra el plugin y su proveedor.

```go
package main

import (
	"github.com/hashicorp/terraform-plugin-sdk/v2/plugin"
	"terraform-provider-s3html/s3html"
)

func main() {
	plugin.Serve(&plugin.ServeOpts{
		ProviderFunc: s3html.Provider,
	})
}
```

#### **Archivo `s3html/resource_s3html.go`**

Este archivo define el proveedor y el recurso `s3html` que crea un bucket S3 y sube un archivo HTML.

```go
package s3html

import (
	"context"
	"fmt"
	"os"

	"github.com/aws/aws-sdk-go/aws"
	"github.com/aws/aws-sdk-go/aws/session"
	"github.com/aws/aws-sdk-go/service/s3"
	"github.com/hashicorp/terraform-plugin-sdk/v2/diag"
	"github.com/hashicorp/terraform-plugin-sdk/v2/helper/schema"
)

func Provider() *schema.Provider {
	return &schema.Provider{
		ResourcesMap: map[string]*schema.Resource{
			"s3html_bucket": resourceS3HTML(),
		},
	}
}

func resourceS3HTML() *schema.Resource {
	return &schema.Resource{
		CreateContext: resourceS3HTMLCreate,
		ReadContext:   resourceS3HTMLRead,
		DeleteContext: resourceS3HTMLDelete,

		Schema: map[string]*schema.Schema{
			"bucket_name": {
				Type:     schema.TypeString,
				Required: true,
			},
			"html_file_path": {
				Type:     schema.TypeString,
				Required: true,
			},
		},
	}
}

func resourceS3HTMLCreate(ctx context.Context, d *schema.ResourceData, meta interface{}) diag.Diagnostics {
	bucketName := d.Get("bucket_name").(string)
	htmlFilePath := d.Get("html_file_path").(string)

	sess, err := session.NewSession(&aws.Config{Region: aws.String("us-east-1")})
	if err != nil {
		return diag.FromErr(err)
	}

	svc := s3.New(sess)

	// Crear el bucket
	_, err = svc.CreateBucket(&s3.CreateBucketInput{
		Bucket: aws.String(bucketName),
	})
	if err != nil {
		return diag.FromErr(err)
	}

	// Subir el archivo HTML
	htmlFile, err := os.Open(htmlFilePath)
	if err != nil {
		return diag.FromErr(err)
	}
	defer htmlFile.Close()

	_, err = svc.PutObject(&s3.PutObjectInput{
		Bucket: aws.String(bucketName),
		Key:    aws.String("index.html"),
		Body:   htmlFile,
	})
	if err != nil {
		return diag.FromErr(err)
	}

	d.SetId(bucketName)
	return nil
}

func resourceS3HTMLRead(ctx context.Context, d *schema.ResourceData, meta interface{}) diag.Diagnostics {
	return nil
}

func resourceS3HTMLDelete(ctx context.Context, d *schema.ResourceData, meta interface{}) diag.Diagnostics {
	bucketName := d.Id()

	sess, err := session.NewSession(&aws.Config{Region: aws.String("us-east-1")})
	if err != nil {
		return diag.FromErr(err)
	}

	svc := s3.New(sess)

	// Eliminar el bucket
	_, err = svc.DeleteBucket(&s3.DeleteBucketInput{
		Bucket: aws.String(bucketName),
	})
	if err != nil {
		return diag.FromErr(err)
	}

	d.SetId("")
	return nil
}
```

---

### 3. Compilar el Plugin

1. **Compilar el binario:**
   ```bash
   go build -o terraform-provider-s3html
   ```

2. **Mover el binario al directorio de plugins de Terraform:**
   ```bash
   mkdir -p ~/.terraform.d/plugins/example.com/user/s3html/1.0.0/
   mv terraform-provider-s3html ~/.terraform.d/plugins/example.com/user/s3html/1.0.0/
   ```

---

### 4. Usar el Plugin en un Proyecto Terraform

1. **Crear un archivo de configuración Terraform:**
   ```hcl
   terraform {
     required_providers {
       s3html = {
         source  = "example.com/user/s3html"
         version = "1.0.0"
       }
     }
   }

   provider "s3html" {}

   resource "s3html_bucket" "example" {
     bucket_name    = "mi-bucket-ejemplo"
     html_file_path = "./index.html"
   }
   ```

2. **Ejecutar el proyecto:**
   ```bash
   terraform init
   terraform apply
   ```

---

## Recursos Adicionales
- [SDK de Terraform](https://github.com/hashicorp/terraform-plugin-sdk)
- [Guía para construir plugins personalizados](https://learn.hashicorp.com/collections/terraform/providers)
- [AWS SDK para Go](https://aws.github.io/aws-sdk-go-v2/)

---

¡Felicitaciones! Has creado y utilizado un plugin personalizado en Terraform para gestionar un bucket S3 y subir un archivo HTML.

