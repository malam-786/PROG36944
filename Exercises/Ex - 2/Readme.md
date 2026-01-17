# ASP.NET Core MVC Exercise: Library Catalog System

## Objective
Students will implement a **complete ASP.NET Core MVC application** with **Dependency Injection container** to build a Library Catalog System.

**Skills Practiced:**
- ✓ ASP.NET Core MVC architecture (Model, View, Controller)
- ✓ Built-in Dependency Injection container (`AddSingleton`)
- ✓ Service layer pattern with in-memory data
- ✓ Razor views and forms
- ✓ Routing and HTTP operations

**Time:** 30-45 minutes
**Difficulty:** Beginner (DI focus, no database)

---

## Project Overview

You're building a **Library Catalog System** where:
- **Librarians** can add books to catalog
- **Users** can view available books
- **System** tracks book information in memory
- **Services** handle business logic with DI (Singleton)
- **No database needed** - focus on DI patterns

### Architecture
```
Book (Data Model)
   ↓
IBookService (Interface)
   ↓
BookService (Implementation - Singleton)
   ↓
BookController (MVC Controller)
   ↓
View (Razor Pages)
```

---

## Part 1: Project Setup (Provided)

### Create New ASP.NET Core MVC Project

```bash
dotnet new mvc -n LibraryCatalog
cd LibraryCatalog
```

No additional NuGet packages needed!

---

## Part 2: Data Model (Provided)

### Book.cs

```csharp
namespace LibraryCatalog.Models
{
    public class Book
    {
        public int BookId { get; set; }
        public string Title { get; set; } = string.Empty;
        public string Author { get; set; } = string.Empty;
        public string Category { get; set; } = string.Empty;  // Fiction, Science, History, etc.
        public int CopiesAvailable { get; set; }
        public bool IsAvailable { get; set; } = true;
        public DateTime AddedDate { get; set; } = DateTime.Now;
    }
}
```
---
### AddBookViewModel.cs

```csharp
using System.ComponentModel.DataAnnotations;

namespace LibraryCatalog.Models
{
    public class AddBookViewModel
    {
        [Required(ErrorMessage = "Title is required")]
        [StringLength(200, MinimumLength = 2)]
        public string Title { get; set; } = string.Empty;

        [Required(ErrorMessage = "Author is required")]
        [StringLength(150, MinimumLength = 2)]
        public string Author { get; set; } = string.Empty;

        [Required(ErrorMessage = "Category is required")]
        public string Category { get; set; } = string.Empty;

        [Required(ErrorMessage = "Copies are required")]
        [Range(1, 100, ErrorMessage = "Copies must be between 1 and 100")]
        public int CopiesAvailable { get; set; }
    }
}
```

---

## Part 3: Service Interface (Students Complete)

### IBookService.cs - TODO

**Task:** Define the interface for book management

```csharp
using LibraryCatalog.Models;
using System.Collections.Generic;

namespace LibraryCatalog.Services
{
    public interface IBookService
    {
        // TODO: Add method to get all books
        // Return type: List<Book>
        // Method name: GetAllBooks()

        // TODO: Add method to get book by ID
        // Parameters: int bookId
        // Return type: Book?
        // Method name: GetBookById(int bookId)

        // TODO: Add method to get available books only
        // Return type: List<Book>
        // Method name: GetAvailableBooks()

        // TODO: Add method to add new book
        // Parameters: Book book
        // Return type: Book
        // Method name: AddBook(Book book)

        // TODO: Add method to remove book
        // Parameters: int bookId
        // Return type: bool
        // Method name: RemoveBook(int bookId)

        // TODO: Add method to borrow a book
        // Parameters: int bookId
        // Return type: bool
        // Method name: BorrowBook(int bookId)
    }
}
```
---
## Part 4: Service Implementation (Students Complete)

### BookService.cs - TODO

**Task:** Implement the IBookService with in-memory storage

```csharp
using LibraryCatalog.Models;
using System.Collections.Generic;
using System.Linq;

namespace LibraryCatalog.Services
{
    public class BookService : IBookService
    {
        // In-memory list to store books (no database)
        private List<Book> books = new()
        {
            new Book { BookId = 1, Title = "The Great Gatsby", Author = "F. Scott Fitzgerald", Category = "Fiction", CopiesAvailable = 3, IsAvailable = true },
            new Book { BookId = 2, Title = "To Kill a Mockingbird", Author = "Harper Lee", Category = "Fiction", CopiesAvailable = 0, IsAvailable = false },
            new Book { BookId = 3, Title = "1984", Author = "George Orwell", Category = "Science Fiction", CopiesAvailable = 5, IsAvailable = true },
            new Book { BookId = 4, Title = "The Origin of Species", Author = "Charles Darwin", Category = "Science", CopiesAvailable = 2, IsAvailable = true }
        };

        private int nextBookId = 5;  // For assigning new book IDs

        // TODO: Implement GetAllBooks()
        // Should return all books
        public List<Book> GetAllBooks()
        {
            // TODO: Return list of all books sorted by title
            throw new NotImplementedException();
        }

        // TODO: Implement GetBookById(int bookId)
        // Should return book with matching ID or null
        public Book? GetBookById(int bookId)
        {
            // TODO: Find and return book by ID, or null if not found
            throw new NotImplementedException();
        }

        // TODO: Implement GetAvailableBooks()
        // Should return only books with copies available
        public List<Book> GetAvailableBooks()
        {
            // TODO: Filter books where CopiesAvailable > 0
            throw new NotImplementedException();
        }

        // TODO: Implement AddBook(Book book)
        // Should add new book to list
        public Book AddBook(Book book)
        {
            // TODO: Assign new ID using nextBookId
            // TODO: Increment nextBookId
            // TODO: Add book to list
            // TODO: Return the book
            throw new NotImplementedException();
        }

        // TODO: Implement RemoveBook(int bookId)
        // Should remove book from list
        public bool RemoveBook(int bookId)
        {
            // TODO: Find book by ID
            // TODO: If found, remove it and return true
            // TODO: If not found, return false
            throw new NotImplementedException();
        }

        // TODO: Implement BorrowBook(int bookId)
        // Should decrease copies available
        public bool BorrowBook(int bookId)
        {
            // TODO: Find book by ID
            // TODO: If found and copies > 0, decrease CopiesAvailable by 1
            // TODO: If copies become 0, set IsAvailable to false
            // TODO: Return true if borrowed, false if not available
            throw new NotImplementedException();
        }
    }
}
```
---
## Part 5: Dependency Injection Setup (Students Complete)

### Program.cs - TODO

**Task:** Register service as Singleton in DI container

```csharp
using LibraryCatalog.Services;

var builder = WebApplication.CreateBuilder(args);

// TODO: Register BookService as Singleton
// All requests share the SAME instance
// This preserves book list across all users
// builder.Services.AddSingleton<IBookService, BookService>();

builder.Services.AddControllersWithViews();

var app = builder.Build();

if (!app.Environment.IsDevelopment())
{
    app.UseExceptionHandler("/Home/Error");
    app.UseHsts();
}

app.UseHttpsRedirection();
app.UseStaticFiles();
app.UseRouting();
app.UseAuthorization();

app.MapControllerRoute(
    name: "default",
    pattern: "{controller=Home}/{action=Index}/{id?}");

app.Run();
```
---
## Part 6: Controller (Students Complete)

### BookController.cs - TODO

**Task:** Implement controller with dependency injection

```csharp
using LibraryCatalog.Models;
using LibraryCatalog.Services;
using Microsoft.AspNetCore.Mvc;

namespace LibraryCatalog.Controllers
{
    public class BookController : Controller
    {
        // TODO: Declare private readonly field for IBookService
        private readonly IBookService _bookService;

        // TODO: Implement constructor that accepts IBookService
        // DI container automatically provides the singleton instance
        public BookController(IBookService bookService)
        {
            // TODO: Store the injected service
            throw new NotImplementedException();
        }

        // TODO: Implement Index action (GET /book)
        // Display all books
        public IActionResult Index()
        {
            // TODO: Get all books from service
            // TODO: Return View with books
            throw new NotImplementedException();
        }

        // TODO: Implement Available action (GET /book/available)
        // Display only available books
        public IActionResult Available()
        {
            // TODO: Get available books from service
            // TODO: Return View with available books
            throw new NotImplementedException();
        }

        // TODO: Implement Details action (GET /book/details/{id})
        // Display book details
        public IActionResult Details(int id)
        {
            // TODO: Get book by ID from service
            // TODO: If not found, return NotFound()
            // TODO: Return View with book
            throw new NotImplementedException();
        }

        // TODO: Implement Add GET action (GET /book/add)
        public IActionResult Add()
        {
            // TODO: Return View with empty AddBookViewModel
            throw new NotImplementedException();
        }

        // TODO: Implement Add POST action (POST /book/add)
        [HttpPost]
        public IActionResult Add(AddBookViewModel model)
        {
            // TODO: Check if ModelState is valid
            // TODO: Create new Book from model
            // TODO: Call service to add book
            // TODO: Redirect to Index
            throw new NotImplementedException();
        }

        // TODO: Implement Borrow action (POST /book/borrow/{id})
        // Decrease copies available
        [HttpPost]
        public IActionResult Borrow(int id)
        {
            // TODO: Call service to borrow book
            // TODO: If successful, redirect to Available
            // TODO: If not successful, return error message
            throw new NotImplementedException();
        }

        // TODO: Implement Remove action (POST /book/remove/{id})
        [HttpPost]
        public IActionResult Remove(int id)
        {
            // TODO: Call service to remove book
            // TODO: Redirect to Index
            throw new NotImplementedException();
        }
    }
}
```
---

## Part 7: Views (Students Complete)

### Views/Book/Index.cshtml - TODO

**Task:** Display all books in a table

```html
@model List<Book>

<!-- TODO: Add page title "All Books" -->
<!-- TODO: Add link to Add action -->

<!-- TODO: Create table with columns: -->
<!-- ID, Title, Author, Category, Copies, Actions -->

<!-- TODO: Loop through books and display each row -->

<!-- TODO: In Actions column, add links/buttons for: -->
<!-- Details, Borrow, Remove -->

<!-- TODO: Show message if no books found -->
```

---
### Views/Book/Add.cshtml - TODO

**Task:** Display form to add new book

```html
@model AddBookViewModel

<!-- TODO: Add page title "Add Book to Catalog" -->

<!-- TODO: Create form with fields: -->
<!-- Title, Author, Category, CopiesAvailable -->

<!-- TODO: Add validation error display -->
<!-- TODO: Add Submit button -->
<!-- TODO: Add Cancel link -->
```

---
### Views/Book/Details.cshtml - TODO

```html
@model Book

<!-- TODO: Display book title as heading -->
<!-- TODO: Display all book information -->
<!-- TODO: Add Borrow button (if available), Remove button, Back link -->
```

---
### Views/Book/Available.cshtml - TODO

```html
@model List<Book>

<!-- TODO: Display only available books (similar to Index) -->
<!-- TODO: Title: "Available Books" -->
<!-- TODO: Add Back to Full Catalog link -->
```

---

## Part 8: Testing Checklist

Students should verify:

- [ ] Service interface defined with all 6 methods
- [ ] BookService implements all interface methods
- [ ] Service registered as **Singleton** in Program.cs
- [ ] Controller receives service via constructor injection
- [ ] Index shows all books
- [ ] Available shows only books with copies > 0
- [ ] Details shows single book
- [ ] Add form works (adds to service)
- [ ] Borrow button decreases copies
- [ ] Remove button deletes book
- [ ] Data persists across page refreshes (Singleton!)

---

## Key Learning Points

### ✓ Dependency Injection with Singleton
```csharp
builder.Services.AddSingleton<IBookService, BookService>();
// ONE instance shared by ALL requests
// All users see same book list

public BookController(IBookService bookService)
{
    _bookService = bookService;  // Injected!
}
```

### ✓ Service Interface
```csharp
public interface IBookService
{
    List<Book> GetAllBooks();
    Book? GetBookById(int bookId);
    // ... more methods
}
```

### ✓ In-Memory Data
```csharp
private List<Book> books = new() { ... };
// Data lives in memory, resets when app restarts
```

### ✓ MVC Pattern
```
Model → Controller → View
```

---

## Expected Behavior

```
1. User visits /book/index
   → Controller calls _bookService.GetAllBooks()
   → Service returns books from memory
   → View displays all 4 seed books

2. User clicks "Add New Book"
   → GET /book/add
   → Shows empty form

3. User fills form and submits
   → POST /book/add
   → Controller calls _bookService.AddBook()
   → Service adds to in-memory list
   → Redirects to Index (new book appears!)

4. User clicks "Borrow"
   → POST /book/borrow/{id}
   → Service decreases CopiesAvailable by 1
   → Redirects to Available
   → Book still visible but with fewer copies

5. User clicks "Remove"
   → POST /book/remove/{id}
   → Service removes book from list
   → Redirects to Index (book gone!)

6. KEY: Same service instance is used by all requests
   → When User A adds a book, User B sees it immediately
   → Data persists across requests (Singleton!)
```

---

## Summary

This exercise teaches students:
1. ✓ ASP.NET Core MVC architecture
2. ✓ Dependency Injection (Singleton lifetime)
3. ✓ Service layer pattern
4. ✓ Razor views and forms
5. ✓ In-memory data management
6. ✓ Full CRUD with MVC

**Perfect introduction to ASP.NET Core DI before adding databases!**

