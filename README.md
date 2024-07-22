# Stable Diffusion API Documentation

Welcome to the Stable Diffusion API, provided by Qura. This API allows you to integrate image generation capabilities directly into your applications through a WebSocket interface. The goal is to provide a simple yet powerful way for users to leverage the Stable Diffusion model to generate images based on textual prompts. Note that the API is running on limited count of GPUs, so please be mindful of the number of requests you make and understand that there may be times when the service is fully utilized.

## API Overview

### Endpoint
- **URL:** `wss://stable-diffusion.qura.law/generate`
- **Protocol:** WebSocket

### Authentication

The API requires a valid API key to authenticate requests. You have been provided with an API key for your use. Please include the API key in the `Authorization` header of your WebSocket connection request as follows:

```bash
Authorization: Bearer <API_KEY>
```

### Flow

1. **Connect to the WebSocket endpoint:** Establish a WebSocket connection to the API endpoint with the required authentication header.
2. **Send a request:** Send a JSON-formatted request to the API with the desired parameters.
3. **Receive real-time generation progress:** The API will respond with real-time updates on the image generation process. You will receive multiple responses as the model iteratively refines the image.
4. **Get the final image:** Once the image generation is complete, the API will send the final image as a base64-encoded string with the completion status.

### Request Format
Requests to the API must be formatted as JSON objects with the following properties:

- **prompt** (`string`): The textual description to generate the image from.
- **negative_prompt** (`string`, optional): Describes what should not appear in the image, helping guide the model's output away from undesired elements.
- **image_size** (`list` of two `integers`, optional): Specifies the dimensions of the generated image. Default is `[512, 512]`. Allowed sizes are `[256, 256]`, `[512, 512]`, `[1024, 1024]`, and `[2048, 2048]`.
- **steps** (`integer`, optional): Determines the number of iterations the model uses to refine the image. More steps generally improve image quality. Default is `100`.
- **guidance_scale** (`float`, optional): Adjusts the influence of the prompt on the generated image. Higher values make the image adhere more closely to the prompt. Default is `7.0`.

### Response Format
Responses from the API are also JSON objects, which include:

- **image** (`string`): The generated image encoded as a base64 string.
- **step** (`integer`): Indicates the number of steps completed in the image generation process.
- **completed** (`boolean`): Shows whether the image generation is complete.
- **request** (`object`): Echoes the request sent to the API.

### Example Usage

#### Request
```json
{
    "prompt": "A beautiful sunset over the ocean."
}
```

#### Response
```json
{
    "image": "base64-encoded-image-string",
    "step": 100,
    "completed": true,
    "request": {
        "prompt": "A beautiful sunset over the ocean.",
        "image_size": [512, 512],
        "steps": 100,
        "guidance_scale": 0.5
    }
}
```

## Additional Information
- **Connection Handling:** Ensure your client maintains an active WebSocket connection to handle real-time data exchange.
- **Error Handling:** Properly handle errors such as connection timeouts, malformed requests, and service availability.
- **Security:** Utilize secure coding practices to protect the data and privacy of users.


## Problems

If you encounter any issues or have questions about the API, please contact Kevin at kevin@qura.law.
