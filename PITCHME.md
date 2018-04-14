@title[First]

(This slide is intentionally left blank)

---

@title[Story - once upon a time]

# Once upon a time...

---

@title[Story - team]

# Fearless team of developers

---

@title[Story - team - picture]

![Team - https://unsplash.com/photos/2_3c4dIFYFU](images/developers.jpg)

---

@title[The usual way]

# The usual way

---

@title[Primitive types]

# Primitive types

---

@title[addToOrder method]

```java
public void addToOrder(
  long orderId, long itemId, long quantity
);
```
@[1]
@[2]

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

---

@title[Everything was fine - usually]

# Everything was fine

### usually...

---

@title[Everything was fine - picture]

![Joy https://unsplash.com/photos/e3OUQGT9bWU](images/joy.jpg)

---

@title[Stringly-typed: GDPR]

# GDPR

##### General Data Protection Regulation

---

@title[GDPR - hell]

# Hell

---

@title[GDPR - hell - image]

![Hell https://unsplash.com/photos/VU03qDREAgU](images/fire.jpg)

---

@title[Road to hell]

### The road to hell is paved with JavaScript and primitive types.

---

@title[Stringly-typed: Ctrl + F]

# GDPR?
## Ctrl + f

* emailAddress
* email_address
* email
* mail
* e_mail

---?color=#FF0000
@title[Avoid primitive types]

# Avoid primitive types

---

@title[Stringly-typed]

# Stringly-typed

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

---

@title[Stringly-typed: Text]

# Stringly-typed

## Text


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

---

@title[Constraints - 1]

## Value = Type
## ???

---

@title[Constraints - 2]

## Value = Type + Constraints

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

---?color=#FFF994

@title[DDD]

# The core of DDD
## Entities and value objects

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


