@title[First]

(This slide is intentionally left blank)

---

@title[Road to hell]

### The road to hell is paved with JavaScript and primitive types.

Note:
The sad truth is the fact that the road to hell is paved with JavaScript and primitive types.
---

@title[Story - once upon a time]

# Once upon a time...

---

@title[Story - team]

# Fearless team of developers

![Team - https://unsplash.com/photos/2_3c4dIFYFU](images/developers.jpg)

Note:
There was a fearless team of developers who wrote code in an usual Scala way.
It was also similar to an usual way of writing code in Java.
---

@title[The usual way]

# The usual way

Note:
The usual way means there were primitive types. Everywhere.
If there is an email address of the user, you store it in a String type.

---

@title[Primitive types]

# Primitive types

Note:
What is wrong with primitive types?
The type does not mean anything. You can store anything in such variable.
The only thing stopping you from storing an email address in the first name field is common sense.
We all know that there is nothing less common than common sense.

---

@title[addToOrder method]

```java
public void addToOrder(
  long orderId, long itemId, long quantity
);
```
@[1]
@[2]

Note:
Let's look at this add to order method.
Imagine that the method adds the item identified by the given itemId to the order identified by the orderId. The method add the given number of items.

What can go wrong when you write code in such way? We will talk about that later.

---

@title[User class]

```java
class User {
  private String firstName;
  private String lastName;
  private String emailAddress;
  private String phoneNumber;
}
```
@[1]
@[2-5]

Note:
The same team of developers had to store some user data. They created a normally looking User class.

---

@title[Everything was fine - usually]

# Everything was fine

### usually...

Note:
Everyone is familiar with such style of writing code. There is nothing unusual about that. You can add a new person to the team and they will fell good about that. They won't understand the logic and the data model, but that is exactly what the expect. So maybe there is nothing wrong about it?

---

@title[Everything was fine - picture]

![Joy https://unsplash.com/photos/e3OUQGT9bWU](images/joy.jpg)

Note:
We did not care. You can write code using primitive types and deliver features fast. What can go wrong?

---

@title[Stringly-typed: GDPR]

# GDPR

##### General Data Protection Regulation


Note:
One day we realized that we will need to find all the personal data and be prepared to remove them. All of them! 
---

@title[GDPR - hell]

# Hell

![Hell https://unsplash.com/photos/VU03qDREAgU](images/fire.jpg)

Note:
Suddenly, life was hell. What was the biggest problem?

---

@title[Road to hell]

### The road to hell is paved with JavaScript and primitive types.

Note:
JavaScript and primitive types. We could do anything about JavaScript, because it was already too late. What was wrong with the primitive types? 

---

@title[Stringly-typed: Ctrl + F]

# GDPR?
## Ctrl + f

* emailAddress
* email_address
* email
* mail
* e_mail

Note:
Try looking for email addresses. Can you find it by the type? Not, because it is a String. You have to look for the name, but programmers are very creative. So, there are many names you have to look for. 

---?color=#FF0000
@title[Avoid primitive types]

# Avoid primitive types

---

@title[Stringly-typed]

# Stringly-typed

Note:
When people write code in such way we say the code is Stringly-typed. From the perspective of the programmer there are no types at all.

---

@title[Stringly-typed: Numbers]

# Stringly-typed

## Numbers

---

@title[Stringly-typed: addToOrder method]
```java
public void addToOrder(
  long orderId, long itemId, long quantity
);

addToOrder(order, item, quantity);

addToOrder(item, order, quantity);

addToOrder(order, item, -1);

addToOrder(order, item, 1000000);

(orderId + itemId) * quantity
```
@[2]
@[5]
@[7]
@[9]
@[11]
@[13]

Note:
We already know that method. What can go wrong?
There are only numeric parameters. All of them have the same type.
We don't really know what those parameters mean. 
Sure, we can look at the names, but as Java developers we know that the parameter names are not a part of the method signature. 

The first example is correct. It may not be obvious after such introduction ;)
Has anyone notice that I swapped the parameters in the second one? The code still compiles. Most likely the tests still pass because a lazy developer probably used the same test values for both ;)

In the third one we have an invalid value. Again, there is nothing stopping me from doing that.

The next one has a ridiculusly huge value. Is it a correct quantity? Most likely it is not a normal situation.

Actually, nothing stops me from adding both ids and multiplying the result by the quantity. 
---

@title[Stringly-typed: addToOrder implementation]

## Defensive programming

```java
public void addToOrder(
  long orderId, long itemId, long quantity
) {
if(quantity <= 0 || quantity > MAX_ORDER)
	throw new IllegalArgumentException(“foo bar”);

if(!orderService.orderExists(orderId))
	throw new IllegalStateException(“foo bar”);

if(!itemService.itemExists(itemId))
	throw new IllegalStateException(“foo bar”);

 //
}
```
@[4-5]
@[7-8]
@[10-11]
@[5,8,11](throw new RuntimeException("validation error"))

Note:
But we have validation, don't we? At the beginning of the method we check the parameters.
We check if the value is within the expected range. We use some external services to verify whether the values are correct. The first 6 lines of this method do not do anything related to adding an item to the order!


---

@title[Stringly-typed: Text]

# Stringly-typed

## Text

Note:
You don't need numbers to create a mess. String is great too!

---

@title[Stringly-typed: User example]
```java
class User {
  private String firstName;
  private String lastName;
  private String emailAddress;
  private String phoneNumber;
}
```
@[2-5]

Note:
A typical model class. We have some information about an user. Everything stored in a string.
Once again I can swap parameters, I can put 1000 characters long string as a first name.

---

@title[Stringly-typed: email validation]
```java
public void sendNotification(String email) {
  if(!emailValidator.isValid(email)) {
    throw new RuntimeException("...")
  }
  if(!emailService.isVerified(email)) {
    throw new RuntimeException("...")
  }
}
```
@[2,5]

Note:
Once again we have validation. This time we want to send a notification to the given email address. So we have to check whether the value is a correct email address.
We cannot send it to some random email, so we must also check whether the email has been verified.
 
Again 6 lines of code, not related to sending the email.
And you have to do it in every method which uses the email address. Because if you forget in one place, we all know it will cause errors.
---

@title[Constraints - 1]

## Value = Type
## ???


Note:
What is a value?
Is it only a type? 

---

@title[Constraints - 2]

## Value = Type + Constraints

Note:
Do we have some expectations?
Constraints perhaps?
When I store email address as a String? Does it tell you anything? You don't know if I validated that value, you don't know if the length is limited. 

---

@title[Constraints - code example]

```java
public void addToOrder(
  OrderId orderId, ItemId itemId, Quantity quantity
)

public void sendNotification(
  VerifiedEmail email
)
```
@[2]
@[6]

Note:
What if I wrote the code in this way?
Insted of longs I have separate types for every parameter.
I cannot swap two parameters, now the code would not compile.
I cannot use an invalid value, it would not be possible to create an instance of the class if I passed an incorrect value to its constructor (hopefully, if I wrote validation).

Look at the second example. Now I can even enforce business rules. Send notification method needs a verified email. It is obvious. I do not need to check it inside of this method again. 

---?color=#FFF994

@title[DDD]

# The core of DDD
## Entities and value objects

Note:
There are two important parts of Domain Driven Design.
Entities and value objects. Often overlooked. Too simple to write blog post about them. Not cool enough. 

TODO 

---

@title[DDD - requirements]
# Requirements

@ul[squares]

- specific types (not stringly-typed)
- immutability

@ulend

---

@title[DDD - types]
# Simple types

```java
public class FirstName {
  private final String value;
  
  public FirstName(String value) {
    this.value = value;
  }
  
  public String getValue() {
    return this.value;
  }
}
```

---

@title[DDD - complex type]

# Complex types

```java
public class User {
  private final FirstName firstName;
  private final LastName lastName;
  private final EmailAddress emailAddress;
  private final TelephoneNumber telephoneNumber;
  
  (...)
}
```

---

@title[DDD - complex part - the easy part]

# That was the easy part...

---

@title[DDD - complex part - change value]

```java
public User withFirstName(FirstName firstName) {
  return new User(
    firstName,
    this.lastName,
    this.emailAddress,
    this.phoneNumber
  );
}
```
@[1]
@[3]
@[2,3-5]

---?color=#FF0000
@title[Avoid primitive types]

# Avoid primitive types

---

@title[Scala]

# SCALA

---

@title[Scala - effortless]

# SCALA

### Effortless

---

@title[DDD - Scala - simple types]

# Simple types

```scala
class FirstName(val value: String) {}

case class FirstName(value: String)

class FirstName(val value: String) extends AnyVal {}
```
@[1]
@[3]
@[5]

---

@title[DDD - Scala - complex types]

# Complex types

```scala
case class User(
  firstName: FirstName,
  lastName: LastName,
  emailAddress: EmailAddress,
  phoneNumber: PhoneNumber
) 
```
@[1]

---

@title[DDD - Scala - immutability]

# Immutability?

#### Out of the box

---

@title[DDD - Scala - copy]

# Immutability.

```scala
user.copy(firstName = FirstName("John"))
```

---

@title[Case classes]

# Case classes

---

@title[Companion object]

```scala
case class FirstName(value: String)

object FirstName {
  def apply(firstName: String): FirstName = new FirstName(firstName)
}

val firstName = FirstName("John")

```
@[1]
@[3-5]
@[7]

---?color=#FFF994

@title[Case classes - features]

# Case classes

@ul[squares]

- companion object
- pattern matching
- `copy` method (shallow copy)
- case classes are compared by structure and not by reference (x1 == x2)

@ulend

---

@title[Excuses]

# Excuses

---

@title[Code is longer]

# Excuse 1

## More code

---

@title[Removes duplicates]

## No defensive programming

## Removed duplicate code

## Validation in only one place = less errors

---

@title[No defensive programming]

```java
public void addToOrder(
  long orderId, long itemId, long quantity
) {
if(quantity <= 0 || quantity > MAX_ORDER)
        throw new IllegalArgumentException(“foo bar”);

if(!orderService.orderExists(orderId))
        throw new IllegalStateException(“foo bar”);

if(!itemService.itemExists(itemId))
        throw new IllegalStateException(“foo bar”);

 //
}
```
@[4-11](You can remove that code)

---

@title[Less tests]

## Less tests

## Invalid code does not even compile

---
@title[Memory allocation]

# Excuse 2

## Memory allocation = slower code

---

@title[Memory allocation - busted]

# Less code = less db/rest calls

```scala
class FirstName(val value: String) extends AnyVal {}
```

### At runtime it is just a String*

*most of the time

---

@title[Code duplicates]

# Duplicated code

### Domain model
### DTO
### ORM entities

---

@title[Code duplicates - busted]

# Single Responsibility Principle

---

@title[Not the way we write code]

# It is not the way we write code

---

@title[Not the way we write code - busted]

*"The most dangerous phrase in the language is, "We've always done it this way."* - Grace Murray Hopper 

---

@title[It is hard]

# Difficult

---

@title[It is hard - busted]

#### The difference between:
#### Software engineering + software craftsmanship
##### vs.
#### Coding + using ugly hacks

---
@title[It is not cool]

It is not cool.

---

~~functional~~
~~monad~~
~~monoid~~
~~applicative~~
~~traversable~~

---?color=#FFF994

@title[The one thing]

#### What’s the one thing you can do, such that by doing it, everything else will be easier or unnecessary?

"The One Thing" - Gary Keller

---?color=#FF0000
@title[Avoid primitive types]

# Avoid primitive types

---

@title[Summary]

## Easy in Scala, but you can do it in Java too

---

@title[Summary 2]

## Doing DDD is not a binary choice

---

@title[Summary 3]

## Value objects and entities = the easiest part of DDD

---

@title[Disclaimer]

## Disclaimer

#### Sample size: 1

#### Microservices?

---?color=#FF0000
@title[Avoid primitive types]

# Avoid primitive types

---

@title[Title]

# Effortless domain-driven design

## The real Power of Scala

##### Bartosz Mikulski
##### @mikulskibartosz

---

## Questions?

---

## 1. Are there any other excuses?

---

## 2. When is it a bad idea?

---

# Questions?

---

@title[Title]

# Effortless domain-driven design

## The real Power of Scala

##### Bartosz Mikulski
##### @mikulskibartosz
