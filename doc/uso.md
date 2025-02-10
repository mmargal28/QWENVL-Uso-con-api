## Codigo python el cual ejecutaremos 
```python
import os
import base64
import requests  # Importa las bibliotecas necesarias para manejar archivos, codificación y solicitudes HTTP

# Función para codificar una imagen en formato Base64
def encode_image(image_path):
    with open(image_path, "rb") as image_file:  # Abre la imagen en modo binario
        return base64.b64encode(image_file.read()).decode("utf-8")  # Codifica la imagen en Base64 y la convierte en cadena de texto

# Codifica la imagen "test.png" en Base64
base64_image = encode_image("test.png")

# Clave de API para autenticación (se recomienda almacenarla de forma segura, no en el código)
api_key = "tu-api-key"

# Definición de los encabezados HTTP para la solicitud
headers = {
    "Content-Type": "application/json",  # Especifica que se enviará un JSON
    "Authorization": f"Bearer {api_key}",  # Autenticación con la clave de API
}

# Construcción del payload para la solicitud a la API
payload = {
    "model": "qwen-vl-max",  # Modelo de IA que se usará para procesar la imagen y extraer el texto
    "messages": [
        {
            "role": "user",  # Define el rol del usuario en la conversación con la IA
            "content": [
                {
                    "type": "image_url",
                    "image_url": {"url": f"data:image/png;base64,{base64_image}"},  # Inserta la imagen codificada en Base64
                },
                {
                    "type": "text",
                    "text": "Returns the full transcript, verbatim, respecting typography, bold, italics, etc. "
                            "Show typographical embellishments. Use Temperature 0. Don't ask if you want to continue, "
                            "full transcript from the first line of the first page to the last line of the last page. "
                            "Include header and footer. Don't add extra messages. If the word is cut between lines, show it together, no hyphen.",
                },  # Mensaje con instrucciones precisas para obtener una transcripción completa
            ],
        }
    ],
    "temperature": 0,  # Controla la aleatoriedad en la respuesta (0 significa respuestas determinísticas)
    "top_p": 0.01      # Controla la diversidad del texto generado limitando las opciones con mayor probabilidad
}

# Envía la solicitud POST a la API de DashScope
response = requests.post(
    "https://dashscope-intl.aliyuncs.com/compatible-mode/v1/chat/completions",
    headers=headers,
    json=payload,
)

# Extrae e imprime el contenido de la respuesta JSON
print(response.json()["choices"][0]["message"]["content"])  # Imprime solo la transcripción extraída
print(response.json())  # Imprime la respuesta completa de la API (útil para depuración)
```
# Explicacion.
Con este codigo conseguiremos que Qwen nos transcriba una imagen de un texto cualquiera.
