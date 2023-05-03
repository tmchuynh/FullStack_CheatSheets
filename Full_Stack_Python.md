# Full Stack Python
To be able to connect our Flask applications to a database, we're going to need a package to help us. We're going to use one called `PyMySQL`. In the project level, run the following command in your command line to start a new project.

`$ pipenv install PyMySQL flask`

### 1. Create your database
Let's create one in the workbench called first_flask with a single table.

### 2. mysqlconnection.py
```py
# a cursor is the object we use to interact with the database
import pymysql.cursors
# this class will give us an instance of a connection to our database
class MySQLConnection:
    def __init__(self, db):
        # change the user and password as needed
        connection = pymysql.connect(host = 'localhost',
                                    user = 'root', 
                                    password = 'Admin@00', 
                                    db = db,
                                    charset = 'utf8mb4',
                                    cursorclass = pymysql.cursors.DictCursor,
                                    autocommit = True)
        # establish the connection to the database
        self.connection = connection
    # the method to query the database
    def query_db(self, query, data=None):
        with self.connection.cursor() as cursor:
            try:
                query = cursor.mogrify(query, data)
                print("Running Query:", query)
     
                cursor.execute(query, data)
                if query.lower().find("insert") >= 0:
                    # INSERT queries will return the ID NUMBER of the row inserted
                    self.connection.commit()
                    return cursor.lastrowid
                elif query.lower().find("select") >= 0:
                    # SELECT queries will return the data from the database as a LIST OF DICTIONARIES
                    result = cursor.fetchall()
                    return result
                else:
                    # UPDATE and DELETE queries will return nothing
                    self.connection.commit()
            except Exception as e:
                # if the query fails the method will return FALSE
                print("Something went wrong", e)
                return False
            finally:
                # close the connection
                self.connection.close() 
# connectToMySQL receives the database we're using and uses it to create an instance of MySQLConnection
def connectToMySQL(db):
    return MySQLConnection(db)
    
```

### 3. Create your init.py
```py
# __init__.py
from flask import Flask
app = Flask(__name__)
app.secret_key = "shhhhhh"
DATABASE = "CHANGEME"
```

### 4. Create your server file
```py
from flask_app import app
from flask_app.controllers import users

if __name__ == "__main__":
    app.run( debug = True)
```

### 5. Create your controllers
```py
from flask import Flask, render_template, request, redirect, session
from flask_app import app
from flask_bcrypt import Bcrypt
bcrypt = Bcrypt(app)
from flask_app.models.dojo import Dojo
```

### 6. Create your model
```py
from flask_app.config.mysqlconnection import connectToMySQL
from flask_app import DATABASE
from flask import flash

class Dojo:
    def __init__(self, data):
```

### 7. Create your base .html file
```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta http-equiv="X-UA-Compatible" content="IE=edge">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title></title>
        <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@5.2.3/dist/css/bootstrap.min.css">
        <link rel="stylesheet" href={{ url_for( 'static', filename='index.css' ) }}>
    </head>
    <body>
        <div class="container">
           
        </div>
        <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0-alpha1/dist/js/bootstrap.bundle.min.js" integrity="sha384-w76AqPfDkMBDXo30jS1Sgez6pr3x5MlQ1ZAGC+nuZB+EYdgRZgiwxhTBTkF7CXvN" crossorigin="anonymous"></script>
    </body>
</html>
```

> Login Page
```html
<!DOCTYPE html>
<html lang="en" data-bs-theme="dark">
    <head>
        <meta charset="UTF-8">
        <meta http-equiv="X-UA-Compatible" content="IE=edge">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title></title>
        <link rel="stylesheet" href={{ url_for( 'static', filename='index.css' ) }}>
        <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@5.2.3/dist/css/bootstrap.min.css">
    </head>
    <body>
        <div class="container">
            <h1>Register</h1>
            <form action="/users/registration" method="post" class="form-horizontal">
                <label for="first_name">First Name</label>
                <input type="text" name="first_name">

                <label for="last_name">Last Name</label>
                <input type="text" name="last_name">

                <label for="email">Email</label>
                <input type="text" name="email">

                <label for="password">Password</label>
                <input type="text" name="password">

                <label for="password_confirmation">Confirm Password</label>
                <input type="text" name="password_confirmation">

                {% for message in get_flashed_messages(category_filter = ['']) %}
                    <div class="alert alert-danger">
                        {{ message }}
                    </div>
                {% endfor %}

                <button type="submit" class="btn btn-secondary">Register</button>
            </form>
        </div>
        <div class="container">
            <h1>Login</h1>
            <form action="/users/login" method="post" class="form-horizontal">
                <label for="email">Email</label>
                <input type="email" name="email">

                <label for="password">Password</label>
                <input type="password" name="password">

                {% for message in get_flashed_messages(category_filter = ['']) %}
                    <div class="alert alert-danger">
                        {{ message }}
                    </div>
                {% endfor %}

                <button type="submit" class="btn btn-primary">Login</button>
            </form>

        </div>
        <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0-alpha1/dist/js/bootstrap.bundle.min.js" integrity="sha384-w76AqPfDkMBDXo30jS1Sgez6pr3x5MlQ1ZAGC+nuZB+EYdgRZgiwxhTBTkF7CXvN" crossorigin="anonymous"></script>
    </body>
</html>
```

## Tips and tricks
---
### 1. Validating a user
```py
@staticmethod
def validate_user(user):
    regex = re.compile('[@_!#$%^&*()<>?/\|}{~:]')
    
    is_valid = True
    
    if (regex.search(user['first_name']) != None or len(user['first_name']) < 2):
        # first name contains special characters
        flash('First name contains special characters or does not meet minimum length requirements.', "registration")
        is_valid = False
    
    if (regex.search(user['last_name'])!= None or len(user['last_name']) < 2):
        # last name contains special characters
        flash('Last name contains special characters or does not meet minimum length requirements.', "registration")
        is_valid = False
    
    if (len(user['password']) < 8):
        # password is too short
        flash('Password is too short', "registration")
        is_valid = False
        
    if (re.search('[0-9]', user['password']) == None):
        # password does not contain digits
        flash('Password does not contain digits', "registration")
        is_valid = False
        
    if (re.search('[A-Z]', user['password']) == None):
        # password does not contain uppercase letters
        flash('Password does not contain a uppercase letters', "registration")
        is_valid = False
        
    if (regex.search(user['password']) == None):
        # password contains not special characters
        flash('Password contains not a special characters', "registration")
        is_valid = False
    
    if (user['password']!= user['password_confirmation']):
        flash('Passwords do not match', "registration")
        is_valid = False
    
    regex = r'\b[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Z|a-z]{2,7}\b'
    
    if (re.fullmatch(regex, user['email'])):
        # email is valid
        # need to check if the email is already in use
        this_user = {
            'email': user['email']
        }
        results = User.check_database(this_user)
        
        if len(results) != 0:
            flash('Email is already in use, please use a different email', "registration")
            is_valid = False
    else:
        flash('Email contains special characters', "registration")
        is_valid = False
    
    return is_valid
```

### 2. Flash messages
```
{% for message in get_flashed_messages(category_filter = ['']) %}
    <div class="alert alert-danger">
        {{ message }}
    </div>
{% endfor %}
```

### 3. Show only ones not listed
```py
@classmethod
def get_other_books(cls, data):
    query = "select * from books where books.id not in (SELECT books.id FROM books LEFT JOIN favorites ON books.id = favorites.book_id WHERE favorites.author_id = %(author_id)s);"
    results = connectToMySQL(DATABASE).query_db(query, data)
    list_of_books = []
    for result in results:
        list_of_books.append(result)
    return list_of_books
```

### 4. Register a user
```py
@app.route('/users/registration', methods=['POST'])
def registration():
    # check if passwords match
    print(request.form)
    if not User.validate_user(request.form):
        return redirect('/users')
    
    # create the hash
    hashed_password = bcrypt.generate_password_hash(request.form['password'])
    print(hashed_password)
    
    new_user = {
        'first_name': request.form['first_name'],
        'last_name': request.form['last_name'],
        'email': request.form['email'],
        'password': hashed_password
    }
    User.create_user(new_user)
    return redirect('/users')
```

### 5. User login
```py
@app.route('/users/login', methods=['POST'])
def login():
    get_email = {
        'email': request.form['email']
    }
    this_user = User.get_user_by_email(get_email)
    
    if not this_user:
        flash("Invalid email or password", "login")
        return redirect('/users')
    
    if not bcrypt.check_password_hash(this_user.password, request.form['password']):
        flash('Incorrect password', "login")
        return redirect('/users')

    return redirect(f'/users/logout/{this_user.first_name}')
```


### 6. Flask table
```html
<div class="container pt-5">
    <h1>{{current_dojo.name}} Ninjas</h1>
    <table>
        <thead>
            <th>First Name</th>
            <th>Last Name</th>
            <th>Age</th>
        </thead>
        <tbody>
            {% for ninja in list_of_ninjas: %}
            <tr>
                <td>{{ninja.first_name}}</td>
                <td>{{ninja.last_name}}</td>
                <td>{{ninja.age}}</td>
            </tr>
            {% endfor %}
        </tbody>
    </table>
    <a href="/dojos" class="btn btn-primary mt-4">Home</a>
</div>
```


### 7. Flask selection
```html
<label for="dojo_id">Dojo</label>
<div class="dropdown">
    <select class="form-select form-select-lg" name="dojo_id">
        {% for dojo in list_of_dojos: %}
        <option value="{{dojo.id}}">{{dojo.name}}</option>
        {% endfor %}
    </select>
</div>
```