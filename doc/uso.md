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
## Ejemplo del output
{'choices': [{'message': {'content': '4\n\nEn la calle de la Cruz de la Madera, num. 53; sigue en venta de la afamada legítima carne y lenguas ahumadas de Hamburgo. Quien quisiere comprar una partida de ébano carbón negro del superior, acudirá a la calle del Rosario, núm. 103, tienda de pintor, preguntando por D. José Berri.\n\nEn el antiguo puesto de libros y papeles, plaza de S. Antonio, num. 6, se darán razón de sirvientes de ambos sexos, nodrizas, costureras, planchadoras, bordadoras, cocineras, mayordomos, ayuda de cámara y casa de pupilage con asistencia o sin ella, todo con la mayor exactitud por estar ejerciéndolo algunos años y tener los conocimientos necesarios tanto con los amos como con los criados.\n\nUn matrimonio que sabe de toda clase de servicio, desea acomodarse para el de alguna familia residente en esta plaza o que tenga que viajar para cualquier parte; darán razón en la lonja de la calle de la Carneería del Rey.\n\nTEATRO DE SAN FERNANDO.—A beneficio de José Castellanos y Vicente Santa María.—El castigo del Penseque (comedia en tres actos, del célebre maestro Tirso de Molina).—Las seguidillas madrileñas, cantadas y bailadas por los Sres. Lazaro Quintana, José Lopez y Antonio Romero.—El triunfo de las mugeres (tonadilla).—Seguidillas murcianas.—Los tres novios imperfectos, sordo, tartamudo y tuerto (sainete, en el que el gracioso cantará un aria al arpa).—A las 4½.\n\nTEATRO PRINCIPAL.—A beneficio del Sr. Estevan Ferrero se ejecutará la función siguiente: la sinfonía de la Represaña; una cabatina de la misma ópera por el Sr. Vaccani; un dúo por la Sra. Pedroti y el Sr. Cartagenova; una cabatina de la Caritea por el Sr. Pasini; el Sr. Daelli tocará un solo de oboe; una cabatina de la Caritea por la Sra. Fornacciari; escena y aria de la Ipermenestra por el Sr. Cartagenova; la Sra. Adelaida Ferrero bailará un solo serio; y la ópera jocosa nueva en un acto D. Quixote en las bodas de Camacho compuesta por el Sr. Ferrero y la música del maestro Mercadante.—A las 6½.\n\nEl argumento de dicha ópera, en español e italiano, se halla de venta en el despacho de boletines a 4 rva.\n\nNo siendo esta representación una de las comprendidas en el abono, se previene a los Sres. abonados que se les conservarán sus localidades hasta las dos de la tarde de hoy.\n\nCON REAL PERMISO: En la imprenta Gaditana, plazuela del Palillero, número 111.', 'role': 'assistant'}, 'finish_reason': 'stop', 'index': 0, 'logprobs': None}], 'object': 'chat.completion', 'usage': {'prompt_tokens': 1338, 'completion_tokens': 717, 'total_tokens': 2055}, 'created': 1739192014, 'system_fingerprint': None, 'model': 'qwen-vl-max', 'id': 'chatcmpl-bdfe5806-8945-91e6-88ef-1aea35d15337'
