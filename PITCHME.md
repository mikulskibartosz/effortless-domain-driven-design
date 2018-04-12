@title[Introduction]

# Effortless domain-driven design

## The real Power of Scala

##### Bartosz Mikulski
##### @mikulskibartosz

---

@title[The usual way]

# The usual way

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
```
@[2]
@[5]
@[7]
@[9]
@[11]

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

@title[Stringly-typed: GDPR]

# GDPR?

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
