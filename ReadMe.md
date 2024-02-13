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


-----------------------------------------------------------------------------
Some of you don't understand the OOP concepts properly
Everyone focusses on DSA - grind leetcode / interviewbit / hackerrank
Most people they ignore the rest of Computer Science
Topics to be a "good" entry-mid level developer (SDE-1 / 2)
1. How computers work in general (Operating systems, databases)
- Memory management
- Resource managment
- Deadlocks prevention
- Caching
- Threading
- practical - hands on manner
- write SQL questions
- nested ones, recursive, pagination, ...
- add an index on a db table
- when to add an index and when not to
- pros and cons of adding indexes
- how to normalize the data correctly
- how to create an entity-relationship model in the database
2. Low Level Design (how to architect and design the codebase)
- Object Oriented Programming
- inheritance
- composition
- composition over inheritance
- abstract classes
- interfaces
- multiple inheritance
- multi-level inheritance
- polymorphism (runtime polymorphism)
- encapsulation/abstraction/generalization
- SOLID principles
- How to dissect a problem
- case studies
- design a parking lot
- design snake ladder / chess
- design Splitwise
- REST API design
- Database Schema design
3. High Level Design (senior positions) - Scalability
- how do you go from 1000 users, to 5 billion users
- staff engineer @ google interview question
- given files containing strings, sort the strings alphabetically
- how do you sort a list in python
- how to ask my crush out
- how to boil water
- why is the sky blue
- catch: there is 50 Peta bytes of data
- won't fit on 1 machine
- won't fit in the RAM
- distributed computing - Map Reduce


🐓 Can all the birds fly?
=========================

```java
abstract class Bird {
abstract void fly();
void eat() { ... }
void poop() { ... }
void speak() { ... }
}
class Sparrow extends Bird {
void fly() {
print("fly low")
}
}
class Eagle extends Bird {
void fly() {
print("fly high")
}
}
class Kiwi extends Bird {
void fly() {
}
}
```
Are there some species of birds which can't fly?
Penguins, Kiwi, Ostrich, Emu, Dodo ...
>
> ❓ How do we solve this?
>
> • Throw exception with a proper message
> • Don't implement the `fly()` method
> • Return `null`
> • Redesign the system
>
Run away from the problem - Simply don't implement the fly() method


```java
abstract class Bird { // abstract class - incomplete blueprint
abstract void fly(); // hey Mr. compiler, we don't know how to implement fly method
void eat() { ... }
void poop() { ... }
void speak() { ... }
}
class Kiwi extends Bird {
// for Kiwi to extend Bird
// either it should supply the implementation of void fly
// or, if it doesn't, then Kiwi will be an incomplete concept as well
// no void fly here
}
```
🐞 Compiler Error: either class Kiwi should implement void fly, or it should be marked as an abstract class
⚠️ Throw a proper exception / return null
```java

abstract class Bird { // abstract class - incomplete blueprint
abstract void fly(); // hey Mr. compiler, we don't know how to implement fly method
void eat() { ... }
void poop() { ... }
void speak() { ... }
}
class Kiwi extends Bird {
void fly() {
throw new FlightlessBirdException("Kiwis can't fly")
}
}
```

🐞 Violates expectations!
```java
abstract class Bird {
abstract void fly();
}
class Sparrow extends Bird { void fly() {print("fly low") }}
class Eagle extends Bird { void fly() {print("fly high") }}

class ZooGame {
Bird getBirdFromUserChoice() {
// user can choose any type of bird
// and based on their choice, we will return the appropriate object
// Sparrow sparrow = new Sparrow(); // Runtime Polymorphism
// reutrn sparrow;
// class type - Sparrow / Eagle / ...
// data type - Bird
}
void main() {
Bird bird = getBirdFromUserChoice();
bird.fly();
}
}
```
✅ Before extension
Code works perfectly fine. I tested it, we've deployed it
Dev is happy, customers are happy

❌ After extension
1. did they modify existing code? No
2. was the code working previously? Yes
3. Given 1. and 2. should the old code continue working? Yes
4. In reality, is the old code working now? No

==================================

⭐ Liskov's Subtitution Principle
==================================
- don't break contracts!
- Any object of child class `class Child extends Parent` should be able to replace any object of a parent
class, without any issues - the code should still work as before

🎨 Redesign the system!
```java
// accept the fact that not all birds can fly
abstract class Bird {
// we should NOT have abstract void fly
// because if we do, then our children might be forced to voilate our contract
abstract void poop();
abstract void eat();
}
interface ICanFly { // java gets around the "diamond problem" by preventing multiple inheritance and
using interfaces
void fly();
}
class Sparrow extends Bird implements ICanFly {
void fly() { print("fly low") }
}
class Eagle extends Bird implements ICanFly {
void fly() { print("fly low") }
}
class Kiwi extends Bird {
// because it doesn't implement ICanFly, we don't have to provide the void fly()
}
```

```py
from abc import ABC, abstractmethod
class Bird(ABC):
@abstractmethod
def poop(self):
raise NotImplementedError()
@abstractmethod
def eat(self):
raise NotImplementedError()
class ICanFly(ABC): # python gets around the diamond problem by using MRO - allows multiple
inheritance
@abstractmethod
def fly(self):
raise NotImplementedError()
class Sparrow(Bird, ICanFly):
def fly(self):
print('fly low')
class Kiwi(Bird):
...
# no need of def fly() here
```

-----------------------------------------------------------------------------

✈️What else can fly?

=====================
```java
abstract class Bird {
abstract void poop();
abstract void eat();
}
interface ICanFly {
void fly();
void kickToTakeOff(); // actions that the bird will take
void flapWings(); // before starting to fly
}
class Sparrow extends Bird implements ICanFly {
void fly() { print("fly low") }
}
class Shaktiman implements ICanFly {
void fly() { print("spin fast") }
void flapWings() {
// Sorry Shaktiman!
}
}
```
Insects, Helicopters, Aeroplanes, Papi ki Pari, Doremon, Shaktiman, Mom's Chappal, Aladdin
>
> ❓ Should these additional methods be part of the ICanFly interface?
>
> • Yes, obviously. All things methods are related to flying

==================================
⭐ Interface Segregation Principle
==================================
- keep your interfaces minimal
- don't have fat interfaces
- your clients should not be forced to implement methods that they don't require
How will you fix `ICanFly`?
```java
abstract class Bird {
abstract void poop();
abstract void eat();
}
interface ICanFly {
void fly();
}
interface IHasWings {
void flapWings(); // before starting to fly
}
interface ICanJump {
void kickToTakeOff(); // actions that the bird will take
}
class Sparrow extends Bird implements ICanFly, IHasWings, ICanJump {
void fly() { print("fly low") }
void flapWings() { print("flap flap") }
void kickToTakeOff() { print("small little jumpie jumpie") }
}
class Shaktiman implements ICanFly {
void fly() { print("spin fast") }

// shaktiman doesn't jump
// shaktiman doesn't flap wings
}
```

? Isn't this just the Single Responsibility applied to interfaces?
Yes
🔗 SOLID principles are linked together

-----------------------------------------------------------------------------



🗑 Design a Cage
================
```java
// dependencies
interface IDoor { // High level abstraction
void resistEscape(Animal animal) // "do this"
void resistAttack(Attack attack) // I don't know how
}
class WoodenDoor implement IDoor { // Low level implementation
void resistEscape(Animal animal) {
// put the animal back in the cage
animal.setLocation(this.cage.center); // this is exactly how
// to resist excape
}
void resistAttack(Attack attack) {
if(attack.power <= this.cage.power) {
this.cage.health -= attack.power;
} else {
this.cage.destroy()
}
}
}
class IronDoor implement IDoor { ... } // Low Level Implementation
class AdamantiumDoor implement IDoor { ... } // Low Level Implementation
class VibraniumDoor implement IDoor { ... } // Low Level Implementation
interface FeedingBowl { // High Level Abstraction
void feedAnimal(Animal animal);
}
class FruitBowl implements FeedingBowl { ... } // Low Level Implementation
class MeatBowl implements FeedingBowl { ... } // Low Level Implementation
class GrainBowl implements FeedingBowl { ... } // Low Level Implementation
class Cage1 { // High level code
// this cage is for tigers // because it delegates
// "Controller"
IronDoor door;
MeatBowl bowl;
List<Tiger> kitties;
public Cage1() {
// create the dependencies
this.door = new IronDoor();
this.bowl = new MeatBowl();
this.kitties.add(new Tiger("simba"));
this.kitties.add(new Tiger("black panther"));
}
void resistAttack(Attack attack) {
// delegate the task to the door
this.door.resistAttack(attack);
}
void resistEscape(Animal animal) {
this.door.resistEscape(animal);
}
void feed() {

for(Tiger t: this.kitties)
this.bowl.feed(t)
}
}
class Cage2 {
// this cage is for peacocks
WoodenDoor door;
FruitBowl bowl;
List<Peacock> beauties;
public Cage1() {
// create the dependencies
this.door = new WoodenDoor();
this.bowl = new FruitBowl();
this.beauties.add(new Peacock("pea1"));
this.beauties.add(new Peacock("pea2"));
}
}
// 100 more different cages

class ZooGame {
void main() {
Cage1 forTigers = new Cage1();
Cage2 forPeacocks = new Cage2();
// to add a new cage, say for ducks, I will first have to create a new class
}
}
```

🐞 Lot of code repetition
- every cage is a new class
- if I wish to create a new cage - I need to implement a new class
Another Issue
- first need to understand what "high level" and "low level" code is
- High Level abstractions: they tell you what to do, but not how to do it
- on a factory floor,
- managers are high level employees: they tell you what to do, not how to do it
- CEO - high level - set the goals - what needs to be done
- Low Level implementation details: they tell you how exactly to do something
- engineers / workers
- actually performs the job
- they know exactly how to do things

```
------- --------- -------
IBowl IAnimal IDoor high level abstractions
------- --------- -------
║ ║ ║
║ ║ ║
┏━━━━━━━━━━┓ ┏━━━━━━━┓ ┏━━━━━━━━━━┓
┃ MeatBowl ┃ ┃ Tiger ┃ ┃ IronDoor ┃ low level implementations
┗━━━━━━━━━━┛ ┗━━━━━━━┛ ┗━━━━━━━━━━┛
│ │ │
╰───────────────╁───────────────╯
┃
┏━━━━━━━┓

┃ Cage1 ┃ high level controller

┗━━━━━━━┛
```
High level class `Cage1` depends on low level implementations `Tiger`, `MeatBowl`, `IronDoor`

=================================
⭐ Dependency Inversion Principle - what to do? guideline
=================================

- High level code should NEVER depend on low level implementation details
- Instead, it should depend only on high level abstractions

```
------- --------- -------
IBowl IAnimal IDoor
------- --------- -------
│ │ │
╰───────────────╁──────────────╯
┃
┏━━━━━━┓
┃ Cage ┃
┗━━━━━━┛

```

But how?

=======================
💉 Dependency Injection - how to acieve that guideline
=======================
- don't create the dependencies yourself
- instead, let your client create the dependencies, and inject them into you
- client = any piece of code that uses you

```java

