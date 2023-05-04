# Intro

- [Database Side Part 1](#database-Side-Part-1)
- [Client side](#client-side)
- [Database Side Part 2](#database-Side-Part-2)
  - [Create your controller file](#create-your-controller-file)
  - [Create your routes file](#create-your-routes-file)
  - [Create your model file](#create-your-model-file)
  - [Create a form](#create-a-form)
  - [Create your Main.js](#create-your-Main.js)
  - [Include validations](#include-validations)
- [Advanced MERN](#advanced-MERN)
  - [Downloading Bootstrap](#downloading-bootstrap)
  - [Install React Bootstrap](#install-react-bootstrap)
  - [Install Material-UI](#install-material-UI)
  - [Install Semantic UI React](#install-semantic-UI-react)


  
Up to this point, we have been separating our front end and backend entirely. When we use create-react-app, we use the command npm run start to serve our React application. However, we will want to include React within our Express project.

The way we will do this is by building our React project so that it creates the HTML, CSS, and Javascript files we need for our SPA. Then, we can point to it within our Express project. In the following lessons, we will learn how to set up our Full Stack MERN project and tie all of the technologies together.

We will be using Axios in our React project to communicate with our server. It is important to remember that React does not care about the language running on our server. React will simply make requests to our server and get responses.

---

## Full Stack MERN
![MERN_File_Structure](/images/MERN_File_Structure.png)


Using a server folder with the config, routes, etc. and a client folder with the npx create-react-app... etc. 

### Database Side Part 1
---

#### 1. Create your project folder, and `cd` into it:
```
mkdir myNewProject

cd myNewProject
```

#### 2. Create your config, controller, model, and routes folders
```
mkdir config controller model routes

cd ..
```

#### 3. Create the `package.json` file, and install the dependencies:
```
npm init -y

npm install express mongoose dotenv 
```

#### 4. Create the `server.js`, `.env`, and `.gitignore` files:
```
touch server.js
touch .env
touch .gitignore
```

#### 5. Within the `server.js` file, add the folloding code:
```js
const express = require('express');
const app = express();
require('dotenv').config();
const port = process.env.PORT;

require('./server/routes/product.routes')(app);
app.listen(port, () => console.log(`Listening on port: ${port}`) );
```

#### 6. Add the following into an `.gitignore` file:
```
/node_modules
.env
```


#### 7. Add to your `.env` file:
```
PORT=8000
DB_NAME=my_db
# mongo atlas connection
DB_USERNAME=YOUR_DB_USERNAME
DB_PASSWORD=YOUR_DB_PASSWORD
```

#### 8. In the `config` folder, add the `mongoose.config.js` file:
```
touch mongoose.config.js
```
`mongoose.config.js`:
```js
const mongoose = require('mongoose');
const dbName = process.env.DB_NAME;
const username = process.env.DB_USERNAME;
const pw = process.env.DB_PASSWORD;
// update the "@cluster0.cycjgio.mongodb.net/" with the appropriate address for your mongodb
const uri = `mongodb+srv://${username}:${pw}@cluster0.cycjgio.mongodb.net/${dbName}?retryWrites=true&w=majority`;
mongoose.connect(uri)
    .then(() => console.log("Established a connection to the database"))
    .catch(err => console.log("Something went wrong when connecting to the database", err));
```

### Client side
---
Let's create our modularized project structure by making a folder called "server" and then create four more folders within that called "config", "controllers", "models" and "routes".

#### 1. Leave the `server` folder, and create the `client` app using React:
```
npx create-react-app client
```
> This file should be beside your `server` file NOT inside it

Now that you have your React project built, you will be running two different servers: your front end React server with live reloading and your Express server.


#### 2. `cd` into the `client` folder, and run:

```
cd client
npm install
npm install axios react-router-dom
```
#### 3. In the `src` folder these folders:
```
mkdir views components
```
#### 4. In the `src/views` folder, create and open the `Main.js` file:
```
touch Main.js && code .
```
Paste in this (note that you will be changing the components for whatever project):
```js
import React, { useEffect, useState } from 'react';
import INSERT_COMPONENT_HERE from '../components/INSERT_COMPONENT_HERE';
export default () => {
    return (
        <div>
           <INSERT_COMPONENT_HERE/>
        </div>
    )
}
```

#### 5. In the `App.js`:
```js
import React from 'react';
import Main from './Main';
function App() {
  return (
    <div className="App">
      <Main />
    </div>
  );
}
export default App;
```

#### 6. Edit your `server.js` file:
`server.js`:
```js
const express = require('express');
const cors = require('cors');
const app = express();
require('dotenv').config();
const port = process.env.PORT;
require('./server/config/mongoose.config');
app.use(cors());
app.use(express.json());
app.use(express.urlencoded({ extended: true }));
require('./server/routes/COMPONENT_NAME.routes')(app);
    
app.listen(port, () => console.log(`Listening on port: ${port}`) );

```


Now that you have your React project built, you will be running two different servers: your front end React server with live reloading and your Express server.

#### 8. Run your application
1. In the main folder (that contains the `server.js`), run:
```
nodemon server.js
```
2. In the `client` folder, run:
```
npm run dev
```

### Database Side Part 2
---

#### 1. Create your controller file

`{item}.controller.js`
```js
import Product from '../models/products.model.js';

export async function getAllProducts(req, res) {
    try {
        const products = await Product.find();
        res.status(200).json(products);
    } catch (err) {
        res.status(500).json({ message: err.message });
    }
}


export async function getProductById(req, res) {
    try {
        const product = await Product.findById(req.params.id);
        if (!product) {
            return res.status(404).json({ error: 'Product not found' });
        }
        return res.status(200).json(product);
    } catch (error) {
        return res.status(500).json({ error: error.message });
    }
};


export async function createProduct(req, res) {
    try {
      const product = await Product.create(req.body);
      res.status(201).json(product);
    } catch (err) {
      res.status(500).json({ message: err.message });
    }
  }


export async function updateProduct(req, res) {
    const { id } = req.params;
    const { setup, punchline } = req.body;

    try {
        const product = await Product.findById(id);

        if (!product) {
            return res.status(404).json({ message: 'Product not found' });
        }

        product.setup = setup || product.setup;
        product.punchline = punchline || product.punchline;

        const updatedProduct = await product.save();

        res.json(updatedProduct);
    } catch (err) {
        res.status(500).json({ message: err.message });
    }
};


export async function deleteProduct(req, res) {
    try {
      const deletedProduct = await Product.findByIdAndDelete(req.params.id);
      if (!deletedProduct) {
        return res.status(404).json({ message: 'Product not found' });
      }
      res.status(200).json({ message: 'Product deleted successfully' });
    } catch (err) {
      res.status(500).json({ message: err.message });
    }
  };



```

#### 2. Create your routes file

`{item}.routes.js`
```js
import express from 'express';
import { getAllProducts, getProductById, createProduct, updateProduct, deleteProduct } from '../controllers/jokes.controller.js';

const router = express.Router();

// GET all jokes
router.get('/', getAllProducts);

// GET single joke by id
router.get('/:id', getProductById);

// POST a new joke
router.post('/', createProduct);

// PUT update an existing joke by id
router.put('/:id', updateProduct);

// DELETE a joke by id
router.delete('/:id', deleteProduct);

export default router;

```

#### 3. Create your model file
`{item}.models.js`
```js
import { Schema, model } from 'mongoose';
const ProductSchema = new Schema(
{
    productName: {
        type: String,
        required: [true, "Please enter a product name"],
    },
    productPrice: {
        type: Number,
        required: [true, "Please enter a product price"],
    },
    productDescription: {
        type: String,
        required: [true, "Please enter a product description"],
    }
}, { timestamps: true });
export const Product = model('Produt', ProductSchema);
```

#### 4. Create a form
> Note: to use react-bootstrap, you need to `npm install react-bootstrap bootstrap`

```js
import { useEffect, useRef, useState } from 'react';
import { Button, Card, Form } from 'react-bootstrap';
import axios from 'axios';

const initialProduct = {
  productName: '',
  productPrice: 0,
  productDescription: '',
};

function ProductForm({ setIsLoaded }) {
  const nameInputRef = useRef();
  const priceInputRef = useRef();
  const [product, setProduct] = useState(initialProduct);

  useEffect(() => {
    nameInputRef.current.focus();
  }, []);

  const handleChange = (e) => {
    setProduct((prevProduct) => ({
      ...prevProduct,
      [e.target.name]: e.target.value,
    }));
  };

  const handleSubmit = (e) => {
    e.preventDefault();
    axios
      .post('http://localhost:8000/api/Products', product)
      .then((res) => console.log(res.data))
      .catch((err) => console.error(err));
    setProduct(initialProduct);
    setIsLoaded(false);
  };

  return (
    <Card className="shadow mb-3">
      <Card.Header>
        <h3>Add Your Product</h3>
      </Card.Header>
      <Card.Body>
        <Form onSubmit={handleSubmit}>
          <Form.Group className="mb-3">
            <Form.Label htmlFor="productName">Name:</Form.Label>
            <Form.Control
              type="text"
              name="productName"
              id="productName"
              value={product.productName}
              onChange={handleChange}
              ref={nameInputRef}
            />
          </Form.Group>
          <Form.Group className="mb-3">
            <Form.Label htmlFor="productPrice">Price:</Form.Label>
            <Form.Control
              type="text"
              name="productPrice"
              id="productPrice"
              value={product.productPrice}
              onChange={handleChange}
              ref={priceInputRef}
            />
          </Form.Group>
          <Form.Group className="mb-3">
            <Form.Label htmlFor="productDescription">Description:</Form.Label>
            <Form.Control
              as="textarea"
              name="productDescription"
              id="productDescription"
              value={product.productDescription}
              onChange={handleChange}
            />
          </Form.Group>
          <div className="text-end">
            <Button type="submit" variant="primary">
              Add Product
            </Button>
          </div>
        </Form>
      </Card.Body>
    </Card>
  );
}

export default ProductForm;
```

#### 5. Create your Main.js
```js
import React, { useEffect, useState } from 'react';
import ProductForm from '../components/AddProductForm';
export default () => {
    return (
        <div>
           <ProductForm/>
        </div>
    )
}
```


#### 6. Include validations
```js
import { Schema, model } from 'mongoose';
const ProductSchema = new Schema(
{
    productName: {
        type: String,
        required: [true, "Please enter a product name"],
    },
    productPrice: {
        type: Number,
        required: [true, "Please enter a product price"],
    },
    productDescription: {
        type: String,
        required: [true, "Please enter a product description"],
    }
}, { timestamps: true });
export const Product = model('Produt', ProductSchema);
```

```js
import React, { useState } from 'react';

import styles from '../Style_modules/UserForm.module.css';


const UserForm = (props) => {
    const [firstName, setFirstName] = useState("");
    const [lastName, setLastName] = useState("");
    const [email, setEmail] = useState("");
    const [password, setPassword] = useState("");
    const [passwordConfirmation, setPasswordConfirmation] = useState("");
    const [firstNameError, setFirstNameError] = useState("");
    const [lastNameError, setLastNameError] = useState("");
    const [emailError, setEmailError] = useState("");
    const [passwordError, setPasswordError] = useState("");

    const createUser = (e) => {
        e.preventDefault();
        const newUser = { firstName, lastName, email, password, passwordConfirmation };
        setFirstName("")
        setLastName("")
        setEmail("")
        setPassword("")
        setPasswordConfirmation("")
    };

    const handleFirstName = (e) => {
        setFirstName(e.target.value);
        if (e.target.value.length < 2 && e.target.value > 0) {
            setFirstNameError("First name is required!");
        } else {
            setFirstNameError("");
        }
    }

    const handleLastName = (e) => {
        setLastName(e.target.value);
        if (e.target.value.length < 2 && e.target.value > 0) {
            setLastNameError("Last name is required!");
        } else {
            setLastNameError("");
        }
    }

    const handleEmail = (e) => {
        setEmail(e.target.value);
        if (e.target.value.length < 5 && e.target.value > 0) {
            setEmailError("Email must be at least 5 characters long!");
        } else {
            setEmailError("");
        }
    }

    const handlePassword = (e) => {
        setPasswordConfirmation(e.target.value);
        if (e.target.value > 0 && e.target.value !== password) {
            setPasswordError("Passwords do not match!");
        } else {
            setPasswordError("");
        }
    }

    return (
        <div className={styles.container}>
            <form onSubmit={createUser} className={styles.form}>
                <div className={styles.formContainer}>
                    <label className={styles.label}>First Name: </label>
                    <input className={styles.input} type="text" onChange={handleFirstName} value={firstName} />
                </div>
                {
                    firstNameError ?
                        <p className={styles.error}>{firstNameError}</p> :
                        ''
                }
                <div className={styles.formContainer}>
                    <label className={styles.label}>Last Name: </label>
                    <input className={styles.input} type="text" onChange={handleLastName} value={lastName} />
                </div>
                {
                    lastNameError ?
                        <p className={styles.error}>{lastNameError}</p> :
                        ''
                }
                <div className={styles.formContainer}>
                    <label className={styles.label}>Email: </label>
                    <input className={styles.input} type="text" onChange={handleEmail} value={email} />
                </div>
                {
                    emailError ?
                        <p className={styles.error}>{emailError}</p> :
                        ''
                }
                <div className={styles.formContainer}>
                    <label className={styles.label}>Password: </label>
                    <input className={styles.input} type="text" onChange={(e) => setPassword(e.target.value)} value={password} />
                </div>
                <div className={styles.formContainer}>
                    <label className={styles.label}>Confirm Password: </label>
                    <input className={styles.input} type="text" onChange={handlePassword} value={passwordConfirmation} />
                </div>
                {
                    passwordError ?
                        <p className={styles.error}>{passwordError}</p> :
                        ''
                }
                <input className={styles.submit} type="submit" value="Clear Form" />
            </form>

        </div>
    );
};

export default UserForm;
```

```js
import Product from '../models/products.model.js';

export async function getAllProducts(req, res) {
  try {
    const products = await Product.find();
    res.status(200).json(products);
  } catch (err) {
    res.status(500).json({ message: err.message });
  }
}

export async function getProductById(req, res) {
  try {
    const product = await Product.findById(req.params.id);
    if (!product) {
      return res.status(404).json({ error: 'Product not found' });
    }
    return res.status(200).json(product);
  } catch (error) {
    return res.status(500).json({ error: error.message });
  }
};

export async function createProduct(req, res) {
  try {
    const { name, description, price } = req.body;
    if (!name || !description || !price) {
      return res.status(400).json({ error: 'Please provide all required fields' });
    }
    const product = await Product.create(req.body);
    res.status(201).json(product);
  } catch (err) {
    res.status(500).json({ message: err.message });
  }
}

export async function updateProduct(req, res) {
  const { id } = req.params;
  const { name, description, price } = req.body;
  try {
    const product = await Product.findById(id);
    if (!product) {
      return res.status(404).json({ error: 'Product not found' });
    }
    product.name = name || product.name;
    product.description = description || product.description;
    product.price = price || product.price;
    const updatedProduct = await product.save();
    res.json(updatedProduct);
  } catch (err) {
    res.status(500).json({ message: err.message });
  }
};

export async function deleteProduct(req, res) {
  try {
    const deletedProduct = await Product.findByIdAndDelete(req.params.id);
    if (!deletedProduct) {
      return res.status(404).json({ error: 'Product not found' });
    }
    res.status(200).json({ message: 'Product deleted successfully' });
  } catch (err) {
    res.status(500).json({ message: err.message });
  }
};
```


`Main component`
```js
import React, { useState } from 'react';
import axios from 'axios';
export default function Main() {
    const [title, setTitle] = useState("");
    const [pages, setPages] = useState(0);
    //Create an array to store errors from the API
    const [errors, setErrors] = useState([]); 
    const onSubmitHandler = e => {
        e.preventDefault();
        //Send a post request to our API to create a Book
        axios.post('http://localhost:8000/books', {
            title,
            pages
        })
            .then(res=>console.log(res)) // If successful, do something with the response. 
            .catch(err=>{
                const errorResponse = err.response.data.errors; // Get the errors from err.response.data
                const errorArr = []; // Define a temp error array to push the messages in
                for (const key of Object.keys(errorResponse)) { // Loop through all errors and get the messages
                    errorArr.push(errorResponse[key].message)
                }
                // Set Errors
                setErrors(errorArr);
            })            
    }
    return (
        <div>
            <form onSubmit={onSubmitHandler}>
                {errors.map((err, index) => <p key={index}>{err}</p>)}
                <p>
                    <label>Title</label>
                    <input type="text" onChange={e => setTitle(e.target.value)} />
                </p>
                <p>
                    <label>Pages</label>
                    <input type="text" onChange={e => setPages(e.target.value)} />
                </p>
                <input type="submit" />
            </form>
        </div>
    )
}
```



# Advanced MERN

## Reusing Components
In the previous example, you may have noticed us failing to adhere to an important programming principle: Don't repeat yourself. A lot of the logic for the form for creating and updating an instance is the same. Why should we write similar code over again, especially if we can make one Form component in React? Let's look at how we would do this by refactoring our code.

Similarities between creating and updating an instance:
* They both will have the same input fields and same state

Differences:
* One will send a request to our create endpoint. The other will send a request to our update endpoint.

So, let's consider this to be a difference we will resolve by sending down a prop. This prop will be a function that we will execute when we submit the form.

# Details
### Using bootswatch for your css
#### 1. Downloading Bootstrap
Go to [https://bootswatch.com/](https://bootswatch.com/) to download your bootstrap selection

#### 2. Include the Bootstrap in your project
Check your `index.js` file
```js
import React from 'react';
.
.
.
import './bootstrap.min.css';
.
.
.
reportWebVitals();
```

#### 3. Reload your application

### Using `React-Boostrap` for your css [https://react-bootstrap.github.io/forms/form-text/](https://react-bootstrap.github.io/forms/form-text/)
#### 1. Install React Bootstrap
```
npm install react-bootstrap bootstrap
```

#### 2. Import into your project
```js
import Form from 'react-bootstrap/Form';

function FormTextExample() {
  return (
    <>
      <Form.Label htmlFor="inputPassword5">Password</Form.Label>
      <Form.Control
        type="password"
        id="inputPassword5"
        aria-describedby="passwordHelpBlock"
      />
      <Form.Text id="passwordHelpBlock" muted>
        Your password must be 8-20 characters long, contain letters and numbers,
        and must not contain spaces, special characters, or emoji.
      </Form.Text>
    </>
  );
}

export default FormTextExample;
```


### Using `Material-UI` for your css [https://mui.com/core/](https://mui.com/core/)
#### 1. Install Material-UI
```
npm install @material-ui/core
```

#### 2. Use similar to `react-boostrap`
```js
import { Button } from '@material-ui/core';
...
<Button>
    Click Me
</Button>
```

```js
import React from 'react';
import {
    Paper,
    FormControl,
    InputLabel,
    OutlinedInput,
    Button
} from '@material-ui/core';
const styles = {
    paper: {
        width: "20rem", padding: "1rem"
    },
    input: {
        marginBottom: "1rem"
    },
    button: {
        width: "100%"
    }
}
export default function LoginForm() {
    return (
        <Paper elevation={3} style={styles.paper}>
            <h2>Login Form</h2>
            <form>
                <FormControl variant="outlined" style={styles.input}>
                    <InputLabel>Username</InputLabel>
                    <OutlinedInput type="text"/>
                </FormControl>
                <FormControl variant="outlined" style={styles.input}>
                    <InputLabel>E-mail</InputLabel>
                    <OutlinedInput type="email"/>
                </FormControl>
                <FormControl variant="outlined" style={styles.input}>
                    <InputLabel>Password</InputLabel>
                    <OutlinedInput type="password"/>
                </FormControl>
                <FormControl variant="outlined" style={styles.input}>
                    <InputLabel>Password</InputLabel>
                    <OutlinedInput type="password"/>
                </FormControl>
                <Button type="submit" variant="contained" color="primary">
                    Register
                </Button>
            </form>
        </Paper>
    )
}
```

### Using `Semantic UI React` [https://react.semantic-ui.com/usage](https://react.semantic-ui.com/usage)
#### 1. Install Semantic UI React
```
npm install semantic-ui-react semantic-ui-css
```

#### 2. After install, import the minified CSS file in your app's entry file:
```
import 'semantic-ui-css/semantic.min.css'
```

#### 3. Use in your application
```js
import React, { Component } from 'react'
import {
  Button,
  Checkbox,
  Form,
  Input,
  Radio,
  Select,
  TextArea,
} from 'semantic-ui-react'

const options = [
  { key: 'm', text: 'Male', value: 'male' },
  { key: 'f', text: 'Female', value: 'female' },
  { key: 'o', text: 'Other', value: 'other' },
]

class FormExampleFieldControl extends Component {
  state = {}

  handleChange = (e, { value }) => this.setState({ value })

  render() {
    const { value } = this.state
    return (
      <Form>
        <Form.Group widths='equal'>
          <Form.Field
            control={Input}
            label='First name'
            placeholder='First name'
          />
          <Form.Field
            control={Input}
            label='Last name'
            placeholder='Last name'
          />
          <Form.Field
            control={Select}
            label='Gender'
            options={options}
            placeholder='Gender'
          />
        </Form.Group>
        <Form.Group inline>
          <label>Quantity</label>
          <Form.Field
            control={Radio}
            label='One'
            value='1'
            checked={value === '1'}
            onChange={this.handleChange}
          />
          <Form.Field
            control={Radio}
            label='Two'
            value='2'
            checked={value === '2'}
            onChange={this.handleChange}
          />
          <Form.Field
            control={Radio}
            label='Three'
            value='3'
            checked={value === '3'}
            onChange={this.handleChange}
          />
        </Form.Group>
        <Form.Field
          control={TextArea}
          label='About'
          placeholder='Tell us more about you...'
        />
        <Form.Field
          control={Checkbox}
          label='I agree to the Terms and Conditions'
        />
        <Form.Field control={Button}>Submit</Form.Field>
      </Form>
    )
  }
}

export default FormExampleFieldControl
```
