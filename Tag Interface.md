> [!note]
> Also known as **Marker Interface**

A Tag Interface is an Interface with no methods and no fields.



## What is the purpose of tag interface?
- At first it looks useless because it contains nothing.
- But it's **purpose** is:
	- "The class belongs to a special category"

When a class implements the tag interface the program can check if an object is an instance of that interface

```java
public class Bird implements Flyable {
}

if (obj instanceof Flyable) {
	System.out.println("This object can fly");
}
```


> [!note] 
> The interface acts like a **tag/sticker/label**

## Real-life analogy
Some bags have a sticker:
- "FRAGILE"
- "PRIORITY"
- "VIP"

The sticker does not add functionality to the bag itself.
But airport staff treat it differently because of the tag.
Tag interfaces work the same way.


## Why were Marker Interface user?
Before [[annotations]] existed, Java needed a special way to attach metadata to classes.

Marker Interface were a simple solution.

They allow:
- runtime checking using `instanceof`
- grouping classes into categories
- enabling special behavior


## Problems with Marker Interfaces

### 1. No actual behavior
Interfaces are supposed to define behavior. But marker interfaces define nothing. So they misuse the idea of interfaces a bit.

### 2. Weak expressiveness
Their implementation doesn't explain much by itself.
```java
implements Flyable
```
Annotations are better

### 3. Can't add parameters
Marker Interfaces can not store extra metadata.
```java
implements Cacheable(timeout=5) // is not possible
```
Annotations can do it


## Tagging
It usually means marking a class with special information so that Java or a framework can recognize it and treat it differently.
