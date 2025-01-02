# Práctica 2: Creación de un Plugin Personalizado con Terraform

## Objetivo
Aprender a crear un plugin personalizado sencillo en Terraform para extender su funcionalidad y adaptarla a tus necesidades específicas.

---

## Introducción
Terraform permite la extensión de sus capacidades mediante plugins personalizados. Estos plugins se construyen generalmente en Go y pueden interactuar con APIs de terceros o implementar lógicas específicas.

En esta práctica, crearás un plugin que simula la creación de un recurso ficticio llamado `example_service`.

---

## Requisitos Previos
- Tener instalado:
  - [Terraform](https://www.terraform.io/downloads)
  - [Go](https://golang.org/dl/)
- Conocimiento básico de Terraform y Go.

---

## Pasos

### 1. Configurar el Entorno de Desarrollo

1. **Crear un directorio de proyecto:**
   ```bash
   mkdir terraform-provider-example
   cd terraform-provider-example
   ```

2. **Inicializar un módulo Go:**
   ```bash
   go mod init terraform-provider-example
   ```

3. **Configurar la estructura de directorios:**
   ```bash
   mkdir -p example
   touch main.go example/resource_example.go
   ```

---

### 2. Implementar el Plugin

#### **Archivo `main.go`**

Este archivo registra el plugin y su proveedor.

```go
package main

import (
	"github.com/hashicorp/terraform-plugin-sdk/v2/plugin"
	"terraform-provider-example/example"
)

func main() {
	plugin.Serve(&plugin.ServeOpts{
		ProviderFunc: example.Provider,
	})
}
```

#### **Archivo `example/resource_example.go`**

Este archivo define el proveedor y el recurso `example_service`.

```go
package example

import (
	"context"

	"github.com/hashicorp/terraform-plugin-sdk/v2/diag"
	"github.com/hashicorp/terraform-plugin-sdk/v2/helper/schema"
)

func Provider() *schema.Provider {
	return &schema.Provider{
		ResourcesMap: map[string]*schema.Resource{
			"example_service": resourceExampleService(),
		},
	}
}

func resourceExampleService() *schema.Resource {
	return &schema.Resource{
		CreateContext: resourceExampleServiceCreate,
		ReadContext:   resourceExampleServiceRead,
		DeleteContext: resourceExampleServiceDelete,

		Schema: map[string]*schema.Schema{
			"name": {
				Type:     schema.TypeString,
				Required: true,
			},
		},
	}
}

func resourceExampleServiceCreate(ctx context.Context, d *schema.ResourceData, meta interface{}) diag.Diagnostics {
	d.SetId("example-id")
	return nil
}

func resourceExampleServiceRead(ctx context.Context, d *schema.ResourceData, meta interface{}) diag.Diagnostics {
	return nil
}

func resourceExampleServiceDelete(ctx context.Context, d *schema.ResourceData, meta interface{}) diag.Diagnostics {
	d.SetId("")
	return nil
}
```

---

### 3. Compilar el Plugin

1. **Compilar el binario:**
   ```bash
   go build -o terraform-provider-example
   ```

2. **Mover el binario al directorio de plugins de Terraform:**
   ```bash
   mkdir -p ~/.terraform.d/plugins/example.com/user/example/1.0.0/
   mv terraform-provider-example ~/.terraform.d/plugins/example.com/user/example/1.0.0/
   ```

---

### 4. Usar el Plugin en un Proyecto Terraform

1. **Crear un archivo de configuración Terraform:**
   ```hcl
   terraform {
     required_providers {
       example = {
         source  = "example.com/user/example"
         version = "1.0.0"
       }
     }
   }

   provider "example" {}

   resource "example_service" "example" {
     name = "mi-recurso-ejemplo"
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

---

¡Felicitaciones! Has creado y utilizado tu primer plugin personalizado en Terraform.

