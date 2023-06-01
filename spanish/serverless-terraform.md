# Creando un API Rest con Infra como código (Terraform) & Serverless (Lambda + Python) - Parte 1

## Desplegar un API en minutos en vez de días

Desplegar en producción es cada día más sencillo. Mejores prácticas y herramientas están a nuestra disposición para crear software que eventualmente contribuye a la automatización de tareas dispendiosas, la creación de nuevos negocios y startups, y en última instancia, generar valor para la sociedad 🌎.

Como ingeniero de software, considero firmemente que el código libre (OSS) nos brinda una gran cantidad de beneficios, entre ellos una comunidad orientada a la excelencia y a compartir conocimiento. Esta es una de las razones por las cuales intento publicar el código de lo que he ido creando y apalancarme en herramientas de código abierto como **Terraform** para desplegar la infraestructura.

Una idea clave para mí, y por la cual me uní a la comunidad de AWS, es siempre **estudiar, construir y compartir** para crear un efecto multiplicador con más y mejores ingenieros en una carrera donde la creatividad y la cooperación son pilares fundamentales.

> En una segunda entrega, explicaré y llevaré el código a un nivel más avanzado con: **Despliegue Continuo** en CircleCI, **Lambda Layers** y **Buenas prácticas** o pasos a seguir al momento de publicar un API en producción (Autenticación, Rate-Limiting, etc).


## Anti-Patrones de despliegue 🙅🏻‍♂️

- Infraestructura creada manualmente
- Desplegar en horarios específicos por falta de capacidad técnica (errores en producción, software inestable, indisponibilidad del servicio) y no por decisiones del negocio
- Tener que enviar correos a otro equipo para desplegar
- Ramas como "development" que difieren de lo que existe en "main" y que a veces generan conflictos en la fusión (merge)
- Pruebas manuales
- Tickets para desplegar
- SOAP como única alternativa

Todos los puntos mencionados anteriormente, y una cantidad excesiva de burocracia, comités y reuniones, todavía son la norma en muchas organizaciones y fue mi realidad durante muchos años. Pero existe una alternativa, una forma de ver el proceso de desarrollo de software donde la simplicidad es fundamental, tanto en los despliegues con pipelines y automatización, como en la creación de software, donde el enfoque debería ser reducir la complejidad accidental, aplicaciones modulares, testeables y en resumen, fáciles de entender para el equipo.

## Un futuro más brillante 💻

- Pruebas unitarias y automatización
- Desarrollo basado en tronco (Trunk based development)
- IA, ChatGPT, Copilot
- DevOps como mentalidad y no como un rol
- Infraestructura como código
- Serverless y contenedores
- REST, SOAP, GraphQL, gRPC

Este post tiene como propósito mostrar una parte esencial de la entrega de software con Terraform y Serverless.

## ¡Hablemos del código!

**Código en GitHub:** [serverless-terraform](https://github.com/jorgetovar/serverless-terraform)

### Estructura del proyecto 🏭

![Project Structure](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/80bk086lppmisj8rysm0.png)

Los siguientes archivos son típicos en un proyecto de Terraform, y es la convención que estamos usando en éste proyecto.

- *main.tf* - Entry point for terraform
- *variables.tf* - Input variables
- *terraform.tfvars* - Variables definition file
- *providers.tf* - Providers declarations
- *outputs.tf* - Output values
- *versions.tf* - Provider version locking 

### Por qué Terraform ☁️

- Open source
- Multi-nube
- Infraestructura inmutable
- Paradigma declarativo
- DSL (Lenguaje Específico del Dominio)
- No requiere agentes ni nodos maestros
- Gratuito
- Con una comunidad amplia
- Software maduro

### Por qué Serverless y Python 🐍

- Escalabilidad automática
- Pago por uso
- Alta disponibilidad
- Facilidad de desarrollo y despliegue
- Integración con servicios gestionados

### Código

**Lambda:** El código de una lambda en general debería ser muy simple y responder a única responsabilidad o razón para cambiar, el nombre del método debe corresponder al punto de entrada definido en el código de Terraform.

```Python
import json

import requests

def lambda_handler(event, context):

    response = requests.get("https://test-api.k6.io/public/crocodiles/")
    if response.status_code == 200:
        data = response.json()
        random_info = data
    else:
        random_info = "No data!"

    return {
        "statusCode": 200,
        "body": json.dumps({
            "message": "hello world - We are going to create a DevOps as a service powered by IA",
            "random_info": random_info

        }),
    }
```

**.gitignore:** Definición de los archivos que no queremos agregar al repositorio, es clave no publicar información de las credenciales de AWS.

```terraform
# Terraform-specific files
.terraform/
terraform.tfstate
terraform.tfstate.backup
*.tfvars


hello_world.zip
response.json
.layer
layer.zip

# AWS credentials file
.aws/credentials
```

**Provider:** El primer paso es usualmente definir el provider donde usualmente especificamos la nube y región del despliegue

```terraform
provider "aws" {
  region = var.aws_region

  default_tags {
    tags = {
      created-by = "terraform"
    }
  }
}
```

**Recursos de la Lambda:** Configura la lambda para obtener el código fuente de un bucket creado también por Terraform, y definimos `lambda_handler`como el método que recibirá y procesará el evento del API.

```terraform
resource "aws_lambda_function" "hello_world" {
  function_name    = "HelloCommunityBuilders"
  s3_bucket        = aws_s3_bucket.lambda_bucket.id
  s3_key           = aws_s3_object.lambda_hello_world.key
  runtime          = "python3.9"
  handler          = "app.lambda_handler"
  source_code_hash = data.archive_file.lambda_hello_world.output_base64sha256
  role             = aws_iam_role.lambda_exec.arn
  layers           = [aws_lambda_layer_version.lambda_layer.arn]
}
```

**Permisos:** Define los permisos requeridos para ejecutar la lambda y acceder a servicios de AWS

```terraform
resource "aws_iam_role" "lambda_exec" {
  name = "serverless_lambda"

  assume_role_policy = jsonencode({
    Version   = "2012-10-17"
    Statement = [
      {
        Action    = "sts:AssumeRole"
        Effect    = "Allow"
        Sid       = ""
        Principal = {
          Service = "lambda.amazonaws.com"
        }
      }
    ]
  })
}

resource "aws_iam_role_policy_attachment" "lambda_policy" {
  role       = aws_iam_role.lambda_exec.name
  policy_arn = "arn:aws:iam::aws:policy/service-role/AWSLambdaBasicExecutionRole"
}

```

**Definición del API:** Especificamos el verbo y nombre del recurso según las convenciones de un API Rest.

```terraform
resource "aws_apigatewayv2_integration" "hello_world" {
  api_id             = aws_apigatewayv2_api.lambda.id
  integration_uri    = aws_lambda_function.hello_world.invoke_arn
  integration_type   = "AWS_PROXY"
  integration_method = "POST"
}

resource "aws_apigatewayv2_route" "hello_world" {
  api_id    = aws_apigatewayv2_api.lambda.id
  route_key = "GET /hello"
  target    = "integrations/${aws_apigatewayv2_integration.hello_world.id}"
}
```

### Terraform Despliegue

Desplegar el API debería ser tan simple como clonar el código y ejecutar un par de comandos de Terraform.

El comando `terraform init` se utiliza para inicializar un proyecto en un directorio de trabajo. A través de este comando se descargan el código de los providers, y sé configura el backend, que es donde se almacenan los estados y otros datos de claves.

```sh
terraform init
```

El comando `terraform apply` se utiliza para aplicar los cambios definidos en tu configuración de infraestructura y crear o modificar los recursos correspondientes en tu proveedor de infraestructura.

```sh
terraform apply
```

Finalmente puedes obtener la información de los *Outputs* y ejecutar el llamado al API.

```sh
http "$(terraform output -raw base_url)/hello"
```

![Http API Call](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/zep5h1akb7o9t5hryf6q.png)

## Conclusión 🤔

En este artículo, hemos explorado la combinación de Terraform y Serverless para crear una API en minutos.

Finalmente, al aprovechar las mejores prácticas de CI/CD, la infraestructura como código y las pruebas automatizadas, podemos lograr implementaciones más rápidas y confiables, al tiempo que mantenemos un código de alta calidad.

No hace muchos años, desplegar una API en producción era una tarea tediosa y que requería de muchos actores. Hoy en día, contamos con una gran cantidad de software de código abierto y herramientas enfocadas en la experiencia del desarrollador, la inmutabilidad, la automatización, la inteligencia artificial para aumentar la productividad y los IDE poderosos. En resumen, no hay un mejor momento para ser ingeniero y generar valor en la sociedad a través del software.

Es importante recalcar que los mayores problemas que existen en el proceso de desarrollo y entrega de software son de organización y personas. Al final, construir aplicaciones es una actividad social, pero cuanto más resolvamos los problemas técnicos y mejor sea la comunicación, más fácil debería ser llegar a producción. ¡Feliz codificación! 🎉

- [LinkedIn](https://www.linkedin.com/in/%F0%9F%91%A8%E2%80%8D%F0%9F%8F%AB-jorge-tovar-71847669/)
- [Twitter](https://twitter.com/jorgetovar621)
- [GitHub](https://github.com/jorgetovar)

Si te gustaron los artículos, visita mi blog [jorgetovar.dev](jorgetovar.dev)




