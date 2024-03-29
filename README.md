# Python_REST_API_CRUD_operations

This Python project sets up a simple REST API using Flask, which provides endpoints for adding, retrieving, updating, and deleting products. The API handles different HTTP request methods (GET, POST, PUT, DELETE) to perform these operations. The data is stored in memory as a list of product dictionaries.

This is a basic example for demo purposes. In a real-world scenario, you might want to use a database to store and manage the products, implement proper error handling, and use authentication for secure access to the API endpoints.

Here's a summary of the API endpoints and their functionalities:

1. Retrieve all products - GET Request Method:
   Endpoint: `/products`
   Function: `get_products()`
   Description: Returns a JSON response containing all the products.

2. Retrieve a product by its ID - GET Request Method:
   Endpoint: `/products/<id>`
   Function: `get_product(id)`
   Description: Returns a JSON response containing the product with the specified ID.

3. Add a product - POST Request Method:
   Endpoint: `/products`
   Function: `add_product()`
   Description: Adds a new product to the list. The product data should be provided in the request body as a JSON object.

4. Update a product by its ID - PUT Request Method:
   Endpoint: `/products/<id>`
   Function: `update_product(id)`
   Description: Updates the product with the specified ID. The updated product data should be provided in the request body as a JSON object.

5. Delete a product by its ID - DELETE Request Method:
   Endpoint: `/products/<id>`
   Function: `remove_product(id)`
   Description: Deletes the product with the specified ID from the list.

To run this application, save the code in a Python file (e.g., `app.py`) and execute it. The API will be accessible at `http://localhost:5000`.

The project involves:

- Creating API endpoints to perform Create, Retrieve, Update, and Delete operations on transient data with a Flask server.
- Creating REST API endpoints, and use POSTMAN to test your REST APIs.

We create a Products's list using a Flask server. Your application should allow you to add a product, retrieve the products, retrieve a specific product with its id, update a specific product with its id, and delete a product with its id. All these operations will be achieved through the REST API endpoints in your Flask server.

We will use cURL and POSTMAN to test the implemented endpoints.

This is a basic example for demo purposes. In a real-world scenario, you might want to use a database to store and manage the products, implement proper error handling, and use authentication for secure access to the API endpoints.

## Pre-steps:
1. Create an EC2 instance
2. SSH to the created instance
3. install git ```sudo yum install git```
4. install pip ```sudo yum install python3-pip```
5. install flask and install flask_cors```python3 -m pip install flask flask_cors```
6. Open port 5000 to access the API endpoint running on the flask server (modify the EC2 sequrity group)

## Create application
```mkdir project```

```[ ! -d 'jmgdo-microservices' ] && git clone https://github.com/ibm-developer-skills-network/jmgdo-microservices.git```

```cd jmgdo-microservices/CRUD```

3. In the Files Explorer open the jmgdo-microservices/CRUD folder and view products.py.

```python
#import the packages required to create REST APIs with Flask.

from flask import Flask, jsonify, request
import json
from flask_cors import CORS

# create the Flask application, which will service all the REST APIs for adding, retrieving, updating, and deleting products.

app = Flask("Product Server")
CORS(app)


products = [
    {'id': 143, 'name': 'Notebook', 'price': 5.49},
    {'id': 144, 'name': 'Black Marker', 'price': 1.99}
]

# create the REST API endpoints and define the routes or paths, one for each of the following operations.
# Retrieve all the products - GET Request Method
# Retrieve a product by its id - GET Request Method
# Add a product - POST Request Method
# Update a product by its id - PUT Request Method
# Delete a product by its id - DELETE Request Method

# Example request - http://localhost:5000/products
@app.route('/products', methods=['GET'])
def get_products():
    return jsonify(products)

# Example request - http://localhost:5000/products/144 - with method GET
@app.route('/products/<id>', methods=['GET'])
def get_product(id):
    id = int(id)
    product = [x for x in products if x["id"] == id][0]
    return jsonify(product)

# Example request - http://localhost:5000/products - with method POST
@app.route('/products', methods=['POST'])
def add_product():
    products.append(request.get_json())
    return '', 201

# Example request - http://localhost:5000/products/144 - with method PUT
@app.route('/products/<id>', methods=['PUT'])
def update_product(id):
    id = int(id)
    updated_product = json.loads(request.data)
    product = [x for x in products if x["id"] == id][0]
    for key, value in updated_product.items():
        product[key] = value
    return '', 204

# Example request - http://localhost:5000/products/144 - with method DELETE
@app.route('/products/<id>', methods=['DELETE'])
def remove_product(id):
    id = int(id)
    product = [x for x in products if x["id"] == id][0]
    products.remove(product)
    return '', 204

app.run(host='0.0.0.0',port=5000,debug=True)
```
## Run and test the server with cURL
1. In the terminal, run the python server using the following command.

```python3 products.py```

2. The server starts running and listening at port 5000.

<p align="left" >
  <img src="https://github.com/otammato/Python_API_CRUD_operations/assets/104728608/7beaee9e-9ef7-4efc-b209-8d9c5cf2e244" width="600px"/>
</p>

3. Open another Terminal window
4. In the new terminal, run the following command to access the http://localhost:5000/products API endpoint. ```curl``` command stands for Client URL and is used as command line interfacing with the server serving REST API endpoints. It is, by default, a GET request.

```curl http://localhost:5000/products```

This returns a JSON with the products that have been preloaded.

<p align="left" >
  <img src="https://github.com/otammato/Python_API_CRUD_operations/assets/104728608/2d9468ec-e64f-414c-94c5-d42673f4d417" width="600px"/>
</p>

5. In the terminal, run the following command to add a product to the list. This will be a POST request to which you will pass the product parameter as a JSON.

```bash
curl -X POST -H "Content-Type: application/json" \
    -d '{"id": 145, "name": "Pen", "price": 2.5}' \
    http://localhost:5000/products
```
This command will not return any output. It will add the product to the list of products.

6. Verify if the product is added by running the following command.

```curl http://localhost:5000/products/145```

This should return the details of the product you just added with a POST request.

<p align="left" >
  <img src="https://github.com/otammato/Python_API_CRUD_operations/assets/104728608/93a7be67-d123-43bc-b69a-5c93a64b5b25" width="600px"/>
</p>

<br>

## Using POSTMAN to test the REST API endpoints

While the ```GET``` endpoints are easy to test with curl using a command line interface, the ```POST```, ```PUT``` and ```DELETE``` commands can be cumbersome. To circumvent this problem, you can use Postman, which is a software that is available as a service.

<p align="left" >
  <img src="https://github.com/otammato/Python_API_CRUD_operations/assets/104728608/a425c892-2f16-4157-9b00-bc6fa58fac4f" width="600px"/>
</p>

1. Change the request type to POST. To send input as JSON, choose raw->body->JSON.

<p align="left" >
  <img src="https://github.com/otammato/Python_API_CRUD_operations/assets/104728608/6a49cab0-6f85-4073-83fa-d7b09bae1917" width="600px"/>
</p>

2. Enter the following JSON object and click the Send button. This will add the product to your previous list of products.

```JSON
{
    "id":146,
    "name":"Laptop Bag",
    "price":45.00
}
```
3. Verify the list of products to ensure that the request has gone through and the product is added. Change the request type to ```GET``` and remove the ```JSON``` object from the request body. Click Send and observe the output in the response window.

<p align="left" >
  <img src="https://github.com/otammato/Python_API_CRUD_operations/assets/104728608/14e5e938-b92f-4313-81ab-68c167e3c529" width="600px"/>
</p>

4. Test the ```PUT``` endpoint to update product details by changing the price of the item with id 146 to 42.00. Set the request type to ```PUT``` and add the product id to the end of the ```URL``` and add the value to be changed as a ```JSON``` body and click Send.

```JSON
{
    "price":42.00
}
```
<p align="left" >
  <img src="https://github.com/otammato/Python_API_CRUD_operations/assets/104728608/f22b3690-2979-4c43-8c89-9dbe43dd342d" width="600px"/>
</p>

5. Verify the product with id 146 to ensure that the request has gone through and the product is updated. Change the request type to ```GET``` and remove the ```JSON``` object from the request body. Click Send and observe the output in the response window.

<p align="left" >
  <img src="https://github.com/otammato/Python_API_CRUD_operations/assets/104728608/50dfc652-2680-48a3-8d3f-651e9c8c514a" width="600px"/>
</p>

