# Opinionated Java Coding Standards

The purpose of the [47 Degrees](http://47deg.com) Java Coding Standards is to create a collaboration baseline; helpful for scenarios where many people are creating, modifying, and contributing to the same project. The end goal is unity, consistency, and the notion that a single entity worked on the project.

Java applications developed at *47 Degrees* should follow the [Code Conventions for the Java Programming Language](http://www.oracle.com/technetwork/java/codeconv-138413.html)

*47 Degrees* uses the Java Programming Language and Java Related Technologies in different scenarios and applications. As a norm *47 Degrees*  follows industry standards and notifying of any deviation of this document from these standards will be welcomed.

**Table of Contents**  *generated with [DocToc](http://doctoc.herokuapp.com/)*

- [Java Coding Standards](#java-coding-standards)
	- [0. Rebel](#0-rebel)
	- [1. General](#1-general)
	- [2. Comments](#2-comments)
	- [3. Copyrights](#3-copyrights)
		- [3.1. Open Source](#31-open-source)
		- [3.2. Propietary - Not Open Source](#32-propietary---not-open-source)
		- [3.3. 3rd Parties](#33-3rd-parties)
	- [4. Good Practices](#4-good-practices)
		- [4.1. Design for interfaces](#41-design-for-interfaces)
		- [4.2. Define Application Layers.](#42-define-application-layers)
		- [4.3. Avoid multiple return statements](#43-avoid-multiple-return-statements)
		- [4.4. Boolean comparisons](#44-boolean-comparisons)
		- [4.5. for loops Vs for-each loops](#45-for-loops-vs-for-each-loops)
		- [4.5. String concatenation](#45-string-concatenation)
		- [4.6. Exceptions](#46-exceptions)
		- [4.7. Collections](#47-collections)
		- [4.8. Raw types](#48-raw-types)
		- [4.9. Use of final](#49-use-of-final)
		- [4.10. Name return values result](#410-name-return-values-result)
		- [4.11. Consider setters and getters for field access](#411-consider-setters-and-getters-for-field-access)
		- [4.12. Know your Java](#412-know-your-java)
	- [5. Design Patterns](#5-design-patterns)
		- [5.1. Abstract Factory](#51-abstract-factory)
		- [5.2. Factory method](#52-factory-method)
		- [5.3. Lazy Delegate Wrapper](#53-lazy-delegate-wrapper)
		- [5.4. Singleton](#54-singleton)
		- [5.5. Enums](#55-enums)
		- [5.6. Private Helpers](#56-private-helpers)
	- [6. Other Java Technologies](#6-other-java-technologies)
	- [7. Active enforcement](#7-active-enforcement)

## 0. Prerequirements

TODO: add initialization part
TODO: http://geosoft.no/development/javastyle.html
TODO: group and rearrange


### 0.1. Follow IDEA default settings and code formatting
Use following key bindings to format code:
`Ctrl + Cmd + L`
To format imports:
`Ctrl + Alt + O`

### 0.2. Turn off star imports
`Settings > Editor > Code Style > Java > Imports > Class count to use import with '*'`
Set it to a high value, e.g. 77.

Wildcard imports make the source of an imported class less clear.  They also tend to hide a high
class [fan-out](http://en.wikipedia.org/wiki/Coupling_(computer_programming)#Module_coupling).
*See also [texas imports](#stay-out-of-texas)*

    :::java
    // Bad.
    //   - Where did Foo come from?
    import com.twitter.baz.foo.*;
    import com.twitter.*;

    interface Bar extends Foo {
      ...
    }

    // Good.
```java
    import com.twitter.baz.foo.BazFoo;
    import com.twitter.Foo;

    interface Bar extends Foo {
      ...
    }
```

### 0.3. Install Lombok plugin
Get familiar with Lombok library and use it in your projects.
(https://projectlombok.org)
Alternative: Google AutoValue library, but it doesn't follow DRY principle.

Add the Lombok IntelliJ plugin to add lombok support IntelliJ:

- Go to File > Settings > Plugins
- Click on Browse repositories...
- Search for Lombok Plugin
- Click on Install plugin
- Restart IntelliJ IDEA
(https://plugins.jetbrains.com/plugin/6317-lombok-plugin)

## 1. General

* All code, regardless of who touches it, should look like a single person created it.

### POLA: Principle of least astonishment
(https://en.wikipedia.org/wiki/Principle_of_least_astonishment)
Minimize number of assumtions needed to understand the code.
Do not:
- assume that all item ids are numbers from 1 to n
- assume that ZonedDateTime is in UTC
- JSON attributes are sorted
- no one will change this piece of code and parameters order
- while loop with break will stop at some point(TODO move out)

Use focal points, imagine how it would look like 6 months from now.
In game theory, a focal point (also called Schelling point) is a solution that people will tend to use in the absence of communication.
Example: "Tomorrow you have to meet a stranger in NYC. Where and when do you meet them? Schelling asked a group of students this question, and found the most common answer was: noon at Grand Central Terminal."

#### LoD: Principle of least knowledge
(https://en.wikipedia.org/wiki/Law_of_Demeter)

### Recommended reading
- [Effective Java](http://www.amazon.com/Effective-Java-Edition-Joshua-Bloch/dp/0321356683)

### Design principles

#### DRY: (https://en.wikipedia.org/wiki/Don%27t_repeat_yourself)


#### SRP: (https://en.wikipedia.org/wiki/Single_responsibility_principle)



## 2. Naming

### 2.1. Should package names be singular or plural?
Use the plural for packages with homogeneous contents and the singular for packages with heterogeneous contents.
*TBD...*
(https://softwareengineering.stackexchange.com/questions/75919/should-package-names-be-singular-or-plural)

### 2.1. Variables and methods

- "is" prefix should be used for boolean variables and methods: isEncoded, isRight;
- start counting variables with "n": nPoints, nPeople;
- do not use short names: ptsArr -> pointsArray;
- do not put "get" everywhere: getSize -> size;
- Generic variables should have the same name as their type: Country -> country;

#### Extremely short variable names should be reserved for instances like loop indices.

    :::java
    // Bad.
    //   - Field names give little insight into what fields are used for.
    class User {
      private final int a;
      private final String m;

      ...
    }

// Good:
```java
    class User {
      private final int ageInYears;
      private final String maidenName;

      ...
    }
```

#### Include units in variable names

    :::java
    // Bad.
    long pollInterval;
    int fileSize;

// Good:
```java
    long pollIntervalMs;
    int fileSizeGb.

    // Better.
    //   - Unit is built in to the type.
    //   - The field is easily adaptable between units, readability is high.
    Amount<Long, Time> pollInterval;
    Amount<Integer, Data> fileSize;
```



#### Don't embed metadata in variable names
A variable name should describe the variable's purpose.  Adding extra information like scope and
type is generally a sign of a bad variable name.

Avoid embedding the field type in the field name.

    :::java
    // Bad.
    Map<Integer, User> idToUserMap;
    String valueString;

// Good:
```java
    Map<Integer, User> usersById;
    String value;
```
Also avoid embedding scope information in a variable.  Hierarchy-based naming suggests that a class
is too complex and should be broken apart.

    :::java
    // Bad.
    String _value;
    String mValue;

// Good:
```java
    String value;
```

### Composition Over Inheritance
Why:
Less coupling between classes.
Using inheritance, subclasses easily make assumptions, and break LSP.

How:
Test for LSP (substitutability) to decide when to inherit.
Compose when there is a “has a” (or “uses a”) relationship, inherit when “is a”.

(https://blogs.msdn.microsoft.com/thalesc/2012/09/05/favor-composition-over-inheritance/)


### Constrain arguments by using type safe enumerations

**Correct:**

```java
public enum Options {
	YES, NO
}
```

**Incorrect:**

```java
String yes = "YES";
String no = "NO";
```

### Convert to a builder if 3+ parameters have the same type

**Correct:**
```java
@Builder
public class Person {
  private String name;
  private String secondName;
  private String nickname;
  private int age;
}

Person person = Person.builder().name("James").secondName("Gosling").nickname("hacker").age(30).build();
```

**Incorrect:**

```
@AllArgsConstructor
public class BuilderExample {
  private String name;
  private String secondName;
  private String nickname;
  private int age;
}

Person person = new Person("James", "hacker", "Gosling", 30);
```

### Prefer explicit and value types over default types

**Correct:**
```java
@AllArgsConstructor
public class Person {
  private Name name;
  private Age age;
  private SchoolId school;
}
```

**Incorrect:**

```
@AllArgsConstructor
public class BuilderExample {
  private String name;
  private String age;
  private String school;
}
```

### Provide fields using constructor parameters, not setters
- use lombok annotations: @NoArgsConstructor, @RequiredArgsConstructor, @AllArgsConstructor
- use @NonNull and final for every field to make them more visible
- place parameters to the top of the class prior to static fieds

**Correct:**
```java
@RequiredArgsConstructor(staticName = "of")
public class Person {
    @NonNull
    private final Name name;
    @NonNull
    private final Age age;
    @NonNull
    private final SchoolId school;

    public static final BEAN_NAME = "Person";
    // ...
}
```

**Incorrect:**

```
public class Person {

    public static final BEAN_NAME = "Person";

    private Name name;
    private Age age;
    private SchoolId school;

    public void setName(Name name) {
        name = name;
    }
}
```

### 4.10. Name return values *result*

Consider using 'result' as the name for the returned variable. This eases the pain when debugging and increases code legibility.

```java
public Object doSomething() {
	Object result = null;
	if (something) {
		result = new Object();
	}
	return result;
}
```

### 4.6. Exceptions

Don't swallow exception, catch those you can recover or do something about, let the rest reach their destination

**Correct:**

```java
	try {
		do();
	} catch (SomethingWeCanHandleException ex) {
		log.error(ex);
		notifyUser(ex);
	} finally {
		cleanUp();
	}
```

```java
	public void doSomething() throws SomethingSomeoneElseCanHandleException {
	}
```

**Incorrect:**

```java
	try {
		do();
	} catch (Throwable ex) {
		log.error(ex);
	} 
```

```java
	try {
		do();
	} catch (SomethingWeCanHandleException ex) {
		//do nothing
	} 
```

### 4.6.1. Do not catch `Throwable` and `java.lang.Error`!
Otherwise unforeseen bugs might creep away this way.

Besides, Throwable covers Error as well and that's usually no point of return. You don't want to catch/handle that, you want your program to die immediately so that you can fix it properly.
Generally, never. However, sometimes you need to catch specific Errors.

If you're writing framework-ish code (loading 3rd party classes), it might be wise to catch LinkageErrors (no class def found, unsatisfied link, incompatible class change). I've also seen some stupid 3rd-party code throwing sublcasses of Errors, so you'll have to handle these either.

By the way, I'm not sure it isn't possible to recover from OutOfMemory.

### 4.6.2. Prefer unchecked exceptions

**Correct:**

```java
private final int lineCount;
try {
    lineCount = LineCounter.countLines(sFileName);
} catch(IOException ex){
    throw new UncheckedIOException(ex);
}
```

### 4.6.3. Always close/release resources where they are created
```java
AmazonS3 s3Client = new AmazonS3Client(new ProfileCredentialsProvider());        
S3Object object = s3Client.getObject(
                  new GetObjectRequest(bucketName, key));
try (InputStream objectData = object.getObjectContent()) {
    // Process the objectData stream.
}
```

```java
AmazonS3 s3Client = new AmazonS3Client(new ProfileCredentialsProvider());        
S3Object object = s3Client.getObject(
                  new GetObjectRequest(bucketName, key));
InputStream objectData = object.getObjectContent();
try {
    // Process the objectData stream.
} finally {
    objectData.close();
}
```




### Use annotations wisely

#### @Nullable
By default - disallow `null`.  When a variable, parameter, or method return value may be `null`,
be explicit about it by marking
[@Nullable](http://code.google.com/p/jsr-305/source/browse/trunk/ri/src/main/java/javax/annotation/Nullable.java?r=24).
This is advisable even for fields/methods with private visibility.

```java
    class Database {
      @Nullable private Connection connection;

      @Nullable
      Connection getConnection() {
        return connection;
      }

      void setConnection(@Nullable Connection connection) {
        this.connection = connection;
      }
    }
```

#### @VisibleForTesting
Sometimes it makes sense to hide members and functions in general, but they may still be required
for good test coverage.  It's usually preferred to make these package-private and tag with
[@VisibleForTesting](http://docs.guava-libraries.googlecode.com/git-history/v11.0.2/javadoc/com/google/common/annotations/VisibleForTesting.html)
to indicate the purpose for visibility.

Constants are a great example of things that are frequently exposed in this way.

    :::java
    // Bad.
    //   - Any adjustments to field names need to be duplicated in the test.
    class ConfigReader {
      private static final String USER_FIELD = "user";

      Config parseConfig(String configData) {
        ...
      }
    }
    public class ConfigReaderTest {
      @Test
      public void testParseConfig() {
        ...
        assertEquals(expectedConfig, reader.parseConfig("{user: bob}"));
      }
    }

    // Good.
```java
    //   - The test borrows directly from the same constant.
    class ConfigReader {
      @VisibleForTesting static final String USER_FIELD = "user";

      Config parseConfig(String configData) {
        ...
      }
    }
    public class ConfigReaderTest {
      @Test
      public void testParseConfig() {
        ...
        assertEquals(expectedConfig,
            reader.parseConfig(String.format("{%s: bob}", ConfigReader.USER_FIELD)));
      }
    }
```

#### Time-dependence
Code that captures real wall time can be difficult to test repeatably, especially when time deltas
are meaningful.  Therefore, try to avoid `new Date()`, `System.currentTimeMillis()`, and
`System.nanoTime()`.  A suitable replacement for these is
[Clock](https://github.com/twitter/commons/blob/master/src/java/com/twitter/common/util/Clock.java); using
[Clock.SYSTEM_CLOCK](https://github.com/twitter/commons/blob/master/src/java/com/twitter/common/util/Clock.java#L32)
when running normally, and
[FakeClock](https://github.com/twitter/commons/blob/master/src/java/com/twitter/common/util/testing/FakeClock.java)
in tests.

### Defensive programming

#### Avoid assert
We avoid the assert statement since it can be
[disabled](http://docs.oracle.com/javase/7/docs/technotes/guides/language/assert.html#enable-disable)
at execution time, and prefer to enforce these types of invariants at all times.


#### Minimize visibility

In a class API, you should support access to any methods and fields that you make accessible.
Therefore, only expose what you intend the caller to use.  This can be imperative when
writing thread-safe code.

    :::java
    public class Parser {
      // Bad.
      //   - Callers can directly access and mutate, possibly breaking internal assumptions.
      public Map<String, String> rawFields;

      // Bad.
      //   - This is probably intended to be an internal utility function.
      public String readConfigLine() {
        ..
      }
    }

    // Good.
```java
    //   - rawFields and the utility function are hidden
    //   - The class is package-private, indicating that it should only be accessed indirectly.
    class Parser {
      private final Map<String, String> rawFields;

      private String readConfigLine() {
        ..
      }
    }
```

#### Favor immutability

Mutable objects carry a burden - you need to make sure that those who are *able* to mutate it are
not violating expectations of other users of the object, and that it's even safe for them to modify.

    :::java
    // Bad.
    //   - Anyone with a reference to User can modify the user's birthday.
    //   - Calling getAttributes() gives mutable access to the underlying map.
    public class User {
      public Date birthday;
      private final Map<String, String> attributes = Maps.newHashMap();

      ...

      public Map<String, String> getAttributes() {
        return attributes;
      }
    }

    // Good.
```java
    public class User {
      private final Date birthday;
      private final Map<String, String> attributes = Maps.newHashMap();

      ...

      public Map<String, String> getAttributes() {
        return ImmutableMap.copyOf(attributes);
      }

      // If you realize the users don't need the full map, you can avoid the map copy
      // by providing access to individual members.
      @Nullable
      public String getAttribute(String attributeName) {
        return attributes.get(attributeName);
      }
    }
```

#### Be wary of null
Use `@Nullable` where prudent, but favor
[Optional](http://docs.guava-libraries.googlecode.com/git-history/v11.0.2/javadoc/com/google/common/base/Optional.html)
over `@Nullable`.  `Optional` provides better semantics around absence of a value.

#### Clean up with finally (TODO deduplicate)

Even if there are no checked exceptions, there are still cases where you should use try/finally
to guarantee resource symmetry.

    :::java
    // Bad.
    //   - Mutex is never unlocked.
    mutex.lock();
    throw new NullPointerException();
    mutex.unlock();

    // Good.
```java
    mutex.lock();
    try {
      throw new NullPointerException();
    } finally {
      mutex.unlock();
    }

    // Bad.
    //   - Connection is not closed if sendMessage throws.
    if (receivedBadMessage) {
      conn.sendMessage("Bad request.");
      conn.close();
    }

    // Good.
    if (receivedBadMessage) {
      try {
        conn.sendMessage("Bad request.");
      } finally {
        conn.close();
      }
    }
```

### Obey the Law of Demeter ([LoD](http://en.wikipedia.org/wiki/Law_of_Demeter))
The Law of Demeter is most obviously violated by breaking the
[one dot rule](http://en.wikipedia.org/wiki/Law_of_Demeter#In_object-oriented_programming), but
there are other code structures that lead to violations of the spirit of the law.

#### In classes
Take what you need, nothing more.  This often relates to [texas constructors](#stay-out-of-texas)
but it can also hide in constructors or methods that take few parameters.  The key idea is
to defer assembly to the layers of the code that know enough to assemble and instead just
take the minimal interface you need to get your work done.

    :::java
    // Bad.
    //   - Weigher uses hosts and port only to immediately construct another object.
    class Weigher {
      private final double defaultInitialRate;

      Weigher(Iterable<String> hosts, int port, double defaultInitialRate) {
        this.defaultInitialRate = validateRate(defaultInitialRate);
        this.weightingService = createWeightingServiceClient(hosts, port);
      }
    }

    // Good.
    class Weigher {
      private final double defaultInitialRate;

      Weigher(WeightingService weightingService, double defaultInitialRate) {
        this.defaultInitialRate = validateRate(defaultInitialRate);
        this.weightingService = checkNotNull(weightingService);
      }
    }

If you want to provide a convenience constructor, a factory method or an external factory
in the form of a builder you still can, but by making the fundamental constructor of a
Weigher only take the things it actually uses it becomes easier to unit-test and adapt as
the system involves.

#### In methods
If a method has multiple isolated blocks consider naming these blocks by extracting them
to helper methods that do just one thing.  Besides making the calling sites read less
like code and more like english, the extracted sites are often easier to flow-analyse for
human eyes.  The classic case is branched variable assignment.  In the extreme, never do
this:

    :::java
    void calculate(Subject subject) {
      double weight;
      if (useWeightingService(subject)) {
        try {
          weight = weightingService.weight(subject.id);
        } catch (RemoteException e) {
          throw new LayerSpecificException("Failed to look up weight for " + subject, e)
        }
      } else {
        weight = defaultInitialRate * (1 + onlineLearnedBoost);
      }

      // Use weight here for further calculations
    }

Instead do this:

    :::java
    void calculate(Subject subject) {
      double weight = calculateWeight(subject);

      // Use weight here for further calculations
    }

    private double calculateWeight(Subject subject) throws LayerSpecificException {
      if (useWeightingService(subject)) {
        return fetchSubjectWeight(subject.id)
      } else {
        return currentDefaultRate();
      }
    }

    private double fetchSubjectWeight(long subjectId) {
      try {
        return weightingService.weight(subjectId);
      } catch (RemoteException e) {
        throw new LayerSpecificException("Failed to look up weight for " + subject, e)
      }
    }

    private double currentDefaultRate() {
      defaultInitialRate * (1 + onlineLearnedBoost);
    }

A code reader that generally trusts methods do what they say can scan calculate
quickly now and drill down only to those methods where I want to learn more.

### Don't Repeat Yourself ([DRY](http://en.wikipedia.org/wiki/Don't_repeat_yourself))
For a more long-winded discussion on this topic, read
[here](http://c2.com/cgi/wiki?DontRepeatYourself).



# 4. Follow Effective Java when appropriate

Presents the most practical, authoritative guidelines available for writing efficient, well-designed programs for the Java platform
Completely updated for Java releases since 2008
Java programming paradigm has evolved significantly in last 5 years and new material covered in this edition is critical to modern Java programming


Thoroughly revised and updated to cover language and library features added in Java 7, 8, and 9, and recent trends in Java programming. Many new items have been added, including a chapter devoted to lambdas and streams. New topics include:
--Functional interfaces, lambda expressions, method references, and streams
--Default and static methods in interfaces
--Type inference, including the diamond operator for generic types
--The @SafeVarargs annotation
--The try-with-resources statement
--New library features such as java.time and the convenience factory methods for collections


* [CREATING AND DESTROYING OBJECTS](https://github.com/HugoMatilla/Effective-JAVA-Summary#2-creating-and-destroying-objects)
    * Item 1: [Use STATIC FACTORY METHODS instead of constructors](https://github.com/HugoMatilla/Effective-JAVA-Summary#1-use-static-factory-methods-instead-of-constructors)
    * Item 2: [Use BUILDERS when faced with many constructors](https://github.com/HugoMatilla/Effective-JAVA-Summary#2-use-builders-when-faced-with-many-constructors)
    * Item 3: [Enforce the singleton property with a private constructor or an enum type](https://github.com/HugoMatilla/Effective-JAVA-Summary#3-enforce-the-singleton-property-with-a-private-constructor-or-an-enum-type)
    * Item 4: [Enforce noninstantiability with a private constructor](https://github.com/HugoMatilla/Effective-JAVA-Summary#4-enforce-noninstantiability-with-a-private-constructor)
    * Item 5: Prefer dependency injection to hardwiring resources 20
    * Item 6: [Avoid creating objects](https://github.com/HugoMatilla/Effective-JAVA-Summary#5-avoid-creating-objects)
    * Item 7: [Eliminate obsolete object references](https://github.com/HugoMatilla/Effective-JAVA-Summary#6-eliminate-obsolete-object-references)
    * Item 8: [Avoid finalizers](https://github.com/HugoMatilla/Effective-JAVA-Summary#7-avoid-finalizers)
    * Item 9: Prefer try-with-resources to try-finally 34
* [METHODS COMMON TO ALL OBJECTS](https://github.com/HugoMatilla/Effective-JAVA-Summary#3-methods-common-to-all-objects)
    * Item 10: [Obey the general contract when overriding *equals*](https://github.com/HugoMatilla/Effective-JAVA-Summary#8-obey-the-general-contract-when-overriding-equals)
    * Item 11: [Always override *hashCode* when you override *equals*](https://github.com/HugoMatilla/Effective-JAVA-Summary#9-always-override-hashcode-when-you-override-equals)
    * Item 12: [Always override *toString*](https://github.com/HugoMatilla/Effective-JAVA-Summary#10-always-override-tostring)
    * Item 13: [Override *clone* judiciously](https://github.com/HugoMatilla/Effective-JAVA-Summary#11-override-clone-judiciously)
    * Item 14: [Consider implementing *Comparable*](https://github.com/HugoMatilla/Effective-JAVA-Summary#12-consider-implementing-comparable)
* [CLASSES AND INTERFACES](https://github.com/HugoMatilla/Effective-JAVA-Summary#4-classes-and-interfaces)
    * [15. Minimize the accessibility of classes and members](https://github.com/HugoMatilla/Effective-JAVA-Summary#13-minimize-the-accesibility-of-classes-and-members)
    * [16. In public classes, use accessor methods, not public fields](https://github.com/HugoMatilla/Effective-JAVA-Summary#14-in-public-classes-use-accessor-methods-not-public-fields)
    * [17. Minimize Mutability](https://github.com/HugoMatilla/Effective-JAVA-Summary#15-minimize-mutability)
    * [18. Favor composition over inheritance](https://github.com/HugoMatilla/Effective-JAVA-Summary#16-favor-composition-over-inheritance)
    * [19. Design and document for inheritance or else prohibit it.](https://github.com/HugoMatilla/Effective-JAVA-Summary#17-design-and-document-for-inheritance-or-else-prohibit-it)
    * [20. Prefer interfaces to abstract classes](https://github.com/HugoMatilla/Effective-JAVA-Summary#18-prefer-interfaces-to-abstract-classes)
    * Item 21: Design interfaces for posterity 104
    * Item 22: [Use interfaces only to define types](https://github.com/HugoMatilla/Effective-JAVA-Summary#19-use-interfaces-only-to-define-types)
    * Item 23: [Prefer class hierarchies to tagged classes](https://github.com/HugoMatilla/Effective-JAVA-Summary#20-prefer-class-hierarchies-to-tagged-classes)
    * Item 24: [Favor static member classes over nonstatic](https://github.com/HugoMatilla/Effective-JAVA-Summary#22-favor-static-member-classes-over-nonstatic)
    * Item 25: Limit source files to a single top-level class 115
* [GENERICS](https://github.com/HugoMatilla/Effective-JAVA-Summary#5-generics)
    * [26. Don't use raw types in new code](https://github.com/HugoMatilla/Effective-JAVA-Summary#23-dont-use-raw-types-in-new-code)
    * [27. Eliminate unchecked warnings](https://github.com/HugoMatilla/Effective-JAVA-Summary#24-eliminate-unchecked-warnings)
    * [28. Prefer lists to arrays](https://github.com/HugoMatilla/Effective-JAVA-Summary#25-prefer-lists-to-arrays)
    * [29. Favor generic types](https://github.com/HugoMatilla/Effective-JAVA-Summary#26-favor-generic-types)
    * [30. Favor generic Methods](https://github.com/HugoMatilla/Effective-JAVA-Summary#27-favor-generic-methods)
    * [31. Use bounded wildcards to increase API flexibility](https://github.com/HugoMatilla/Effective-JAVA-Summary#28-use-bounded-wildcards-to-increase-api-flexibility)
    * Item 32: Combine generics and varargs judiciously 146
    * Item 33: [Consider *typesafe heterogeneous containers*](https://github.com/HugoMatilla/Effective-JAVA-Summary#29-consider-typesafe-heterogeneous-containers)
* [ENUMS AND ANNOTATIONS](https://github.com/HugoMatilla/Effective-JAVA-Summary#6-enums-and-annotations)
    * [34. Use enums instead of *int* constants](https://github.com/HugoMatilla/Effective-JAVA-Summary#30-use-enums-instead-of-int-constants)
    * [35. Use instance fields instead of ordinals](https://github.com/HugoMatilla/Effective-JAVA-Summary#31-use-instance-fields-instead-of-ordinals)
    * [36. Use EnumSet instead of bit fields](https://github.com/HugoMatilla/Effective-JAVA-Summary#32-use-enumset-instead-of-bit-fields)
    * [37. Use EnumMap instead of ordinal indexing](https://github.com/HugoMatilla/Effective-JAVA-Summary#33-use-enummap-instead-of-ordinal-indexing)
    * [38. Emulate extensible enums with interfaces](https://github.com/HugoMatilla/Effective-JAVA-Summary#34-emulate-extensible-enums-with-interfaces)
    * [39. Prefer annotations to naming patterns](https://github.com/HugoMatilla/Effective-JAVA-Summary#35-prefer-annotations-to-naming-patterns)
    * [40. Consistently use the *Override* annotation](https://github.com/HugoMatilla/Effective-JAVA-Summary#36-consistently-use-the-override-annotation)
    * [41. Use marker interfaces to define types](https://github.com/HugoMatilla/Effective-JAVA-Summary#37-use-marker-interfaces-to-define-types)
* LAMBDAS AND STREAMS
    * Item 42: Prefer lambdas to anonymous classes 193
    * Item 43: Prefer method references to lambdas 197
    * Item 44: Favor the use of standard functional interfaces 199
    * Item 45: Use streams judiciously 203
    * Item 46: Prefer side-effect-free functions in streams 210
    * Item 47: Prefer Collection to Stream as a return type 216
    * Item 48: Use caution when making streams parallel 222
* [METHODS](https://github.com/HugoMatilla/Effective-JAVA-Summary#6-methods)
    * [49. Check parameters for validity](https://github.com/HugoMatilla/Effective-JAVA-Summary#38-check-parameters-for-validity)
    * [50. Make defensive copies when needed.](https://github.com/HugoMatilla/Effective-JAVA-Summary#39-make-defensive-copies-when-needed)
    * [51. Design method signatures carefully](https://github.com/HugoMatilla/Effective-JAVA-Summary#40-design-method-signatures-carefully)
    * [52. Use overloading judiciously](https://github.com/HugoMatilla/Effective-JAVA-Summary#41-use-overloading-judiciously)
    * [53. Use varargs judiciously](https://github.com/HugoMatilla/Effective-JAVA-Summary#42-use-varargs-judiciously)
    * [54. Return empty arrays or collections, not nulls](https://github.com/HugoMatilla/Effective-JAVA-Summary#43-return-empty-arrays-or-collections-not-nulls)
    * Item 55: Return optionals judiciously 249
    * Item 56: [Write *doc comments* for all exposed API elements](https://github.com/HugoMatilla/Effective-JAVA-Summary#44-write-doc-comments-for-all-exposed-api-elemnts)
* [GENERAL PROGRAMMING](https://github.com/HugoMatilla/Effective-JAVA-Summary#7-general-programming)
    * [57. Minimize the scope of local variables.](https://github.com/HugoMatilla/Effective-JAVA-Summary#45-minimize-the-scope-of-local-variables)
    * [58. Prefer for-each loops to traditional for loops.](https://github.com/HugoMatilla/Effective-JAVA-Summary#46-prefer-for-each-loops-to-traditional-for-loops)
    * [59. Know and use libraries](https://github.com/HugoMatilla/Effective-JAVA-Summary#47-know-and-use-libraries)
    * [60. Avoid float and double if exact answer are required](https://github.com/HugoMatilla/Effective-JAVA-Summary#48-avoid-float-and-double-if-exact-answer-are-required)
    * [61. Prefer primitive types to boxed primitives](https://github.com/HugoMatilla/Effective-JAVA-Summary#49-prefer-primitive-types-to-boxed-primitives)
    * [62. Avoid Strings where other types are more appropriate](https://github.com/HugoMatilla/Effective-JAVA-Summary#50-avoid-strings-where-other-types-are-more-appropriate)
    * [63. Beware the performance of string concatenation](https://github.com/HugoMatilla/Effective-JAVA-Summary#51-beware-the-performance-of-string-concatenation)
    * [64. Refer to objects by their interface](https://github.com/HugoMatilla/Effective-JAVA-Summary#52-refer-to-objects-by-their-interface)
    * [65. Prefer interfaces to reflection](https://github.com/HugoMatilla/Effective-JAVA-Summary#53-prefer-interfaces-to-reflection)
    * [66. Use native methods judiciously](https://github.com/HugoMatilla/Effective-JAVA-Summary#54-use-native-methods-judiciously)
    * [67. Optimize judiciously](https://github.com/HugoMatilla/Effective-JAVA-Summary#55-optimize-judiciously)
    * [68. Adhere to generally accepted naming conventions](https://github.com/HugoMatilla/Effective-JAVA-Summary#56-adhere-to-generally-accepted-naming-conventions)
* [EXCEPTIONS](https://github.com/HugoMatilla/Effective-JAVA-Summary#9-exceptions)
    * [57. Use exceptions only for exceptional conditions](https://github.com/HugoMatilla/Effective-JAVA-Summary#57-use-exceptions-only-for-exceptional-conditions)
    * [58. Use checked exceptions for recoverable conditions and runtime exceptions for programming errors](https://github.com/HugoMatilla/Effective-JAVA-Summary#58-use-checked-exceptions-for-recoverable-conditions-and-runtime-exceptions-for-programming-errors)
    * [59. Avoid unnecessary use of checked exceptions](https://github.com/HugoMatilla/Effective-JAVA-Summary#59-avoid-unnecessary-use-of-checked-exceptions)
    * [60. Favor the use of standard exceptions](https://github.com/HugoMatilla/Effective-JAVA-Summary#60-favor-the-use-of-standard-exceptions)
    * [61. Throw exceptions appropriate to the abstraction](https://github.com/HugoMatilla/Effective-JAVA-Summary#61-throw-exceptions-appropriate-to-the-abstraction)
    * [62. Document all exceptions thrown by each method](https://github.com/HugoMatilla/Effective-JAVA-Summary#62-document-all-exceptions-thrown-by-each-method)
    * [63. Include failure-capture information in detail messages](https://github.com/HugoMatilla/Effective-JAVA-Summary#63-include-failure-capture-information-in-detail-messages)
    * [64. Strive for failure atomicity](https://github.com/HugoMatilla/Effective-JAVA-Summary#64-strive-for-failure-atomicity)
    * [65. Don't ignore exceptions](https://github.com/HugoMatilla/Effective-JAVA-Summary#65-dont-ignore-exceptions)
* [CONCURRENCY](https://github.com/HugoMatilla/Effective-JAVA-Summary#10-concurrency)
    * [66. Synchronize access to shared mutable data](https://github.com/HugoMatilla/Effective-JAVA-Summary#66-synchronize-access-to-shared-mutable-data)
    * [67. Avoid excessive synchronization](https://github.com/HugoMatilla/Effective-JAVA-Summary#67-avoid-excessive-synchronization)
    * [68. Prefer executors and tasks to threads](https://github.com/HugoMatilla/Effective-JAVA-Summary#68-prefer-executors-and-tasks-to-threads)
    * [69. Prefer concurrency utilities to *wait* and *notify*](https://github.com/HugoMatilla/Effective-JAVA-Summary#69-prefer-concurrency-utilities-to-wait-and-notify)
    * [70. Document thread safety](https://github.com/HugoMatilla/Effective-JAVA-Summary#70-document-thread-safety)
    * [71. Use lazy initialization judiciously](https://github.com/HugoMatilla/Effective-JAVA-Summary#71-use-lazy-initialization-judiciously)
    * [72. Don't depend on thread scheduler](https://github.com/HugoMatilla/Effective-JAVA-Summary#72-dont-depend-on-thread-scheduler)
    * [73. Avoid thread groups](https://github.com/HugoMatilla/Effective-JAVA-Summary#73-avoid-thread-groups)
* [SERIALIZATION](https://github.com/HugoMatilla/Effective-JAVA-Summary#11-serialization)
    * 85: Prefer alternatives to Java serialization 339
    * [86. Implement Serializable with great caution](https://github.com/HugoMatilla/Effective-JAVA-Summary#74-implement-serializable-judiciously)
    * [87. Consider using a custom serialized form](https://github.com/HugoMatilla/Effective-JAVA-Summary#75-consider-using-a-custom-serialized-form)
    * [88. Write *readObject* methods defensively](https://github.com/HugoMatilla/Effective-JAVA-Summary#76-write-readobject-methods-defensively)
    * [89. For instance control, prefer *enum* types to *readResolve*](https://github.com/HugoMatilla/Effective-JAVA-Summary#77-for-instance-control-prefer-enum-types-to-readresolve)
    * [90. Consider serialization proxies instead of serialized instances](https://github.com/HugoMatilla/Effective-JAVA-Summary#78-consider-serialization-proxies-instead-of-serialized-instances)




# 5. SEI CERT Oracle Coding Standard for Java

## [Rule 00. Input Validation and Data Sanitization (IDS)](https://wiki.sei.cmu.edu/confluence/pages/viewpage.action?pageId=88487865)

* [IDS00-J. Prevent SQL injection](https://wiki.sei.cmu.edu/confluence/display/java/IDS00-J.+Prevent+SQL+injection)
* [IDS01-J. Normalize strings before validating them](https://wiki.sei.cmu.edu/confluence/display/java/IDS01-J.+Normalize+strings+before+validating+them)
* [IDS02-J. Canonicalize path names before validating them](https://wiki.sei.cmu.edu/confluence/display/java/IDS02-J.+Canonicalize+path+names+before+validating+them)
* [IDS03-J. Do not log unsanitized user input](https://wiki.sei.cmu.edu/confluence/display/java/IDS03-J.+Do+not+log+unsanitized+user+input)
* [IDS04-J. Safely extract files from ZipInputStream](https://wiki.sei.cmu.edu/confluence/display/java/IDS04-J.+Safely+extract+files+from+ZipInputStream)
* [IDS05-J. Use a safe subset of ASCII for file and path names](https://wiki.sei.cmu.edu/confluence/display/java/IDS05-J.+Use+a+safe+subset+of+ASCII+for+file+and+path+names)
* [IDS06-J. Exclude unsanitized user input from format strings](https://wiki.sei.cmu.edu/confluence/display/java/IDS06-J.+Exclude+unsanitized+user+input+from+format+strings)
* [IDS07-J. Sanitize untrusted data passed to the Runtime.exec() method](https://wiki.sei.cmu.edu/confluence/display/java/IDS07-J.+Sanitize+untrusted+data+passed+to+the+Runtime.exec%28%29+method)
* [IDS08-J. Sanitize untrusted data included in a regular expression](https://wiki.sei.cmu.edu/confluence/display/java/IDS08-J.+Sanitize+untrusted+data+included+in+a+regular+expression)
* [IDS09-J. Specify an appropriate locale when comparing locale-dependent data](https://wiki.sei.cmu.edu/confluence/display/java/IDS09-J.+Specify+an+appropriate+locale+when+comparing+locale-dependent+data)
* [IDS10-J. Don't form strings containing partial characters](https://wiki.sei.cmu.edu/confluence/display/java/IDS10-J.+Don%27t+form+strings+containing+partial+characters)
* [IDS11-J. Perform any string modifications before validation](https://wiki.sei.cmu.edu/confluence/display/java/IDS11-J.+Perform+any+string+modifications+before+validation)
* [IDS13-J. Use compatible character encodings on both sides of file or network IO](https://wiki.sei.cmu.edu/confluence/display/java/IDS13-J.+Use+compatible+character+encodings+on+both+sides+of+file+or+network+IO)
* [IDS14-J. Do not trust the contents of hidden form fields](https://wiki.sei.cmu.edu/confluence/display/java/IDS14-J.+Do+not+trust+the+contents+of+hidden+form+fields)
* [IDS15-J. Do not allow sensitive information to leak outside a trust boundary](https://wiki.sei.cmu.edu/confluence/display/java/IDS15-J.+Do+not+allow+sensitive+information+to+leak+outside+a+trust+boundary)
* [IDS16-J. Prevent XML Injection](https://wiki.sei.cmu.edu/confluence/display/java/IDS16-J.+Prevent+XML+Injection)
* [IDS17-J. Prevent XML External Entity Attacks](https://wiki.sei.cmu.edu/confluence/display/java/IDS17-J.+Prevent+XML+External+Entity+Attacks)
* [IDS50-J. Use conservative file naming conventions](https://wiki.sei.cmu.edu/confluence/display/java/IDS50-J.+Use+conservative+file+naming+conventions)
* [IDS51-J. Properly encode or escape output](https://wiki.sei.cmu.edu/confluence/display/java/IDS51-J.+Properly+encode+or+escape+output)
* [IDS52-J. Prevent code injection](https://wiki.sei.cmu.edu/confluence/display/java/IDS52-J.+Prevent+code+injection)
* [IDS53-J. Prevent XPath Injection](https://wiki.sei.cmu.edu/confluence/display/java/IDS53-J.+Prevent+XPath+Injection)
* [IDS54-J. Prevent LDAP injection](https://wiki.sei.cmu.edu/confluence/display/java/IDS54-J.+Prevent+LDAP+injection)
* [IDS55-J. Understand how escape characters are interpreted when strings are loaded](https://wiki.sei.cmu.edu/confluence/display/java/IDS55-J.+Understand+how+escape+characters+are+interpreted+when+strings+are+loaded)
* [IDS56-J. Prevent arbitrary file upload](https://wiki.sei.cmu.edu/confluence/display/java/IDS56-J.+Prevent+arbitrary+file+upload)





## [Rule 01. Declarations and Initialization (DCL)](https://wiki.sei.cmu.edu/confluence/pages/viewpage.action?pageId=88487858)

* [DCL00-J. Prevent class initialization cycles](https://wiki.sei.cmu.edu/confluence/display/java/DCL00-J.+Prevent+class+initialization+cycles)
* [DCL01-J. Do not reuse public identifiers from the Java Standard Library](https://wiki.sei.cmu.edu/confluence/display/java/DCL01-J.+Do+not+reuse+public+identifiers+from+the+Java+Standard+Library)
* [DCL02-J. Do not modify the collection's elements during an enhanced for statement](https://wiki.sei.cmu.edu/confluence/display/java/DCL02-J.+Do+not+modify+the+collection%27s+elements+during+an+enhanced+for+statement)
* [DCL50-J. Use visually distinct identifiers](https://wiki.sei.cmu.edu/confluence/display/java/DCL50-J.+Use+visually+distinct+identifiers)
* [DCL51-J. Do not shadow or obscure identifiers in subscopes](https://wiki.sei.cmu.edu/confluence/display/java/DCL51-J.+Do+not+shadow+or+obscure+identifiers+in+subscopes)
* [DCL52-J. Do not declare more than one variable per declaration](https://wiki.sei.cmu.edu/confluence/display/java/DCL52-J.+Do+not+declare+more+than+one+variable+per+declaration)
* [DCL53-J. Minimize the scope of variables](https://wiki.sei.cmu.edu/confluence/display/java/DCL53-J.+Minimize+the+scope+of+variables)
* [DCL54-J. Use meaningful symbolic constants to represent literal values in program logic](https://wiki.sei.cmu.edu/confluence/display/java/DCL54-J.+Use+meaningful+symbolic+constants+to+represent+literal+values+in+program+logic)
* [DCL55-J. Properly encode relationships in constant definitions](https://wiki.sei.cmu.edu/confluence/display/java/DCL55-J.+Properly+encode+relationships+in+constant+definitions)
* [DCL56-J. Do not attach significance to the ordinal associated with an enum](https://wiki.sei.cmu.edu/confluence/display/java/DCL56-J.+Do+not+attach+significance+to+the+ordinal+associated+with+an+enum)
* [DCL57-J. Avoid ambiguous overloading of variable arity methods](https://wiki.sei.cmu.edu/confluence/display/java/DCL57-J.+Avoid+ambiguous+overloading+of+variable+arity+methods)
* [DCL58-J. Enable compile-time type checking of variable arity parameter types](https://wiki.sei.cmu.edu/confluence/display/java/DCL58-J.+Enable+compile-time+type+checking+of+variable+arity+parameter+types)
* [DCL59-J. Do not apply public final to constants whose value might change in later releases](https://wiki.sei.cmu.edu/confluence/display/java/DCL59-J.+Do+not+apply+public+final+to+constants+whose+value+might+change+in+later+releases)
* [DCL60-J. Avoid cyclic dependencies between packages](https://wiki.sei.cmu.edu/confluence/display/java/DCL60-J.+Avoid+cyclic+dependencies+between+packages)
* [DCL61-J. Do not use raw types](https://wiki.sei.cmu.edu/confluence/display/java/DCL61-J.+Do+not+use+raw+types)




## [Rule 02. Expressions (EXP)](https://wiki.sei.cmu.edu/confluence/pages/viewpage.action?pageId=88487704)

* [EXP00-J. Do not ignore values returned by methods](https://wiki.sei.cmu.edu/confluence/display/java/EXP00-J.+Do+not+ignore+values+returned+by+methods)
* [EXP01-J. Do not use a null in a case where an object is required](https://wiki.sei.cmu.edu/confluence/display/java/EXP01-J.+Do+not+use+a+null+in+a+case+where+an+object+is+required)
* [EXP02-J. Do not use the Object.equals() method to compare two arrays](https://wiki.sei.cmu.edu/confluence/display/java/EXP02-J.+Do+not+use+the+Object.equals%28%29+method+to+compare+two+arrays)
* [EXP03-J. Do not use the equality operators when comparing values of boxed primitives](https://wiki.sei.cmu.edu/confluence/display/java/EXP03-J.+Do+not+use+the+equality+operators+when+comparing+values+of+boxed+primitives)
* [EXP04-J. Do not pass arguments to certain Java Collections Framework methods that are a different type than the collection parameter type](https://wiki.sei.cmu.edu/confluence/display/java/EXP04-J.+Do+not+pass+arguments+to+certain+Java+Collections+Framework+methods+that+are+a+different+type+than+the+collection+parameter+type)
* [EXP05-J. Do not follow a write by a subsequent write or read of the same object within an expression](https://wiki.sei.cmu.edu/confluence/display/java/EXP05-J.+Do+not+follow+a+write+by+a+subsequent+write+or+read+of+the+same+object+within+an+expression)
* [EXP06-J. Expressions used in assertions must not produce side effects](https://wiki.sei.cmu.edu/confluence/display/java/EXP06-J.+Expressions+used+in+assertions+must+not+produce+side+effects)
* [EXP07-J. Prevent loss of useful data due to weak references](https://wiki.sei.cmu.edu/confluence/display/java/EXP07-J.+Prevent+loss+of+useful+data+due+to+weak+references)
* [EXP50-J. Do not confuse abstract object equality with reference equality](https://wiki.sei.cmu.edu/confluence/display/java/EXP50-J.+Do+not+confuse+abstract+object+equality+with+reference+equality)
* [EXP51-J. Do not perform assignments in conditional expressions](https://wiki.sei.cmu.edu/confluence/display/java/EXP51-J.+Do+not+perform+assignments+in+conditional+expressions)
* [EXP52-J. Use braces for the body of an if, for, or while statement](https://wiki.sei.cmu.edu/confluence/display/java/EXP52-J.+Use+braces+for+the+body+of+an+if%2C+for%2C+or+while+statement)
* [EXP53-J. Use parentheses for precedence of operation](https://wiki.sei.cmu.edu/confluence/display/java/EXP53-J.+Use+parentheses+for+precedence+of+operation)
* [EXP54-J. Understand the differences between bitwise and logical operators](https://wiki.sei.cmu.edu/confluence/display/java/EXP54-J.+Understand+the+differences+between+bitwise+and+logical+operators)
* [EXP55-J. Use the same type for the second and third operands in conditional expressions](https://wiki.sei.cmu.edu/confluence/display/java/EXP55-J.+Use+the+same+type+for+the+second+and+third+operands+in+conditional+expressions)





## [Rule 03. Numeric Types and Operations (NUM)](https://wiki.sei.cmu.edu/confluence/pages/viewpage.action?pageId=88487628)

* [NUM00-J. Detect or prevent integer overflow](https://wiki.sei.cmu.edu/confluence/display/java/NUM00-J.+Detect+or+prevent+integer+overflow)
* [NUM01-J. Do not perform bitwise and arithmetic operations on the same data](https://wiki.sei.cmu.edu/confluence/display/java/NUM01-J.+Do+not+perform+bitwise+and+arithmetic+operations+on+the+same+data)
* [NUM02-J. Ensure that division and remainder operations do not result in divide-by-zero errors](https://wiki.sei.cmu.edu/confluence/display/java/NUM02-J.+Ensure+that+division+and+remainder+operations+do+not+result+in+divide-by-zero+errors)
* [NUM03-J. Use integer types that can fully represent the possible range of unsigned data](https://wiki.sei.cmu.edu/confluence/display/java/NUM03-J.+Use+integer+types+that+can+fully+represent+the+possible+range+of++unsigned+data)
* [NUM04-J. Do not use floating-point numbers if precise computation is required](https://wiki.sei.cmu.edu/confluence/display/java/NUM04-J.+Do+not+use+floating-point+numbers+if+precise+computation+is+required)
* [NUM07-J. Do not attempt comparisons with NaN](https://wiki.sei.cmu.edu/confluence/display/java/NUM07-J.+Do+not+attempt+comparisons+with+NaN)
* [NUM08-J. Check floating-point inputs for exceptional values](https://wiki.sei.cmu.edu/confluence/display/java/NUM08-J.+Check+floating-point+inputs+for+exceptional+values)
* [NUM09-J. Do not use floating-point variables as loop counters](https://wiki.sei.cmu.edu/confluence/display/java/NUM09-J.+Do+not+use+floating-point+variables+as+loop+counters)
* [NUM10-J. Do not construct BigDecimal objects from floating-point literals](https://wiki.sei.cmu.edu/confluence/display/java/NUM10-J.+Do+not+construct+BigDecimal+objects+from+floating-point+literals)
* [NUM11-J. Do not compare or inspect the string representation of floating-point values](https://wiki.sei.cmu.edu/confluence/display/java/NUM11-J.+Do+not+compare+or+inspect+the+string+representation+of+floating-point+values)
* [NUM12-J. Ensure conversions of numeric types to narrower types do not result in lost or misinterpreted data](https://wiki.sei.cmu.edu/confluence/display/java/NUM12-J.+Ensure+conversions+of+numeric+types+to+narrower+types+do+not+result+in+lost+or+misinterpreted+data)
* [NUM13-J. Avoid loss of precision when converting primitive integers to floating-point](https://wiki.sei.cmu.edu/confluence/display/java/NUM13-J.+Avoid+loss+of+precision+when+converting+primitive+integers+to+floating-point)
* [NUM14-J. Use shift operators correctly](https://wiki.sei.cmu.edu/confluence/display/java/NUM14-J.+Use+shift+operators+correctly)
* [NUM50-J. Convert integers to floating point for floating-point operations](https://wiki.sei.cmu.edu/confluence/display/java/NUM50-J.+Convert+integers+to+floating+point+for+floating-point+operations)
* [NUM51-J. Do not assume that the remainder operator always returns a nonnegative result for integral operands](https://wiki.sei.cmu.edu/confluence/display/java/NUM51-J.+Do+not+assume+that+the+remainder+operator+always+returns+a+nonnegative+result+for+integral+operands)
* [NUM52-J. Be aware of numeric promotion behavior](https://wiki.sei.cmu.edu/confluence/display/java/NUM52-J.+Be+aware+of+numeric+promotion+behavior)
* [NUM53-J. Use the strictfp modifier for floating-point calculation consistency across platforms](https://wiki.sei.cmu.edu/confluence/display/java/NUM53-J.+Use+the+strictfp+modifier+for+floating-point+calculation+consistency+across+platforms)
* [NUM54-J. Do not use denormalized numbers](https://wiki.sei.cmu.edu/confluence/display/java/NUM54-J.+Do+not+use+denormalized+numbers)
* 




## [Rule 04. Characters and Strings (STR)](https://wiki.sei.cmu.edu/confluence/pages/viewpage.action?pageId=88487607)

* [STR00-J. Don't form strings containing partial characters from variable-width encodings](https://wiki.sei.cmu.edu/confluence/display/java/STR00-J.+Don%27t+form+strings+containing+partial+characters+from+variable-width+encodings)
* [STR01-J. Do not assume that a Java char fully represents a Unicode code point](https://wiki.sei.cmu.edu/confluence/display/java/STR01-J.+Do+not+assume+that+a+Java+char+fully+represents+a+Unicode+code+point)
* [STR02-J. Specify an appropriate locale when comparing locale-dependent data](https://wiki.sei.cmu.edu/confluence/display/java/STR02-J.+Specify+an+appropriate+locale+when+comparing+locale-dependent+data)
* [STR03-J. Do not encode noncharacter data as a string](https://wiki.sei.cmu.edu/confluence/display/java/STR03-J.+Do+not+encode+noncharacter+data+as+a+string)
* [STR04-J. Use compatible character encodings when communicating string data between JVMs](https://wiki.sei.cmu.edu/confluence/display/java/STR04-J.+Use+compatible+character+encodings+when+communicating+string+data+between+JVMs)
* [STR50-J. Use the appropriate method for counting characters in a string](https://wiki.sei.cmu.edu/confluence/display/java/STR50-J.+Use+the+appropriate+method+for+counting+characters+in+a+string)
* [STR51-J. Use the charset encoder and decoder classes when more control over the encoding process is required](https://wiki.sei.cmu.edu/confluence/display/java/STR51-J.+Use+the+charset+encoder+and+decoder+classes+when+more+control+over+the+encoding+process+is+required)





## [Rule 05. Object Orientation (OBJ)](https://wiki.sei.cmu.edu/confluence/pages/viewpage.action?pageId=88487715)

* [OBJ01-J. Limit accessibility of fields](https://wiki.sei.cmu.edu/confluence/display/java/OBJ01-J.+Limit+accessibility+of+fields)
* [OBJ02-J. Preserve dependencies in subclasses when changing superclasses](https://wiki.sei.cmu.edu/confluence/display/java/OBJ02-J.+Preserve+dependencies+in+subclasses+when+changing+superclasses)
* [OBJ03-J. Prevent heap pollution](https://wiki.sei.cmu.edu/confluence/display/java/OBJ03-J.+Prevent+heap+pollution)
* [OBJ04-J. Provide mutable classes with copy functionality to safely allow passing instances to untrusted code](https://wiki.sei.cmu.edu/confluence/display/java/OBJ04-J.+Provide+mutable+classes+with+copy+functionality+to+safely+allow+passing+instances+to+untrusted+code)
* [OBJ05-J. Do not return references to private mutable class members](https://wiki.sei.cmu.edu/confluence/display/java/OBJ05-J.+Do+not+return+references+to+private+mutable+class+members)
* [OBJ06-J. Defensively copy mutable inputs and mutable internal components](https://wiki.sei.cmu.edu/confluence/display/java/OBJ06-J.+Defensively+copy+mutable+inputs+and+mutable+internal+components)
* [OBJ07-J. Sensitive classes must not let themselves be copied](https://wiki.sei.cmu.edu/confluence/display/java/OBJ07-J.+Sensitive+classes+must+not+let+themselves+be+copied)
* [OBJ08-J. Do not expose private members of an outer class from within a nested class](https://wiki.sei.cmu.edu/confluence/display/java/OBJ08-J.+Do+not+expose+private+members+of+an+outer+class+from+within+a+nested+class)
* [OBJ09-J. Compare classes and not class names](https://wiki.sei.cmu.edu/confluence/display/java/OBJ09-J.+Compare+classes+and+not+class+names)
* [OBJ10-J. Do not use public static nonfinal fields](https://wiki.sei.cmu.edu/confluence/display/java/OBJ10-J.+Do+not+use+public+static+nonfinal+fields)
* [OBJ11-J. Be wary of letting constructors throw exceptions](https://wiki.sei.cmu.edu/confluence/display/java/OBJ11-J.+Be+wary+of+letting+constructors+throw+exceptions)
* [OBJ12-J. Respect object-based annotations](https://wiki.sei.cmu.edu/confluence/display/java/OBJ12-J.+Respect+object-based+annotations)
* [OBJ13-J. Ensure that references to mutable objects are not exposed](https://wiki.sei.cmu.edu/confluence/display/java/OBJ13-J.+Ensure+that+references+to+mutable+objects+are+not+exposed)
* [OBJ14-J. Do not use an object that has been freed.](https://wiki.sei.cmu.edu/confluence/pages/viewpage.action?pageId=88487820)
* [OBJ50-J. Never confuse the immutability of a reference with that of the referenced object](https://wiki.sei.cmu.edu/confluence/display/java/OBJ50-J.+Never+confuse+the+immutability+of+a+reference+with+that+of+the+referenced+object)
* [OBJ51-J. Minimize the accessibility of classes and their members](https://wiki.sei.cmu.edu/confluence/display/java/OBJ51-J.+Minimize+the+accessibility+of+classes+and+their+members)
* [OBJ52-J. Write garbage-collection-friendly code](https://wiki.sei.cmu.edu/confluence/display/java/OBJ52-J.+Write+garbage-collection-friendly+code)
* [OBJ53-J. Do not use direct buffers for short-lived, infrequently used objects](https://wiki.sei.cmu.edu/confluence/display/java/OBJ53-J.+Do+not+use+direct+buffers+for+short-lived%2C+infrequently+used+objects)
* [OBJ54-J. Do not attempt to help the garbage collector by setting local reference variables to null](https://wiki.sei.cmu.edu/confluence/display/java/OBJ54-J.+Do+not+attempt+to+help+the+garbage+collector+by+setting+local+reference+variables+to+null)
* [OBJ55-J. Remove short-lived objects from long-lived container objects](https://wiki.sei.cmu.edu/confluence/display/java/OBJ55-J.+Remove+short-lived+objects+from+long-lived+container+objects)
* [OBJ56-J. Provide sensitive mutable classes with unmodifiable wrappers](https://wiki.sei.cmu.edu/confluence/display/java/OBJ56-J.+Provide+sensitive+mutable+classes+with+unmodifiable+wrappers)
* [OBJ57-J. Do not rely on methods that can be overridden by untrusted code](https://wiki.sei.cmu.edu/confluence/display/java/OBJ57-J.+Do+not+rely+on+methods+that+can+be+overridden+by+untrusted+code)
* [OBJ58-J. Limit the extensibility of classes and methods with invarian](https://wiki.sei.cmu.edu/confluence/display/java/OBJ58-J.+Limit+the+extensibility+of+classes+and+methods+with+invariants)




## [Rule 06. Methods (MET)](https://wiki.sei.cmu.edu/confluence/pages/viewpage.action?pageId=88487441)

* [MET00-J. Validate method arguments](https://wiki.sei.cmu.edu/confluence/display/java/MET00-J.+Validate+method+arguments)
* [MET01-J. Never use assertions to validate method arguments](https://wiki.sei.cmu.edu/confluence/display/java/MET01-J.+Never+use+assertions+to+validate+method+arguments)
* [MET02-J. Do not use deprecated or obsolete classes or methods](https://wiki.sei.cmu.edu/confluence/display/java/MET02-J.+Do+not+use+deprecated+or+obsolete+classes+or+methods)
* [MET03-J. Methods that perform a security check must be declared private or final](https://wiki.sei.cmu.edu/confluence/display/java/MET03-J.+Methods+that+perform+a+security+check+must+be+declared+private+or+final)
* [MET04-J. Do not increase the accessibility of overridden or hidden methods](https://wiki.sei.cmu.edu/confluence/display/java/MET04-J.+Do+not+increase+the+accessibility+of+overridden+or+hidden+methods)
* [MET05-J. Ensure that constructors do not call overridable methods](https://wiki.sei.cmu.edu/confluence/display/java/MET05-J.+Ensure+that+constructors+do+not+call+overridable+methods)
* [MET06-J. Do not invoke overridable methods in clone()](https://wiki.sei.cmu.edu/confluence/pages/viewpage.action?pageId=88487921)
* [MET07-J. Never declare a class method that hides a method declared in a superclass or superinterface](https://wiki.sei.cmu.edu/confluence/display/java/MET07-J.+Never+declare+a+class+method+that+hides+a+method+declared+in+a+superclass+or+superinterface)
* [MET08-J. Preserve the equality contract when overriding the equals() method](https://wiki.sei.cmu.edu/confluence/display/java/MET08-J.+Preserve+the+equality+contract+when+overriding+the+equals%28%29+method)
* [MET09-J. Classes that define an equals() method must also define a hashCode() method](https://wiki.sei.cmu.edu/confluence/display/java/MET09-J.+Classes+that+define+an+equals%28%29+method+must+also+define+a+hashCode%28%29+method)
* [MET10-J. Follow the general contract when implementing the compareTo() method](https://wiki.sei.cmu.edu/confluence/display/java/MET10-J.+Follow+the+general+contract+when+implementing+the+compareTo%28%29+method)
* [MET11-J. Ensure that keys used in comparison operations are immutable](https://wiki.sei.cmu.edu/confluence/display/java/MET11-J.+Ensure+that+keys+used+in+comparison+operations+are+immutable)
* [MET12-J. Do not use finalizers](https://wiki.sei.cmu.edu/confluence/display/java/MET12-J.+Do+not+use+finalizers)
* [MET13-J. Do not assume that reassigning method arguments modifies the calling environment](https://wiki.sei.cmu.edu/confluence/display/java/MET13-J.+Do+not+assume+that+reassigning+method+arguments+modifies+the+calling+environment)
* [MET50-J. Avoid ambiguous or confusing uses of overloading](https://wiki.sei.cmu.edu/confluence/display/java/MET50-J.+Avoid+ambiguous+or+confusing+uses+of+overloading)
* [MET51-J. Do not use overloaded methods to differentiate between runtime types](https://wiki.sei.cmu.edu/confluence/display/java/MET51-J.+Do+not+use+overloaded+methods+to+differentiate+between+runtime+types)
* [MET52-J. Do not use the clone() method to copy untrusted method parameters](https://wiki.sei.cmu.edu/confluence/display/java/MET52-J.+Do+not+use+the+clone%28%29+method+to+copy+untrusted+method+parameters)
* [MET53-J. Ensure that the clone() method calls super.clone()](https://wiki.sei.cmu.edu/confluence/pages/viewpage.action?pageId=88487446)
* [MET54-J. Always provide feedback about the resulting value of a method](https://wiki.sei.cmu.edu/confluence/display/java/MET54-J.+Always+provide+feedback+about+the+resulting+value+of+a+method)
* [MET55-J. Return an empty array or collection instead of a null value for methods that return an array or collection](https://wiki.sei.cmu.edu/confluence/display/java/MET55-J.+Return+an+empty+array+or+collection+instead+of+a+null+value+for+methods+that+return+an+array+or+collection)
* [MET56-J. Do not use Object.equals() to compare cryptographic keys](https://wiki.sei.cmu.edu/confluence/display/java/MET56-J.+Do+not+use+Object.equals%28%29+to+compare+cryptographic+keys)




## [Rule 07. Exceptional Behavior (ERR)](https://wiki.sei.cmu.edu/confluence/pages/viewpage.action?pageId=88487665)

* [ERR00-J. Do not suppress or ignore checked exceptions](https://wiki.sei.cmu.edu/confluence/display/java/ERR00-J.+Do+not+suppress+or+ignore+checked+exceptions)
* [ERR01-J. Do not allow exceptions to expose sensitive information](https://wiki.sei.cmu.edu/confluence/display/java/ERR01-J.+Do+not+allow+exceptions+to+expose+sensitive+information)
* [ERR02-J. Prevent exceptions while logging data](https://wiki.sei.cmu.edu/confluence/display/java/ERR02-J.+Prevent+exceptions+while+logging+data)
* [ERR03-J. Restore prior object state on method failure](https://wiki.sei.cmu.edu/confluence/display/java/ERR03-J.+Restore+prior+object+state+on+method+failure)
* [ERR04-J. Do not complete abruptly from a finally block](https://wiki.sei.cmu.edu/confluence/display/java/ERR04-J.+Do+not+complete+abruptly+from+a+finally+block)
* [ERR05-J. Do not let checked exceptions escape from a finally block](https://wiki.sei.cmu.edu/confluence/display/java/ERR05-J.+Do+not+let+checked+exceptions+escape+from+a+finally+block)
* [ERR06-J. Do not throw undeclared checked exceptions](https://wiki.sei.cmu.edu/confluence/display/java/ERR06-J.+Do+not+throw+undeclared+checked+exceptions)
* [ERR07-J. Do not throw RuntimeException, Exception, or Throwable](https://wiki.sei.cmu.edu/confluence/display/java/ERR07-J.+Do+not+throw+RuntimeException%2C+Exception%2C+or+Throwable)
* [ERR08-J. Do not catch NullPointerException or any of its ancestors](https://wiki.sei.cmu.edu/confluence/display/java/ERR08-J.+Do+not+catch+NullPointerException+or+any+of+its+ancestors)
* [ERR09-J. Do not allow untrusted code to terminate the JVM](https://wiki.sei.cmu.edu/confluence/display/java/ERR09-J.+Do+not+allow+untrusted+code+to+terminate+the+JVM)
* [ERR50-J. Use exceptions only for exceptional conditions](https://wiki.sei.cmu.edu/confluence/display/java/ERR50-J.+Use+exceptions+only+for+exceptional+conditions)
* [ERR51-J. Prefer user-defined exceptions over more general exception types](https://wiki.sei.cmu.edu/confluence/display/java/ERR51-J.+Prefer+user-defined+exceptions+over+more+general+exception+types)
* [ERR52-J. Avoid in-band error indicators](https://wiki.sei.cmu.edu/confluence/display/java/ERR52-J.+Avoid+in-band+error+indicators)
* [ERR53-J. Try to gracefully recover from system errors](https://wiki.sei.cmu.edu/confluence/display/java/ERR53-J.+Try+to+gracefully+recover+from+system+errors)
* [ERR54-J. Use a try-with-resources statement to safely handle closeable resources](https://wiki.sei.cmu.edu/confluence/display/java/ERR54-J.+Use+a+try-with-resources+statement+to+safely+handle+closeable+resources)






## [Rule 08. Visibility and Atomicity (VNA)](https://wiki.sei.cmu.edu/confluence/pages/viewpage.action?pageId=88487824)

* [VNA00-J. Ensure visibility when accessing shared primitive variables](https://wiki.sei.cmu.edu/confluence/display/java/VNA00-J.+Ensure+visibility+when+accessing+shared+primitive+variables)
* [VNA01-J. Ensure visibility of shared references to immutable objects](https://wiki.sei.cmu.edu/confluence/display/java/VNA01-J.+Ensure+visibility+of+shared+references+to+immutable+objects)
* [VNA02-J. Ensure that compound operations on shared variables are atomic](https://wiki.sei.cmu.edu/confluence/display/java/VNA02-J.+Ensure+that+compound+operations+on+shared+variables+are+atomic)
* [VNA03-J. Do not assume that a group of calls to independently atomic methods is atomic](https://wiki.sei.cmu.edu/confluence/display/java/VNA03-J.+Do+not+assume+that+a+group+of+calls+to+independently+atomic+methods+is+atomic)
* [VNA04-J. Ensure that calls to chained methods are atomic](https://wiki.sei.cmu.edu/confluence/display/java/VNA04-J.+Ensure+that+calls+to+chained+methods+are+atomic)
* [VNA05-J. Ensure atomicity when reading and writing 64-bit values](https://wiki.sei.cmu.edu/confluence/display/java/VNA05-J.+Ensure+atomicity+when+reading+and+writing+64-bit+values)




## [Rule 09. Locking (LCK)](https://wiki.sei.cmu.edu/confluence/pages/viewpage.action?pageId=88487666)

* [LCK00-J. Use private final lock objects to synchronize classes that may interact with untrusted code](https://wiki.sei.cmu.edu/confluence/display/java/LCK00-J.+Use+private+final+lock+objects+to+synchronize+classes+that+may+interact+with+untrusted+code)
* [LCK01-J. Do not synchronize on objects that may be reused](https://wiki.sei.cmu.edu/confluence/display/java/LCK01-J.+Do+not+synchronize+on+objects+that+may+be+reused)
* [LCK02-J. Do not synchronize on the class object returned by getClass()](https://wiki.sei.cmu.edu/confluence/pages/viewpage.action?pageId=88487849)
* [LCK03-J. Do not synchronize on the intrinsic locks of high-level concurrency objects](https://wiki.sei.cmu.edu/confluence/display/java/LCK03-J.+Do+not+synchronize+on+the+intrinsic+locks+of+high-level+concurrency+objects)
* [LCK04-J. Do not synchronize on a collection view if the backing collection is accessible](https://wiki.sei.cmu.edu/confluence/display/java/LCK04-J.+Do+not+synchronize+on+a+collection+view+if+the+backing+collection+is+accessible)
* [LCK05-J. Synchronize access to static fields that can be modified by untrusted code](https://wiki.sei.cmu.edu/confluence/display/java/LCK05-J.+Synchronize+access+to+static+fields+that+can+be+modified+by+untrusted+code)
* [LCK06-J. Do not use an instance lock to protect shared static data](https://wiki.sei.cmu.edu/confluence/display/java/LCK06-J.+Do+not+use+an+instance+lock+to+protect+shared+static+data)
* [LCK07-J. Avoid deadlock by requesting and releasing locks in the same order](https://wiki.sei.cmu.edu/confluence/display/java/LCK07-J.+Avoid+deadlock+by+requesting+and+releasing+locks+in+the+same+order)
* [LCK08-J. Ensure actively held locks are released on exceptional conditions](https://wiki.sei.cmu.edu/confluence/display/java/LCK08-J.+Ensure+actively+held+locks+are+released+on+exceptional+conditions)
* [LCK09-J. Do not perform operations that can block while holding a lock](https://wiki.sei.cmu.edu/confluence/display/java/LCK09-J.+Do+not+perform+operations+that+can+block+while+holding+a+lock)
* [LCK10-J. Use a correct form of the double-checked locking idiom](https://wiki.sei.cmu.edu/confluence/display/java/LCK10-J.+Use+a+correct+form+of+the+double-checked+locking+idiom)
* [LCK11-J. Avoid client-side locking when using classes that do not commit to their locking strategy](https://wiki.sei.cmu.edu/confluence/display/java/LCK11-J.+Avoid+client-side+locking+when+using+classes+that+do+not+commit+to+their+locking+strategy)





## [Rule 10. Thread APIs (THI)](https://wiki.sei.cmu.edu/confluence/pages/viewpage.action?pageId=88487735)

* [THI00-J. Do not invoke Thread.run()](https://wiki.sei.cmu.edu/confluence/pages/viewpage.action?pageId=88487912)
* [THI01-J. Do not invoke ThreadGroup methods](https://wiki.sei.cmu.edu/confluence/display/java/THI01-J.+Do+not+invoke+ThreadGroup+methods)
* [THI02-J. Notify all waiting threads rather than a single thread](https://wiki.sei.cmu.edu/confluence/display/java/THI02-J.+Notify+all+waiting+threads+rather+than+a+single+thread)
* [THI03-J. Always invoke wait() and await() methods inside a loop](https://wiki.sei.cmu.edu/confluence/display/java/THI03-J.+Always+invoke+wait%28%29+and+await%28%29+methods+inside+a+loop)
* [THI04-J. Ensure that threads performing blocking operations can be terminated](https://wiki.sei.cmu.edu/confluence/display/java/THI04-J.+Ensure+that+threads+performing+blocking+operations+can+be+terminated)
* [THI05-J. Do not use Thread.stop() to terminate threads](https://wiki.sei.cmu.edu/confluence/display/java/THI05-J.+Do+not+use+Thread.stop%28%29+to+terminate+threads)
* [TPS00-J. Use thread pools to enable graceful degradation of service during traffic bursts](https://wiki.sei.cmu.edu/confluence/display/java/TPS00-J.+Use+thread+pools+to+enable+graceful+degradation+of+service+during+traffic+bursts)
* [TPS01-J. Do not execute interdependent tasks in a bounded thread pool](https://wiki.sei.cmu.edu/confluence/display/java/TPS01-J.+Do+not+execute+interdependent+tasks+in+a+bounded+thread+pool)
* [TPS02-J. Ensure that tasks submitted to a thread pool are interruptible](https://wiki.sei.cmu.edu/confluence/display/java/TPS02-J.+Ensure+that+tasks+submitted+to+a+thread+pool+are+interruptible)
* [TPS03-J. Ensure that tasks executing in a thread pool do not fail silently](https://wiki.sei.cmu.edu/confluence/display/java/TPS03-J.+Ensure+that+tasks+executing+in+a+thread+pool+do+not+fail+silently)
* [TPS04-J. Ensure ThreadLocal variables are reinitialized when using thread pools](https://wiki.sei.cmu.edu/confluence/display/java/TPS04-J.+Ensure+ThreadLocal+variables+are+reinitialized+when+using+thread+pools)
* [TSM00-J. Do not override thread-safe methods with methods that are not thread-safe](https://wiki.sei.cmu.edu/confluence/display/java/TSM00-J.+Do+not+override+thread-safe+methods+with+methods+that+are+not+thread-safe)
* [TSM01-J. Do not let the this reference escape during object construction](https://wiki.sei.cmu.edu/confluence/display/java/TSM01-J.+Do+not+let+the+this+reference+escape+during+object+construction)
* [TSM02-J. Do not use background threads during class initialization](https://wiki.sei.cmu.edu/confluence/display/java/TSM02-J.+Do+not+use+background+threads+during+class+initialization)
* [TSM03-J. Do not publish partially initialized objects](https://wiki.sei.cmu.edu/confluence/display/java/TSM03-J.+Do+not+publish+partially+initialized+objects)



## [Rule 13. Input Output (FIO)](https://wiki.sei.cmu.edu/confluence/pages/viewpage.action?pageId=88487725)

* Page
    [FIO00-J. Do not operate on files in shared directories](https://wiki.sei.cmu.edu/confluence/display/java/FIO00-J.+Do+not+operate+on+files+in+shared+directories)
* [FIO01-J. Create files with appropriate access permissions](https://wiki.sei.cmu.edu/confluence/display/java/FIO01-J.+Create+files+with+appropriate+access+permissions)
* [FIO02-J. Detect and handle file-related errors](https://wiki.sei.cmu.edu/confluence/display/java/FIO02-J.+Detect+and+handle+file-related+errors)
* [FIO03-J. Remove temporary files before termination](https://wiki.sei.cmu.edu/confluence/display/java/FIO03-J.+Remove+temporary+files+before+termination)
* [FIO04-J. Release resources when they are no longer needed](https://wiki.sei.cmu.edu/confluence/display/java/FIO04-J.+Release+resources+when+they+are+no+longer+needed)
* [FIO05-J. Do not expose buffers created using the wrap() or duplicate() methods to untrusted code](https://wiki.sei.cmu.edu/confluence/display/java/FIO05-J.+Do+not+expose+buffers+created+using+the+wrap%28%29+or+duplicate%28%29+methods+to+untrusted+code)
* Page[FIO06-J. Do not create multiple buffered wrappers on a single byte or character stream](https://wiki.sei.cmu.edu/confluence/display/java/FIO06-J.+Do+not+create+multiple+buffered+wrappers+on+a+single+byte+or+character+stream)
* [FIO07-J. Do not let external processes block on IO buffers](https://wiki.sei.cmu.edu/confluence/display/java/FIO07-J.+Do+not+let+external+processes+block+on+IO+buffers)
* [FIO08-J. Distinguish between characters or bytes read from a stream and -1](https://wiki.sei.cmu.edu/confluence/display/java/FIO08-J.+Distinguish+between+characters+or+bytes+read+from+a+stream+and+-1)
* [FIO09-J. Do not rely on the write() method to output integers outside the range 0 to 255](https://wiki.sei.cmu.edu/confluence/display/java/FIO09-J.+Do+not+rely+on+the+write%28%29+method+to+output+integers+outside+the+range+0+to+255)
* [FIO10-J. Ensure the array is filled when using read() to fill an array](https://wiki.sei.cmu.edu/confluence/display/java/FIO10-J.+Ensure+the+array+is+filled+when+using+read%28%29+to+fill+an+array)
* [FIO11-J. Do not convert between strings and bytes without specifying a valid character encoding](https://wiki.sei.cmu.edu/confluence/display/java/FIO11-J.+Do+not+convert+between+strings+and+bytes+without+specifying+a+valid+character+encoding)
* [FIO12-J. Provide methods to read and write little-endian data](https://wiki.sei.cmu.edu/confluence/display/java/FIO12-J.+Provide+methods+to+read+and+write+little-endian+data)
* [FIO13-J. Do not log sensitive information outside a trust boundary](https://wiki.sei.cmu.edu/confluence/display/java/FIO13-J.+Do+not+log+sensitive+information+outside+a+trust+boundary)
* [FIO14-J. Perform proper cleanup at program termination](https://wiki.sei.cmu.edu/confluence/display/java/FIO14-J.+Perform+proper+cleanup+at+program+termination)
* [FIO15-J. Do not reset a servlet's output stream after committing it](https://wiki.sei.cmu.edu/confluence/display/java/FIO15-J.+Do+not+reset+a+servlet%27s+output+stream+after+committing+it)
* [FIO16-J. Canonicalize path names before validating them](https://wiki.sei.cmu.edu/confluence/display/java/FIO16-J.+Canonicalize+path+names+before+validating+them)
* [FIO50-J. Do not make assumptions about file creation](https://wiki.sei.cmu.edu/confluence/display/java/FIO50-J.+Do+not+make+assumptions+about+file+creation)
* [FIO51-J. Identify files using multiple file attributes](https://wiki.sei.cmu.edu/confluence/display/java/FIO51-J.+Identify+files+using+multiple+file+attributes)
* [FIO52-J. Do not store unencrypted sensitive information on the client side](https://wiki.sei.cmu.edu/confluence/display/java/FIO52-J.+Do+not+store+unencrypted+sensitive+information+on+the+client+side)
* [FIO53-J. Use the serialization methods writeUnshared() and readUnshared() with care](https://wiki.sei.cmu.edu/confluence/display/java/FIO53-J.+Use+the+serialization+methods+writeUnshared%28%29+and+readUnshared%28%29+with+care)





## [Rule 14. Serialization (SER)](https://wiki.sei.cmu.edu/confluence/pages/viewpage.action?pageId=88487787)

* [SER00-J. Enable serialization compatibility during class evolution](https://wiki.sei.cmu.edu/confluence/display/java/SER00-J.+Enable+serialization+compatibility+during+class+evolution)
* [SER01-J. Do not deviate from the proper signatures of serialization methods](https://wiki.sei.cmu.edu/confluence/display/java/SER01-J.+Do+not+deviate+from+the+proper+signatures+of+serialization+methods)
* [SER02-J. Sign then seal objects before sending them outside a trust boundary](https://wiki.sei.cmu.edu/confluence/display/java/SER02-J.+Sign+then+seal+objects+before+sending+them+outside+a+trust+boundary)
* [SER03-J. Do not serialize unencrypted sensitive data](https://wiki.sei.cmu.edu/confluence/display/java/SER03-J.+Do+not+serialize+unencrypted+sensitive+data)
* [SER04-J. Do not allow serialization and deserialization to bypass the security manager](https://wiki.sei.cmu.edu/confluence/display/java/SER04-J.+Do+not+allow+serialization+and+deserialization+to+bypass+the+security+manager)
* [SER05-J. Do not serialize instances of inner classes](https://wiki.sei.cmu.edu/confluence/display/java/SER05-J.+Do+not+serialize+instances+of+inner+classes)
* [SER06-J. Make defensive copies of private mutable components during deserialization](https://wiki.sei.cmu.edu/confluence/display/java/SER06-J.+Make+defensive+copies+of+private+mutable+components+during+deserialization)
* [SER07-J. Do not use the default serialized form for classes with implementation-defined invariants](https://wiki.sei.cmu.edu/confluence/display/java/SER07-J.+Do+not+use+the+default+serialized+form+for+classes+with+implementation-defined+invariants)
* [SER08-J. Minimize privileges before deserializing from a privileged context](https://wiki.sei.cmu.edu/confluence/display/java/SER08-J.+Minimize+privileges+before+deserializing+from+a+privileged+context)
* [SER09-J. Do not invoke overridable methods from the readObject() method](https://wiki.sei.cmu.edu/confluence/display/java/SER09-J.+Do+not+invoke+overridable+methods+from+the+readObject%28%29+method)
* [SER10-J. Avoid memory and resource leaks during serialization](https://wiki.sei.cmu.edu/confluence/display/java/SER10-J.+Avoid+memory+and+resource+leaks+during+serialization)
* [SER11-J. Prevent overwriting of externalizable objects](https://wiki.sei.cmu.edu/confluence/display/java/SER11-J.+Prevent+overwriting+of+externalizable+objects)
* [SER12-J. Prevent deserialization of untrusted data](https://wiki.sei.cmu.edu/confluence/display/java/SER12-J.+Prevent+deserialization+of+untrusted+data)
* [SER13-J. Deserialization methods should not perform potentially dangerous operations](https://wiki.sei.cmu.edu/confluence/display/java/SER13-J.+Deserialization+methods+should+not+perform+potentially+dangerous+operations)




## [Rule 15. Platform Security (SEC)](https://wiki.sei.cmu.edu/confluence/pages/viewpage.action?pageId=88487683)

* [SEC00-J. Do not allow privileged blocks to leak sensitive information across a trust boundary](https://wiki.sei.cmu.edu/confluence/display/java/SEC00-J.+Do+not+allow+privileged+blocks+to+leak+sensitive+information+across+a+trust+boundary)
* [SEC01-J. Do not allow tainted variables in privileged blocks](https://wiki.sei.cmu.edu/confluence/display/java/SEC01-J.+Do+not+allow+tainted+variables+in+privileged+blocks)
* [SEC02-J. Do not base security checks on untrusted sources](https://wiki.sei.cmu.edu/confluence/display/java/SEC02-J.+Do+not+base+security+checks+on+untrusted+sources)
* [SEC03-J. Do not load trusted classes after allowing untrusted code to load arbitrary classes](https://wiki.sei.cmu.edu/confluence/display/java/SEC03-J.+Do+not+load+trusted+classes+after+allowing+untrusted+code+to+load+arbitrary+classes)
* [SEC04-J. Protect sensitive operations with security manager checks](https://wiki.sei.cmu.edu/confluence/display/java/SEC04-J.+Protect+sensitive+operations+with+security+manager+checks)
* [SEC05-J. Do not use reflection to increase accessibility of classes, methods, or fields](https://wiki.sei.cmu.edu/confluence/display/java/SEC05-J.+Do+not+use+reflection+to+increase+accessibility+of+classes%2C+methods%2C+or+fields)
* [SEC06-J. Do not rely on the default automatic signature verification provided by URLClassLoader and java.util.jar](https://wiki.sei.cmu.edu/confluence/display/java/SEC06-J.+Do+not+rely+on+the+default+automatic+signature+verification+provided+by+URLClassLoader+and+java.util.jar)
* [SEC07-J. Call the superclass's getPermissions() method when writing a custom class loader](https://wiki.sei.cmu.edu/confluence/display/java/SEC07-J.+Call+the+superclass%27s+getPermissions%28%29+method+when+writing+a+custom+class+loader)
* [SEC08-J Trusted code must discard or clean any arguments provided by untrusted code](https://wiki.sei.cmu.edu/confluence/display/java/SEC08-J+Trusted+code+must+discard+or+clean+any+arguments+provided+by+untrusted+code)
* [SEC09-J Never leak the results of certain standard API methods from trusted code to untrusted code](https://wiki.sei.cmu.edu/confluence/display/java/SEC09-J+Never+leak+the+results+of+certain+standard+API+methods+from+trusted+code+to+untrusted+code)
* [SEC10-J Never permit untrusted code to invoke any API that may (possibly transitively) invoke the reflection APIs](https://wiki.sei.cmu.edu/confluence/display/java/SEC10-J+Never+permit+untrusted+code+to+invoke+any+API+that+may+%28possibly+transitively%29+invoke+the+reflection+APIs)
* [SEC50-J. Avoid granting excess privileges](https://wiki.sei.cmu.edu/confluence/display/java/SEC50-J.+Avoid+granting+excess+privileges)
* [SEC51-J. Minimize privileged code](https://wiki.sei.cmu.edu/confluence/display/java/SEC51-J.+Minimize+privileged+code)
* [SEC52-J. Do not expose methods that use reduced-security checks to untrusted code](https://wiki.sei.cmu.edu/confluence/display/java/SEC52-J.+Do+not+expose+methods+that+use+reduced-security+checks+to+untrusted+code)
* [SEC53-J. Define custom security permissions for fine-grained security](https://wiki.sei.cmu.edu/confluence/display/java/SEC53-J.+Define+custom+security+permissions+for+fine-grained+security)
* [SEC54-J. Create a secure sandbox using a security manager](https://wiki.sei.cmu.edu/confluence/display/java/SEC54-J.+Create+a+secure+sandbox+using+a+security+manager)
* [SEC55-J. Ensure that security-sensitive methods are called with validated arguments](https://wiki.sei.cmu.edu/confluence/display/java/SEC55-J.+Ensure+that+security-sensitive+methods+are+called+with+validated+arguments)
* [SEC56-J. Do not serialize direct handles to system resources](https://wiki.sei.cmu.edu/confluence/display/java/SEC56-J.+Do+not+serialize+direct+handles+to+system+resources)
* [SEC57-J. Do not let untrusted code misuse privileges of callback methods](https://wiki.sei.cmu.edu/confluence/display/java/SEC57-J.+Do+not+let+untrusted+code+misuse+privileges+of+callback+methods)
* [SEC58-J. Deserialization methods should not perform potentially dangerous operations](https://wiki.sei.cmu.edu/confluence/display/java/SEC58-J.+Deserialization+methods+should+not+perform+potentially+dangerous+operations)



## [Rule 16. Runtime Environment (ENV)](https://wiki.sei.cmu.edu/confluence/pages/viewpage.action?pageId=88487764)

* [ENV00-J. Do not sign code that performs only unprivileged operations](https://wiki.sei.cmu.edu/confluence/display/java/ENV00-J.+Do+not+sign+code+that+performs+only+unprivileged+operations)
* [ENV01-J. Place all security-sensitive code in a single JAR and sign and seal it](https://wiki.sei.cmu.edu/confluence/display/java/ENV01-J.+Place+all+security-sensitive+code+in+a+single+JAR+and+sign+and+seal+it)
* [ENV02-J. Do not trust the values of environment variables](https://wiki.sei.cmu.edu/confluence/display/java/ENV02-J.+Do+not+trust+the+values+of+environment+variables)
* [ENV03-J. Do not grant dangerous combinations of permissions](https://wiki.sei.cmu.edu/confluence/display/java/ENV03-J.+Do+not+grant+dangerous+combinations+of+permissions)
* [ENV04-J. Do not disable bytecode verification](https://wiki.sei.cmu.edu/confluence/display/java/ENV04-J.+Do+not+disable+bytecode+verification)
* [ENV05-J. Do not deploy an application that can be remotely monitored](https://wiki.sei.cmu.edu/confluence/display/java/ENV05-J.+Do+not+deploy+an+application+that+can+be+remotely+monitored)
* [ENV06-J. Production code must not contain debugging entry points](https://wiki.sei.cmu.edu/confluence/display/java/ENV06-J.+Production+code+must+not+contain+debugging+entry+points)





## [Rule 17. Java Native Interface (JNI)](https://wiki.sei.cmu.edu/confluence/pages/viewpage.action?pageId=88487346)

* [JNI00-J. Define wrappers around native methods](https://wiki.sei.cmu.edu/confluence/display/java/JNI00-J.+Define+wrappers+around+native+methods)
* [JNI01-J. Safely invoke standard APIs that perform tasks using the immediate caller's class loader instance (loadLibrary)](https://wiki.sei.cmu.edu/confluence/pages/viewpage.action?pageId=88487334)
* [JNI02-J. Do not assume object references are constant or unique](https://wiki.sei.cmu.edu/confluence/display/java/JNI02-J.+Do+not+assume+object+references+are+constant+or+unique)
* [JNI03-J. Do not use direct pointers to Java objects in JNI code](https://wiki.sei.cmu.edu/confluence/display/java/JNI03-J.+Do+not+use+direct+pointers+to+Java+objects+in+JNI+code)
* [JNI04-J. Do not assume that Java strings are null-terminated](https://wiki.sei.cmu.edu/confluence/display/java/JNI04-J.+Do+not+assume+that+Java+strings+are+null-terminated)




## [Rec. 18. Concurrency (CON)](https://wiki.sei.cmu.edu/confluence/pages/viewpage.action?pageId=88487352)

* [CON50-J. Do not assume that declaring a reference volatile guarantees safe publication of the members of the referenced object](https://wiki.sei.cmu.edu/confluence/display/java/CON50-J.+Do+not+assume+that+declaring+a+reference+volatile+guarantees+safe+publication+of+the+members+of+the+referenced+object)
* [CON51-J. Do not assume that the sleep(), yield(), or getState() methods provide synchronization semantics](https://wiki.sei.cmu.edu/confluence/display/java/CON51-J.+Do+not+assume+that+the+sleep%28%29%2C+yield%28%29%2C+or+getState%28%29+methods+provide+synchronization+semantics)
* [CON52-J. Document thread-safety and use annotations where applicable](https://wiki.sei.cmu.edu/confluence/display/java/CON52-J.+Document+thread-safety+and+use+annotations+where+applicable)



## [Rule 49. Miscellaneous (MSC)](https://wiki.sei.cmu.edu/confluence/pages/viewpage.action?pageId=88487686)

* [MSC00-J. Use SSLSocket rather than Socket for secure data exchange](https://wiki.sei.cmu.edu/confluence/display/java/MSC00-J.+Use+SSLSocket+rather+than+Socket+for+secure+data+exchange)
* [MSC01-J. Do not use an empty infinite loop](https://wiki.sei.cmu.edu/confluence/display/java/MSC01-J.+Do+not+use+an+empty+infinite+loop)
* [MSC02-J. Generate strong random numbers](https://wiki.sei.cmu.edu/confluence/display/java/MSC02-J.+Generate+strong+random+numbers)
* [MSC03-J. Never hard code sensitive information](https://wiki.sei.cmu.edu/confluence/display/java/MSC03-J.+Never+hard+code+sensitive+information)
* [MSC04-J. Do not leak memory](https://wiki.sei.cmu.edu/confluence/display/java/MSC04-J.+Do+not+leak+memory)
* [MSC05-J. Do not exhaust heap space](https://wiki.sei.cmu.edu/confluence/display/java/MSC05-J.+Do+not+exhaust+heap+space)
* [MSC06-J. Do not modify the underlying collection when an iteration is in progress](https://wiki.sei.cmu.edu/confluence/display/java/MSC06-J.+Do+not+modify+the+underlying+collection+when+an+iteration+is+in+progress)
* [MSC07-J. Prevent multiple instantiations of singleton objects](https://wiki.sei.cmu.edu/confluence/display/java/MSC07-J.+Prevent+multiple+instantiations+of+singleton+objects)
* [MSC08-J. Do not store nonserializable objects as attributes in an HTTP session](https://wiki.sei.cmu.edu/confluence/display/java/MSC08-J.+Do+not+store+nonserializable+objects+as+attributes+in+an+HTTP+session)
* [MSC09-J. For OAuth, ensure (a) [relying party receiving user's ID in last step] is same as (b) [relying party the access token was granted to].](https://wiki.sei.cmu.edu/confluence/pages/viewpage.action?pageId=88487366)
* [MSC10-J. Do not use OAuth 2.0 implicit grant (unmodified) for authentication](https://wiki.sei.cmu.edu/confluence/display/java/MSC10-J.+Do+not+use+OAuth+2.0+implicit+grant+%28unmodified%29+for+authentication)
* [MSC11-J. Do not let session information leak within a servlet](https://wiki.sei.cmu.edu/confluence/display/java/MSC11-J.+Do+not+let+session+information+leak+within+a+servlet)
* Page:[MSC50-J. Minimize the scope of the @SuppressWarnings annotation](https://wiki.sei.cmu.edu/confluence/display/java/MSC50-J.+Minimize+the+scope+of+the+@SuppressWarnings+annotation)
* [MSC51-J. Do not place a semicolon immediately following an if, for, or while condition](https://wiki.sei.cmu.edu/confluence/display/java/MSC51-J.+Do+not+place+a+semicolon+immediately+following+an+if%2C+for%2C+or+while+condition)
* [MSC52-J. Finish every set of statements associated with a case label with a break statement](https://wiki.sei.cmu.edu/confluence/display/java/MSC52-J.+Finish+every+set+of+statements+associated+with+a+case+label+with+a+break+statement)
* [MSC53-J. Carefully design interfaces before releasing them](https://wiki.sei.cmu.edu/confluence/display/java/MSC53-J.+Carefully+design+interfaces+before+releasing+them)
* [MSC54-J. Avoid inadvertent wrapping of loop counters](https://wiki.sei.cmu.edu/confluence/display/java/MSC54-J.+Avoid+inadvertent+wrapping+of+loop+counters)
* [MSC55-J. Use comments consistently and in a readable fashion](https://wiki.sei.cmu.edu/confluence/display/java/MSC55-J.+Use+comments+consistently+and+in+a+readable+fashion)
* [MSC56-J. Detect and remove superfluous code and values](https://wiki.sei.cmu.edu/confluence/display/java/MSC56-J.+Detect+and+remove+superfluous+code+and+values)
* [MSC57-J. Strive for logical completeness](https://wiki.sei.cmu.edu/confluence/display/java/MSC57-J.+Strive+for+logical+completeness)
* [MSC58-J. Prefer using iterators over enumerations](https://wiki.sei.cmu.edu/confluence/display/java/MSC58-J.+Prefer+using+iterators+over+enumerations)
* [MSC59-J. Limit the lifetime of sensitive data](https://wiki.sei.cmu.edu/confluence/display/java/MSC59-J.+Limit+the+lifetime+of+sensitive+data)
* [MSC60-J. Do not use assertions to verify the absence of runtime errors](https://wiki.sei.cmu.edu/confluence/display/java/MSC60-J.+Do+not+use+assertions+to+verify+the+absence+of+runtime+errors)
* [MSC61-J. Do not use insecure or weak cryptographic algorithms](https://wiki.sei.cmu.edu/confluence/display/java/MSC61-J.+Do+not+use+insecure+or+weak+cryptographic+algorithms)
* [MSC62-J. Store passwords using a hash function](https://wiki.sei.cmu.edu/confluence/display/java/MSC62-J.+Store+passwords+using+a+hash+function)
* [MSC63-J. Ensure that SecureRandom is properly seeded](https://wiki.sei.cmu.edu/confluence/display/java/MSC63-J.+Ensure+that+SecureRandom+is+properly+seeded)

