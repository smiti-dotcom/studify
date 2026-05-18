Solid is a set of 5 design principles used in OOP to make software:
- easy to maintain
- easier to extend
- less fragile
- more testable
- more scalable in large codebases

## Before SOLID: The Real Problem

```java
public class UserService {

    public void registerUser(User user) {
        // validation
        // save to database
        // send email
        // generate report
        // log activity
        // notify admin
    }
}
```

The above example looks fine initially.
But as the code increases:
- 5000 lines
- 20 developers modifying it
- bugs everywhere
- impossible to test
- changing one feature breaks another

This is the problem SOLID tries to solve.

## 1. Single Responsibility Principle
A class should only have one reason to change. It means a class should only do one job.
#### Bad Example
```java
public class InvoiceService {

    public void calculateTotal() {}

    public void saveToDatabase() {}

    public void printInvoice() {}

    public void sendEmail() {}
}
```

This class does the following:
- calculates
- stores
- prints
- emails
Too many responsibilities.

#### Why is it needed
Without Single Responsibility Principle:
- classes become gigantic
- code becomes tightly coupled
- testing becomes hard
- changes become dangerous

#### What Problem does it solve
It solves:
- [[God Class]]
- maintenance nightmare
- merge conflicts
- unexpected side effects

#### Good Example
```java
public class InvoiceCalculator {
    public double calculate(Invoice invoice) {}
}

public class InvoiceRepository {
    public void save(Invoice invoice) {}
}

public class InvoicePrinter {
    public void print(Invoice invoice) {}
}

public class EmailService {
    public void sendInvoice(Invoice invoice) {}
}
```
Now each class has one responsibility.

#### Why SRP Is NOT Always Needed
Overusing SRP creates:
- too many tiny classes
- difficult navigation
- unnecessary abstraction
- class explosion

Example:  
Creating separate classes for:
- addition
- subtraction
- multiplication
for a tiny calculator app is overengineering.

#### When SRP Is Used
Used heavily in:
- enterprise applications
- Spring Boot
- microservices
- banking systems
- large backend systems

#### When SRP Should NOT Be Used Excessively
Avoid extreme SRP in:
- small scripts
- prototypes
- small utilities
- interview projects

## 2. Open/Closed Principle
Software entities should be Open for extension but Closed for modification. You should add new behavior without changing old code.

#### Why is it needed
Changing old code is dangerous.
Production systems may:
- already work
- already by tested
- already server millions of users
Modifying stable code increases bug risks.

#### Bad Example
```java
public class PaymentService {

    public void pay(String type) {

        if(type.equals("CARD")) {

        } else if(type.equals("PAYPAL")) {

        }
    }
}
```
Every new payment method requires modifying this class. This is a bad design.

#### Good Example
```java
public interface PaymentMethod {
    void pay();
}

public class CardPayment implements PaymentMethod
public class PaypalPayment implements PaymentMethod
```

Usage:
```java
public class PaymentService {

    private PaymentMethod paymentMethod;

    public PaymentService(PaymentMethod paymentMethod) {
        this.paymentMethod = paymentMethod;
    }

    public void process() {
        paymentMethod.pay();
    }
}
```
Add new payment methods without modifying old code.

#### What Problem OCP Solves
- fragile systems
- risky modifications
- giant switch statements
- endless if-else chains

#### Why OCP Is NOT Always Needed
Sometimes modification is simpler.
For tiny applications:
- abstraction may be unnecessary
- interfaces may add noise

#### Problems OCP Creates
Can create:
- too many abstractions
- unnecessary interfaces
- premature architecture

## 3. Liskov Substitution Principle
Child classes should be replaceable for parent classes without breaking behavior.
It states that objects of a superclass should be replaceable with objects of its subclass without breaking the application.

#### Bad example
```java
class Bird {
    void fly() {}
}

class Penguin extends Bird {
    void fly() {
        throw new UnsupportedOperationException();
    }
}
```
Penguin is not substitutable as Bird.

#### Why is it needed
Inheritance can create fake relationships.
Just because something "looks like" something else does NOT mean it behaves correctly.

#### What problem does LSP solve
- broken inheritance
- incorrect polymorphism
- runtime surprises

#### Better Design
```java
interface Bird {}

interface Flyable {
    void fly();
}
```
Now:
- Sparrow implements Flyable
- Penguin does not

#### Why LSP Is NOT Always Needed
If inheritance is avoided entirely:
- LSP becomes less relevant
Modern systems prefer:
- composition
- interfaces
- delegation

#### Alternative Solutions
- Composition
- strategy pattern
- delegation
- functional programming

## 4. Interface Segregation Principle
Clients should NOT depend on methods they do NOT use.
Clients should not be forced to depend upon interfaces that they do not use.
Clients refers to:
- any class
- module
- service
- or component
that uses another interface.

So the client is usually another piece of code, not the developer directly.

#### Bad Example
```java
interface Worker {
    void work();
    void eat();
}
```

```java
class HumanWorker implements Worker {
    public void work() { }
    public void eat() { }
}
```
This is fine. But now, if we create a `RobotWorker`, it is forced to implement the eat behavior even though robot's don't eat.

```java
class RobotWorker implements Worker {
    public void work() { }

    public void eat() {
        // robots don't eat
    }
}
```

So `RobotWorker` is being forced to depend on a method it does not need.

Here:
- `RobotWorker` is the client
- `Worker` is the interface it depends on
This violates ISP.

#### Better Design
Split the interface:
```java
interface Workable {
    void work();
}

interface Eatable {
    void eat();
}
```

```java
class HumanWorker implements Workable, Eatable {
    public void work() { }
    public void eat() { }
}

class RobotWorker implements Workable {
    public void work() { }
}
```

Now each class depends only on what it actually uses

#### What Problem ISP Solves
- fat interfaces
- unnecessary implementations
- unused methods
- fake behavior

#### Why ISP Is Needed
Large interfaces become painful.
Example:
```java
interface Machine {    
	50 methods
}
```
Every implementation suffers.

#### Problems ISP Creates
Too much ISP causes:
- interface explosion
- confusion
- excessive granularity

#### Real Production Example
Java collections.
Instead of one giant interface:
- List
- Set
- Queue
- Deque
Each serves different clients.

## 5. Dependency Inversion Principle
High-level [[Modules]] should not depend on low-level modules. 
Both should depend on abstractions.

#### Bad Example
```java
class Engine {}

class Car {

    private Engine engine = new Engine();
}
```
Car tightly depends on Engine.
Hard to:
- replace
- text
- extend

#### Better Example
 ```java
 interface Engine {
    void start();
}
 ```

Implementations:
```java
class PetrolEngine implements Engine
class ElectricEngine implements Engine
```

Usage:
```java
class Car {

    private Engine engine;

    public Car(Engine engine) {
        this.engine = engine;
    }
}
```
Now Car depends upon abstraction
