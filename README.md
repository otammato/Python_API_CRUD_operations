# Python_REST_API_CRUD_operations

- Create API endpoint to perform Create, Retrieve, Update, and Delete operations on transient data with a Flask server.
- Create REST API endpoint, and use POSTMAN to test your REST APIs.

We create a Products's list using a Flask server. Your application should allow you to add a product, retrieve the products, retrieve a specific product with its id, update a specific product with its id, and delete a product with its id. All these operations will be achieved through the REST API endpoints in your Flask server.

You will create an application with API endpoints to perform Create, Retrieve, Update, and Delete operations on the above data using a Flask server.

You will use cURL and POSTMAN to test the implemented endpoints.

## Pre-steps:
1. Create an EC2 instance
2. SSH to the created instance
3. install git ```sudo yum install git```
4. install pip ```sudo yum install python3-pip```
5. install flask and install flask_cors```python3 -m pip install flask flask_cors```

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

app.run(port=5000,debug=True)
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

