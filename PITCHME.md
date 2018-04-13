@title[Introduction]

# Effortless domain-driven design

## The real Power of Scala

##### Bartosz Mikulski
##### @mikulskibartosz

---

@title[Story - once upon a time]

# Once upon a time...

---

@title[Story - team]

# Fearless team of developers

---

@title[Story - team - picture]

![Team](images/developers.jpg)

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

@title[Stringly-typed: GDPR]

# GDPR

##### General Data Protection Regulation

---

@title[GDPR - hell]

# Hell

---

@title[Stringly-typed: Ctrl + F]

# GDPR?
## Ctrl + f

* emailAddress
* email_address
* email
* mail
* e_mail

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

---
