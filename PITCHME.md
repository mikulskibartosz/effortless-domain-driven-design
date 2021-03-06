@title[First]

(This slide is intentionally left blank)

---

@title[Road to hell]

### The road to hell is paved with primitive types.

Note:
The sad truth is the fact that the road to hell is paved with primitive types.
---

@title[Story - once upon a time]

# Once upon a time...

---

@title[Story - team]

# Fearless team of developers

Note:
There was a fearless team of developers who wrote code in an usual Scala way.

---
@title[The usual way]

# The usual way

Note:
The usual way means there were primitive types. Everywhere.
If there is an email address of the user, they store it in a String type.
If there was an order id to be stored, they used an Int.

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

```scala
def addToOrder(
  orderId: Long, itemId: Long, quantity: Long
): Unit
```
@[1]
@[2]

Note:
Let's look at this add to order method.
Imagine that the method adds the item identified by the given itemId to the order identified by the orderId. The method add the given number of items.

What can go wrong when you write code in such way? We will talk about that later.

---

@title[User class]

```scala
case class User {
  firstName: String,
  lastName: String,
  emailAddress: String,
  phoneNumber: String
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
Everyone is familiar with such style of writing code. There is nothing unusual about that.

You can add a new person to the team and they will fell good about that. They won't understand the logic and the data model, but that is exactly what the expect.

---

@title[Stringly-typed: GDPR]

# GDPR

##### General Data Protection Regulation


Note:
One day we realized that we will need to find all the personal data and be prepared to remove them. All of them! 
---

@title[GDPR - hell]

![Hell https://unsplash.com/photos/VU03qDREAgU](images/fire.jpg)

Note:
Suddenly, life was hell. What was the biggest problem?

---

@title[Road to hell]

### The road to hell is paved with primitive types.

Note:
What was wrong with the primitive types? 

---

@title[GDPR - problems]

## GDPR - our problems
@ul[squares]
* many services
* manual work
* no common domain/data model
* primitive types (userId = String ? Long ? Int ?)
@ulend

---

@title[Stringly-typed: Ctrl + F]

# GDPR
## Ctrl + f

* emailAddress
* email_address
* email
* mail
* e_mail

Note:
Try looking for email addresses. Can you find it by the type? Not, because it is a String. You have to look for the name, but programmers are very creative. So, there are many names you have to look for. 

---?color=#FFF994
@title[Avoid primitive types]

# Avoid primitive types

---

@title[Stringly-typed]

# Stringly-typed

Note:
When people write code in such way we say the code is Stringly-typed.

From the perspective of the programmer there are no types at all.

---

@title[Stringly-typed: Numbers]

# Stringly-typed

## Numbers

---

@title[Stringly-typed: addToOrder method]
```scala
def addToOrder(
  orderId: Long, itemId: Long, quantity: Long
): Unit

addToOrder(order, item, quantity)

addToOrder(item, order, quantity)

addToOrder(order, item, -1)

addToOrder(order, item, 1000000)

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

```scala
def addToOrder(
  orderId: Long, itemId: Long, quantity: Long
): Unit {
if(quantity <= 0 || quantity > MAX_ORDER)
	throw new IllegalArgumentException(“foo bar”)

if(!orderService.orderExists(orderId))
	throw new IllegalStateException(“foo bar”)

if(!itemService.itemExists(itemId))
	throw new IllegalStateException(“foo bar”)
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
```scala
case class User {
  firstName: String,
  lastName: String,
  emailAddress: String,
  phoneNumber: String
}
```
@[2-5]

Note:
A typical model class. We have some information about an user. Everything stored in a string.

Once again I can swap parameters, I can put 1000 characters long string as a first name.

---

@title[Stringly-typed: email validation]
```scala
def sendNotification(email: String) {
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

@title[Constraints - 3]

## Value = Type + Constraints + Units

---

@title[Constraints - code example]

```scala
def addToOrder(
  orderId: OrderId, itemId: ItemId, quantity: Quantity
)

def sendNotification(
  email: VerifiedEmail
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

---

@title[DDD]

# The core of DDD
## Entities and value objects

Note:
There are two important parts of Domain Driven Design.
Entities and value objects. Often overlooked. Too simple to write blog post about them. Not cool enough. 

They are importante because they are the building blocks. You cannot have aggregates, commands, factories, or even events, without value objects and entities.

Unfortunately, oftentimes we ignore those simple parts. Let's look at the benefits of using them.

---

@title[DDD - requirements]
# Requirements

@ul[squares]

- specific types (not stringly-typed)
- immutability

@ulend

Note:
What are the requirements of DDD objects? We need specific types, something that describes the business domain. You cannot just pass around strings and numbers.

The objects must be immutable. It is not a DDD requirement, but it makes you life so much easier.

---?color=#FFF994
@title[Avoid primitive types]

# Avoid primitive types

Note:
All of that to avoid primitive types. Looks like a lot of work.
I was supposed to talk about Scala, so how does it look in Scala?

---

@title[Scala]

# SCALA

---

@title[Scala - effortless]

# SCALA

### Effortless

Note:
I promised that in Scala such things will be easy, effortless and extremly simple.

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

Note:
- just a scala class with an immutable field
- case classes
- value classes

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
@[2-5]

Note:
Immutable fields of a complex type.

---

@title[DDD - Scala - immutability]

# Immutability?

#### Out of the box

---
@title[Case classes]

# Case classes

---

@title[Companion object]

## Companion object
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

Note:
When you use case classes you get an automatically generated companion object which defines the apply function.

The apply function shortens the notation in Scala, you can just write the name of the type and it will call the apply function of this type.

---

@title[Companion object as factory]

```scala
case class VerifiedEmail(email: String)

object VerifiedEmail{
  def apply(
    checkIfVerified: (String) => Future[Boolean]
    )(email: String): Future[VerifiedEmail] = 
    checkIfVerified(email).map {
      case true => VerifiedEmail(email)
      case false => throw new IllegalArgumentException(...)
  }
}
```
@[5-6]

---

@title[Companion object as factory - with trait]

```scala
sealed trait VerifiedEmail {
  def email: String
}

object VerifiedEmail {
  def apply(
    checkIfVerified: (String) => Future[Boolean]
    )(email: String): Future[VerifiedEmail] = 
    checkIfVerified(email).map {
      case true => new VerifiedEmail {
        def email: String = email
      }
      case false => throw new IllegalArgumentException(...)
  }
}
```
@[1,10-12]

---
@title[Case classes - function]
## Case class - functions

```scala
case class TemperatureFahrenheit(val value: Double) {
  def asCelsius = TemperatureCelsius(value * 9/5 + 32)
}

case class TemperatureCelsius(val value: Double) {
  def asFahrenheit = TemperatureFahrenheit((value - 32) * 5/9)
}
```

Note:

Case classes can have functions, so you can keep the data and the behavior in one place.

Like you should do in object oriented programming.

---

@title[Shapeless - tagged types]

# Shapeless - tagged types

```scala
import shapeless.tag
import shapeless.tag.@@
```

---

@title[Shapeless - define tags and types]

# Shapeless - define tags and types

```scala
trait FirstNameTag
trait LastNameTag
trait EmailAddressTag

type FirstName = String @@ FirstNameTag
type LastName = String @@ LastNameTag
type EmailAddress = String @@ EmailAddressTag
```

---

@title[Shapeless - usage]

# Shapeless - usage

```scala
case class User(
  firstName: FirstName,
  lastName: LastName,
  emailAddress: EmailAddress
)

val user = User(
  firstName = tag[FirstNameTag][String]("first name"),
  lastName = tag[LastNameTag][String]("last name"),
  emailAddress = tag[EmailAddressTag][String]("mail@example.com")
)
```

---

@title[Scala 3 - opaque types]

# Scala 3 - opaque types (SIP-35)

```scala
// All examples copied from SIP-35
// (https://docs.scala-lang.org/sips/opaque-types.html):

package object opaquetypes {
  opaque type Logarithm = Double
}
```

---

@title[Scala 3 - opaque types - companion object]

# Scala 3 - opaque type - companion object

---

@title[Scala 3 - opaque types - companion object - example]

```scala
package object opaquetypes {
  object Logarithm {
    // These are the ways to lift to the logarithm type
    def apply(d: Double): Logarithm = math.log(d)

    def safe(d: Double): Option[Logarithm] =
      if (d > 0.0) Some(math.log(d)) else None

    // This is the first way to unlift the logarithm type
    def exponent(l: Logarithm): Double = l
  }
}
```

---

@title[Scala 3 - opaque types - public API]

# Scala 3 - opaque type - public API

---

@title[Scala 3 - opaque types - public API - example]

```scala
package object opaquetypes {
  object Logarithm {
    // Extension methods define opaque types' public APIs
    implicit class LogarithmOps(
      val `this`: Logarithm
    ) extends AnyVal {
      // This is the second way to unlift the logarithm type
      def toDouble: Double = math.exp(`this`)
      def +(that: Logarithm): Logarithm =
        Logarithm(math.exp(`this`) + math.exp(that))
      def *(that: Logarithm): Logarithm =
        Logarithm(`this` + that)
    }
  }
}
```

---
@title[Excuses]

# Avoid primitive types
## Excuses

---

@title[Code is longer]

#### Excuse 1

## More code

Note:
You have to write more code. It was easy to just pass Strings everywhere. Now you have to create a new type, it is problematic, isn't it?

---

@title[Removes duplicates]

## No defensive programming

## Removed duplicate code

## Validation in only one place = less errors

Note:
Let's think about the code you will remove.

You no longer need defensive programming. There was a lot of code to check the parameters at the beginning of every method. Now you check the parameters only in the type constructor or its companion object. 

So, you also don't have duplicate code. And you don't have code which was supposed to be duplicate, but someone forgot to update all places when the conditions are checked so some of them pass and some of the fail. We love such things, don't we? 

---

@title[No defensive programming]

```scala
def addToOrder(
  orderId: Long, itemId: Long, quantity: Long
) {
if(quantity <= 0 || quantity > MAX_ORDER)
        throw new IllegalArgumentException(“foo bar”)

if(!orderService.orderExists(orderId))
        throw new IllegalStateException(“foo bar”)

if(!itemService.itemExists(itemId))
        throw new IllegalStateException(“foo bar”)
}
```
@[4-11](You can remove that code)

Note:
Now the ifs are in constructors. Most likely one if per constructor, so they are also easy to test.

---

@title[Less tests]

## Less tests

## Invalid code does not even compile

Note:
You don't need to check the behavior of addToOrder function when you pass -1 as the quantity. It will never happen. 

You can't pass an itemId as the orderId. Such code will not even compile.

---
@title[Memory allocation]

#### Excuse 2

## Memory allocation = slower code

Note:
You have to create a lot of small objects. They have headers, a lock, a reference to the value. You need to allocate more memory.

You need more RAM and your code is slower.

---

@title[Memory allocation - busted]

## less code = less db/rest calls = faster code

```scala
class FirstName(val value: String) extends AnyVal {}
```

### At runtime it is just a String*

*most of the time

Note:
I was using external services to validate values in the addToOrder method. This is not cheap code. You want to avoid doing such things more than you want to avoid creating new objects.

Also value classes in Scala exist because of that. You need to verify the types only during compilation, at runtime it can be a String. 

---

@title[Code duplicates]

#### Excuse 3

# Duplicated code

### Domain model
### DTO
### ORM entities

Note:
You cannot use you model classes in the REST API, writing mappers would be a nightmare. You need a separate DTO model and a little bit simpler mappers to map between domain and DTO.

The same problem with ORM, you need another model. 

---

@title[Code duplicates - busted]

# Single Responsibility Principle

Note:
It is a good engineering practice to have all such models separated.

---

@title[Not the way we write code]

#### Excuse 4

# It is not the way we write code

Note:
It is not the usual way of writing code. When people hear about that they may even agree, but that does not change their behavior because the code is not familiar. 

---

@title[Not the way we write code - busted]

*"The most dangerous phrase in the language is, "We've always done it this way."* - Grace Murray Hopper 

---

@title[It is hard]

#### Excuse 5

# Difficult

Note:
It is hard to write such code.

---

@title[It is hard - busted]

#### Software engineering + software craft
##### vs.
#### Coding + using ugly hacks

Note:
* software craftsmanship
* following good practices
* avoiding ugly hacks
* writing clean code
* writing self-documenting code
* code that is easy to understand for the next programmers
* code that makes it hard to make a mistake

---
@title[Good practices]

## Software craft

@ul[squares]

* clean code
* self-documenting code
* code that is easy to understand for the next programmers
* code that makes it hard to make a mistake


@ulend

---

@title[It is easy]

#### Excuse 6

# Easy

Note:
So easy that we don't talk about it, but also don't do it correctly because of that.


---
@title[Everyone can understand my code]

#### Excuse 7
# Everyone can understand my code

### What about job security?

Note:
In every company there is an employee who cannot be fired or who cannot quit, because he or she knows everything about the software.

---
@title[Apache Spark]

#### Excuse 8
# Apache Spark

Note:
Ok, fine. That excuse is a valid one. In this case you want your code to be as machine-friendly as possible.

---
@title[It is not cool]

#### Excuse 9
# It is not cool

Note:
When people hear about Scala they think about functional programming.
For many people functional programming means using all concepts useful in Haskell.

In my opinion it usually does not make sense in Scala.

You were supposed to solve the business problem.

---

#### FP

~~functional~~

~~monad~~

~~monoid~~

~~applicative~~

~~traversable~~

Note:
sure you can use monads, monoids, applicatives, etc. and feel superior to anyone who don't even know what those words mean.

That is not software craftsmanship, you were supposed to write code that is easy to modify. 

---

#### FP 

```scala
Option[A]

Either[A, B]

Future[C]

IO[C]

...
```

Note:
We pay a lot of attention to those technical types. The problem is that the important part is hidden inside.
We neglect the domain, even though it is the part that makes money for the business. The technical part only costs us money.

---

@title[Summary 2]

## Doing DDD is not a binary choice

Note:
You can choose some parts of DDD, in this example I talked about value objects and entities. If you incorporate that in you code, you will see a huge difference.

---

@title[Summary 3]

## Value objects and entities = the easiest part of DDD

---

@title[The one thing]

#### What’s the one thing you can do, such that by doing it, everything else will be easier or unnecessary?

"The One Thing" - Gary Keller

---?color=#FFF994
@title[Avoid primitive types]

# Avoid primitive types

---

@title[Road to hell]

### The road to hell is paved with primitive types.

---

@title[Title]

# Effortless domain-driven design

## The real Power of Scala

##### Bartosz Mikulski
##### @mikulskibartosz
