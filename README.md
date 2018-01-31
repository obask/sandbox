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

