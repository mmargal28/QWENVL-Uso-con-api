# Clave API en Alibaba Cloud

Para utilizar una clave API en una plataforma de inteligencia artificial, primero debemos generar y configurar nuestra clave en un proveedor adecuado. En este caso, utilizaremos **Alibaba Cloud**. Sin embargo, este proceso es similar en cualquier otra plataforma que ofrezca acceso mediante API.

## 1. Creación de cuenta

Para obtener una clave API en **Alibaba Cloud**, primero debemos crear una cuenta en su plataforma:

1. Accede a [Alibaba Cloud](https://www.alibabacloud.com/).
2. Regístrate con tu correo electrónico o inicia sesión si ya tienes una cuenta.
3. Completa la verificación de identidad si es necesario.

## 2. Obtencion de la clave API

### Generación de la clave API

Para generar una clave API en **Alibaba Cloud**:

1. Ve a **alibaba model studio**.
2. Accede a la sección **View api keys**.
3. Crea una nueva **Access Key** y copia los valores generados (Access Key ID y Access Key Secret).

### Uso seguro de la clave API

Es **recomendable** no incluir la clave API directamente en el código. En su lugar, podemos utilizar métodos más seguros, como:

#### Variables de entorno

Guarda la clave API en una variable de entorno para evitar exponerla en el código:

**En Windows (PowerShell):**
```powershell
$env:ALIBABA_API_KEY = "tu_clave_api"
```
**En Linux:**
```bash
export ALIBABA_API_KEY="tu_clave_api"
```

#### Lectura de un archivo python
Guardamos la clave en un archivo json
```json
{
  "api_key": "tu_clave_api"
}
```
En el codigo de python correspondiente:
```python
import json

with open("config.json") as config_file:
    config = json.load(config_file)
    api_key = config["api_key"]

#print("Clave API cargada:", api_key)

```
