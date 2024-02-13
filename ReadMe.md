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

