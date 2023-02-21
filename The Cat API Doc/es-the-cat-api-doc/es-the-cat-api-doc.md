# La-Documentación-de-la-API-de-Cat

>
> La API de Cat es una API pública para **gestión de información** e **imágenes cat**. 🐈

Con la API de Cat, puede:

1. **Insertar** imágenes;
2. **Buscar** imágenes por ID;
3. **Eliminar** imágenes.

⚠️ _Se rechazan las imágenes que no sean de gato o inapropiadas_. El objeto 'images' representa las fotos de gatos enviadas.

# Prerrequisitos

- Regístrese en [https://thecatapi.com/signup](https://thecatapi.com/signup) para recibir su clave API por correo electrónico.
- Introduzca la clave API en el encabezado de las llamadas utilizando la variable 'x-api-key'.
- Utilice la ruta [https://api.thecatapi.com/v1](https://api.thecatapi.com/v1) en https://api.thecatapi.com/v1.

# Endpoints

>
> Consulte cada punto de enlace, sus formularios de solicitud y ejemplos de _responses_.

## POST

🐱 **`POST / Images / Upload`** `https://api.thecatapi.com/v1/images/upload`

Este endpoint **inserta** una nueva imagen en el sistema cargando un archivo de imagen cat **válido** de tipo:

- .gif
- .jpg
- .png

**Request body**

| Nome | Descripción | Tipo | Obligatorio |  
|------|-----------|------|-------------|
| `file` | Archivo en .gif, .png o .jpg | `file` | Sí |
| `sub_id` | ID para identificación interno. | `string` | No |


### **Ejemplo de solicitud:**

```
curl --location --request POST 'https://api.thecatapi.com/v1/images/upload' \
--header 'x-api-key: live_g6EUZSGbkMsKSuQm1OyWDVeLrLSnoCMcps2f7BMcDq6Alt2Y9Z606aj1uF6sPF35' \
--form 'file=@"iQzJdW8Ds/Mia.jpg"'
```

![Mia](https://user-images.githubusercontent.com/105396649/207432035-c638a387-f243-4983-8af5-fcf7e0c94011.jpg)

😻 La respuesta de código '201 Created' indica que el registro de imagen se creó correctamente. Devuelve un JSON que incluye el nuevo ID.

### **Ejemplo de _response body_:**

``` json
{
    "id": "RyOzgrhxk",    
    "url": "https://cdn2.thecatapi.com/images/RyOzgrhxk.jpg",   
    "width": 1047,    
    "height": 647,    
    "original_filename": "Mia.jpg",    
    "pending": 0,    
    "approved": 1  
}
```

### **Respuestas de error:**

-  `400 Bad request` Invalid file data. Check you are sending the formdata.append('file', ...} format'.

-  `400 Bad request` Classifcation failed: correct animal found.

-  `401 Unauthorized` Unauthorized.

-  `500 Internal Server Error` Internal Server Error. 

## GET by ID

🐱 **`GET /images/{image_id}`** `https://api.thecatapi.com/v1/images/{{image_id}}`

Este endpoint **recupera** la imagen correspondiente a **_path param_** 'image_id'.

### **Ejemplo de solicitud:**

```
curl --location --request GET 'https://api.thecatapi.com/v1/images/6qmirugX0' \
--header 'x-api-key: live_g6EUZSGbkMsKSuQm1OyWDVeLrLSnoCMcps2f7BMcDq6Alt2Y9Z606aj1uF6sPF35'
```

![Mia2](https://user-images.githubusercontent.com/105396649/207641262-48edba35-f423-4835-84ce-7f9109237485.jpg)

😻 La respuesta de código '200 OK' indica que la consulta se ejecutó correctamente. Devuelve un JSON con toda la información de la imagen.

### **Ejemplo de _reponse body_:**

``` json

{
    "id": "6qmirugX0",
    "url": "https://cdn2.thecatapi.com/images/6qmirugX0.jpg",
    "width": 1047,
    "height": 647,
    "sub_id": null
}

```

### **Respuesta de error:**

-  `400 Bad request`Couldn't find an image matching the passed 'id' of xxxxx. Si el image_id insertado era incorrecto. 

## GET your uploaded images 

🐱 **`GET /images`** `https://api.thecatapi.com/v1/images`

Este endpoint **recupera** todas las imágenes cargadas en su cuenta a través de '/images/upload'.

Filtre los resultados utilizando los siguientes parámetros de 'query':

| Parámetro |    Descripción      | Tipo | Obligatorio |
|------------|--------------------|------|------------|
| `limit`  | Número de resultados a devolver. El valor máximo es 25. El valor predeterminado es 1. | `integer` | Sí |
| `mime_types` | Los tipos de imágenes que se devolverán: .gif, .jpg o .png. Devuelve todos los tipos como predeterminados. | `string` delimitada por comas. | No |
| `order` | Orden de devolución: RANDOM, ASC o DESC. El valor predeterminado es RANDOM. | `string` | No |

### **Ejemplo de _request_ con los parámetros 'limit' 25 y 'order' ASC:**

``` 
curl --location --request GET 'https://api.thecatapi.com/v1/images?limit=25&order=ASC' \
--header 'x-api-key: live_g6EUZSGbkMsKSuQm1OyWDVeLrLSnoCMcps2f7BMcDq6Alt2Y9Z606aj1uF6sPF35'
```

### **Ejemplo de _response body_:**

``` json
[
    {
        "breeds": [],
        "id": "RyOzgrhxk",
        "url": "https://cdn2.thecatapi.com/images/RyOzgrhxk.jpg",
        "width": 1047,
        "height": 647,
        "sub_id": null,
        "created_at": "2022-12-13T16:43:59.000Z",
        "original_filename": "Mia.jpg",
        "breed_ids": null
    },
    {
        "breeds": [],
        "id": "6qmirugX0",
        "url": "https://cdn2.thecatapi.com/images/6qmirugX0.jpg",
        "width": 1047,
        "height": 647,
        "sub_id": null,
        "created_at": "2022-12-14T13:13:49.000Z",
        "original_filename": "081E825A-1D2D-49C4-A659-818CC053E421.jpeg",
        "breed_ids": null
    }
]

```

## DELETE a specific image 

🐱 **`DELETE /images/{image_id}`** `https://api.thecatapi.com/v1/images/{{image_id}}`

Este endpoint **elimina** la imagen correspondiente al parámetro 'image_id' pasado como parámetro 'path'.

### **Ejemplo de _request_:**

``` 
curl --location --request DELETE 'https://api.thecatapi.com/v1/images/FBqMvFgx5' \
--header 'x-api-key: live_g6EUZSGbkMsKSuQm1OyWDVeLrLSnoCMcps2f7BMcDq6Alt2Y9Z606aj1uF6sPF35'
``` 

![Mia 3](https://user-images.githubusercontent.com/105396649/207685891-74ba01fd-4ff1-4953-a095-41d839b3c02c.jpg)


😻 La respuesta de código '204 No Content' indica que la eliminación se realizó correctamente. Devuelve un JSON vacío.


### **Respuesta de error:**

-  `400 Bad request` INVALID_DATA. Si el 'image_id' se introduce incorrectamente.
