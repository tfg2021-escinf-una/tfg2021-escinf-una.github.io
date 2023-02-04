---
layout: default
title: Notification Microservice Setup
permalink: /Tutorial/Microservicios/Notification/Setup
---
[Regresar]({% link src/pages/main.md %})
# Setup

En el siguiente link encontrará el repositorio creado para este ejemplo práctico: https://github.com/tfg2021-escinf-una/covid-info/ . En caso de tener alguna duda con el código, puede revisar directamente en el repositorio, o bien, clonarlo y modificarlo.

1. Abrir el repositorio `microservices-tutorial/notification-microservice` en el editor de texto

2. En este momento la lista de archivos en el proyecto es la siguiente:
```
microservices-tutorial
│
└─── notification-microservice
│   │   README.md
│   │   .gitignore
```


3. Para inicializar el proyecto de Flask es necesario crear un archivo de texto llamado `requirements.txt` donde se manejan las dependencias 

En este archivo se pueden ver todas las dependencias que se instalan para el proyecto. Un ejemplo del archivo requirements.txt es el siguiente: [requirements.txt](https://github.com/tfg2021-escinf-una/eg.notification/blob/development/requirements.txt)

4. Ejecute el siguiente comando para instalar las dependencias listadas en el archivo `requirements.txt`
```
pip install -r requirements.txt
```

5. Cree un archivo llamado `serve.sh`. Este debe ser ejecutado mediante una consola para levantar el servidor. 
```
#!/bin/bash
cd app
export MJ_APIKEY_PUBLIC="YOUR PUBLIC API_KEY HERE"
export MJ_APIKEY_PRIVATE="YOUR PRIVATE SECRET HERE"
export EMAIL_SENDER="YOUR AUTHORIZED EMAIL ON MAILJET HERE"
export FLASK_APP="app"
flask run
```

6. Cree un folder llamado app y adentro un archivo app.py y copie el contenido del siguiente [link: app.py](https://github.com/tfg2021-escinf-una/eg.notification/blob/development/app/app.py).

### Análisis del código copiado

```
from flask import Flask, request
from flask_cors import CORS
from mailjet_rest import Client
import os
```

- Se realiza el import de las dependencias para ejecutar el microservicio. Las más importantes se explican a continuación: 
    - ```Flask``` Framework para la creación de APIs
    - ```os``` permite la utilización de features del Sistema Operativo.
    - ```request``` se utiliza para hacer llamados HTTP.
    - ```Client``` libreria que nos permite realizar conexion con MailJet para lograr enviar los correos electrónicos

```
app = Flask(__name__)
CORS(app)
```
- Variables creadas para inicializar el framework 

```
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
sender_address = os.environ['EMAIL_SENDER']
mailjet = Client(auth=(api_key, api_secret), version='v3.1')
```
- Son variables de ambiente que se utilizarán para efectuar la conexion con Mail Jet y enviar el correo.

```
@app.route("/notify", methods=['POST'])
def notification():
    args = request.get_json()
    email_to_send = args["address"]
    subject = args["subject"]
    message = args["message"]
    data = {
        'Messages': [
            { "From": {	"Email": sender_address, "Name": "TFG - Sitio Didáctico" },
              "To": [{ "Email": email_to_send, "Name": "You" }],
              "Subject": subject,
              "TextPart": "Greetings from Mailjet!",
              "HTMLPart": f"<h4>{message}</h4><br />Este es un mensaje autogenerado por nuestro sitio utilizando MailJet."
            }
        ]
    }
    result = mailjet.send.create(data=data)
    return result.json()

if __name__ == '__main__':
   app.run(port=5000, debug = True)

```

- Se define una ruta de tipo `POST` utilizando el decorador `@app.route`
- Se define el método `notification`, este será ejecutado cada vez que la ruta sea llamada.
- Se obtienen los valores que provienen como parte del cuerpo de la peticion `POST`
- Se crea el objeto data. Este contiene la información que se requiere para enviar un correo utilizando `mailjet`
- Finalmente se envia el correo.


7. Ejecutar el servidor
```
chmod +x <fileName> -- Esto le dara permisos de ejecución en caso de no tenerlos
./serve.sh
```

Una vez realizados todos estos pasos, el servidor está listo para recibir peticiones HTTP en el puerto 5000

[Anterior]({% link src/pages/tutorial/microservicios/notification-microservice/main.md %}){: .btn-previous}[Siguiente]({% link src/pages/tutorial/microservicios/main.md %}){: .btn-next}
