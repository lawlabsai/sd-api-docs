# Stable Diffusion API Documentation

### Endpoint
- **URL:** `wss://stable-diffusion.qura.law/generate`
- **Protocol:** WebSocket

### Authentication

Authentication is required and can either be passed as a `Authorization` header as follows:

```bash
Authorization: Bearer <API_KEY>
```
Or as a parameter as follows:
```bash
wss://stable-diffusion.qura.law/generate?token=<API_KEY>
```

### Flow

1. **Connect to the WebSocket endpoint:** Establish a WebSocket connection to the API endpoint with the required authentication header.
2. **Send a request:** Send a JSON-formatted request to the API with the desired parameters.
3. **Receive real-time generation progress:** The API will respond with real-time updates on the image generation process. You will receive multiple responses as the model iteratively refines the image.
4. **Get the final image:** Once the image generation is complete, the API will send the final image as a base64-encoded string with the completion status.

### Request Format

| Parameter          | Type               | Description                                                                                          | Optional      |
|--------------------|--------------------|------------------------------------------------------------------------------------------------------|---------------|
| **prompt**         | `string`           | The textual description to generate the image from.                                                  | No            |
| **negative_prompt**| `string`           | Describes what should not appear in the image, helping guide the model's output away from undesired elements. | Yes           |
| **image_size**     | `list` of `integers`| Specifies the dimensions of the generated image. Default is `[512, 512]`. Allowed sizes: `[256, 256]`, `[512, 512]`, `[1024, 1024]`, `[2048, 2048]`. | Yes           |
| **steps**          | `integer`          | Determines the number of iterations the model uses to refine the image. More steps generally improve image quality. Default is `100`. | Yes           |
| **guidance_scale** | `float`            | Adjusts the influence of the prompt on the generated image. Higher values make the image adhere more closely to the prompt. Default is `7.0`. | Yes           |

### Response Format

| Field             | Type               | Description                                                                                          |
|-------------------|--------------------|------------------------------------------------------------------------------------------------------|
| **image**         | `string`           | The generated image encoded as a base64 string.                                                      |
| **step**          | `integer`          | Indicates the number of steps completed in the image generation process.                             |
| **completed**     | `boolean`          | Shows whether the image generation is complete.                                                      |
| **request**       | `object`           | Echoes the request sent to the API.                                                                  |

## Errors

| WebSocket Close Code | API Error Code | Description                                                                          |
|----------------------|----------------|--------------------------------------------------------------------------------------|
| 1000                 | 200            | Normal closure after successful image generation.                                    |
| 1003                 | 400            | Invalid request format or invalid JSON.                                              |
| 3000                 | 403            | Unauthorized access due to missing or invalid API key.                               |
| 3003                 | 404            | Invalid endpoint path.                                                               |
| 1011                 | 1011           | Internal server error.                                                               |
| 1013                 | 503            | Service unavailable; all GPUs are in use or GPU had to be offloaded.                 |
| 3008                 | 408            | Request timed out; image generation took too long.                                   |

## Example Flow

#### Open socket:
```bash
websocat -E wss://stable-diffusion.qura.law/generate -H "Authorization: Bearer <token>"
```

#### Send a request

```json
{"prompt": "A beautiful sunset over a calm lake with a small island in the middle."}
```

## Problems

If you encounter any issues or have questions about the API, please contact Kevin at kevin@qura.law.
