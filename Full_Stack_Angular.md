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

 [#](#validation) Validation
===========================

Validating user entered data is usually required both on the client-side and on the server-side, often causing a violation of the [DRY (opens new window)](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself) design principle. **With Remult, validation code can be placed within the entity class, and Remult will run the validation logic on both the frontend and the relevant API requests.**

Handling validation errors

When a validation error occurs, Remult will throw an exception.

In this tutorial, [CRUD operations](/tutorials/angular/crud.html) catch these exceptions, and alert the user. We leave it to you to decide how to handle validation errors in your application.

[#](#validate-the-title-field) Validate the Title Field
-------------------------------------------------------

Task titles are required. Let's add a validity check for this rule.

1.  In the `Task` entity class, modify the `Fields.string` decorator for the `title` field to include an object literal argument and set the object's `validate` property to `Validators.required`.

    // src/shared/Task.ts
    
    @Fields.string({
      validate: Validators.required
    })
    title = ""
    

Import Validators

This code requires adding an import of `Validators` from `remult`.

Manual browser refresh required

For this change to take effect, you **must manually refresh the browser**.

After the browser is refreshed, try creating a new `task` or saving an existing one with an empty title - the _"Should not be empty"_ error message is displayed.

### [#](#implicit-server-side-validation) Implicit server-side validation

The validation code we've added is called by Remult on the server-side to validate any API calls attempting to modify the `title` field.

Try making the following `POST` http request to the `http://localhost:3002/api/tasks` API route, providing an invalid title.

    curl -i http://localhost:3002/api/tasks -d "{\"title\": \"\"}" -H "Content-Type: application/json"
    

An http error is returned and the validation error text is included in the response body,

[#](#custom-validation) Custom Validation
-----------------------------------------

The `validate` property of the first argument of `Remult` field decorators can be set to an arrow function which will be called to validate input on both front-end and back-end.

Try something like this and see what happens:

    // src/shared/Task.ts
    
    @Fields.string<Task>({
      validate: (task) => {
        if (task.title.length < 3) throw "Too Short"
      }
    })
    title = ""


[#](#live-queries) Live Queries 🚀
==================================

New Feature

Live queries are a new feature introduced in version 0.18.

Our todo list app can have multiple users using it at the same time. However, changes made by one user are not seen by others unless they manually refresh the browser.

Let's add realtime multiplayer capabilities to this app.

[#](#one-time-setup) One Time Setup
-----------------------------------

We'll need angular to run it's change detection when we receive messages from the backend - to do that we'll add the following code to `AppModule`


    // src/app/app.module.ts
    
    import { NgModule, NgZone } from "@angular/core"
    import { remult } from "remult"
    //...
    export class AppModule {
      constructor(zone: NgZone) {
        remult.apiClient.wrapMessageHandling = handler => zone.run(() => handler())
      }
    }
    

[#](#realtime-updated-todo-list) Realtime updated todo list
-----------------------------------------------------------

Let's switch from fetching Tasks once when the Angular component is loaded, and manually maintaining state for CRUD operations, to using a realtime updated live query subscription **for both initial data fetching and subsequent state changes**.

1.  Modify the contents of the `ngOnInit` method in the `Todo` component:

Modify the `TodoComponent` with the following changes


    // src/app/todo/todo.component.ts
    
    export class TodoComponent implements OnInit, OnDestroy {
      //...
      taskRepo = remult.repo(Task)
      tasks: Task[] = []
      unsubscribe = () => {}
      ngOnInit() {
        this.unsubscribe = this.taskRepo
          .liveQuery({
            limit: 20,
            orderBy: { completed: "asc" }
            //where: { completed: true },
          })
          .subscribe(info => (this.tasks = info.applyChanges(this.tasks)))
      }
      ngOnDestroy() {
        this.unsubscribe()
      }
    }
    

Let's review the change:

*   Instead of calling the `repository`'s `find` method we now call the `liveQuery` method to define the query, and then call its `subscribe` method to establish a subscription which will update the Tasks state in realtime.
*   The `subscribe` method accepts a callback with an `info` object that has 3 members:
    *   `items` - an up to date list of items representing the current result - it's useful for readonly use cases.
    *   `applyChanges` - a method that receives an array and applies the changes to it - we send that method to the `setTasks` state function, to apply the changes to the existing `tasks` state.
    *   `changes` - a detailed list of changes that were received
*   The `subscribe` method return an `unsubscribe` method, which we store in the `unsubscribe` member and call in the `ngOnDestroy` hook, so that it'll be called when the component unmounts.

2.  As all relevant CRUD operations (made by all users) will **immediately update the component's state**, we should remove the manual adding of new Tasks to the component's state:


    // src/app/todo/todo.component.ts
    
    async addTask() {
      try {
        const newTask = await this.taskRepo.insert({ title: this.newTaskTitle })
        //this.tasks.push(newTask) <-- this line is no longer needed
        this.newTaskTitle = ""
      } catch (error: any) {
        alert(error.message)
      }
    }
    

3.  Optionally remove other redundant state changing code:


    // src/app/todo/todo.component.ts
    
    async deleteTask(task: Task) {
       await this.taskRepo.delete(task);
       // this.tasks = this.tasks.filter(t => t !== task); <-- this line is no longer needed
    }
    

Open the todo app in two (or more) browser windows/tabs, make some changes in one window and notice how the others are updated in realtime.

Under the hood

The default implementation of live-queries uses HTTP Server-Sent Events (SSE) to push realtime updates to clients, and stores live-query information in-memory.

For scalable production / serverless environments, live-query updates can be pushed using integration with third-party realtime providers, such as [Ably (opens new window)](https://ably.com/), and live-query information can be stored to any database supported by Remult.

[#](#backend-methods) Backend methods
=====================================

When performing operations on multiple entity objects, performance considerations may necessitate running them on the server. **With Remult, moving client-side logic to run on the server is a simple refactoring**.

[#](#set-all-tasks-as-un-completed) Set All Tasks as Un/completed
-----------------------------------------------------------------

Let's add two buttons to the todo app: "Set all as completed" and "Set all as uncompleted".

1.  Add a `setAllCompleted` async method to the `TodoComponent` class, which accepts a `completed` boolean argument and sets the value of the `completed` field of all the tasks accordingly.
    
        // src/app/todo/todo.component.ts
        
        async setAllCompleted(completed: boolean) {
          for (const task of await this.taskRepo.find()) {
            await this.taskRepo.save({ ...task, completed });
          }
        };
        
    
    The `for` loop iterates the array of `Task` objects returned from the backend, and saves each task back to the backend with a modified value in the `completed` field.
    
2.  Add the two buttons to the `TodoComponent` just before the closing `</main>` tag. Both of the buttons' `click` events will call the `setAllCompleted` method with the appropriate value of the `completed` argument.
    
        <!-- src/app/todo/todo.component.html -->
        
        <div>
          <button (click)="setAllCompleted(true)">Set all as completed</button>
          <button (click)="setAllCompleted(false)">Set all as uncompleted</button>
        </div>
        
    

Make sure the buttons are working as expected before moving on to the next step.

[#](#refactor-from-front-end-to-back-end) Refactor from Front-end to Back-end
-----------------------------------------------------------------------------

With the current state of the `setAllCompleted` function, each modified task being saved causes an API `PUT` request handled separately by the server. As the number of tasks in the todo list grows, this may become a performance issue.

A simple way to prevent this is to expose an API endpoint for `setAllCompleted` requests, and run the same logic on the server instead of the client.

1.  Create a new `TasksController` class, in the `shared` folder, and refactor the `for` loop from the `setAllCompleted` method of the `TodoComponent`into a new, `static`, `setAllCompleted` method in the `TasksController` class, which will run on the server.

    // src/shared/TasksController.ts
    
    import { BackendMethod, remult } from "remult"
    import { Task } from "./Task"
    
    export class TasksController {
      @BackendMethod({ allowed: true })
      static async setAllCompleted(completed: boolean) {
        const taskRepo = remult.repo(Task)
    
        for (const task of await taskRepo.find()) {
          await taskRepo.save({ ...task, completed })
        }
      }
    }
    

The `@BackendMethod` decorator tells Remult to expose the method as an API endpoint (the `allowed` property will be discussed later on in this tutorial).

**Unlike the front-end `Remult` object, the server implementation interacts directly with the database.**

2.  Register `TasksController` by adding it to the `controllers` array of the `options` object passed to `remultExpress()`, in the server's `api` module:

    // src/server/api.ts
    
    //...
    import { TasksController } from "../shared/TasksController"
    
    export const api = remultExpress({
      //...
      controllers: [TasksController]
    })
    

3.  Replace the `for` iteration in the `setAllCompleted` function of the `App` component with a call to the `setAllCompleted` method in the `TasksController`.

    // src/app/todo/todo.component.ts
    
    async setAllCompleted(completed: boolean) {
      await TasksController.setAllCompleted(completed);
    }
    

Import TasksController

Remember to add an import of `TasksController` in `todo.component.ts`.

[#](#authentication-and-authorization) Authentication and Authorization
=======================================================================

Our todo app is nearly functionally complete, but it still doesn't fulfill a very basic requirement - that users should log in before they can view, create or modify tasks.

Remult provides a flexible mechanism that enables placing **code-based authorization rules** at various levels of the application's API. To maintain high code cohesion, **entity and field-level authorization code should be placed in entity classes**.

**Remult is completely unopinionated when it comes to user authentication.** You are free to use any kind of authentication mechanism, and only required to provide Remult with an object which implements the Remult `UserInfo` interface.

In this tutorial, we'll use `Express`'s [cookie-session (opens new window)](https://expressjs.com/en/resources/middleware/cookie-session.html) middleware to store an authenticated user's session within a cookie. The `user` property of the session will be set by the API server upon a successful simplistic sign-in (based on username without password).

[#](#tasks-crud-requires-sign-in) Tasks CRUD Requires Sign-in
-------------------------------------------------------------

This rule is implemented within the `Task` `@Entity` decorator, by modifying the value of the `allowApiCrud` property. This property can be set to a function that accepts a `Remult` argument and returns a `boolean` value. Let's use the `Allow.authenticated` function from Remult.

  
  
  

  
  

    // src/shared/Task.ts
    
    @Entity("tasks", {
        allowApiCrud: Allow.authenticated
    })
    

Import Allow

This code requires adding an import of `Allow` from `remult`.

After the browser refreshes, **the list of tasks disappeared** and the user can no longer create new tasks.

Inspect the HTTP error returned by the API using cURL

    curl -i http://localhost:3002/api/tasks
    

Authorized server-side code can still modify tasks

Although client CRUD requests to `tasks` API endpoints now require a signed-in user, the API endpoint created for our `setAllCompleted` server function remains available to unauthenticated requests. Since the `allowApiCrud` rule we implemented does not affect the server-side code's ability to use the `Task` entity class for performing database CRUD operations, **the `setAllCompleted` function still works as before**.

To fix this, let's implement the same rule using the `@BackendMethod` decorator of the `setAllCompleted` method of `TasksController`.

    // src/shared/TasksController.ts
    
    @BackendMethod({ allowed: Allow.authenticated })
    

**This code requires adding an import of `Allow` from `remult`.**

[#](#user-authentication) User Authentication
---------------------------------------------

Let's add a sign-in area to the todo app, with an `input` for typing in a `username` and a sign-in `button`. The app will have two valid `username` values - _"Jane"_ and _"Steve"_. After a successful sign-in, the sign-in area will be replaced by a "Hi \[username\]" message.

### [#](#backend-setup) Backend setup

1.  Open a terminal and run the following command to install the required packages:

    npm i cookie-session
    npm i --save-dev @types/cookie-session
    

2.  Modify the main server module `index.ts` to use the `cookie-session` Express middleware.

        // src/server/index.ts
        
        //...
        
        import session from "cookie-session"
        
        const app = express()
        app.use(
          session({
            secret: process.env["SESSION_SECRET"] || "my secret"
          })
        )
        
        //...
        
    
    The `cookie-session` middleware stores session data, digitally signed using the value of the `secret` property, in an `httpOnly` cookie, sent by the browser to all subsequent API requests.
    
3.  Create a file `src/server/auth.ts` for the `auth` express router and place the following code in it:
    
        // src/server/auth.ts
        
        import express, { Router } from "express"
        import type { UserInfo } from "remult"
        
        const validUsers: UserInfo[] = [
          { id: "1", name: "Jane" },
          { id: "2", name: "Steve" }
        ]
        
        export const auth = Router()
        
        auth.use(express.json())
        
        auth.post("/api/signIn", (req, res) => {
          const user = validUsers.find(user => user.name === req.body.username)
          if (user) {
            req.session!["user"] = user
            res.json(user)
          } else {
            res.status(404).json("Invalid user, try 'Steve' or 'Jane'")
          }
        })
        
        auth.post("/api/signOut", (req, res) => {
          req.session!["user"] = null
          res.json("signed out")
        })
        
        auth.get("/api/currentUser", (req, res) => res.json(req.session!["user"]))
        
    
    *   The (very) simplistic `signIn` endpoint accepts a request body with a `username` property, looks it up in a predefined dictionary of valid users and, if found, sets the user's information to the `user` property of the request's `session`.
        
    *   The `signOut` endpoint clears the `user` value from the current session.
        
    *   The `currentUser` endpoint extracts the value of the current user from the session and returns it in the API response.
        
4.  Register the `auth` router in the main server module.
    
    
        // src/server/index.ts
        
        //...
        
        import { auth } from "./auth"
        
        const app = express()
        app.use(
          session({
            secret: process.env["SESSION_SECRET"] || "my secret"
          })
        )
        app.use(auth)
        
        //...
        
    

### [#](#frontend-setup) Frontend setup

1.  Create an `Auth` component using Angular's cli
    
        ng g c auth
        
    
2.  Add the highlighted code lines to the `AuthComponent` class file:
    
        // src/app/auth/auth.component.ts
        
        import { Component, OnInit } from "@angular/core"
        import { HttpClient } from "@angular/common/http"
        import { remult, UserInfo } from "remult"
        
        @Component({
          selector: "app-auth",
          templateUrl: "./auth.component.html",
          styleUrls: ["./auth.component.css"]
        })
        export class AuthComponent implements OnInit {
          constructor(private http: HttpClient) {}
        
          signInUsername = ""
          remult = remult
        
          signIn() {
            this.http
              .post<UserInfo>("/api/signIn", {
                username: this.signInUsername
              })
              .subscribe({
                next: user => {
                  this.remult.user = user
                  this.signInUsername = ""
                },
                error: error => alert(error.error)
              })
          }
        
          signOut() {
            this.http
              .post("/api/signOut", {})
              .subscribe(() => (this.remult.user = undefined))
          }
        
          ngOnInit() {
            this.http
              .get<UserInfo>("/api/currentUser")
              .subscribe(user => (this.remult.user = user))
          }
        }
        
    
3.  Replace the contents of auth.component.html with the following html:
    
        // src/app/auth/auth.component.ts
        
        <ng-container *ngIf="!remult.authenticated()">
          <h1>todos</h1>
          <main>
            <form (submit)="signIn()">
              <input
                [(ngModel)]="signInUsername"
                placeholder="Username, try Steve or Jane"
                name="username"
              />
              <button>Sign in</button>
            </form>
          </main>
        </ng-container>
        <ng-container *ngIf="remult.authenticated()">
          <header>
            Hello {{ remult.user?.name }}
            <button (click)="signOut()">Sign Out</button>
          </header>
          <app-todo></app-todo>
        </ng-container>
        
    
4.  Change the `app.component.html` to use the `AuthComponent` instead of the `TodoComponent`
    
        <!-- src/app/app.component.html -->
        
        <app-auth></app-auth>
        
    

### [#](#connect-remult-middleware) Connect Remult middleware

Once an authentication flow is established, integrating it with Remult in the backend is as simple as providing Remult with a `getUser` function that extracts a `UserInfo` object from a `Request`.


    // src/server/api.ts
    
    //...
    
    export const api = remultExpress({
      //...
      getUser: req => req.session!["user"]
    })
    

The todo app now supports signing in and out, with **all access restricted to signed in users only**.

[#](#role-based-authorization) Role-based Authorization
-------------------------------------------------------

Usually, not all application users have the same privileges. Let's define an `admin` role for our todo app, and enforce the following authorization rules:

*   All signed in users can see the list of tasks.
*   All signed in users can set specific tasks as `completed`.
*   Only users belonging to the `admin` role can create, delete or edit the titles of tasks.

1.  Modify the highlighted lines in the `Task` entity class to reflect the top three authorization rules.


    // src/shared/Task.ts
    
    import { Allow, Entity, Fields, Validators } from "remult"
    
    @Entity<Task>("tasks", {
      allowApiCrud: Allow.authenticated,
      allowApiInsert: "admin",
      allowApiDelete: "admin"
    })
    export class Task {
      @Fields.uuid()
      id!: string
    
      @Fields.string({
        validate: (task) => {
          if (task.title.length < 3) throw "Too Short"
        }
        allowApiUpdate: "admin"
      })
      title = ""
    
      @Fields.boolean()
      completed = false
    }
    

2.  Let's give the user _"Jane"_ the `admin` role by modifying the `roles` array of her `validUsers` entry.


    // src/server/auth.ts
    
    const validUsers = [
      { id: "1", name: "Jane", roles: ["admin"] },
      { id: "2", name: "Steve" }
    ]
    

**Sign in to the app as _"Steve"_ to test that the actions restricted to `admin` users are not allowed. 🔒**

[#](#role-based-authorization-on-the-frontend) Role-based Authorization on the Frontend
---------------------------------------------------------------------------------------

From a user experience perspective it only makes sense that users that can't add or delete, would not see these buttons.

Let's reuse the same definitions on the Frontend.

Modify the contents of auth.component.html to only display the form and delete buttons if these operations are allowed based on the entity's metadata:



    <!-- src/app/auth/auth.component.ts -->
    
    <h1>todos</h1>
    <main>
      <form *ngIf="taskRepo.metadata.apiInsertAllowed()" (submit)="addTask()">
        <input
          placeholder="What needs to be done?"
          [(ngModel)]="newTaskTitle"
          name="newTaskTitle"
        />
        <button>Add</button>
      </form>
      <div *ngFor="let task of tasks">
        <input
          type="checkbox"
          [(ngModel)]="task.completed"
          (change)="saveTask(task)"
        />
        <input [(ngModel)]="task.title" />
        <button (click)="saveTask(task)">Save</button>
        <button
          *ngIf="taskRepo.metadata.apiDeleteAllowed(task)"
          (click)="deleteTask(task)"
        >
          Delete
        </button>
      </div>
      <div>
        <button (click)="setAllCompleted(true)">Set all as completed</button>
        <button (click)="setAllCompleted(false)">Set all as uncompleted</button>
      </div>
    </main>
    

This way we can keep the frontend consistent with the `api`'s Authorization rules

*   Note We send the `task` to the `apiDeleteAllowed` method, because the `apiDeleteAllowed` option, can be sophisticated and can also be based on the specific item's values,

[#](#database) Database
=======================

Up until now the todo app has been using a plain JSON file to store the list of tasks. **In production, we'd like to use a `Postgres` database table instead.**

Learn more

See the [Connecting to a Database](/docs/databases.html) article for the (long) list of relational and non-relational databases Remult supports.

Don't have Postgres installed? Don't have to.

Don't worry if you don't have Postgres installed locally. In the next step of the tutorial, we'll configure the app to use Postgres in production, and keep using JSON files in our dev environment.

**Simply install `postgres-node` per step 1 below and move on to the [Deployment section of the tutorial](/tutorials/angular/deployment.html).**

1.  Install `postgres-node` ("pg").
    
        npm i pg
        
    
2.  Add the highlighted code to the `api` server module.

        // src/server/api.ts
        
        //...
        
        import { createPostgresConnection } from "remult/postgres"
        
        export const api = remultExpress({
          //...
          dataProvider: createPostgresConnection({
            connectionString: "your connection string"
          })
        })

[#](#deployment) Deployment
===========================

Let's deploy the todo app to [railway.app (opens new window)](https://railway.app/).

[#](#prepare-for-production) Prepare for Production
---------------------------------------------------

In this tutorial, we'll deploy both the Angular app and the API server as [one server-side app (opens new window)](https://create-react-app.dev/docs/deployment/#other-solutions), and redirect all non-API requests to return the Angular app.

In addition, to follow a few basic production best practices, we'll use [compression (opens new window)](https://www.npmjs.com/package/compression) middleware to improve performance and [helmet (opens new window)](https://www.npmjs.com/package/helmet) middleware for security

1.  Install `compression` and `helmet`.

    npm i compression helmet
    npm i @types/compression --save-dev
    

2.  Add the highlighted code lines to `src/server/index.ts`, and modify the `app.listen` function's `port` argument to prefer a port number provided by the production host's `PORT` environment variable.



    // src/server/index.ts
    
    import express from "express"
    import { api } from "./api"
    import session from "cookie-session"
    import { auth } from "./auth"
    import helmet from "helmet"
    import compression from "compression"
    import path from "path"
    
    const app = express()
    app.use(
      session({
        secret: process.env["SESSION_SECRET"] || "my secret"
      })
    )
    app.use(helmet())
    app.use(compression())
    app.use(auth)
    app.use(api)
    app.use(express.static(path.join(__dirname, "../remult-angular-todo")))
    app.get("/*", (req, res) => {
      res.sendFile(path.join(__dirname, "../remult-angular-todo", "index.html"))
    })
    
    app.listen(process.env["PORT"] || 3002, () => console.log("Server started"))
    

3.  Modify the highlighted code in the api server module to prefer a `connectionString` provided by the production host's `DATABASE_URL` environment variable.
    
      
      
      
      
      
      
    
      
      
      
      
    
        // src/server/api.ts
        
        //...
        export const api = remultExpress({
          //...
          dataProvider: createPostgresConnection({
            connectionString: process.env["DATABASE_URL"] || "your connection string"
          })
          //...
        })
        
    
4.  In the root folder, create a TypeScript configuration file `tsconfig.server.json` for the build of the server project using TypeScript.
    

    // tsconfig.server.json
    
    {
      "extends": "./tsconfig.json",
      "compilerOptions": {
        "module": "commonjs",
        "emitDecoratorMetadata": true,
        "esModuleInterop": true,
        "noEmit": false,
        "outDir": "dist",
        "skipLibCheck": true,
        "rootDir": "src"
      },
      "include": ["src/server/index.ts"]
    }
    

5.  Modify the project's `build` npm script to additionally transpile the API server's TypeScript code to JavaScript (using `tsc`).

    // package.json
    
    "build": "ng build && tsc -p tsconfig.server.json"
    

6.  Modify the project's `start` npm script to start the production Node.js server.

    // package.json
    
    "start": "node dist/server/"
    

The todo app is now ready for deployment to production.

[#](#deploy-to-railway) Deploy to Railway
-----------------------------------------

In order to deploy the todo app to [railway (opens new window)](https://railway.app/) you'll need a `railway` account. You'll also need [Railway CLI (opens new window)](https://docs.railway.app/develop/cli#npm) installed, and you'll need to login to railway from the cli, using `railway login`.

Click enter multiple times to answer all its questions with the default answer

1.  Create a Railway `project`.
    
    From the terminal in your project folder run:
    
        railway init
        
    
2.  Select `Empty Project`
    
3.  Set a project name.
    
4.  Once it's done add a database by running the following command:
    
        railway add
        
    
5.  Select `postgressql` as the database.
    
6.  Once that's done run the following command to upload the project to railway:
    
        railway up
        
    
7.  got to the `railway` project's site and click on the project
    
8.  Switch to the `variables` tab
    
9.  Click on `+ New Variable`, and in the `VARIABLE_NAME` click `Add Reference` and select `DATABASE_URL`
    
10.  Add another variable called `SESSION_SECRET` and set it to a random string, you can use an [online UUID generator (opens new window)](https://www.uuidgenerator.net/)
    
11.  Switch to the `settings` tab
    
12.  Under `Environment` click on `Generate Domain`
    
13.  Click on the newly generated url to open the app in the browser and you'll see the app live in production. (it may take a few minutes to go live)
    