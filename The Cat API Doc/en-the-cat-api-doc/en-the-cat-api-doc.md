# The-Cat-API-DOC

>
> The Cat API is a public API for **information management** and **cat imaging**. 🐈

With The Cat API, you can:

1. **Insert** images;
2. **Search** images by ID;
3. **Delete** images.

⚠️ _Images that are not of cats or inappropriate are rejected_. The `images` object represents the photos of cats sent.

# Prerequisites

- Register at [https://thecatapi.com/signup](https://thecatapi.com/signup) to receive your API key by email.
- Inform the API key in the header of the calls through the `x-api-key` variable.
- Use the path [https://api.thecatapi.com/v1](https://api.thecatapi.com/v1) in calls.

# Endpoints

>
> Check below each endpoint, its forms of request and examples of _responses_.

## POST

🐱 **`POST / Images / Upload`** `https://api.thecatapi.com/v1/images/upload`

This endpoint **inserts** a new image into the system by loading a **valid** cat image file, like:

- .gif
- .jpg
- .png

**Request body**

| Name | Description | Type | Required |
|------|-----------|------|-------------|
| `file` | File in .gif, .png, or .jpg | `file` | Yes |
| `sub_id` | ID for internal identification. | `string` | No |


### **Example request:**

```
curl --location --request POST 'https://api.thecatapi.com/v1/images/upload' \
--header 'x-api-key: live_g6EUZSGbkMsKSuQm1OyWDVeLrLSnoCMcps2f7BMcDq6Alt2Y9Z606aj1uF6sPF35' \
--form 'file=@"iQzJdW8Ds/Mia.jpg"'
```

![Mia](https://user-images.githubusercontent.com/105396649/207432035-c638a387-f243-4983-8af5-fcf7e0c94011.jpg)

😻 Code response `201 Created` indicates that the image record was successfully created. Returns a JSON including the new ID.

### **Example of _response body_:**

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

### **Error Responses:**

-  `400 Bad request` Invalid file data. Check you are sending the formdata.append('file', ...} format'.

-  `400 Bad request` Classifcation failed: correct animal found.

-  `401 Unauthorized` Unauthorized.

-  `500 Internal Server Error` Internal Server Error. 

## GET by ID

🐱 **`GET /images/{image_id}`** `https://api.thecatapi.com/v1/images/{{image_id}}`

This endpoint **fetches** the image corresponding to **_path param_** `image_id`.

### **Example request:**

```
curl --location --request GET 'https://api.thecatapi.com/v1/images/6qmirugX0' \
--header 'x-api-key: live_g6EUZSGbkMsKSuQm1OyWDVeLrLSnoCMcps2f7BMcDq6Alt2Y9Z606aj1uF6sPF35'
```

![Mia2](https://user-images.githubusercontent.com/105396649/207641262-48edba35-f423-4835-84ce-7f9109237485.jpg)

😻 The `200 OK` code response indicates that the query was successfully executed. It returns a JSON with all the image information.

### **Example of _reponse body_:**

``` json

{
    "id": "6qmirugX0",
    "url": "https://cdn2.thecatapi.com/images/6qmirugX0.jpg",
    "width": 1047,
    "height": 647,
    "sub_id": null
}

```

### **Error response:**

- `400 Bad request`Couldn't find an image matching the passed 'id' of xxxxx. In case the `image_id` entered was incorrect.

## GET your uploaded images 

🐱 **`GET /images`** `https://api.thecatapi.com/v1/images`

This endpoint **fetches** all images uploaded to your account via `/images/upload`.

Filter the results using the `query` parameters below:

| Parameter | Description | Type | Required |
|------------|--------------------|------|-------- ----|
| `limit` | Number of results to return. The maximum value is 25. The default is 1. | `integer` | Yes |
| `mime_types` | The image types to return: .gif, .jpg, or .png. Returns all types by default. | `string` delimited by commas. | No |
| `order` | Return order: RANDOM, ASC, or DESC. The default is RANDOM. | `string` | No |

### **Example of _request_ with parameters `limit` 25 and `order` ASC:**

``` 
curl --location --request GET 'https://api.thecatapi.com/v1/images?limit=25&order=ASC' \
--header 'x-api-key: live_g6EUZSGbkMsKSuQm1OyWDVeLrLSnoCMcps2f7BMcDq6Alt2Y9Z606aj1uF6sPF35'
```

### **Example of _response body_:**

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

This endpoint **deletes** the image corresponding to the `image_id` parameter passed as the `path` parameter.

### **Example of _request_:**

``` 
curl --location --request DELETE 'https://api.thecatapi.com/v1/images/FBqMvFgx5' \
--header 'x-api-key: live_g6EUZSGbkMsKSuQm1OyWDVeLrLSnoCMcps2f7BMcDq6Alt2Y9Z606aj1uF6sPF35'
``` 

![Mia 3](https://user-images.githubusercontent.com/105396649/207685891-74ba01fd-4ff1-4953-a095-41d839b3c02c.jpg)


😻 Code response `204 No Content` indicates that the deletion was performed successfully. It returns an empty JSON.


### **Error response:**

- `400 Bad request` INVALID_DATA. In case the `image_id` is entered incorrectly.
