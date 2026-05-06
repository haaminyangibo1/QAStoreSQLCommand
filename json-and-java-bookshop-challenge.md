# Building a Bookshop Inventory — JSON & Java in IntelliJ

## What This Day Is About

Today you're going to build something real: a **Bookshop Inventory System**. By the end, you'll have a programme that can add books, list them, search for them, and **save everything to a file** so the data is still there next time you run it.

Along the way, you'll learn:
- What JSON is and why it matters
- How to turn Java objects into JSON (serialisation)
- How to turn JSON back into Java objects (deserialisation)
- How to read and write files
- How to structure a multi-class programme

**Take breaks between exercises.** This is a full day — there's no rush.

---

## Setting Up Your IntelliJ Project

This is the most important part. If the setup is right, everything else flows. Take your time here.

---

### Step 1: Create a New Maven Project

**Why Maven?** Maven is a tool that downloads libraries for you. We need a library called **Gson** (made by Google) to work with JSON. Maven fetches it automatically.

1. Open IntelliJ IDEA
2. Click **File → New → Project**
3. On the left sidebar, make sure **New Project** is selected (not "from existing sources")
4. Fill in:
   - **Name:** `BookshopInventory`
   - **Language:** Java
   - **Build system:** Maven
   - **JDK:** Pick whichever JDK you have (17 or 21 are both fine)
5. Click **Create**

IntelliJ will generate a project with a folder structure. Give it a moment — you'll see a progress bar at the bottom while it sets things up.

---

### Step 2: Add Gson to Your Project

1. In the left panel (Project view), find and open the file called `pom.xml` — it's in the root of your project
2. You'll see some XML. Find the `</project>` closing tag at the very bottom
3. **Just above** that `</project>` tag, paste this block:

```xml
    <dependencies>
        <dependency>
            <groupId>com.google.code.gson</groupId>
            <artifactId>gson</artifactId>
            <version>2.11.0</version>
        </dependency>
    </dependencies>
```

Your `pom.xml` should now look something like this (the exact `groupId` and `artifactId` for YOUR project will differ — that's fine):

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
         http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>org.example</groupId>
    <artifactId>BookshopInventory</artifactId>
    <version>1.0-SNAPSHOT</version>

    <dependencies>
        <dependency>
            <groupId>com.google.code.gson</groupId>
            <artifactId>gson</artifactId>
            <version>2.11.0</version>
        </dependency>
    </dependencies>
</project>
```

4. A small icon will appear near the top right of the editor — it looks like a refresh symbol with an "M". **Click it.** This tells Maven to download Gson.
5. Wait for the progress bar at the bottom to finish. Once it's done, Gson is installed.

**If you don't see the refresh icon:** Right-click on `pom.xml` in the left panel → Maven → Reload Project.

---

### Step 3: Create Your Main Class

1. In the left panel, navigate to: `src → main → java`
2. Right-click on the `java` folder → **New → Java Class**
3. Name it `Main`
4. IntelliJ creates `Main.java` for you. Replace the contents with:

```java
public class Main {
    public static void main(String[] args) {
        System.out.println("Bookshop Inventory — ready to go!");
    }
}
```

5. Click the green **Run** triangle next to `main` (or press Shift+F10)

**Expected Output:**
```
Bookshop Inventory — ready to go!
```

If you see that, your project is set up correctly. Everything from here builds on this.

---

# Exercise 1: What Is JSON? (No Code Yet)

## What You're Learning
- What JSON looks like
- Why programmers use it
- How it relates to Java objects

## Read This First

JSON stands for **JavaScript Object Notation**. Despite the name, it has nothing to do with JavaScript really — it's just a way of writing structured data as text.

Here's what a book looks like in JSON:

```json
{
    "title": "Ficciones",
    "author": "Jorge Luis Borges",
    "price": 8.99,
    "inStock": true
}
```

That's it. Curly braces, key-value pairs, commas between them.

**The rules are simple:**
- Keys are always strings (in double quotes)
- Values can be: strings, numbers, booleans (`true`/`false`), arrays, other objects, or `null`
- Pairs are separated by commas
- The whole thing is wrapped in `{ }`

Here's a list (array) of books in JSON:

```json
[
    {
        "title": "Ficciones",
        "author": "Jorge Luis Borges",
        "price": 8.99,
        "inStock": true
    },
    {
        "title": "Mrs Dalloway",
        "author": "Virginia Woolf",
        "price": 7.99,
        "inStock": false
    }
]
```

Square brackets `[ ]` mean "this is a list." Each item in the list is a full JSON object.

---

## Why Should You Care?

Almost every app you'll ever build needs to **store data** or **send data to another system**. JSON is the most common format for both. When you use an API (like a weather app, a payment system, or a social media feed), the data comes back as JSON. When you save settings or records, JSON is a clean way to do it.

Today, we're going to use JSON to save our bookshop's inventory to a file — and load it back.

---

## Try This (Paper Exercise)

Before touching any code, write out the JSON for these three items by hand (in a text file, on paper, wherever):

1. A book: "The Waves" by Virginia Woolf, £12.99, in stock
2. A book: "Samskara" by U.R. Ananthamurthy, £9.50, not in stock
3. A book: "Census" by Jesse Ball, £10.00, in stock

Check: Does each object have curly braces? Are strings in quotes? Are numbers NOT in quotes? Are booleans lowercase?

---

## Key Takeaway

**JSON is just structured text. It maps almost perfectly onto Java objects — a JSON object is like a Java class, and a JSON array is like a Java List.**

---

# Exercise 2: The Book Class (Your Domain Model)

## What You're Learning
- How to design a class that maps to JSON
- Using proper Java conventions (private fields, getters, constructor)
- The `toString()` method

## The Task
**Create a Book class that represents a single book in the shop.**

### Step-by-Step Instructions

**Step 1:** In IntelliJ, right-click on the `java` folder (under `src/main/java`) → **New → Java Class** → Name it `Book`

**Step 2:** Replace the contents with this:

```java
public class Book {
    private String title;
    private String author;
    private double price;
    private boolean inStock;

    // Constructor
    public Book(String title, String author, double price, boolean inStock) {
        this.title = title;
        this.author = author;
        this.price = price;
        this.inStock = inStock;
    }

    // Getters
    public String getTitle() {
        return title;
    }

    public String getAuthor() {
        return author;
    }

    public double getPrice() {
        return price;
    }

    public boolean isInStock() {
        return inStock;
    }

    // Setter — we'll use this later to update stock
    public void setInStock(boolean inStock) {
        this.inStock = inStock;
    }

    // toString — controls what prints when you print a Book
    @Override
    public String toString() {
        String stock = inStock ? "In Stock" : "Out of Stock";
        return title + " by " + author + " — £" + String.format("%.2f", price) + " (" + stock + ")";
    }
}
```

**Step 3:** Now update `Main.java` to test it:

```java
public class Main {
    public static void main(String[] args) {
        Book book1 = new Book("Ficciones", "Jorge Luis Borges", 8.99, true);
        Book book2 = new Book("Mrs Dalloway", "Virginia Woolf", 7.99, false);

        System.out.println(book1);
        System.out.println(book2);
    }
}
```

**Step 4:** Run it (green triangle or Shift+F10)

**Expected Output:**
```
Ficciones by Jorge Luis Borges — £8.99 (In Stock)
Mrs Dalloway by Virginia Woolf — £7.99 (Out of Stock)
```

---

## Understanding What You Just Did

**`this.title = title;`** — The word `this` means "the object being created." It distinguishes the field (`this.title`) from the parameter (`title`) when they have the same name.

**`@Override`** — This annotation tells Java "I'm deliberately replacing the default `toString()` method." Every Java object has a `toString()`, but the default one prints ugly nonsense like `Book@4a574795`. We're replacing it with something readable.

**`String.format("%.2f", price)`** — This formats the price to 2 decimal places. Without it, `8.99` might print as `8.990000000000001` (floating point weirdness).

**`inStock ? "In Stock" : "Out of Stock"`** — This is a ternary expression. It's a compact if/else: if `inStock` is true, use "In Stock"; otherwise use "Out of Stock".

---

## Try This Yourself

**Challenge 1:** Create a third book and print all three.

**Challenge 2:** Try printing `book1.getTitle()` on its own. Then try `book1.getPrice()`. Make sure you're comfortable calling getters.

**Challenge 3:** Call `book2.setInStock(true)` and then print `book2` again. See how the output changes.

---

## Key Takeaway

**Your Book class is the foundation. Every field in this class will become a key in the JSON. `title` → `"title"`, `author` → `"author"`, and so on. Gson handles that mapping automatically.**

---

# Exercise 3: Turning a Book Into JSON (Serialisation)

## What You're Learning
- What serialisation means
- How to use Gson to convert a Java object into a JSON string
- Pretty printing

## The Concept

**Serialisation** = turning a Java object into text (JSON). You're translating from Java's world to a format that can be saved to a file or sent over the internet.

Java Object → JSON string. That's all serialisation is.

---

## The Task
**Convert a Book object into a JSON string and print it.**

### Step-by-Step Instructions

**Step 1:** Update `Main.java` to this:

```java
import com.google.gson.Gson;
import com.google.gson.GsonBuilder;

public class Main {
    public static void main(String[] args) {
        Book book = new Book("Ficciones", "Jorge Luis Borges", 8.99, true);

        // Create a Gson object with pretty printing
        Gson gson = new GsonBuilder().setPrettyPrinting().create();

        // Convert the Book to JSON
        String json = gson.toJson(book);

        System.out.println("Book as JSON:");
        System.out.println(json);
    }
}
```

**Step 2:** Run it

**Expected Output:**
```
Book as JSON:
{
  "title": "Ficciones",
  "author": "Jorge Luis Borges",
  "price": 8.99,
  "inStock": true
}
```

---

## Understanding What You Just Did

**`import com.google.gson.Gson;`** — This tells Java to use the Gson library you added via Maven. Without this line, Java wouldn't know what `Gson` is.

**`new GsonBuilder().setPrettyPrinting().create();`** — This creates a Gson object that formats JSON nicely with indentation. Without `setPrettyPrinting()`, the output would all be on one line: `{"title":"Ficciones","author":"Jorge Luis Borges","price":8.99,"inStock":true}`. Both are valid JSON — pretty printing is just for human readability.

**`gson.toJson(book)`** — This is where the magic happens. Gson looks at your Book object, finds all the fields, and converts them into a JSON string. It maps Java field names directly to JSON keys.

---

## Try This Yourself

**Challenge 1:** Create a second book and convert it to JSON too. Print both.

**Challenge 2:** Try creating a Gson without pretty printing:
```java
Gson gson = new Gson();
```
Print the result. Notice it's all on one line. Both are valid — just different formatting.

**Challenge 3:** Look at the JSON output and compare it to your Book class. Notice how every private field appears in the JSON. Gson reads private fields directly (using reflection) — it doesn't need your getters.

---

## Key Takeaway

**`gson.toJson(object)` converts any Java object to a JSON string. Gson figures out the field names and values automatically. One line of code.**

---

# Exercise 4: Turning JSON Into a Book (Deserialisation)

## What You're Learning
- What deserialisation means
- How to use Gson to create a Java object from a JSON string
- The importance of matching field names

## The Concept

**Deserialisation** = turning JSON text back into a Java object. The reverse of what you just did.

JSON string → Java Object. That's all deserialisation is.

---

## The Task
**Take a JSON string and convert it into a Book object.**

### Step-by-Step Instructions

**Step 1:** Update `Main.java` to this:

```java
import com.google.gson.Gson;
import com.google.gson.GsonBuilder;

public class Main {
    public static void main(String[] args) {
        // This is a JSON string — imagine it came from a file or an API
        String json = """
                {
                    "title": "Mrs Dalloway",
                    "author": "Virginia Woolf",
                    "price": 7.99,
                    "inStock": false
                }
                """;

        // Create Gson
        Gson gson = new Gson();

        // Convert JSON to a Book object
        Book book = gson.fromJson(json, Book.class);

        // Use it like any normal Book object!
        System.out.println(book);
        System.out.println("Title: " + book.getTitle());
        System.out.println("In stock? " + book.isInStock());
    }
}
```

**Step 2:** Run it

**Expected Output:**
```
Mrs Dalloway by Virginia Woolf — £7.99 (Out of Stock)
Title: Mrs Dalloway
In stock? false
```

---

## Understanding What You Just Did

**`""" ... """`** — These triple quotes are a **text block** (Java 13+). They let you write multi-line strings without messy escape characters. Much cleaner for writing JSON inline.

**`gson.fromJson(json, Book.class)`** — This is the counterpart to `toJson()`. It takes two arguments:
1. The JSON string to parse
2. The class to create (`Book.class` tells Gson "make a Book object from this")

Gson matches JSON keys to field names. `"title"` in the JSON maps to `private String title` in your Book class. **The names must match exactly.** If the JSON said `"bookTitle"` but your field was called `title`, it wouldn't work.

**After deserialisation, you have a normal Java object.** You can call `getTitle()`, `isInStock()`, `toString()` — everything works exactly as if you'd created it with `new Book(...)`.

---

## Try This Yourself

**Challenge 1:** Change the JSON string to a different book. Run it and confirm the object is correct.

**Challenge 2:** What happens if you remove a field from the JSON? Try removing the `"inStock"` line:
```java
String json = """
        {
            "title": "Test Book",
            "author": "Test Author",
            "price": 5.99
        }
        """;
```
Run it. What value does `isInStock()` return? (Spoiler: it defaults to `false` because `boolean` defaults to `false` in Java.)

**Challenge 3:** What happens if you add an EXTRA field to the JSON that doesn't exist in your Book class?
```java
String json = """
        {
            "title": "Test Book",
            "author": "Test Author",
            "price": 5.99,
            "inStock": true,
            "pageCount": 350
        }
        """;
```
Run it. Gson ignores the extra field — no error. This is useful to know.

---

## Key Takeaway

**`gson.fromJson(jsonString, YourClass.class)` creates a Java object from JSON. Field names must match. Missing fields get Java defaults. Extra fields are ignored.**

---

# Exercise 5: Working With Lists of Books (JSON Arrays)

## What You're Learning
- How to serialise a list of objects to a JSON array
- How to deserialise a JSON array back into a list
- Working with `TypeToken` (a Gson thing you need for lists)

## The Task
**Convert a list of books to JSON, and convert JSON back into a list.**

### Step-by-Step Instructions

**Step 1:** Update `Main.java` to this:

```java
import com.google.gson.Gson;
import com.google.gson.GsonBuilder;
import com.google.gson.reflect.TypeToken;
import java.lang.reflect.Type;
import java.util.ArrayList;
import java.util.List;

public class Main {
    public static void main(String[] args) {
        // Create a list of books
        List<Book> books = new ArrayList<>();
        books.add(new Book("Ficciones", "Jorge Luis Borges", 8.99, true));
        books.add(new Book("Mrs Dalloway", "Virginia Woolf", 7.99, false));
        books.add(new Book("Census", "Jesse Ball", 10.00, true));

        Gson gson = new GsonBuilder().setPrettyPrinting().create();

        // --- SERIALISE: List → JSON ---
        String json = gson.toJson(books);
        System.out.println("Books as JSON array:");
        System.out.println(json);

        System.out.println("\n--- Now converting back ---\n");

        // --- DESERIALISE: JSON → List ---
        Type bookListType = new TypeToken<List<Book>>(){}.getType();
        List<Book> loadedBooks = gson.fromJson(json, bookListType);

        System.out.println("Loaded " + loadedBooks.size() + " books:");
        for (Book b : loadedBooks) {
            System.out.println("  " + b);
        }
    }
}
```

**Step 2:** Run it

**Expected Output:**
```
Books as JSON array:
[
  {
    "title": "Ficciones",
    "author": "Jorge Luis Borges",
    "price": 8.99,
    "inStock": true
  },
  {
    "title": "Mrs Dalloway",
    "author": "Virginia Woolf",
    "price": 7.99,
    "inStock": false
  },
  {
    "title": "Census",
    "author": "Jesse Ball",
    "price": 10.0,
    "inStock": true
  }
]

--- Now converting back ---

Loaded 3 books:
  Ficciones by Jorge Luis Borges — £8.99 (In Stock)
  Mrs Dalloway by Virginia Woolf — £7.99 (Out of Stock)
  Census by Jesse Ball — £10.00 (In Stock)
```

---

## Understanding What Changed

**Serialising a list** is easy — `gson.toJson(books)` works with lists exactly the same way as single objects. Gson automatically wraps it in square brackets.

**Deserialising a list** needs one extra piece: `TypeToken`. Here's why.

When you wrote `gson.fromJson(json, Book.class)`, you told Gson the type directly. But Java has a quirk: you can't write `List<Book>.class`. Generics are erased at runtime (it's a Java thing — annoying but true). So Gson provides `TypeToken` as a workaround:

```java
Type bookListType = new TypeToken<List<Book>>(){}.getType();
```

This says: "The type I want is a `List` of `Book` objects." You pass this to `fromJson` instead of a `.class`.

**You only need TypeToken for generic types (lists, maps, etc.).** For single objects like `Book`, `.class` works fine.

---

## Try This Yourself

**Challenge 1:** Add two more books to the list. Run it and check the JSON array now has 5 items.

**Challenge 2:** After loading the books back, use a loop to find and print only the books that are in stock:

```java
for (Book b : loadedBooks) {
    if (b.isInStock()) {
        System.out.println("Available: " + b);
    }
}
```

**Challenge 3:** After loading the books, calculate and print the total value of all in-stock books.

---

## Key Takeaway

**Lists work with `toJson()` just like single objects. For `fromJson()`, lists need `TypeToken` to tell Gson the generic type. Once loaded, it's a normal Java list — loop, search, filter, whatever you need.**

---

# Exercise 6: Reading and Writing JSON Files (Persistence)

## What You're Learning
- How to write JSON to a file on disk
- How to read JSON from a file
- What `try-with-resources` is
- Your data now survives between runs of the programme

## Why This Matters

Right now, every time you run your programme, the books disappear. You create them in `main()`, they exist in memory, and they're gone when the programme stops.

With file I/O, you can **save** your data and **load** it next time. This is the difference between a toy exercise and a usable programme.

---

## The Task
**Write a list of books to a JSON file, then read it back.**

### Step-by-Step Instructions

**Step 1:** Update `Main.java` to this:

```java
import com.google.gson.Gson;
import com.google.gson.GsonBuilder;
import com.google.gson.reflect.TypeToken;
import java.io.FileReader;
import java.io.FileWriter;
import java.io.IOException;
import java.lang.reflect.Type;
import java.util.ArrayList;
import java.util.List;

public class Main {
    public static void main(String[] args) {
        List<Book> books = new ArrayList<>();
        books.add(new Book("Ficciones", "Jorge Luis Borges", 8.99, true));
        books.add(new Book("Mrs Dalloway", "Virginia Woolf", 7.99, false));
        books.add(new Book("Census", "Jesse Ball", 10.00, true));

        Gson gson = new GsonBuilder().setPrettyPrinting().create();
        String filename = "books.json";

        // --- WRITE to file ---
        try (FileWriter writer = new FileWriter(filename)) {
            gson.toJson(books, writer);
            System.out.println("Saved " + books.size() + " books to " + filename);
        } catch (IOException e) {
            System.out.println("Error saving file: " + e.getMessage());
        }

        // --- READ from file ---
        try (FileReader reader = new FileReader(filename)) {
            Type bookListType = new TypeToken<List<Book>>(){}.getType();
            List<Book> loadedBooks = gson.fromJson(reader, bookListType);

            System.out.println("\nLoaded " + loadedBooks.size() + " books from file:");
            for (Book b : loadedBooks) {
                System.out.println("  " + b);
            }
        } catch (IOException e) {
            System.out.println("Error reading file: " + e.getMessage());
        }
    }
}
```

**Step 2:** Run it

**Expected Output:**
```
Saved 3 books to books.json
  
Loaded 3 books from file:
  Ficciones by Jorge Luis Borges — £8.99 (In Stock)
  Mrs Dalloway by Virginia Woolf — £7.99 (Out of Stock)
  Census by Jesse Ball — £10.00 (In Stock)
```

**Step 3:** Now look in your IntelliJ project folder. You should see a new file called `books.json`. Click on it in the left panel to open it. You'll see the actual JSON — this is a real file on your computer.

---

## Understanding What You Just Did

**`FileWriter`** — Opens a file for writing. If the file doesn't exist, Java creates it. If it does exist, it overwrites it.

**`FileReader`** — Opens a file for reading. If the file doesn't exist, it throws an `IOException`.

**`try (FileWriter writer = ...)` — This is called try-with-resources.** The parentheses after `try` say "open this resource, and Java will close it automatically when the block finishes." Without this, you'd need to manually call `writer.close()` — and if you forget, you get resource leaks. Always use try-with-resources for files.

**`gson.toJson(books, writer)`** — Notice this version takes a `Writer` as the second argument. Instead of converting to a String, it writes directly to the file. More efficient.

**`gson.fromJson(reader, bookListType)`** — Same idea: reads directly from the file instead of a String.

**`catch (IOException e)`** — File operations can fail (file not found, permission denied, disk full). The `catch` block handles these errors gracefully instead of crashing.

---

## Try This Yourself

**Challenge 1:** Run the programme. Then open `books.json` in IntelliJ and manually add a fourth book to the JSON (copy the pattern of the existing entries). Save the file. Now **comment out** the writing section and run again — it should load 4 books from the file you edited by hand.

**Challenge 2:** What happens if you try to read a file that doesn't exist? Rename `books.json` to something else, and run just the reading section. See the error message in the catch block.

**Challenge 3:** Instead of `"books.json"`, try saving to a subfolder: `"data/books.json"`. What happens? (It will fail because the `data` folder doesn't exist. You'd need to create it first with `new File("data").mkdirs();`.)

---

## Key Takeaway

**`gson.toJson(object, writer)` saves to a file. `gson.fromJson(reader, type)` loads from a file. Always use try-with-resources to handle files safely. Your data now persists between runs.**

---

# Exercise 7: The Inventory Class (Organising Your Programme)

## What You're Learning
- How to create a class that manages a collection of objects
- Separating responsibilities (the Inventory handles books; Main handles user interaction)
- Search functionality

## Why This Matters

Right now, all the logic is crammed into `Main`. That works for small exercises, but real programmes separate concerns. The `Inventory` class will own the list of books and all the operations on it. `Main` will just be the starting point.

---

## The Task
**Create an Inventory class that manages the book collection with add, list, search, and save/load.**

### Step-by-Step Instructions

**Step 1:** Create a new Java class: right-click `java` folder → **New → Java Class** → Name it `Inventory`

**Step 2:** Replace its contents with this:

```java
import com.google.gson.Gson;
import com.google.gson.GsonBuilder;
import com.google.gson.reflect.TypeToken;
import java.io.FileReader;
import java.io.FileWriter;
import java.io.IOException;
import java.lang.reflect.Type;
import java.util.ArrayList;
import java.util.List;

public class Inventory {
    private List<Book> books;
    private final String filename;
    private final Gson gson;

    // Constructor — sets up the inventory with a file to save/load from
    public Inventory(String filename) {
        this.books = new ArrayList<>();
        this.filename = filename;
        this.gson = new GsonBuilder().setPrettyPrinting().create();
    }

    // Add a book to the inventory
    public void addBook(Book book) {
        books.add(book);
        System.out.println("Added: " + book.getTitle());
    }

    // List all books
    public void listBooks() {
        if (books.isEmpty()) {
            System.out.println("No books in inventory.");
            return;
        }
        System.out.println("\n--- Inventory (" + books.size() + " books) ---");
        for (int i = 0; i < books.size(); i++) {
            System.out.println("  " + (i + 1) + ". " + books.get(i));
        }
        System.out.println();
    }

    // Search by title (case-insensitive, partial match)
    public List<Book> searchByTitle(String query) {
        List<Book> results = new ArrayList<>();
        for (Book book : books) {
            if (book.getTitle().toLowerCase().contains(query.toLowerCase())) {
                results.add(book);
            }
        }
        return results;
    }

    // Search by author (case-insensitive, partial match)
    public List<Book> searchByAuthor(String query) {
        List<Book> results = new ArrayList<>();
        for (Book book : books) {
            if (book.getAuthor().toLowerCase().contains(query.toLowerCase())) {
                results.add(book);
            }
        }
        return results;
    }

    // Save all books to the JSON file
    public void save() {
        try (FileWriter writer = new FileWriter(filename)) {
            gson.toJson(books, writer);
            System.out.println("Saved " + books.size() + " books to " + filename);
        } catch (IOException e) {
            System.out.println("Error saving: " + e.getMessage());
        }
    }

    // Load books from the JSON file
    public void load() {
        try (FileReader reader = new FileReader(filename)) {
            Type bookListType = new TypeToken<List<Book>>(){}.getType();
            List<Book> loaded = gson.fromJson(reader, bookListType);
            if (loaded != null) {
                this.books = loaded;
                System.out.println("Loaded " + books.size() + " books from " + filename);
            }
        } catch (IOException e) {
            System.out.println("No existing file found. Starting with empty inventory.");
        }
    }

    // Get total number of books
    public int getCount() {
        return books.size();
    }
}
```

**Step 3:** Update `Main.java` to test it:

```java
public class Main {
    public static void main(String[] args) {
        Inventory inventory = new Inventory("books.json");

        // Add some books
        inventory.addBook(new Book("Ficciones", "Jorge Luis Borges", 8.99, true));
        inventory.addBook(new Book("Mrs Dalloway", "Virginia Woolf", 7.99, false));
        inventory.addBook(new Book("Census", "Jesse Ball", 10.00, true));
        inventory.addBook(new Book("The Waves", "Virginia Woolf", 12.99, true));

        // List everything
        inventory.listBooks();

        // Search
        System.out.println("Searching for 'Woolf':");
        for (Book b : inventory.searchByAuthor("Woolf")) {
            System.out.println("  Found: " + b);
        }

        // Save
        inventory.save();

        // Create a NEW inventory and load from file
        System.out.println("\n--- Loading into fresh inventory ---");
        Inventory loaded = new Inventory("books.json");
        loaded.load();
        loaded.listBooks();
    }
}
```

**Step 4:** Run it

**Expected Output:**
```
Added: Ficciones
Added: Mrs Dalloway
Added: Census
Added: The Waves

--- Inventory (4 books) ---
  1. Ficciones by Jorge Luis Borges — £8.99 (In Stock)
  2. Mrs Dalloway by Virginia Woolf — £7.99 (Out of Stock)
  3. Census by Jesse Ball — £10.00 (In Stock)
  4. The Waves by Virginia Woolf — £12.99 (In Stock)

Searching for 'Woolf':
  Found: Mrs Dalloway by Virginia Woolf — £7.99 (Out of Stock)
  Found: The Waves by Virginia Woolf — £12.99 (In Stock)
Saved 4 books to books.json

--- Loading into fresh inventory ---
Loaded 4 books from books.json

--- Inventory (4 books) ---
  1. Ficciones by Jorge Luis Borges — £8.99 (In Stock)
  2. Mrs Dalloway by Virginia Woolf — £7.99 (Out of Stock)
  3. Census by Jesse Ball — £10.00 (In Stock)
  4. The Waves by Virginia Woolf — £12.99 (In Stock)

```

---

## Understanding the Design

**Why a separate class?** Main shouldn't know about file I/O, search logic, or how books are stored. The `Inventory` class handles all of that. Main just says "add this," "search for that," "save." This separation makes the code easier to change later.

**`private final String filename`** — The `final` keyword means this value can't change after the constructor sets it. The inventory always saves to/loads from the same file.

**`searchByTitle` returns a `List<Book>`** — It doesn't print anything. It returns the results and lets the caller decide what to do with them. This is good design: methods should do one thing.

**The `load()` catch block** — If no file exists yet (first run), it catches the error and starts with an empty list instead of crashing. Graceful handling.

---

## Try This Yourself

**Challenge 1:** Add a method `searchByPriceRange(double min, double max)` that returns books within a price range.

**Challenge 2:** Add a method `removeBook(String title)` that removes the first book matching that title. Use `books.removeIf(...)` or loop with an iterator.

**Challenge 3:** Add a method `getInStockBooks()` that returns only the books currently in stock.

---

## Key Takeaway

**The Inventory class is a "manager" — it owns the data and the operations. This pattern (a class that wraps a collection and provides methods on it) is extremely common in real Java applications.**

---

# Exercise 8: The Full Programme (Interactive Menu)

## What You're Learning
- Reading user input with `Scanner`
- Building a loop-driven menu
- Connecting everything into a complete, runnable programme

## The Task
**Build an interactive menu so the user can add books, search, list, and save — all from the console.**

### Step-by-Step Instructions

**Step 1:** Replace `Main.java` with this:

```java
import java.util.List;
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        Inventory inventory = new Inventory("books.json");

        // Try to load existing data
        inventory.load();

        boolean running = true;

        System.out.println("===========================");
        System.out.println("  Welcome to the Bookshop  ");
        System.out.println("===========================");

        while (running) {
            System.out.println("What would you like to do?");
            System.out.println("  1. Add a book");
            System.out.println("  2. List all books");
            System.out.println("  3. Search by title");
            System.out.println("  4. Search by author");
            System.out.println("  5. Save and quit");
            System.out.print("Enter choice (1-5): ");

            String choice = scanner.nextLine().trim();

            switch (choice) {
                case "1":
                    addBook(scanner, inventory);
                    break;
                case "2":
                    inventory.listBooks();
                    break;
                case "3":
                    searchByTitle(scanner, inventory);
                    break;
                case "4":
                    searchByAuthor(scanner, inventory);
                    break;
                case "5":
                    inventory.save();
                    System.out.println("Goodbye!");
                    running = false;
                    break;
                default:
                    System.out.println("Please enter a number between 1 and 5.\n");
            }
        }

        scanner.close();
    }

    private static void addBook(Scanner scanner, Inventory inventory) {
        System.out.print("Title: ");
        String title = scanner.nextLine().trim();

        System.out.print("Author: ");
        String author = scanner.nextLine().trim();

        double price = 0;
        boolean validPrice = false;
        while (!validPrice) {
            System.out.print("Price (e.g. 9.99): ");
            try {
                price = Double.parseDouble(scanner.nextLine().trim());
                if (price >= 0) {
                    validPrice = true;
                } else {
                    System.out.println("Price can't be negative. Try again.");
                }
            } catch (NumberFormatException e) {
                System.out.println("That's not a valid number. Try again.");
            }
        }

        System.out.print("In stock? (yes/no): ");
        boolean inStock = scanner.nextLine().trim().toLowerCase().startsWith("y");

        inventory.addBook(new Book(title, author, price, inStock));
        System.out.println();
    }

    private static void searchByTitle(Scanner scanner, Inventory inventory) {
        System.out.print("Search title: ");
        String query = scanner.nextLine().trim();
        List<Book> results = inventory.searchByTitle(query);

        if (results.isEmpty()) {
            System.out.println("No books found matching \"" + query + "\".\n");
        } else {
            System.out.println("Found " + results.size() + " result(s):");
            for (Book b : results) {
                System.out.println("  " + b);
            }
            System.out.println();
        }
    }

    private static void searchByAuthor(Scanner scanner, Inventory inventory) {
        System.out.print("Search author: ");
        String query = scanner.nextLine().trim();
        List<Book> results = inventory.searchByAuthor(query);

        if (results.isEmpty()) {
            System.out.println("No books found by \"" + query + "\".\n");
        } else {
            System.out.println("Found " + results.size() + " result(s):");
            for (Book b : results) {
                System.out.println("  " + b);
            }
            System.out.println();
        }
    }
}
```

**Step 2:** Run it. This time, the programme doesn't just print and stop — it waits for your input in the console at the bottom of IntelliJ.

**Step 3:** Try this sequence:
1. Press `2` → List (might be empty or show previously saved books)
2. Press `1` → Add a book (follow the prompts)
3. Press `1` → Add another book
4. Press `2` → List — you should see both books
5. Press `3` → Search by title
6. Press `5` → Save and quit

**Step 4:** Run the programme again. Press `2` to list. **Your books are still there** — loaded from the file.

---

## Understanding the Structure

**The programme has three files, each with one job:**
- `Book.java` — defines what a book IS (data + toString)
- `Inventory.java` — manages the collection (add, search, save, load)
- `Main.java` — handles user interaction (menu, input, output)

This is separation of concerns. If you wanted to change how books are stored (say, use a database instead of JSON), you'd only change `Inventory.java`. Main and Book wouldn't need to change at all.

**`scanner.nextLine().trim()`** — `nextLine()` reads a full line of input. `trim()` removes any accidental spaces at the start or end.

**`Double.parseDouble()`** — Converts a string like `"9.99"` to the number `9.99`. It throws `NumberFormatException` if the input isn't a valid number, so we catch that and ask again.

**`private static void addBook(...)`** — We broke the menu actions into separate methods to keep `main()` clean. Each method handles one action. This is a habit worth building.

---

## Try This Yourself

**Challenge 1:** Add a menu option `6` that lets the user toggle a book's stock status. Show the list, ask for a book number, and flip the `inStock` value. (You'll need to add a `getBooks()` method to Inventory, or a `toggleStock(int index)` method.)

**Challenge 2:** Add a menu option that shows only in-stock books. If you did Challenge 3 from Exercise 7, you already have the method — just wire it up.

**Challenge 3:** Add input validation for the title and author — don't allow empty strings. Use a `while` loop like the price validation.

**Challenge 4 (Stretch):** Add a `removeBook` option to the menu. Show the numbered list, ask which number to remove, and remove it.

---

## Key Takeaway

**You've built a complete, working programme with three classes, file persistence, user interaction, search, and input validation. This is how real applications are structured — small classes with clear responsibilities, connected through a main entry point.**

---

# Summary: What You Learned Today

## The Concepts

**JSON** — A text format for structured data. Curly braces for objects, square brackets for arrays, key-value pairs.

**Serialisation** — Java object → JSON string. `gson.toJson(object)`.

**Deserialisation** — JSON string → Java object. `gson.fromJson(json, Class.class)`.

**TypeToken** — Needed when deserialising generic types like `List<Book>`. Java's type erasure means Gson can't figure out the type on its own.

**File I/O** — `FileWriter` to save, `FileReader` to load. Always wrap in try-with-resources.

**Separation of Concerns** — `Book` holds data, `Inventory` manages the collection, `Main` handles user interaction.

---

## Your Project Structure

```
BookshopInventory/
├── pom.xml                          ← Maven config (Gson dependency)
├── books.json                       ← Your saved data (created at runtime)
└── src/
    └── main/
        └── java/
            ├── Main.java            ← Entry point, menu, user input
            ├── Book.java            ← Data class for a single book
            └── Inventory.java       ← Manages collection, search, file I/O
```

---

## Quick Reference

**Add Gson (pom.xml):**
```xml
<dependency>
    <groupId>com.google.code.gson</groupId>
    <artifactId>gson</artifactId>
    <version>2.11.0</version>
</dependency>
```

**Object → JSON:**
```java
Gson gson = new GsonBuilder().setPrettyPrinting().create();
String json = gson.toJson(myObject);
```

**JSON → Object:**
```java
Book book = gson.fromJson(jsonString, Book.class);
```

**JSON → List:**
```java
Type listType = new TypeToken<List<Book>>(){}.getType();
List<Book> books = gson.fromJson(jsonString, listType);
```

**Save to file:**
```java
try (FileWriter writer = new FileWriter("data.json")) {
    gson.toJson(myList, writer);
}
```

**Load from file:**
```java
try (FileReader reader = new FileReader("data.json")) {
    List<Book> books = gson.fromJson(reader, listType);
}
```

---

## Extension Ideas (If You Finish Early)

1. **Categories** — Add a `String genre` field to Book. Add a search-by-genre method. Update the menu.

2. **Ratings** — Add an `int rating` field (1–5). Add a method that lists books sorted by rating. (Hint: look up `Collections.sort()` with a `Comparator`.)

3. **Statistics** — Add methods to Inventory: total value of stock, average price, number of out-of-stock books.

4. **Multiple file formats** — Try writing a second save method that outputs CSV instead of JSON. Compare the two formats.

5. **Error-proof the load** — What if someone corrupts the JSON file? Wrap the load in better error handling that tells the user exactly what went wrong.

---

You've got this. Take it one exercise at a time.
