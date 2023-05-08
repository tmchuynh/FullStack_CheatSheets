# Angular

# Create an Angular project
```
ng new remult-angular-todo
```

> The ng new command prompts you for information about features to include in the initial app project. Accept the defaults by pressing the Enter or Return key.

```
cd remult-angular-todo
```

## Install required packages

We need Express to serve our app's API, and, of course, Remult. For development, we'll use tsx (opens new window)to run the API server.

```
npm i express remult
npm i --save-dev @types/express tsx
```

## Create the API server project

1. Add the following entry to the compilerOptions section of the tsconfig.json file to enable the use of Synthetic Default Imports and ES Module Interop in the app.

```json
// tsconfig.json

{
...
  "compilerOptions": {
    ...
    "allowSyntheticDefaultImports": true,
    "esModuleInterop": true,
   ...
  }
...
}
```

2. Create a server folder under the src/ folder created by Angular cli.

3. Create an index.ts file in the src/server/ folder with the following code:

```ts
// src/server/index.ts

import express from "express"
import { api } from "./api"

const app = express()
app.use(api)

app.listen(3002, () => console.log("Server started"))
```

## Bootstrap Remult in the back-end

1. Create an api.ts file in the src/server/ folder with the following code:

```ts
// src/server/api.ts

import { remultExpress } from "remult/remult-express"

export const api = remultExpress()
```

## Add Angular Modules

We'll modify the app.module.ts file to load Angular's HttpClientModule and FormsModule.

```ts
// src/app/app.module.ts

import { NgModule } from "@angular/core"
import { BrowserModule } from "@angular/platform-browser"
import { HttpClientModule } from "@angular/common/http"
import { FormsModule } from "@angular/forms"

import { AppComponent } from "./app.component"

@NgModule({
  declarations: [AppComponent],
  imports: [BrowserModule, HttpClientModule, FormsModule],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule {}
 
 ```

 ## Proxy API requests from Angular DevServer to the API server

 Create a file proxy.conf.json in the root folder, with the following contents:

```json
// proxy.conf.json

{
  "/api": {
    "target": "http://localhost:3002",
    "secure": false
  }
}
 
 ```

 ## Run the app

 1. Add script called dev that will run the angular dev server with the proxy configuration we've set and a script called dev-node to run the api.
 ```json
 // package.json

"dev": "ng serve --proxy-config proxy.conf.json --open",
"dev-node": "tsx watch src/server",
```

2. Open a terminal and start the angular dev server.
```
npm run dev
```
3. Open another terminal and start the node server
```
npm run dev-node
```

> The server is now running and listening on port 3002. tsx is watching for file changes and will restart the server when code changes are saved.

# Entities
The Task entity class will be used:

- As a model class for client-side code
- As a model class for server-side code
- By remult to generate API endpoints, API queries, and database commands

The Task entity class we're creating will have an auto-increment id field a title field and a completed field. The entity's API route ("tasks") will include endpoints for all CRUD operations.

## Define the Model
1. Create a shared folder under the src folder. This folder will contain code shared between frontend and backend.

2. Create a file Task.ts in the src/shared/ folder, with the following code:
```ts
// src/shared/Task.ts

import { Entity, Fields } from "remult"

@Entity("tasks", {
  allowApiCrud: true
})
export class Task {
  @Fields.autoIncrement()
  id = 0

  @Fields.string()
  title = ""

  @Fields.boolean()
  completed = false
}
```

3. In the server's api module, register the Task entity with Remult by adding entities: [Task] to an options object you pass to the remultExpress() middleware:
```ts
// src/server/api.ts

import { remultExpress } from "remult/remult-express"
import { Task } from "../shared/Task"

export const api = remultExpress({
  entities: [Task]
})
```

The @Entity decorator tells Remult this class is an entity class. The decorator accepts a key argument (used to name the API route and as a default database collection/table name), and an options argument used to define entity-related properties and operations, discussed in the next sections of this tutorial.

## Test the API
1. Open a browser with the url: `http://localhost:3002/api/tasks` (opens new window), and you'll see that you get an empty array.
2. Use `curl` to `POST` a new task - Clean car.
```
curl http://localhost:3002/api/tasks -d "{\"title\": \"Clean car\"}" -H "Content-Type: application/json"
```
3. Refresh the browser for the url: `http://localhost:3002/api/tasks` (opens new window)and see that the array now contains one item.
4. Use curl to POST a few more tasks:
```
curl http://localhost:3002/api/tasks -d "[{\"title\": \"Read a book\"},{\"title\": \"Take a nap\", \"completed\":true },{\"title\": \"Pay bills\"},{\"title\": \"Do laundry\"}]" -H "Content-Type: application/json"
```
5. Refresh the browser again, to see that the tasks were stored in the db.

## Display the Task List
1. Create a Todo component using Angular's cli
```
ng g c todo
```

2. Modify the TodoComponent class file:

Let's limit the number of fetched tasks to 20.

In the ngOnInit method, pass an options argument to the find method call and set its limit property to 20.

Uncompleted tasks are important and should appear above completed tasks in the todo app.

Adjust the ngOnInit method to fetch only completed tasks.


```ts
// src/app/todo/todo.component.ts

import { Component, OnInit } from '@angular/core';
import { remult } from 'remult';
import { Task } from '../../shared/Task';

@Component({
  selector: 'app-todo',
  templateUrl: './todo.component.html',
  styleUrls: ['./todo.component.css']
})
export class TodoComponent implements OnInit {
    taskRepo = remult.repo(Task)
    tasks: Task[] = []


    ngOnInit() {
        this.taskRepo.find({
                limit: 20,
                orderBy: { completed:"asc" },
                where: { completed: true }
            }).then((items) => (this.tasks = items));
        }
}
```

3. Replace the app.components.html to use the todo component.
```html
<!-- src/app/todo/todo.component.html -->

<h1>todos</h1>
<main>
  <div *ngFor="let task of tasks">
    <input type="checkbox" [(ngModel)]="task.completed" />
    {{task.title}}
  </div>
</main>
```

# CRUD Operations

## Adding new tasks
1. Add a newTaskTitle field and an addTask method to the ToDoComponent
```ts
// src/app/todo/todo.component.ts

export class TodoComponent implements OnInit {
  newTaskTitle = ""
  async addTask() {
    try {
      const newTask = await this.taskRepo.insert({ title: this.newTaskTitle })
      this.tasks.push(newTask)
      this.newTaskTitle = ""
    } catch (error: any) {
      alert(error.message)
    }
  }
}
```
> the call to taskRepo.insert will make a post request to the server, insert the new task to the db, and return the new Task object with all it's info (including the id generated by the database)

2. Add an Add Task form in the html template:

```html
<!-- src/app/todo/todo.component.html -->

<h1>todos</h1>
<main>
  <form (submit)="addTask()">
    <input
      placeholder="What needs to be done?"
      [(ngModel)]="newTaskTitle"
      name="newTaskTitle"
    />
    <button>Add</button>
  </form>
  <div *ngFor="let task of tasks">
    <input type="checkbox" [(ngModel)]="task.completed" />
    {{ task.title }}
  </div>
</main>
```

## Rename Tasks and Mark as Completed
To make the tasks in the list updatable, we'll bind the input elements to the Task properties and add a Save button to save the changes to the backend database.

1. Add a saveTask method to save the state of a task to the backend database
```ts
// src/app/todo/todo.component.ts

async saveTask(task: Task) {
  try {
    await this.taskRepo.save(task)
  } catch (error: any) {
    alert(error.message)
  }
}
 
 ```

 2. Modify the contents of the tasks div to include the following input elements and a Save button to call the saveTask method.
 ```html
 <!-- src/app/todo/todo.component.html -->

<div *ngFor="let task of tasks">
  <input
    type="checkbox"
    [(ngModel)]="task.completed"
    (change)="saveTask(task)"
  />
  <input [(ngModel)]="task.title" />
  <button (click)="saveTask(task)">Save</button>
</div>
 ```

 ## Delete Tasks
 1. Add the following deleteTask method to the TodoComponent class:
 ```ts
 // src/app/todo/todo.component.ts

async deleteTask(task: Task) {
   await this.taskRepo.delete(task);
   this.tasks = this.tasks.filter(t => t !== task);
}
```

2. Add a Delete button in the html:
```html
<!-- src/app/todo/todo.component.html -->

<div *ngFor="let task of tasks">
  <input
    type="checkbox"
    [(ngModel)]="task.completed"
    (change)="saveTask(task)"
  />
  <input [(ngModel)]="task.title" />
  <button (click)="saveTask(task)">Save</button>
  <button (click)="deleteTask(task)">Delete</button>
</div>
 
 ```

 # Validation
 ## Validate the Title Field

 In the Task entity class, modify the Fields.string decorator for the title field to include an object literal argument and set the object's validate property to Validators.required.

 ```ts
 // src/shared/Task.ts

@Fields.string({
  validate: Validators.required
})
title = ""
```

### Implicit server-side validation
The validation code we've added is called by Remult on the server-side to validate any API calls attempting to modify the title field.

Try making the following POST http request to the http://localhost:3002/api/tasks API route, providing an invalid title.

```
curl -i http://localhost:3002/api/tasks -d "{\"title\": \"\"}" -H "Content-Type: application/json"

```

## Custom Validation
The validate property of the first argument of Remult field decorators can be set to an arrow function which will be called to validate input on both front-end and back-end.

Try something like this and see what happens:

```ts
// src/shared/Task.ts

@Fields.string<Task>({
  validate: (task) => {
    if (task.title.length < 3) throw "Too Short"
  }
})
title = ""
```


IF ANYONE WANTS TO REWRITE AND/OR FINISH THIS
PLEASE VISIT [https://remult.dev/tutorials/angular/validation.html](https://remult.dev/tutorials/angular/validation.html)