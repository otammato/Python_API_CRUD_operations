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
