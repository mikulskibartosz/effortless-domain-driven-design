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
public void addToOrder(long orderId, long itemId, long quantity)
```

---

@title[Stringly-typed: wrong parameter order]
```java
public void addToOrder(long orderId, long itemId, long quantity)

addToOrder(order, item, quantity);

addToOrder(item, order, quantity);
```
@[3]
@[5]

---

@title[Stringly-typed: incorrect value]
```java
public void addToOrder(long orderId, long itemId, long quantity)

addToOrder(order, item, -1);
```
@[3]

---
