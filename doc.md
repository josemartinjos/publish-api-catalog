You're looking for the OpenAPI specification for the Orders Management API, presented in Markdown format. Here it is:

-----

# Orders Management API Documentation

This document describes the RESTful API for managing customer orders.

## Base URL

All API requests should be made to the following base URL:

`https://api.example.com/v1`

-----

## Endpoints

### Retrieve a List of All Orders

`GET /orders`

#### Summary

Retrieve a list of all orders.

#### Operation ID

`getAllOrders`

#### Tags

  * Orders

#### Responses

##### `200 OK`

A list of orders retrieved successfully.

**Content Type:** `application/json`

```json
[
  {
    "orderId": "ORD12345",
    "customerId": "CUST67890",
    "orderDate": "2023-07-29T10:00:00Z",
    "totalAmount": 150.75
  }
]
```

##### `500 Internal Server Error`

An unexpected error occurred on the server.

**Content Type:** `application/problem+json`

```json
{
  "type": "https://api.example.com/probs/internal-error",
  "title": "Internal Server Error",
  "status": 500,
  "detail": "An unexpected error occurred while processing your request.",
  "instance": "/orders"
}
```

-----

### Create a New Order

`POST /orders`

#### Summary

Create a new order.

#### Operation ID

`createOrder`

#### Tags

  * Orders

#### Request Body

Order details to create.

**Required:** Yes

**Content Type:** `application/json`

```json
{
  "customerId": "CUST67890",
  "items": [
    {
      "productId": "PROD001",
      "quantity": 2,
      "price": 25.50
    }
  ]
}
```

**Schema:**
| Name         | Type    | Description                                   | Example     | Required |
| :----------- | :------ | :-------------------------------------------- | :---------- | :------- |
| **`customerId`** | `string` | The ID of the customer placing the order.     | `CUST67890` | Yes      |
| **`items`** | `array` | List of items in the order.                   |             | Yes      |
|     `productId` | `string` | The ID of the product.                        | `PROD001`   | Yes      |
|     `quantity` | `integer` | The quantity of the product.                  | `2`         | Yes      |
|     `price`    | `number` | The price of the product per unit.            | `25.50`     | Yes      |

#### Responses

##### `201 Created`

Order created successfully.

**Headers:**
| Name       | Description                         | Example              |
| :--------- | :---------------------------------- | :------------------- |
| `Location` | URI of the newly created order      | `/orders/ORD12345` |

**Content Type:** `application/json`

```json
{
  "orderId": "ORD12345",
  "message": "Order created successfully."
}
```

##### `400 Bad Request`

Invalid input data.

**Content Type:** `application/problem+json`

```json
{
  "type": "https://api.example.com/probs/validation-error",
  "title": "Your request data is invalid.",
  "status": 400,
  "detail": "The provided data does not comply with the required structure or constraints. Please check the 'errors' array for specific issues.",
  "instance": "/orders",
  "errors": [
    {
      "detail": "Customer ID is required.",
      "pointer": "#/customerId"
    }
  ]
}
```

##### `404 Not Found`

Customer ID does not exist.

**Content Type:** `application/problem+json`

```json
{
  "type": "https://api.example.com/probs/not-found",
  "title": "Resource not found.",
  "status": 404,
  "detail": "The customer with the provided ID was not found.",
  "instance": "/orders",
  "resourceId": "CUST67890"
}
```