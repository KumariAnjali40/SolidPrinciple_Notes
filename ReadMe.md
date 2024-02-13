                                                       **SOLID PRINCIPLES**

The SOLID principles are a set of five design principles that, when followed, help create more maintainable, flexible, and scalable software. These principles were introduced by Robert C. Martin and are widely used in object-oriented programming.


✅ Goals
========
We'd like to make our code
1. readability
2. maintainbility
3. testability
4. extensibility

#### Robert C. Martin

Uncle Bob

===================
💎 SOLID Principles
===================
- Single Responsibilty Principle
- Open/Closed
- Liskov's Substitution
- Interface Segregation
- Dependency Inversion
Inversion of Dependency / Inversion of Control / Interface Segregation
Dependency Inversion / Dependency Injection



💭 Context
==========
- Simple Zoo Game 🦊
- Some characters - zoo staff, visitors, animals, ...
- Some structures - cages, walls, doors, ...




🎨 Design Characters
====================
```java
class ZooEntity {
// classes are blueprints for concepts

// attributes [properties]
// visitor attributes
String visitorName;
Integer age;
String gender;
String address;
// zoo staff attributes
String staffName;
Integer employeeID;
Integer monthlySalary;
String address;
String gender;
// animal attributes
String name;
String gender;
String species;
Integer weight;
// behavior [methods]
// visitor behavior
void roamAroundAndLook();
void takePictures();
Ticket buyTicket();
void eat();
void poop();
// staff behavior
void cleanPremises();
void eat();
void poop();
void routineCheckupOnAnimals();
// animal behavior
}
void testZooEntityRoamAroundAndLook() {
ZooEntity entity = new ZooEntity();
entity.roamAroundAndLook();
assertEquals(...)
}
```





🐞 Problems with the above code?
❓ Readable
It looks pretty readable. But there are some issues.
Imagine if we had more charcters
- different types of employees
- managers
- special (VVIP) guests
- transfer animals
- trainers for the animals
Right now it is readable, but as the complexity grows, it will quickly become unreadable
❓ Testable
Testing is difficult because making changes to staff behavior can cause unexpected changes in visitor
behavior - because all the code is tied to each other - using the same class state
Tightly coupled
❓ Extensible
we'll come back to this later
❓ Maintainable
imagine that there are 10 devs working on different animals / staff / visitors - all of them are modifying
the same file
when they commit the code and submit it, they will get merge conflicts!




🛠 How to fix this?


==================================
⭐ Single Responsibility Principle
==================================
- Every class/function/module (unit-of-code) should have 1 and only 1 "well-defined" responsibility.
- Any piece of code should have only 1 reason to change
- if you identify a piece of code which violates this - you should split that code into multiple pieces
```java
class ZooEntity {
// common attributes
String name;
Integer age;
String gender;
void eat();
void poop();
}
// individual responsibilities will go into separate pieces of code
class Visitor extends ZooEntity {
String address;
void roamAroundAndLook();
void takePictures();
Ticket buyTicket();
}
class ZooStaff extends ZooEntity {
Integer employeeID;
Integer monthlySalary;
String address;
void cleanPremises();
void routineCheckupOnAnimals();

}
class Animal extends ZooEntity {
String species;
Integer weight;
}


```

- Readable
Aren't there wayy too many classes & files now? Yes!
In reality whenever you want to make a code change, you are working with 1 specific feature/request - you
only need to look at a handful of files
Yes, we've more files, but each file is very very easy to read
- Testable
Now the code is decoupled - making changes in one class doesn't effect the other classes!
It becomes easier to test correctly and exhaustively!
- Maintainable
Now if a dev is working on visitor, another dev is working on animal - will they have merge conflicts?
No!
the dev working on visitor doesn't need to know anything about animals


🐦 Design a Bird
================
```java
class Bird extends Animal {
String species;
Integer beakLength;
Integer wingSpan;
void fly() {
if (species == "sparrow")
print("flap wings and fly low")
else if (species == "eagle")
print("glide elegantly very high in the sky")
else if (species == "peacock")
print("only pehens can fly, make peacocks cant")
}
}
```

🕊 Different birds fly differently
Do you always write code yourself? Or do you often import external libraries?


```java
// go to github, search for zoo libraries
[SimpleZooLibrary]
{
class ZooEntity { ... }
class Animal extends ZooEntity { ... }
class Bird extends Animal {
String species;
Integer beakLength;
Integer wingSpan;
void fly() {
if (species == "sparrow")
print("flap wings and fly low")
else if (species == "eagle")
print("glide elegantly very high in the sky")
else if (species == "peacock")
print("only pehens can fly, make peacocks cant")
else if (species == "helibird")
print(...)
}
}
}
// simple-library.dll .so .com .jar .pyc ...
// minified js .exe
// you might not have the source code always
// even if you do have it


[MyZooGame]
{
import SimpleZooLibrary.Bird;
// add a new type of Bird which flies in a different manner
// scientists discovered HeliBird
class ZooGame {
void main() {
Bird sparrow = new Bird(species="sparrow");
sparrow.fly();
Bird eagle = new Bird(species="eagle");

eagle.fly();
}
}
}
```

🐞 Problems with the above code?
- Readable
- Testable
- Maintainable
- Extensible - FOCUS!
A lot of times, we might not have modificiation access to some code. In this case, we might not be able to
extend it.
Zerodha - Kite API
Google Adsense - Developer API
Google Adsense team - exposed an API for the Google Search team
Twitter - Developer API

🛠 How to fix this?


=======================
⭐ Open/Close Principle
=======================
- Your code should be closed for modification, yet, open for extension!
- We wish the code that we've written to be "owned" by us - others should NOT be able to make
modifications
- At the same time, we wish others to be able to add new functionality - they should be able to extend it.
❔ Why is it wrong to modify existing code?
typical flow in large companies - over 1 month!
- dev writes code on their local machine, test it, commit it - make a Pull Request (PR)
- this PR goes for review - rest of the team with review the PR, and they will suggest improvements
- after a bunch or iteratiions - your PR gets merged
- deployment process
+ QA team - they will write more tests (unit, integration)
+ Staging servers - tested there
+ Production servers
* A/B testing
- deploye to only 5% of the userbase
- monitor
- are more exceptions being raised
- are the users complaining
- are ther emore support tickets
- are the reviews going down
- is the revenue being effected
* finally the code is deployed to the entire userbase

* ```java
[SimpleZooLibrary]
{
class ZooEntity { ... }
class Animal extends ZooEntity { ... }
abstract class Bird extends Animal {
String species;
Integer beakLength;
Integer wingSpan;
abstract void fly()
}
class Sparrow extends Bird {

void fly() {
print("flap wings and fly low")
}
}
class Eagle extends Bird {
void fly() {
print("glide elegantly very high in the sky")
}
}
}

[MyZooGame]
{
import SimpleZooLibrary.Bird;
// add a new type of Bird which flies in a different manner
// scientists discovered HeliBird
class ZooGame {
void main() {
Bird sparrow = new Bird(species="sparrow");
sparrow.fly();
Bird eagle = new Bird(species="eagle");
eagle.fly();
}
}
}
```

- Extension
```java
import SimpleZooLibrary.Bird;
// add a new type of Bird which flies in a different manner
// scientists discovered HeliBird
class HellBird extends Bird {
void fly() {
print("fly like a helicopter!")
}
}
class ZooGame {
void main() {
Sparrow sparrow = new Sparrow();
sparrow.fly();
Eagle eagle = new Eagle();
eagle.fly();
HellBird helli = new HellBird();
helli.fly()
}
}
```

You should plan for these future "extensions" preemptively!
As the library author, it is your responsibility to write code that follows Open/Close from day 1
THIS is why companies pay so much to devs
Max salary that devs can get in India - Bangalore/Hyd/Chennai
dev salaries in India - goes upto 3 Cr (base) - Principal Engineer / Staff Engineer (10+ years of experience
in tier 1 companies)
Because these people are able to anticipate requirement changes, and they're able to write code that doesn't
need to change in the face of changing requirements!
❔ Isn't this the same thing that we did for Single Responsibility as well? Al we did was apply OOP
techniques and split a large class into multiple classes by using inheritance!
❔ Does that mean that OCP == SRP?
No. The resolution was the same, but the intent was different

SRP = readability, maintainability, testability
OCP = extensibility
🔗 All the SOLID principles are linked together - if you stick to one, you might get others for free!
