# The-Cat-API-DOC

>
> The Cat API é uma API pública de **gerenciamento de informaçoes** e **imagens de gatos**. 🐈

Com a The Cat API, é possível: 

1. **Inserir** imagens;
2. **Buscar** imagens por ID;
3. **Deletar** imagens.

⚠️ _Imagens que não forem de gatos ou inapropriadas são rejeitadas_. O objeto `images` representa as fotos de gatos enviadas.

# Pré-requisitos

- Faça o registro em [https://thecatapi.com/signup](https://thecatapi.com/signup) para receber sua API key por email.
- Informe a API key no header das chamadas através da variável `x-api-key`.
- Utilize nas chamadas o path [https://api.thecatapi.com/v1](https://api.thecatapi.com/v1).

# Endpoints

>
> Confira abaixo cada endpoint, suas formas de requisição e exemplos de _respostas_. 

## POST

🐱 **`POST / Images / Upload`** `https://api.thecatapi.com/v1/images/upload`

Este endpoint **insere** uma nova imagem no sistema carregando um arquivo **válido** de imagem de gato, do tipo:

- .gif
- .jpg
- .png

**Request body**

| Nome | Descrição | Tipo | Obrigatório |
|------|-----------|------|-------------|
| `file` | Arquivo em .gif, .png, ou .jpg | `file` | Sim |
| `sub_id` | ID para identificação interna. | `string` | Não |


### **Exemplo de requisição:** 

```
curl --location --request POST 'https://api.thecatapi.com/v1/images/upload' \
--header 'x-api-key: live_g6EUZSGbkMsKSuQm1OyWDVeLrLSnoCMcps2f7BMcDq6Alt2Y9Z606aj1uF6sPF35' \
--form 'file=@"iQzJdW8Ds/Mia.jpg"'
```

![Mia](https://user-images.githubusercontent.com/105396649/207432035-c638a387-f243-4983-8af5-fcf7e0c94011.jpg)

😻 A resposta de código `201 Created` indica que o registro da imagem foi criado com sucesso. Retorna um JSON incluindo o novo ID.

### **Exemplo de _response body_:**

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

### **Respostas de erro:**

-  `400 Bad request` Invalid file data. Check you are sending the formdata.append('file', ...} format'.

-  `400 Bad request` Classifcation failed: correct animal found.

-  `401 Unauthorized` Unauthorized.

-  `500 Internal Server Error` Internal Server Error. 

## GET by ID

🐱 **`GET /images/{image_id}`** `https://api.thecatapi.com/v1/images/{{image_id}}`

Este endpoint **busca** a imagem correspondente ao **_path param_** `image_id`.

### **Exemplo de requisição:** 

```
curl --location --request GET 'https://api.thecatapi.com/v1/images/6qmirugX0' \
--header 'x-api-key: live_g6EUZSGbkMsKSuQm1OyWDVeLrLSnoCMcps2f7BMcDq6Alt2Y9Z606aj1uF6sPF35'
```

![Mia2](https://user-images.githubusercontent.com/105396649/207641262-48edba35-f423-4835-84ce-7f9109237485.jpg)

😻 A resposta de código `200 OK` indica que a consulta foi executada com sucesso. Ela retorna um JSON com todas as informações da imagem.

### **Exemplo de _reponse body_:**

``` json

{
    "id": "6qmirugX0",
    "url": "https://cdn2.thecatapi.com/images/6qmirugX0.jpg",
    "width": 1047,
    "height": 647,
    "sub_id": null
}

```

### **Resposta de erro:**

-  `400 Bad request`Couldn't find an image matching the passed 'id' of xxxxx. Caso o `image_id` inserido estivesse incorreto. 

## GET your uploaded images 

🐱 **`GET /images`** `https://api.thecatapi.com/v1/images`

Este endpoint **busca** todas as imagens enviadas para sua conta via `/images/upload`.

Filtre os resultados através dos parâmetros `query` abaixo:

| Parâmetro |    Descrição      | Tipo | Obrigatório |
|------------|--------------------|------|------------|
| `limit`  | Número de resultados a serem retornados. O valor máximo é 25. O padrão é 1. | `integer` | Sim |
| `mime_types` | Os tipos de imagem a serem retornados: .gif, .jpg, ou .png. Retorna todos os tipos como padrão. | `string` delimitado por vírgulas. | Não |
| `order` | A ordem de retorno: RANDOM, ASC ou DESC. O padrão é RANDOM. | `string` | Não  |

### **Exemplo de _requisição_ com parâmetros `limit` 25 e `order` ASC:**

``` 
curl --location --request GET 'https://api.thecatapi.com/v1/images?limit=25&order=ASC' \
--header 'x-api-key: live_g6EUZSGbkMsKSuQm1OyWDVeLrLSnoCMcps2f7BMcDq6Alt2Y9Z606aj1uF6sPF35'
```

### **Exemplo de _response body_:**

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

Este endpoint **deleta** a imagem correspondente ao parâmetro `image_id` passado como parâmetro `path`.

### **Exemplo de _requisição_:**

``` 
curl --location --request DELETE 'https://api.thecatapi.com/v1/images/FBqMvFgx5' \
--header 'x-api-key: live_g6EUZSGbkMsKSuQm1OyWDVeLrLSnoCMcps2f7BMcDq6Alt2Y9Z606aj1uF6sPF35'
``` 

![Mia 3](https://user-images.githubusercontent.com/105396649/207685891-74ba01fd-4ff1-4953-a095-41d839b3c02c.jpg)


😻 A resposta de código `204 No Content` indica que a exclusão foi executada com sucesso. Ela retorna um JSON vazio.


### **Resposta de erro:**

-  `400 Bad request` INVALID_DATA. Caso o `image_id` esteja inserido incorretamente.
