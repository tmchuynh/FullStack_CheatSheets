# C# Cheat Sheet

## Making Our Project And Adding Packages

1. The first thing we need to get our project started is...a project! By this point, you should feel familiar with using `dotnet new mvc --no-https -o ProjectName` to create a project. Remember it's still important to use the `--no-https` flag to prevent any conflicts with adding security features! 

2.  Head into your project and run these two commands from the terminal: 

```
dotnet add package Pomelo.EntityFrameworkCore.MySql --version 6.0.1 

dotnet add package Microsoft.EntityFrameworkCore.Design --version 6.0.3 
```

## Making Our Models

3. The next step is to create our models, the blueprints on which everything relating to our database and LINQ commands will run. This part will vary depending on how many models you need. 

- The name you give your model should be a singular name (i.e. User instead of Users). We will pluralize it for our database later. 
- The names you give to your properties will be the names of your columns in the database. 
- All names should follow PascalCasing standards (CapitalFirstLetter) 
- We will be using an auto-incremented integer ID for each model which we need to specify (see the example below). 
- Since this is going into a database, best practices state we should add CreatedAt and UpdatedAt properties to all models. 

```cs
using System;
using System.ComponentModel.DataAnnotations;

namespace YourProjectName.Models
{
    public class Monster
    {
        [Key]
        public int MonsterId { get; set; }
        
        [Required]
        public string Name { get; set; }
        
        public double Height { get; set; }
        
        [Required]
        public string Description { get; set; }
        
        [DataType(DataType.DateTime)]
        public DateTime CreatedAt { get; } = DateTime.Now;
        
        [DataType(DataType.DateTime)]
        public DateTime UpdatedAt { get; set; } = DateTime.Now;
        
        // Add any additional properties or navigation properties here
    }
}

```
> Important: It is highly recommended to name your ID with the naming convention: ModelNameId! Failure to do so may cause confusion when the table is migrated into MySQL. Sometimes students experience that a separate column that follows this standard will automatically populate to "fix" the issue. This will cause several problems going forward! 

## The Context Class
4. Once you have all your models created, there is one more model we need to make in our Models folder. This is our context file. The context class is what forms the relationship between our models and the database. Think of it as an object that sits between our app and the database that translates our queries for us. Context classes, by convention, always have names that end in "Context". You can name the file anything (though something informative is your best option), just remember to add "Context" to the end!

```cs
using Microsoft.EntityFrameworkCore;

namespace YourProjectName.Models
{
    public class AppDbContext : DbContext
    {
        public AppDbContext(DbContextOptions<AppDbContext> options) : base(options) { }

        public DbSet<Monster> Monsters { get; set; }

        protected override void OnModelCreating(ModelBuilder modelBuilder)
        {
            base.OnModelCreating(modelBuilder);

            // Configure the Monster entity
            modelBuilder.Entity<Monster>()
                .ToTable("Monsters")
                .HasKey(m => m.MonsterId);

            // Add any additional configuration here
        }
    }
}

```
> Take a moment to go through everything happening in this file. It is very important that this is set up correctly so that everything we need is put into our database. If you forget to add a DbSet for a model, it will not appear in your database when you migrate.

## Connecting to MySQL

5. If you already have this file, you will see some content written inside. It is okay to overwrite this content with the code below. The important piece we need is the ConnectionStrings. Inside this object is our DefaultConnection which has all our login information for MySQL. 

```cs
{   

    "Logging": {     

        "LogLevel": {       

            "Default": "Information",       

            "Microsoft.AspNetCore": "Warning"     

        }   

    }, 

    "AllowedHosts": "*",     

    "ConnectionStrings":     

    {         

        "DefaultConnection": "Server=localhost;port=3306;userid=root;password=root;database=monsterdb;"     

    } 

} 
```

- Update userid to your userid for MySQL (this is often root, but you may have changed it or made your own. You can verify this by logging in to MySQL Workbench and seeing what options you have). 
- Update password to your password to MySQL. You can verify what this password is by logging into Workbench. If you have taken courses with us previously that use MySQL, you likely set up this password to be "root", but it is good to check. 
- Update database to be a unique name for your project. You must always do this. This will be the name of your database in MySQL. If you leave it at a default name when the time comes to migrate it will add your tables to an existing database. That could cause some major problems! Be sure to change this every time you start a project. 
- Optional: If for whatever reason you had to change the port MySQL runs on (by default it is 3306) then update that as well. If you are unsure if this has happened to you, it is unlikely it has changed. 

6. Now that we have a connection string for MySQL, it's time to use it and establish our connection. To do this, we'll turn our attention to Program.cs, where we have some new lines of code to add:

```cs
// Add this using statement 

using Microsoft.EntityFrameworkCore; 

// You will need access to your models for your context file 

using ProjectName.Models; 

// Builder code from before 

var builder = WebApplication.CreateBuilder(args); 

// Create a variable to hold your connection string 

var connectionString = builder.Configuration.GetConnectionString("DefaultConnection"); 

// All your builder.services go here 

// And we will add one more service 

// Make sure this is BEFORE var app = builder.Build()!! 

builder.Services.AddDbContext<MyContext>(options => 

{ 

    options.UseMySql(connectionString, ServerVersion.AutoDetect(connectionString)); 

}); 

// The rest of the code below 
```

## Migrations and Controllers
7. Migrations are created using Entity Framework's command line tools. First, we have to add a migration by running the following line:

```bash
dotnet ef migrations add FirstMigration 
```

8. Once that is done, all that is left is to apply it to the database. We do this with another console command:

```bash
dotnet ef database update 
```

9. The final step is to set up our controller to be able to access the information from our database. We will do this using that handy Context file we made before. With it, we can do what is called dependency injection. We will be initializing our context in our controller's constructor method, where the service can be injected to provide us with all the tools we need without the bloat we could have had without our middleman handling talking to the database.

```cs
// Using statements 

using System.Diagnostics; 

using Microsoft.AspNetCore.Mvc; 

using YourProjectName.Models; 

namespace YourProjectName.Controllers;     

public class HomeController : Controller 

{     

    private readonly ILogger<HomeController> _logger; 

    private MyContext _context;          
    public HomeController(ILogger<HomeController> logger, MyContext context)     

    {         

        _logger = logger; 
        _context = context;     

    }          

    [HttpGet("")]     

    public IActionResult Index()     

    {      
        List<Monster> AllMonsters = _context.Monsters.ToList(); 

        return View();     

    }  

} 
```

`CREATE`

`Add data to a database using EF Core's built-in tools. `

Let's say we have a form that takes in information about a monster. You have all your validations set up and all you need now is to save your new monster to the database. To do this, you will Add the new monster to the database. Our code will look the exact same as it has in previous assignments, only now we will have two new lines of code for talking to our database: 

```cs
[HttpPost("monsters/create")]
public IActionResult CreateMonster(Monster newMon)
{
    if (ModelState.IsValid)
    {
        _context.Add(newMon);
        _context.SaveChanges();

        return RedirectToAction("SomeAction"); // Replace "SomeAction" with the name of the action method to redirect to
    }
    else
    {
        // If validation fails, add errors to the ModelState
        foreach (var error in ModelState.Values.SelectMany(v => v.Errors))
        {
            // Log or handle validation errors as needed
        }

        // Return to the view to display validation errors
        return View("Create", newMon);
    }
}

```

`READ`

`Extract data from a database to render on a cshtml page. `

`Identify the different types of data that can result from querying a database. `

Now that we have data in our database, it's time to bring it out and render it on the screen. This is where all that work we did earlier with LINQ comes into play. Let's take a look at a few examples to see this in action: 

```cs
[HttpGet("")]
public IActionResult Index()
{
    try
    {
        ViewBag.AllMonsters = _context.Monsters.ToList();
        ViewBag.AllMikes = _context.Monsters.Where(n => n.Name == "Mike").ToList(); // Add .ToList() to execute the query
        ViewBag.MostRecent = _context.Monsters.OrderByDescending(u => u.CreatedAt).Take(5).ToList();
        ViewBag.MatchedDesc = _context.Monsters.FirstOrDefault(u => u.Description == "Super scary");
        return View();
    }
    catch (Exception ex)
    {
        // Log or handle the exception as needed
        return StatusCode(500); // Return a 500 Internal Server Error response
    }
}

```

`UPDATE`

`Render an edit form in an ASP.NET Core MVC application. `

`Apply changes to an existing item in a database and save the changes. `

Once you can get to an edit page, the next task is to auto-populate it with data about the item. The best way to do this is to query for it and pass the item down through ViewModel 

```html
<form asp-action="UpdateMonster" asp-controller="Home" asp-route-MonsterId="@Model.MonsterId" method="post"> 

@* the rest of our form *@ 
```

```cs
[HttpPost("monsters/{MonsterId}/update")]
public IActionResult UpdateMonster(Monster newMon, int MonsterId)
{
    try
    {
        Monster oldMonster = _context.Monsters.FirstOrDefault(i => i.MonsterId == MonsterId);
        if (oldMonster != null && ModelState.IsValid)
        {
            oldMonster.Name = newMon.Name;
            oldMonster.Height = newMon.Height;
            oldMonster.Description = newMon.Description;
            oldMonster.UpdatedAt = DateTime.Now;
            _context.SaveChanges();
            return RedirectToAction("Index");
        }
        else
        {
            return View("EditMonster", oldMonster);
        }
    }
    catch (Exception ex)
    {
        // Log or handle the exception as needed
        return StatusCode(500); // Return a 500 Internal Server Error response
    }
}

```

### Handling a POST Request
The steps to update our object go like this: 
- Trigger a post request that contains the updated instance from our form and the ID of that instance. 
-  Find the old version of the instance in your database 
> Tip: you may want to write something that verifies what you get is not null. 
- Verify that the new version of our instance passes validations. 
- If it does not, show error messages and try again. 
- Overwrite the old version with the new version. 
- Save your changes. 
- Redirect to an appropriate page. 

We use asp-action now, which relies on the name of the action rather than the name of the route. So how do we pass parameters in our routes? Like this: 

```html
<form asp-action="UpdateMonster" asp-controller="Home" asp-route-MonsterId="@Model.MonsterId" method="post">
    .
    .
    .
</form>
```

We can pass any parameter we want through our route using asp-route-YourParamNameHere (and you can use as many of these as you like in one form). Once this is done, we can get into the rest of the steps fairly quickly. Most of the steps are commands that should already feel familiar to you. 

```cs
[HttpPost("monsters/{MonsterId}/update")]
public IActionResult UpdateMonster(Monster newMon, int MonsterId)
{
    try
    {
        Monster oldMonster = _context.Monsters.FirstOrDefault(i => i.MonsterId == MonsterId);
        if (oldMonster != null && ModelState.IsValid)
        {
            oldMonster.Name = newMon.Name;
            oldMonster.Height = newMon.Height;
            oldMonster.Description = newMon.Description;
            oldMonster.UpdatedAt = DateTime.Now;
            _context.SaveChanges();
            return RedirectToAction("Index");
        }
        else
        {
            return View("EditMonster", oldMonster);
        }
    }
    catch (Exception ex)
    {
        // Log or handle the exception as needed
        return StatusCode(500); // Return a 500 Internal Server Error response
    }
}

```

`DELETE`
`Query data for the purpose of deleting it from a database. `

`Compare and contrast SingleOrDefault and FirstOrDefault. `

 

Also similarly to the update method, we will want to trigger a delete using a POST request. This is because we want to ensure it is not so easy for people to manipulate our website to delete or edit things they have no business modifying.You can make your delete feature by creating a form that only has a submit button. This is your delete icon. Below is an example of what your delete button may look like: 

```html
<form asp-action="DestroyMonster" asp-controller="Home" asp-route-MonsterId="@Model.MonsterId" method="post"> 

    <input type="submit" value="Delete"> 

</form> 
```

With our post request triggered, we can head to our controller to handle it. The key term for deleting something from a database is to Remove it. 

```cs
[HttpPost("monsters/{MonsterId}/destroy")] 
public IActionResult DestroyMonster(int MonsterId) 
{ 
    Monster? MonToDelete = _context.Monsters.SingleOrDefault(i => i.MonsterId == MonsterId); 
    if (MonToDelete == null) 
    { 
        return NotFound(); // return a 404 Not Found status code
    } 
    else 
    { 
        _context.Monsters.Remove(MonToDelete); 
        _context.SaveChanges(); 
        return RedirectToAction("Index"); 
    } 
}

```

### SingleOrDefault vs FirstOrDefault 

Deleting is a pretty serious action that could have major consequences if done incorrectly. When a FirstOrDefault query runs, it finds all instances in a database that could match our query and simply returns the first one it finds. With SingleOrDefault, it is designed to throw an error if it encounters more than one instance that matches our query. 

 