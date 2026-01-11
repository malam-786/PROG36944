# Hands-On DI Exercise: Library Management System

## Objective
Students will implement a **manual Dependency Injection** system with **Singleton pattern** to build a simple Library Management System.

**Skills Practiced:**
- ✓ Constructor Injection
- ✓ Interface-based abstraction
- ✓ Singleton lifetime management
- ✓ Dependency wiring

**Time:** 45-60 minutes

---

## Project Overview

You're building a **Library Management System** where:
- **Students** can borrow and return books
- **Books** are stored in a library database
- **Library** manages inventory and tracks borrowing
- **Librarian** performs operations (singleton resource)

### Architecture
```
Book (Data Model)
   ↓
IBookRepository (Interface)
   ↓
BookRepository (Implementation)
   ↓
LibraryService (Business Logic)
   ↓
Librarian (Singleton Resource)
   ↓
Program.cs (Manual DI Wiring)
```

---

## Part 1: Data Models (Provided)

### Book.cs

```csharp
namespace LibraryManagement
{
    public class Book
    {
        public int BookId { get; set; }
        public string Title { get; set; } = string.Empty;
        public string Author { get; set; } = string.Empty;
        public bool IsAvailable { get; set; } = true;
    }
}
```

### Student.cs

```csharp
namespace LibraryManagement
{
    public class Student
    {
        public int StudentId { get; set; }
        public string Name { get; set; } = string.Empty;
        public List<int> BorrowedBooks { get; set; } = new();
    }
}
```

---

## Part 2: Interfaces (Students Complete)

### IBookRepository.cs - TODO

**Task:** Define the interface for book repository operations

```csharp
using System.Collections.Generic;

namespace LibraryManagement
{
    public interface IBookRepository
    {
        // TODO: Add method to get all books
        // Method signature: List<Book> GetAllBooks();

        // TODO: Add method to get a book by ID
        // Method signature: Book? GetBookById(int bookId);

        // TODO: Add method to check if a book is available
        // Method signature: bool IsBookAvailable(int bookId);

        // TODO: Add method to mark a book as borrowed
        // Method signature: void BorrowBook(int bookId);

        // TODO: Add method to mark a book as returned
        // Method signature: void ReturnBook(int bookId);
    }
}
```
---
## Part 3: Repository Implementation (Students Complete)

### BookRepository.cs - TODO

**Task:** Implement the IBookRepository interface with sample data

```csharp
using System;
using System.Collections.Generic;
using System.Linq;

namespace LibraryManagement
{
    public class BookRepository : IBookRepository
    {
        // Track how many instances are created
        private static int instanceCount = 0;
        private int instanceNumber;

        public BookRepository()
        {
            instanceCount++;
            instanceNumber = instanceCount;
            Console.WriteLine($"[BookRepository] Instance #{instanceNumber} created");
        }

        // In-memory database
        private List<Book> books = new()
        {
            new Book { BookId = 1, Title = "Clean Code", Author = "Robert Martin", IsAvailable = true },
            new Book { BookId = 2, Title = "Design Patterns", Author = "Gang of Four", IsAvailable = true },
            new Book { BookId = 3, Title = "SOLID Principles", Author = "Uncle Bob", IsAvailable = false },
            new Book { BookId = 4, Title = "C# Deep Dive", Author = "Jon Skeet", IsAvailable = true },
        };

        // TODO: Implement GetAllBooks()
        // Should return all books
        public List<Book> GetAllBooks()
        {
            // TODO: Return list of all books
            throw new NotImplementedException();
        }

        // TODO: Implement GetBookById()
        // Should return book with matching ID or null
        public Book? GetBookById(int bookId)
        {
            // TODO: Find and return book by ID
            throw new NotImplementedException();
        }

        // TODO: Implement IsBookAvailable()
        // Should check if book is available
        public bool IsBookAvailable(int bookId)
        {
            // TODO: Check book availability
            throw new NotImplementedException();
        }

        // TODO: Implement BorrowBook()
        // Should set book as not available
        public void BorrowBook(int bookId)
        {
            // TODO: Mark book as borrowed
            throw new NotImplementedException();
        }

        // TODO: Implement ReturnBook()
        // Should set book as available
        public void ReturnBook(int bookId)
        {
            // TODO: Mark book as returned
            throw new NotImplementedException();
        }

        public int GetInstanceNumber()
        {
            return instanceNumber;
        }
    }
}
```

---
## Part 4: Business Logic Service (Students Complete)

### LibraryService.cs - TODO

**Task:** Implement service that uses injected repository

```csharp
using System;
using System.Collections.Generic;

namespace LibraryManagement
{
    public class LibraryService
    {
        // TODO: Declare private readonly field for repository dependency
        private IBookRepository _repository;

        // TODO: Implement constructor injection
        // Should accept IBookRepository as parameter
        public LibraryService(IBookRepository repository)
        {
            // TODO: Store the injected repository
            throw new NotImplementedException();
        }

        // TODO: Implement method to list all available books
        // Should return only available books
        public List<Book> GetAvailableBooks()
        {
            // TODO: Get all books and filter for available ones
            throw new NotImplementedException();
        }

        // TODO: Implement method to borrow a book
        // Should check availability first, then borrow
        public bool BorrowBook(int bookId)
        {
            // TODO: Check if available, then borrow and return true/false
            throw new NotImplementedException();
        }

        // TODO: Implement method to return a book
        // Should return the book
        public void ReturnBook(int bookId)
        {
            // TODO: Call repository to return book
            throw new NotImplementedException();
        }

        // TODO: Add method to get repository instance number (for testing singleton)
        // Should return repository's instance number
        public int GetRepositoryInstanceNumber()
        {
            // TODO: Return the repository instance number
            throw new NotImplementedException();
        }
    }
}
```

---
## Part 5: Singleton Librarian (Students Complete)

### Librarian.cs - TODO

**Task:** Implement a singleton resource that tracks library operations

```csharp
using System;
using System.Collections.Generic;

namespace LibraryManagement
{
    /// <summary>
    /// The Librarian is a SINGLETON resource
    /// Only ONE instance should exist for the entire application
    /// Tracks all library operations
    /// </summary>
    public class Librarian
    {
        // TODO: Add static instance field for singleton
        // This should be private static

        // TODO: Add lock for thread safety
        // private static readonly object _lock = new object();

        // NOTE FOR STUDENTS:
        // The field below uses "Guid" to give the Librarian a unique ID.
        // You can research what it is, why it's useful, and how it works in C#.
        // Start here: https://learn.microsoft.com/en-us/dotnet/api/system.guid?view=net-10.0

        private Guid _librarianId;
        private List<string> _operationLog = new();
        private static int totalOperations = 0;

        // TODO: Make constructor private so only GetInstance() can create it
        public Librarian()
        {
            _librarianId = Guid.NewGuid();
            Console.WriteLine($"[Librarian {_librarianId.ToString().Substring(0, 8)}] Created (This should print ONCE)");
        }

        // TODO: Implement GetInstance() static method for singleton
        // Should create instance if null, then return it
        public static Librarian GetInstance()
        {
            // TODO: Implement singleton pattern
            // Check if instance is null
            // If null, create new Librarian()
            // Return the instance
            throw new NotImplementedException();
        }

        public void LogOperation(string operation)
        {
            totalOperations++;
            string logEntry = $"[Op #{totalOperations}] {DateTime.Now:HH:mm:ss} - {operation}";
            _operationLog.Add(logEntry);
            Console.WriteLine(logEntry);
        }

        public void PrintOperationLog()
        {
            Console.WriteLine("\n=== Librarian Operation Log ===");
            foreach (var log in _operationLog)
            {
                Console.WriteLine(log);
            }
        }

        public Guid GetLibrarianId()
        {
            return _librarianId;
        }
    }
}
```

---
## Part 6: Manual DI Setup (Students Complete)

### Program.cs - TODO

**Task:** Wire up all dependencies manually and test the system

```csharp
using System;
using System.Collections.Generic;

namespace LibraryManagement
{
    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine("=== Library Management System - DI Exercise ===\n");

            // TODO: STEP 1 - Create the repository (dependency)
            // IBookRepository repository = ???

            // TODO: STEP 2 - Create first library service (inject repository)
            // LibraryService libraryService1 = ???

            // TODO: STEP 3 - Create second library service (inject SAME repository - Singleton pattern)
            // LibraryService libraryService2 = ???

            // TODO: STEP 4 - Get singleton librarian
            // Librarian librarian = ???

            // ===== TESTING SECTION =====

            // TODO: TEST 1 - Display all available books
            Console.WriteLine("--- TEST 1: Available Books ---");
            // Use libraryService1 to get available books
            // Print each book: ID, Title, Author

            // TODO: TEST 2 - Borrow a book
            Console.WriteLine("\n--- TEST 2: Borrow Book ---");
            // Use libraryService1 to borrow book ID 1
            // Log operation with librarian
            // Result should be: true (successfully borrowed)

            // TODO: TEST 3 - Try to borrow same book again (should fail)
            Console.WriteLine("\n--- TEST 3: Try Borrow Again (Should Fail) ---");
            // Use libraryService2 to borrow book ID 1
            // Log operation with librarian
            // Result should be: false (not available)

            // TODO: TEST 4 - Return the book
            Console.WriteLine("\n--- TEST 4: Return Book ---");
            // Use libraryService1 to return book ID 1
            // Log operation with librarian

            // TODO: TEST 5 - Try to borrow again (should succeed now)
            Console.WriteLine("\n--- TEST 5: Borrow Again (Should Succeed) ---");
            // Use libraryService2 to borrow book ID 1
            // Log operation with librarian
            // Result should be: true (now available again)

            // TODO: TEST 6 - Verify Singleton Repository
            Console.WriteLine("\n--- TEST 6: Singleton Repository Verification ---");
            Console.WriteLine($"Service 1 repository instance: {libraryService1.GetRepositoryInstanceNumber()}");
            Console.WriteLine($"Service 2 repository instance: {libraryService2.GetRepositoryInstanceNumber()}");
            Console.WriteLine($"Are they the SAME? {libraryService1.GetRepositoryInstanceNumber() == libraryService2.GetRepositoryInstanceNumber()}");

            // TODO: TEST 7 - Verify Singleton Librarian
            Console.WriteLine("\n--- TEST 7: Singleton Librarian Verification ---");
            Librarian librarian2 = Librarian.GetInstance();
            Librarian librarian3 = Librarian.GetInstance();
            Console.WriteLine($"First librarian ID: {librarian.GetLibrarianId()}");
            Console.WriteLine($"Second call librarian ID: {librarian2.GetLibrarianId()}");
            Console.WriteLine($"Third call librarian ID: {librarian3.GetLibrarianId()}");
            Console.WriteLine($"Are they all the SAME? {librarian.GetLibrarianId() == librarian2.GetLibrarianId()}");
            
            // Print operation log
            librarian.PrintOperationLog();
        }
    }
}
```

---
## Expected Output

When completed correctly, the output should show:

```
=== Library Management System - DI Exercise ===

[BookRepository] Instance #1 created
--- TEST 1: Available Books ---
[ID: 1] Clean Code by Robert Martin
[ID: 2] Design Patterns by Gang of Four
[ID: 4] C# Deep Dive by Jon Skeet
[Op #1] HH:mm:ss - Listed available books

--- TEST 2: Borrow Book ---
Borrow result: SUCCESS
[Op #2] HH:mm:ss - Borrowed book ID 1

--- TEST 3: Try Borrow Again (Should Fail) ---
Borrow result: FAILED
[Op #3] HH:mm:ss - Attempted to borrow book ID 1 (already borrowed)

--- TEST 4: Return Book ---
Book returned successfully
[Op #4] HH:mm:ss - Returned book ID 1

--- TEST 5: Borrow Again (Should Succeed) ---
Borrow result: SUCCESS
[Op #5] HH:mm:ss - Borrowed book ID 1 (second time)

--- TEST 6: Singleton Repository Verification ---
Service 1 repository instance: 1
Service 2 repository instance: 1
Are they the SAME? True (Should be TRUE)

--- TEST 7: Singleton Librarian Verification ---
[Librarian 7f5c8d9a] Created (This should print ONCE)
First librarian ID: 7f5c8d9a-...
Second call librarian ID: 7f5c8d9a-...
Third call librarian ID: 7f5c8d9a-...
Are they all the SAME? True (Should be TRUE)

=== Librarian Operation Log ===
[Op #1] HH:mm:ss - Listed available books
[Op #2] HH:mm:ss - Borrowed book ID 1
[Op #3] HH:mm:ss - Attempted to borrow book ID 1 (already borrowed)
[Op #4] HH:mm:ss - Returned book ID 1
[Op #5] HH:mm:ss - Borrowed book ID 1 (second time)
```

---

## Key Learning Points

### ✓ Constructor Injection
```csharp
public LibraryService(IBookRepository repository)
{
    _repository = repository;  // Dependency injected!
}
```

### ✓ Interface-Based Design
```csharp
private IBookRepository _repository;  // Depends on abstraction, not concrete class
```

### ✓ Singleton Pattern
```csharp
public static Librarian GetInstance()
{
    if (_instance == null)
        _instance = new Librarian();  // Created once
    return _instance;                  // Always same instance
}
```

### ✓ Dependency Wiring
```csharp
IBookRepository repository = new BookRepository();
LibraryService service1 = new LibraryService(repository);
LibraryService service2 = new LibraryService(repository);  // Same repository!
```

---

## Verification Checklist

Students should verify:

- [ ] All TODOs are completed
- [ ] Code compiles without errors
- [ ] BookRepository instance shows "#1" (created once - singleton)
- [ ] Both services use same repository instance (singleton)
- [ ] Librarian constructor prints ONCE only (singleton)
- [ ] Cannot borrow unavailable book (business logic)
- [ ] Can borrow book after returning it (state change)
- [ ] All operations logged by librarian
- [ ] Singleton librarian has same ID across multiple GetInstance() calls

---

## Extension Challenges (Optional)

1. **Add Student Tracking**
   - Track which student borrowed which book
   - Add `StudentRepository` for student management

2. **Add Validation**
   - Validate book IDs before operations
   - Add error handling

3. **Add Transient Logger**
   - Create a `TransientLogger` service
   - Compare with Singleton Librarian
   - Show difference in instance creation

4. **Add Scoped Transaction**
   - Implement a `BorrowTransaction` scoped service
   - Handle multiple operations in single transaction

---

## Summary

This exercise teaches students:
1. How to design with interfaces
2. How to implement constructor injection
3. How to manage singleton lifetime manually
4. How to wire dependencies without a framework
5. How to test DI implementation
