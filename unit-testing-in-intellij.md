# Getting Started with Unit Testing in IntelliJ

## What You're Learning
- How to set up IntelliJ IDEA (a professional Java IDE)
- What unit testing is and why developers use it
- How to write and run JUnit tests
- How to test the classes you've already built (Box, Shape, Circle)

---

# Part 1: Setting Up IntelliJ IDEA

## Step 1: Download and Install IntelliJ

**Step 1.1:** Go to https://www.jetbrains.com/idea/download/

**Step 1.2:** Download the **Community Edition** (it's free — the one on the right). Choose the version for your operating system (Windows or Mac).

**Step 1.3:** Run the installer. Accept all the defaults — just keep clicking "Next" until it installs.

**Step 1.4:** Open IntelliJ IDEA when it finishes.

---

## Step 2: Create Your First IntelliJ Project

**Step 2.1:** On the welcome screen, click **"New Project"**

**Step 2.2:** Fill in the settings:
- **Name:** `BoxProject` (or whatever you like)
- **Location:** leave as default, or choose your Desktop
- **Language:** Java
- **Build system:** Maven
- **JDK:** If there's a dropdown, select any JDK 17 or higher. If none is listed, click "Download JDK" and pick one (e.g., Oracle OpenJDK 21).

**Step 2.3:** Click **"Create"**

**Step 2.4:** Wait a moment — IntelliJ will set things up. You'll see a project structure appear on the left.

---

## Step 3: Understand the Project Structure

Once your project opens, look at the left panel. You'll see something like:

```
BoxProject
├── src
│   ├── main
│   │   └── java
│   │       └── org.example
│   │           └── Main.java
│   └── test
│       └── java
```

**What does this mean?**

- `src/main/java` — This is where your **actual code** goes (your Box class, your Shape class, etc.)
- `src/test/java` — This is where your **test code** goes (code that checks your actual code works)

**This separation is important.** Your real code and your test code live in different places. Think of it like: the kitchen is where you cook, and the quality control lab is next door where someone checks the food is right.

---

## Step 4: Add Your Box Class

**Step 4.1:** In the left panel, right-click on the `java` folder under `src > main > java`

**Step 4.2:** Click **New > Java Class**

**Step 4.3:** Name it `Box` and press Enter

**Step 4.4:** Delete whatever IntelliJ puts in the file, and type this (your familiar Box class!):

```java
public class Box {
    private int width;
    private int height;

    public Box(int w, int h) {
        width = w;
        height = h;
    }

    public int getWidth() {
        return width;
    }

    public int getHeight() {
        return height;
    }

    public int calculateArea() {
        return width * height;
    }

    public int calculatePerimeter() {
        return 2 * (width + height);
    }
}
```

> **Notice something different?** We've added `public` in front of the class, constructor, and methods. In IntelliJ projects, classes are usually in separate files, and `public` means other files (like our tests) can see and use them.

**Step 4.5:** Press `Ctrl + S` (Windows) or `Cmd + S` (Mac) to save.

---

## Step 5: Check It Works (Run Without Tests First)

**Step 5.1:** Open the `Main.java` file that IntelliJ created for you

**Step 5.2:** Replace its contents with:

```java
public class Main {
    public static void main(String[] args) {
        Box myBox = new Box(10, 20);
        System.out.println("Width: " + myBox.getWidth());
        System.out.println("Height: " + myBox.getHeight());
        System.out.println("Area: " + myBox.calculateArea());
    }
}
```

**Step 5.3:** Click the green **play button** (▶) next to `public static void main` or at the top of the screen

**Expected Output** (in the panel at the bottom):
```
Width: 10
Height: 20
Area: 200
```

If you see that, your project is working. Now let's add testing.

---

# Part 2: Setting Up JUnit (The Testing Library)

## What is JUnit?

JUnit is a **testing library** for Java. It lets you write small pieces of code that automatically check whether your classes work correctly.

Instead of running your program and looking at the output with your eyes, JUnit does the checking for you and tells you ✅ **pass** or ❌ **fail**.

---

## Step 6: Add JUnit to Your Project

**Step 6.1:** In the left panel, find and open the file called `pom.xml` (it's at the root of your project)

**Step 6.2:** Inside `pom.xml`, find the `</project>` tag at the very end. **Just above it**, paste this block:

```xml
    <dependencies>
        <dependency>
            <groupId>org.junit.jupiter</groupId>
            <artifactId>junit-jupiter</artifactId>
            <version>5.10.2</version>
            <scope>test</scope>
        </dependency>
    </dependencies>
```

> If there's already a `<dependencies>` block, just add the `<dependency>...</dependency>` part inside it instead of creating a new one.

**Step 6.3:** A small popup may appear saying **"Maven projects need to be imported"** or you'll see a little floating 🔄 icon. Click **"Load Maven Changes"** or the refresh icon. This downloads JUnit.

**Step 6.4:** Wait a few seconds for it to finish downloading.

---

# Part 3: Writing Your First Test

## Step 7: Create a Test Class

**Step 7.1:** In the left panel, right-click on the `java` folder under `src > test > java`

**Step 7.2:** Click **New > Java Class**

**Step 7.3:** Name it `BoxTest` and press Enter

**Step 7.4:** Replace the contents with this:

```java
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;

public class BoxTest {

    @Test
    void testCalculateArea() {
        Box myBox = new Box(10, 20);
        assertEquals(200, myBox.calculateArea());
    }
}
```

**Step 7.5:** Save the file.

---

## Step 8: Run Your First Test

**Step 8.1:** Click the green **play button** (▶) next to the class name `BoxTest` or next to the method name `testCalculateArea`

**Step 8.2:** Look at the bottom panel — you should see:

```
✅ testCalculateArea — PASSED
```

**You just ran your first unit test!** 🎉

---

## Understanding What You Just Did

Let's break down the test line by line:

```java
import org.junit.jupiter.api.Test;
```
This imports the `@Test` annotation so JUnit knows which methods are tests.

```java
import static org.junit.jupiter.api.Assertions.*;
```
This imports tools like `assertEquals` that check whether things are correct.

```java
@Test
```
This label tells JUnit: "This method is a test — run it."

```java
void testCalculateArea() {
```
The test method. It doesn't return anything (`void`). Name it something descriptive.

```java
Box myBox = new Box(10, 20);
```
Create the object you want to test. Same as before.

```java
assertEquals(200, myBox.calculateArea());
```
**This is the key line.** It says: "I **expect** the area to be 200. Check it."
- First argument: what you **expect** (200)
- Second argument: what you're **actually getting** (`myBox.calculateArea()`)

If they match → ✅ pass. If they don't → ❌ fail.

---

## Step 9: See a Test Fail (On Purpose)

Seeing a failure is just as important as seeing a pass. Let's break something deliberately.

**Step 9.1:** Add this test to your `BoxTest` class (inside the class, below your first test):

```java
@Test
void testDeliberateFailure() {
    Box myBox = new Box(10, 20);
    assertEquals(999, myBox.calculateArea());  // This SHOULD fail!
}
```

**Step 9.2:** Run the tests again (click ▶ next to the class name to run all tests)

**Step 9.3:** You'll see:
```
✅ testCalculateArea — PASSED
❌ testDeliberateFailure — FAILED
   Expected: 999
   Actual: 200
```

**Step 9.4:** Read the failure message. It tells you exactly what went wrong — it expected 999 but got 200.

**Step 9.5:** Now delete that failing test. It was just for learning.

---

# Part 4: Writing More Useful Tests

## Step 10: Test Multiple Things About Box

Replace your `BoxTest` with this fuller version:

```java
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;

public class BoxTest {

    @Test
    void testGetWidth() {
        Box myBox = new Box(10, 20);
        assertEquals(10, myBox.getWidth());
    }

    @Test
    void testGetHeight() {
        Box myBox = new Box(10, 20);
        assertEquals(20, myBox.getHeight());
    }

    @Test
    void testCalculateArea() {
        Box myBox = new Box(10, 20);
        assertEquals(200, myBox.calculateArea());
    }

    @Test
    void testCalculatePerimeter() {
        Box myBox = new Box(10, 20);
        assertEquals(60, myBox.calculatePerimeter());
    }

    @Test
    void testSmallBox() {
        Box small = new Box(1, 1);
        assertEquals(1, small.calculateArea());
        assertEquals(4, small.calculatePerimeter());
    }

    @Test
    void testDifferentBoxesAreIndependent() {
        Box box1 = new Box(5, 5);
        Box box2 = new Box(10, 10);
        assertNotEquals(box1.calculateArea(), box2.calculateArea());
    }
}
```

**Run all the tests.** You should see six green ticks. ✅✅✅✅✅✅

---

## Understanding the Testing Pattern

Every test follows the same three-step pattern. Developers call this **Arrange, Act, Assert**:

1. **Arrange** — Set things up (create the object)
2. **Act** — Do the thing you're testing (call the method)
3. **Assert** — Check the result is what you expected

```java
@Test
void testCalculateArea() {
    // ARRANGE: create the box
    Box myBox = new Box(10, 20);

    // ACT: calculate the area
    int result = myBox.calculateArea();

    // ASSERT: check it's correct
    assertEquals(200, result);
}
```

Once you see this pattern, every test makes sense.

---

## Useful Assertion Methods

Here's a quick reference of the checking tools you can use:

| Method | What it checks |
|---|---|
| `assertEquals(expected, actual)` | Are these two values the same? |
| `assertNotEquals(a, b)` | Are these two values different? |
| `assertTrue(condition)` | Is this true? |
| `assertFalse(condition)` | Is this false? |
| `assertNull(object)` | Is this null? |
| `assertNotNull(object)` | Is this NOT null? |

---

# Part 5: Challenges

Now it's your turn. Try these in order — each one builds on what you've learned.

---

## Challenge 1: Test Edge Cases for Box

Add these tests to your `BoxTest` class:

- Test a box with width 0 and height 10. What should the area be?
- Test a box with very large numbers (e.g., 1000 x 1000). Does the area come out right?
- Test that two boxes created with the same dimensions have the same area (use `assertEquals`).

---

## Challenge 2: Add and Test a New Method

**Step 1:** Add this method to your `Box` class:

```java
public boolean isSquare() {
    return width == height;
}
```

**Step 2:** Write tests for it in `BoxTest`:
- Test that `new Box(5, 5).isSquare()` returns `true`
- Test that `new Box(5, 10).isSquare()` returns `false`

**Hint:** Use `assertTrue(...)` and `assertFalse(...)`.

---

## Challenge 3: Bring In Your Shape and Circle Classes

**Step 1:** Create a new file `Shape.java` in `src/main/java`:

```java
public class Shape {
    private String color;

    public Shape(String c) {
        color = c;
    }

    public String getColor() {
        return color;
    }

    public double getArea() {
        return 0;
    }
}
```

**Step 2:** Create a new file `Circle.java` in `src/main/java`:

```java
public class Circle extends Shape {
    private double radius;

    public Circle(String c, double r) {
        super(c);
        radius = r;
    }

    public double getRadius() {
        return radius;
    }

    @Override
    public double getArea() {
        return Math.PI * radius * radius;
    }
}
```

**Step 3:** Create a new test file `CircleTest.java` in `src/test/java` and write tests for:
- The color is stored correctly
- The radius is stored correctly
- The area of a circle with radius 5 is approximately 78.54

**Hint for testing doubles:** Floating-point maths isn't always exact, so for doubles you provide a small margin of error:
```java
assertEquals(78.54, myCircle.getArea(), 0.01);
```
The third argument (`0.01`) means "these can differ by up to 0.01 and that's fine."

---

## Challenge 4: Test Inheritance Behaviour

Write tests that prove inheritance is working:
- Create a `Circle` and check that `getColor()` works (inherited from Shape)
- Create a `Circle` and check that `getArea()` returns the circle formula, NOT 0 (which is the Shape default)
- Use `assertTrue(myCircle instanceof Shape)` to prove a Circle IS-A Shape

---

## Challenge 5: Create and Test Your Own Class

This is the big one. Do it all yourself:

**Step 1:** Create a `Rectangle` class that extends `Shape`:
- It should have `width` and `height` fields
- Constructor takes color, width, and height
- Override `getArea()` to return `width * height`
- Add a method `isSquare()` that returns `true` when width equals height

**Step 2:** Create a `RectangleTest` class with at least 5 tests covering:
- Width and height getters
- Area calculation
- `isSquare()` returning true and false
- That it's an instance of `Shape`

**Step 3:** Run all your tests. Everything should be green. ✅

---

# Quick Reference Card

**Create a test class:**
```java
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;

public class MyClassTest {
    @Test
    void testSomething() {
        // Arrange, Act, Assert
    }
}
```

**Run tests:** Click ▶ next to the test class or method

**Key assertions:**
```java
assertEquals(expected, actual);        // check equal
assertNotEquals(a, b);                 // check not equal
assertTrue(condition);                 // check true
assertFalse(condition);                // check false
assertEquals(expected, actual, 0.01);  // check doubles (with tolerance)
assertTrue(obj instanceof ClassName);  // check type
```

**The pattern:**
```
Arrange → create your objects
Act     → call the method you're testing
Assert  → check the result
```

---

Good luck — you've got this! And if IntelliJ does something weird (it will, it's IntelliJ), don't panic. Close and reopen the project, or ask for help. The IDE is a tool, not a test.
