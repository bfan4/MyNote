## Spring AOP（面向切面编程）

### Spring AOP comparison
#### Advantages:
* Simpler to use than AspectJ
* Uses Proxy pattern
* Can migrate to AspectJ when using @Aspect annotation

#### Disadvantages:
* Only supports method-level join points
* can only apply aspects to beans created by Spring app context
* Minor performance cost for aspect execution


### AspectJ Comparison
#### Advantages:
* Support all join points
* Works with any POJO, not just beans from app context
* Faster performance compared to Spring AOP
* Complete AOP support

#### Disadvantages:
* Compile-time weaving requires extra comilation step
* AspectJ pointcut syntax can become complex