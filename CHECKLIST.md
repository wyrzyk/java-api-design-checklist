# Java API Design Checklist

There are many different rules and tradeoffs to consider during Java API design. Like any complex task,
it tests the limits of our attention and memory. Similar to the pilots’ pre-flight checklist,
this list helps software designers remember obvious and not so obvious rules while designing Java APIs.

It is a complement to and intended to be used together with the [API Design Guidelines](https://theamiableapi.com/2011/11/21/api-design-webinar-slides-and-recording/).

We also have some before-and-after [code examples](https://theamiableapi.com/2012/01/21/how-to-use-the-java-api-design-checklist/ "How to use the Java API design checklist")
to show how this list can help you remember overlooked design requirements, spot mistakes,
identify less-than-optimal design choices and opportunities for improvements.

Click the [_explain_] link next to a checklist item (where available) for details about the rationale, examples, design tradeoffs or other limitations of applicability.

```
This list uses the following conventions:

   (Do) **verb...**  - Indicates the required design
    **Favor...**     - Indicates the best of several design alternatives
    **Consider...**  - Indicates a possible design improvement
    **Avoid...**     - Indicates a design weakness
    **Do not... **   - Indicates a design mistake
```

## 1. Package Design Checklist

### 1.1. General

*   1.1.1. **Favor** placing API and implementation into separate packages [explain](#111-favor-placing-api-and-implementation-into-separate-packages)
*   1.1.2. **Favor** placing APIs into high-level packages and implementation into lower-level packages [explain](#112-favor-placing-apis-into-high-level-packages-and-implementation-into-lower-level-packages)
*   1.1.3. **Consider** breaking up large APIs into several packages [explain](#113-consider-breaking-up-large-apis-into-several-packages)
*   1.1.4. **Consider** putting API and implementation packages into separate Java archives [explain](#114-consider-putting-api-and-implementation-packages-into-separate-java-archives)
*   1.1.5. **Avoid** (minimize) internal dependencies on implementation classes in APIs [explain](#115-avoid-minimize-internal-dependencies-on-implementation-classes-in-apis)
*   1.1.6. **Avoid** unnecessary API fragmentation [explain](#116-avoid-unnecessary-api-fragmentation)
*   1.1.7. **Do not** place public implementation classes in the API package [explain](#117-do-not-place-public-implementation-classes-in-the-api-package)
*   1.1.8. **Do not** create dependencies between callers and implementation classes [explain](#118-do-not-create-dependencies-between-callers-and-implementation-classes)
*   1.1.9. **Do not** place unrelated APIs into the same package [explain](#119-do-not-place-unrelated-apis-into-the-same-package)
*   1.1.10. **Avoid** relying on documentation to control evolution of API or SPI [explain](#1110-avoid-relying-on-documentation-to-control-evolution-of-api-or-spi)
*   1.1.11. **Do not** move or rename the package of an already released public API [explain](#1111-do-not-move-or-rename-the-package-of-an-already-released-public-api)

### 1.2. Naming

*   1.2.1. **Start** package names with the company’s official root namespace [explain](#121-start-package-names-with-the-companys-official-root-namespace)
*   1.2.2. **Use** a stable product or product family name at the second level of the package name [explain](#122-use-a-stable-product-or-product-family-name-at-the-second-level-of-the-package-name)
*   1.2.3. **Use** the name of the API as the final part of the package name [explain](#123-use-the-name-of-the-api-as-the-final-part-of-the-package-name)
*   1.2.4. **Consider** marking implementation-only packages by including “internal” in the package name [explain](#124-consider-marking-implementation-only-packages-by-including-internal-in-the-package-name)
*   1.2.5. **Avoid** composite names [explain](#125-avoid-composite-names)
*   1.2.6. **Avoid** using the same name for both package and class inside the package [explain](#126-avoid-using-the-same-name-for-both-package-and-class-inside-the-package)
*   1.2.7. **Avoid** using “api” in package names [explain](#127-avoid-using-api-in-package-names)
*   1.2.8. **Do not** use marketing, project, organizational unit or geographic location names [explain](#128-do-not-use-marketing-project-organizational-unit-or-geographic-location-names)
*   1.2.9. **Do not** use uppercase characters in package names [explain](#129-do-not-use-uppercase-characters-in-package-names)

### 1.3. Documentation

*   1.3.1. **Provide** a package overview (package.html) for each package [explain](#131-provide-a-package-overview-packagehtml-for-each-package)
*   1.3.2. **Follow** standard Javadoc conventions [explain](#132-follow-standard-javadoc-conventions)
*   1.3.3. **Begin** with a short, one sentence summary of the API [explain](#133-begin-with-a-short-one-sentence-summary-of-the-api)
*   1.3.4. **Provide** enough details to help deciding if and how to use the API [explain](#134-provide-enough-details-to-help-deciding-if-and-how-to-use-the-api)
*   1.3.5. **Indicate** the entry points (main classes or methods) of the API [explain](#135-indicate-the-entry-points-main-classes-or-methods-of-the-api)
*   1.3.6. **Include** sample code for the main, most fundamental use case [explain](#136-include-sample-code-for-the-main-most-fundamental-use-case)
*   1.3.7. **Include** a link to the Developer Guide [explain](#137-include-a-link-to-the-developer-guide)
*   1.3.8. **Include** a link to the Cookbook [explain](#138-include-a-link-to-the-cookbook)
*   1.3.9. **Indicate** related API_s_
*   1.3.10. **Include** the API version number [explain](#1310-include-the-api-version-number)
*   1.3.11. **Indicate** deprecated API versions with the @deprecated tag
*   1.3.12. **Consider** including a copyright notice [explain](#1312-consider-including-a-copyright-notice)
*   1.3.13. **Avoid** lengthy package overview_s_
*   1.3.14. **Do not** include implementation packages into published Javadoc

## 2. Type Design Checklist

### 2.1. General

*   2.1.1. **Ensure** each type has a single, well-defined purpose
*   2.1.2. **Ensure** types represent domain concepts, not design abstractions
*   2.1.3. **Limit** the number of types [explain](#213-limit-the-number-of-types)
*   2.1.4. **Limit** the size of types
*   2.1.5. **Follow** consistent design patterns when designing related types
*   2.1.6. **Favor** multiple (private) implementations over multiple public types
*   2.1.7. **Favor** interfaces over class inheritance for expressing simple commonality in behavior [explain](#217-favor-interfaces-over-class-inheritance-for-expressing-simple-commonality-in-behavior)
*   2.1.8. **Favor** abstract classes over interfaces for decoupling API from implementation [explain](#218-favor-abstract-classes-over-interfaces-for-decoupling-api-from-implementation)
*   2.1.9. **Favor** enumeration types over constants
*   2.1.10. **Consider** generic types [explain](#2110-consider-generic-types)
*   2.1.11. **Consider** placing constraints on the generic type parameter
*   2.1.12. **Consider** using interfaces to achieve similar effect to multiple inheritance
*   2.1.13. **Avoid** designing for client extension
*   2.1.14. **Avoid** deep inheritance hierarchies
*   2.1.15. **Do not** use public nested types
*   2.1.16. **Do not** declare public or protected fields
*   2.1.17. **Do not** expose implementation inheritance to the client

### 2.2. Naming

*   2.2.1. **Use** a noun or a noun phrase
*   2.2.2. **Use** PascalCasing
*   2.2.3. **Capitalize** only the first letter of acronyms [explain](#223-capitalize-only-the-first-letter-of-acronyms)
*   2.2.4. **Use** accurate names for purpose of the type [explain](#224-use-accurate-names-for-purpose-of-the-type)
*   2.2.5. **Reserve** the shortest, most memorable name for the most frequently used type
*   2.2.6. **End** the name of all exceptions with the word “Exception” [explain](#226-end-the-name-of-all-exceptions-with-the-word-exception)
*   2.2.7. **Use** singular nouns (Color, not Colors) for naming enumerated types [explain](#227-use-singular-nouns-color-not-colors-for-naming-enumerated-types)
*   2.2.8. **Consider** longer names [explain](#228-consider-longer-names)
*   2.2.9. **Consider** ending the name of derived class with the name of the base class
*   2.2.10. **Consider** starting the name of an abstract class with the word “Abstract” [explain](#2210-consider-starting-the-name-of-an-abstract-class-with-the-word-abstract)
*   2.2.11. **Avoid** abbreviations
*   2.2.12. **Avoid** generic nouns
*   2.2.13. **Avoid** synonyms
*   2.2.14. **Avoid** type names used in related APIs
*   2.2.15. **Do not** use names which differ in case alone
*   2.2.16. **Do not** use prefixes
*   2.2.17. **Do not** prefix interface names with “I”
*   2.2.18. **Do not** use types names used in Java core packages [explain](#2218-do-not-use-types-names-used-in-java-core-packages)

### 2.3. Classes

*   2.3.1. **Minimize** implementation dependencies
*   2.3.2. **List** public methods first [explain](#232-list-public-methods-first)
*   2.3.3. **Declare** implementation methods private
*   2.3.4. **Define** at least one public concrete class which extends a public abstract class _[explain](#234-define-at-least-one-public-concrete-class-which-extends-a-public-abstract-class)_
*   2.3.5. **Provide** adequate defaults for the basic usage scenarios
*   2.3.6. **Design** classes with strong invariants
*   2.3.7. **Group** stateless, accessor and mutator methods together
*   2.3.8. **Keep** the number of mutator methods at a minimum
*   2.3.9. **Consider** providing a default no-parameter constructor [explain](#239-consider-providing-a-default-no-parameter-constructor)
*   2.3.10. **Consider** overriding equals(), hashCode() and toString() [explain](#2310-consider-overriding-equals-hashcode-and-tostring)
*   2.3.11. **Consider** implementing Comparable [explain](#2311-consider-implementing-comparable)
*   2.3.12. **Consider** implementing Serializable [explain](#2312-consider-implementing-serializable)
*   2.3.13. **Consider** making classes re-entrant
*   2.3.14. **Consider** declaring the class as final [explain](#2314-consider-declaring-the-class-as-final)
*   2.3.15. **Consider** preventing class instantiation by not providing a public constructor [explain](#2315-consider-preventing-class-instantiation-by-not-providing-a-public-constructor)
*   2.3.16. **Consider** using custom types to enforce strong preconditions as class invariants
*   2.3.17. **Consider** designing immutable classes [explain](#2317-consider-designing-immutable-classes)
*   2.3.18. **Avoid** static classes
*   2.3.19. **Avoid** using Cloneable
*   2.3.20. **Do not** add instance members to static classes
*   2.3.21. **Do not** define public constructors for public abstract classes clients should not extend [explain](#2321-do-not-define-public-constructors-for-public-abstract-classes-clients-should-not-extend)
*   2.3.22. **Do not** require extensive initialization

### 2.4. Interfaces

*   2.4.1. **Provide** at least one implementing class for every public interface
*   2.4.2. **Provide** at least one consuming method for every public interface
*   2.4.3. **Do not** add methods to a released public Java interface
*   2.4.4. **Do not** use marker interfaces
*   2.4.5. **Do not** use public interfaces as a container for constant fields

### 2.5. Enumerations

*   2.5.1. **Consider** specifying a zero-value (“None” or “Unspecified”, etc) for enumeration types
*   2.5.2. **Avoid** enumeration types with only one value
*   2.5.3. **Do not** use enumeration types for open-ended sets of values
*   2.5.4. **Do not** reserve enumeration values for future use
*   2.5.5. **Do not** add new values to a released enumeration

### 2.6. Exceptions

*   2.6.1. **Ensure** that custom exceptions are serialized correctly
*   2.6.2. **Consider** defining a different exception class for each error type
*   2.6.3. **Consider** providing extra information for programmatic access
*   2.6.4. **Avoid** deep exception hierarchies
*   2.6.5. **Do not** derive custom exceptions from other than Exception and RuntimeException
*   2.6.6. **Do not** derive custom exceptions directly from Throwable
*   2.6.7. **Do not** include sensitive information in error messages

### 2.7. Documentation

*   2.7.1. **Provide** type overview for each type
*   2.7.2. **Follow** standard Javadoc conventions
*   2.7.3. **Begin** with a short, one sentence summary of the type
*   2.7.4. **Provide** enough details to help deciding if and how to use the type
*   2.7.5. **Explain** how to instantiate the type
*   2.7.6. **Provide** code sample to illustrate the main use case for the type
*   2.7.7. **Include** links to relevant sections in the Developer Guide
*   2.7.8. **Include** links to relevant sections in the Cookbook
*   2.7.9. **Indicate** related types
*   2.7.10. **Indicate** deprecated types using the @deprecated tag
*   2.7.11. **Document** class invariants
*   2.7.12. **Avoid** lengthy type overviews
*   2.7.13. **Do not** generate Javadoc for private fields and methods

## 3. Method Design Checklist

### 3.1. General

*   3.1.1. **Make** sure each method does only one thing
*   3.1.2. **Ensure** related methods are at the same level of granularity
*   3.1.3. **Ensure** no boilerplate code is needed to combine method calls
*   3.1.4. **Make** all method calls atomic
*   3.1.5. **Design** protected methods with the same care as public methods
*   3.1.6. **Limit** the number of mutator methods
*   3.1.7. **Design** mutators with strong invariants
*   3.1.8. **Favor** generic methods over a set of overloaded methods
*   3.1.9. **Consider** generic methods
*   3.1.10. **Consider** method pairs, where the effect of one is reversed by the other
*   3.1.11. **Avoid** “helper” methods
*   3.1.12. **Avoid** long-running methods
*   3.1.13. **Avoid** forcing callers to write loops for basic scenarios
*   3.1.14. **Avoid** “option” parameters to modify behavior
*   3.1.15. **Avoid** non-reentrant methods
*   3.1.16. **Do not** remove a released method
*   3.1.17. **Do not** deprecate a released method without providing a replacement
*   3.1.18. **Do not** change the signature of a released method
*   3.1.19. **Do not** change the observable behavior of a released method
*   3.1.20. **Do not** strengthen the precondition of an already released API method
*   3.1.21. **Do not** weaken the postcondition of an already released API method
*   3.1.22. **Do not** add new methods to released interfaces
*   3.1.23. **Do not** add a new overload to a released API

### 3.2. Naming

*   3.2.1. **Begin** names with powerful, expressive verbs
*   3.2.2. **Use** camelCasing
*   3.2.3. **Reserve** “get”, “set” and “is” for JavaBeans methods accessing local fields
*   3.2.4. **Use** words familiar to callers
*   3.2.5. **Stay** close to spoken English
*   3.2.6. **Avoid** abbreviations
*   3.2.7. **Avoid** generic verbs
*   3.2.8. **Avoid** synonyms
*   3.2.9. **Do not** use underscores
*   3.2.10. **Do not** rely on parameter names or types to clarify the meaning of the method

### 3.3. Parameters

*   3.3.1. **Choose** the most precise type for parameters
*   3.3.2. **Keep** the meaning of the null parameter value consistent across related method calls
*   3.3.3. **Use** consistent parameter names, types and ordering in related methods
*   3.3.4. **Place** output parameters after the input parameters in the parameter list
*   3.3.5. **Provide** overloaded methods with shorter parameter lists for frequently used default parameter values
*   3.3.6. **Use** overloaded methods for operations with the same semantics on unrelated types
*   3.3.7. **Favor** interfaces over concrete classes as parameters
*   3.3.8. **Favor** collections over arrays as parameters and return values
*   3.3.9. **Favor** generic collections over raw (untyped) collections
*   3.3.10. **Favor** enumeration types over Boolean or integer types
*   3.3.11. **Favor** putting single object parameters ahead of collection or array parameters
*   3.3.12. **Favor** putting custom type parameters ahead of standard Java type parameters
*   3.3.13. **Favor** putting object parameters ahead of value parameters
*   3.3.14. **Favor** interfaces over concrete classes as return types
*   3.3.15. **Favor** empty collections to null return values
*   3.3.16. **Favor** returning values which are valid input for related methods
*   3.3.17. **Consider** making defensive copies of mutable parameters
*   3.3.18. **Consider** storing weak object references internally
*   3.3.19. **Avoid** variable length parameter lists
*   3.3.20. **Avoid** long parameter lists (more than 3)
*   3.3.21. **Avoid** putting parameters of the same type next to each other
*   3.3.22. **Avoid** out or in-out method parameters
*   3.3.23. **Avoid** method overloading
*   3.3.24. **Avoid** parameter types exposing implementation details
*   3.3.25. **Avoid** Boolean parameters
*   3.3.26. **Avoid** returning null
*   3.3.27. **Avoid** return types defined in unrelated APIs, except core Java APIs
*   3.3.28. **Avoid** returning references to mutable internal objects
*   3.3.29. **Do not** use integer parameters for passing predefined constant values
*   3.3.30. **Do not** reserve parameters for future use
*   3.3.31. **Do not** change the parameter naming or ordering in overloaded methods

### 3.4. Error handling

*   3.4.1. **Throw** exception only for exceptional circumstances
*   3.4.2. **Throw** checked exceptions only for recoverable errors
*   3.4.3. **Throw** runtime exceptions to signal API usage mistakes
*   3.4.4. **Throw** exceptions at the appropriate level of abstraction
*   3.4.5. **Perform** runtime precondition checks
*   3.4.6. **Throw** NullPointerException to indicate a prohibited null parameter value
*   3.4.7. **Throw** IllegalArgumentException to indicate an incorrect parameter value other than null
*   3.4.8. **Throw** IllegalStateException to indicate a method call made in the wrong context
*   3.4.9. **Indicate** in the error message which parameter violated which precondition
*   3.4.10. **Ensure** failed method calls have no side effects
*   3.4.11. **Provide** runtime checks for prohibited API calls made inside callback methods
*   3.4.12. **Favor** standard Java exceptions over custom exceptions
*   3.4.13. **Favor** query methods over exceptions for predictable error conditions

### 3.5. Overriding

*   3.5.1. **Use** the @Override annotation
*   3.5.2. **Preserve** or weaken preconditions
*   3.5.3. **Preserve** or strengthen postconditions
*   3.5.4. **Preserve** or strengthen the invariant
*   3.5.5. **Do not** throw new types of runtime exceptions
*   3.5.6. **Do not** change the type (stateless, accessor or mutator) of the method

### 3.6. Constructors

*   3.6.1. **Minimize** the work done in constructors
*   3.6.2. **Set** the value of all properties to reasonable defaults
*   3.6.3. **Use** constructor parameters only as a shortcut for setting properties
*   3.6.4. **Validate** constructor parameters
*   3.6.5. **Name** constructor parameters the same as corresponding properties
*   3.6.6. **Follow** the guidelines for method overloading when providing multiple constructors
*   3.6.7. **Favor** constructors over static factory methods
*   3.6.8. **Consider** a no parameter default constructor
*   3.6.9. **Consider** a static factory method if you don’t always need a new instance
*   3.6.10. **Consider** a static factory method if you need to decide the precise type of object at runtime
*   3.6.11. **Consider** a static factory method if you need to access external resources
*   3.6.12. **Consider** a builder when faced with many constructor parameters
*   3.6.13. **Consider** private constructors to prevent direct class instantiation
*   3.6.14. **Avoid** creating unnecessary objects
*   3.6.15. **Avoid** finalizers
*   3.6.16. **Do not** throw exceptions from default (no-parameter) constructors
*   3.6.17. **Do not** add a constructor with parameters to a class released without explicit constructors

### 3.7. Setters and getters

*   3.7.1. **Start** the name of methods returning non-Boolean properties with “get”
*   3.7.2. **Start** the name of methods returning Boolean properties with “is”, “can” or similar
*   3.7.3. **Start** the name of methods updating local properties with “set”
*   3.7.4. **Validate** the parameter of setter methods
*   3.7.5. **Minimize** work done in getters and setters
*   3.7.6. **Consider** returning immutable collections from a getter
*   3.7.7. **Consider** implementing a collection interface instead of a public propertie of a collection type
*   3.7.8. **Consider** read-only properties
*   3.7.9. **Consider** making a defensive copy when setting properties of mutable types
*   3.7.10. **Consider** making a defensive copy when returning properties of mutable type
*   3.7.11. **Avoid** returning arrays from getters
*   3.7.12. **Avoid** validations which cannot be done with local knowledge
*   3.7.13. **Do not** throw exceptions from a getter
*   3.7.14. **Do not** design set-only properties (with public setter no public getter)
*   3.7.15. **Do not** rely on the order properties are set

### 3.8. Callbacks

*   3.8.1. **Design** with the strongest possible precondition
*   3.8.2. **Design** with the weakest possible postcondition
*   3.8.3. **Consider** passing a reference to the object initiating the callback as the first parameter of the callback method
*   3.8.4. **Avoid** callbacks with return values

### 3.9. Documentation

*   3.9.1. **Provide** Javadoc comments for each method
*   3.9.2. **Follow** standard Javadoc conventions
*   3.9.3. **Begin** with a short, one sentence summary of the method
*   3.9.4. **Indicate** related methods
*   3.9.5. **Indicate** deprecated methods using the @deprecated tag
*   3.9.6. **Indicate** a replacement for any deprecated methods
*   3.9.7. **Avoid** lengthy comments
*   3.9.8. **Document** common behavioral patterns
*   3.9.9. **Document** the precise meaning of a null parameter value (if permitted)
*   3.9.10. **Document** the type of the method (stateless, accessor or mutator)
*   3.9.11. **Document** method preconditions
*   3.9.12. **Document** the performance characteristics of the algorithm implemented
*   3.9.13. **Document** remote method calls
*   3.9.14. **Document** methods accessing out-of-process resources
*   3.9.15. **Document** which API calls are permitted inside callback methods
*   3.9.16. **Consider** unit tests for illustrating the behavior of the method


## 1. Package Design Checklist

### 1.1.1. **Favor** placing API and implementation into separate packages

**Rationale:**

[Simplicity](https://theamiableapi.com/2011/09/07/api-design-best-practice-keep-it-simple/), [Consistency](https://theamiableapi.com/2011/09/14/api-design-best-practice-consistency/), [Sefety](https://theamiableapi.com/2011/10/12/api-design-best-practice-make-it-safe/) and [Evolution](https://theamiableapi.com/2011/10/18/api-design-best-practice-plan-for-evolution/). Java only supports public and package scoped classes. You should obviously never mix public implementation classes with APIs (see [checklist item](#117-do-not-place-public-implementation-classes-in-the-api-package)). You can only place package scoped classes into API packages if you are certain they will be never needed in any other (implementation) package. Otherwise developers may inadvertently change their access to public, breaking the encapsulation of the API.

More importantly, Java module systems like OSGi use package boundaries for additional class loader isolation, dependency management and versioning. You won’t be able to take advantage of it if you combine API and implementation into one package.

**Do this:**

```
package com.company.product;

public class ApiClass {
   private ImplementationClass m;
}

package com.company.product.internal;

public class ImplementationClass {...}
```

**Don’t do this:**

```
package com.company.product;

public class ApiClass {

   private ImplementationClass m;
}

class ImplementationClass {...} //package scoped
```

**Exceptions:**

Very rarely, a small number of package scoped classes are useful when a separate implementation package adds no benefits, only complexity.

### 1.1.2.    **Favor** placing APIs into high-level packages and implementation into lower-level packages

**Rationale:**

[Simplicity](https://theamiableapi.com/2011/09/07/api-design-best-practice-keep-it-simple/) and [Consistency](https://theamiableapi.com/2011/09/14/api-design-best-practice-consistency/). Java packages are organized into a hierarchy (tree). By convention, packages close to the base of this hierarchy are generic or more frequently used (ex. java.util) while deeper nested packages are more specialized or less frequently used (ex. java.util.concurrent.locks). API packages typically being the only public part of a component, service, or application are expected to be in the root namespace or as close as possible to the root namespace (package) reserved for the said component, service or application. Implementation packages should be at a lower level, preferably under the API package. Developers scanning the package structure for the API should be able to locate it quickly, without the need to scan through uninteresting implementation packages.

**Do this:**

```
package com.company.product.service;

public class ApiClass {...}

package com.company.product.service.internal;

public class ImplementationClass {...}
```

**Don’t do this:**

```
package com.company.product.service.this.that.other.api;

public class ApiClass {...}

package com.company.product.service.this.that;

public class ImplementationClass {...}
```

**Exceptions:**

The package structure of existing applications may be organized using different patterns.
For consistency, follow the convention(s) already in place.

API and implementation may reside in distinct branches of the package tree, especially if multiple implementations are expected or supported.
For example, ActiveMQ in the package org.apache.activemq implements the JMS API defined in the package javax.jms.

If a large API is split into multiple packages, these packages can be placed below a common root namespace.
For example, a streaming API may have two packages: com.company.streaming.audio and com.company.streaming.video.

### 1.1.3.    **Consider** breaking up large APIs into several packages

**Rationale:**

[Simplicity](https://theamiableapi.com/2011/09/07/api-design-best-practice-keep-it-simple/). Packages containing many classes can be difficult to understand and use. If some of the classes are used infrequently or only in specific scenarios, you should move them into their own package. When you do this, you are helping users determine which classes they need for which use cases. They will find the API easier to use if they do not need to understand every single class before they can start working.

**Exceptions:**

Breaking up API packages is only useful if the majority of use cases can be implemented by importing classes from one or two packages. For example, if the API has two packages, most use cases should require classes from one or the other. Remember that importing from two or more packages instead of one makes the use case slightly more complex. If you have strong dependencies between classes, you should leave them in the same package.

You should never break up APIs into packages based on design or implementation considerations, only along usage scenarios. Remember to [consider the perspective of the caller](https://theamiableapi.com/2011/08/29/considering-the-perspective-of-the-caller/).

### 1.1.4.    **Consider** putting API and implementation packages into separate Java archives

**Rationale:**

[Safety](https://theamiableapi.com/2011/10/12/api-design-best-practice-make-it-safe/) and [Evolution](https://theamiableapi.com/2011/10/18/api-design-best-practice-plan-for-evolution/). This separation helps differentiate between build time and run time dependencies. Only the API classes are needed to build the client . Both API and implementation classes are needed to run it. By not requiring Java archives containing public implementation classes on the classpath at build time, you can further reduce (but cannot eliminate) the risk of clients inadvertently accessing implementation classes. This may cause unexpected errors and may break clients when the implementation changes.

**Exceptions:**

This defensive practice can significantly increase the number of Java archives. Not recommended for small modules.

If you use a Java module system like OSGi, you can package API and implementation together and the system ensures that only the API is visible to clients.

### 1.1.5.    **Avoid** (minimize) internal dependencies on implementation classes in APIs

**Rationale:**

[Evolution](https://theamiableapi.com/2011/10/18/api-design-best-practice-plan-for-evolution/). Strong internal dependencies will make it more difficult to change the implementation without making non-backwards compatible changes to either API signature or its behavior. For example, if you tightly couple a simple generic storage API with its implementation using JDBC and RDBMS, it may be prove challenging to re-implement the same API later on top of a file system or an LDAP directory service. Ideally, an API should be a separate layer on top of its implementation, not simply the public part of the implementation.

Imagine an API package which defines just Java interfaces and a few static factory methods to return implementations of these interfaces. All the implementation classes reside in separate packages. With this design, it is trivial to swap implementations (even at runtime) by changing the classes the factory methods return. Although we do not advocate this interface-only design approach to APIs (there are many other design considerations), the number of explicit dependencies between API and implementation packages (coupling) is a good predictor of how difficult it will be to evolve APIs independently from their implementation.

**Exceptions:**

Minimizing implementation dependencies requires more code and the use of appropriate design patterns. You need to weigh these costs against the benefits.

### 1.1.6.    **Avoid** unnecessary API fragmentation

**Rationale:**

[Simplicity](https://theamiableapi.com/2011/09/07/api-design-best-practice-keep-it-simple/). Uses cases which import classes from multiple packages are more complex and harder to understand. Ideally, all API classes needed should come from a single package. Never break up API packages based on implementation needs.

**Do this:**

```
package com.company.product.service;

public class MyClass {...}
public class MyException extends Exception {...}
```

**Don’t do this:**

```
package com.company.product.service.core;

public class MyClass {...}

package com.company.product.service.common.exceptions.

public class MyException extends Exception {...}
```

**Exceptions:**

Large APIs should be breaken up into smaller packages based on usage patterns. See [checklist item](#113-consider-breaking-up-large-apis-into-several-packages).

### 1.1.7.    **Do not** place public implementation classes in the API package

**Rationale:**

[Simplicity](https://theamiableapi.com/2011/09/07/api-design-best-practice-keep-it-simple/), [Consistency](https://theamiableapi.com/2011/09/14/api-design-best-practice-consistency/), [Safety](https://theamiableapi.com/2011/10/12/api-design-best-practice-make-it-safe/) and [Evolution](https://theamiableapi.com/2011/10/18/api-design-best-practice-plan-for-evolution/). Users will have difficulty identifying which classes are part of the API and there is a big risk that they will inadvertently call public methods on implementation classes leading to unexpected errors. The client code may also stop working if implementation changes in later releases.

### 1.1.8.    **Do not** create dependencies between callers and implementation classes

**Rationale:**

[Simplicity](https://theamiableapi.com/2011/09/07/api-design-best-practice-keep-it-simple/), [Safety](https://theamiableapi.com/2011/10/12/api-design-best-practice-make-it-safe/) and [Evolution](https://theamiableapi.com/2011/10/18/api-design-best-practice-plan-for-evolution/). It defeats the purpose of separating API and implementation if callers are either forced or coaxed into importing implementation classes into their code. It is not enough to have implementation classes in separate packages. You cannot use them as the type of public fields, parameters, return values or exceptions either. Even if you put a “Do not use” or “Implementation use only” warning on such fields and methods, callers will still use them. It is much better not to have them at all.

**Don’t do this:**

```
public com.company.product.service;

public class ApiClass {

//IMPLEMENTATION ONLY!! DO NOT USE!!
public ApiClass(ImplementationClass impl) {...}
public void doSomething() throws ImplementationException {...}
}

public com.company.product.service.internal;

public class ImplementationClass {...}
public class ImplementationException extends Exception {...}
```

**Exceptions:**

None. No matter how challenging it feels to avoid implementation types in public API signatures, it is always possible with the correct combination of Java language features and design patterns.

### 1.1.9.    **Do not** place unrelated APIs into the same package

**Rationale:**

[Simplicity](https://theamiableapi.com/2011/09/07/api-design-best-practice-keep-it-simple/) and [Consistency](https://theamiableapi.com/2011/09/14/api-design-best-practice-consistency/). Each API deserves its own package, so that developers can focus on the features they need, without any distracting clutter. The most frequently heard argument for putting several APIs into the same package is their small size. This argument fails to consider that APIs evolve, and what is a small package today can easily become a very large package containing a hodge-podge of unrelated features over time. The java.util package is the best know example.

### 1.1.10.  **Avoid** relying on documentation to control evolution of API or SPI

SPI implementors can always call their own code, therefore treating it like API. This should be encouraged when they
want to test their SPI. The evolution path of SPI or API should be constrained only by the compiler, not documentation.
Therefore if you want to provide polymorphic API without upgrading it to SPI, you need to hide the constructor.

### 1.1.11.  **Do not** move or rename the package of an already released public API

**Rationale:**

[Evolution](https://theamiableapi.com/2011/10/18/api-design-best-practice-plan-for-evolution/). Changing the fully qualified name of the API package breaks both binary and source backwards compatibility with existing clients.

**Exceptions:**

None

# Package Design Checklist, Naming

### 1.2.1. **Start** package names with the company’s official root namespace

**Rationale:**

[Consistency](https://theamiableapi.com/2011/09/14/api-design-best-practice-consistency/). The long-established Java convention is to generate a unique root package name by reversing the internet domain name of the developing organization. When the organization owns multiple internet domain names, the official domain name is used.

**Exceptions:**

This rule applies to all new APIs and all major new API versions. It does _not_ apply to published APIs, for which [item 1.1.11](https://theamiableapi.com/2012/01/16/java-api-design-checklist#cl.item.1.1.11) takes precedence.

If the package naming is changed (for example as part of product rebranding), a major new API version is released. The old package names are deprecated, but kept for backwards compatibility, satisfying [item 1.1.11](https://theamiableapi.com/2012/01/16/java-api-design-checklist#cl.item.1.1.11)

### 1.2.2. **Use** a stable product or product family name at the second level of the package name

**Rationale:**

[Simplicity](https://theamiableapi.com/2011/09/07/api-design-best-practice-keep-it-simple/) and [Consistency](https://theamiableapi.com/2011/09/14/api-design-best-practice-consistency/). Developers like short API package names and ideally you should place API packages directly under the root namespace. However, multiple independent business units or product groups may share the root namespace of larger organizations. It is customary to subdivide the root namespace and give each independent unit full control over their own namespace. When the namespace is organized like this, place the API package directly under the namespace you control.

When nobody is officially in charge of controlling the organization’s root namespace, insert a stable product or product family name between the root namespace and the API package. Don’t use the product’s external marketing name, over which you have no control. Use a generic dictionary term or a widely accepted industry concept which best describes your product.

Talk to your product manager about what product name to use.

**Do this:**

```
com.company.archiving
com.company.rm
com.company.wcm
com.company.bpm
```

**Don’t do this:**

```
com.company.artesia
com.company.reddot
```

**Exceptions:**

If the API itself is the product, place the API package directly under the root namespace.

### 1.2.3. **Use** the name of the API as the final part of the package name

**Rationale:**

[Consistency](https://theamiableapi.com/2011/09/14/api-design-best-practice-consistency/). Developers scan the end of import statements to see what is being imported and expect API names to appear there, so it is important that API classes are imported from a package clearly identified with the name of the API.

If the API is broken up into several packages as suggested in [item 1.1.3](https://theamiableapi.com/2012/01/16/java-api-design-checklist#cl.item.1.1.3), these packages should be named appropriately and placed inside a package bearing the name of the API.

For tips on choosing memorable API names see our [naming guidelines](https://theamiableapi.com/2011/09/21/api-design-best-practice-naming/).

**Do this:**

```
package com.company.product; //OK. API name same as product name

public class ApiClass {};
```

**Do this:**

```
package com.company.product.apiname; //OK

public class ApiClass {};
```

**Do this:**

```
package com.company.product.apiname.apipart; //OK. API name still clear

public class ApiClass {};
```

**Don’t do this:**

```
package com.company; //No API name!

public class ApiClass {};
```

**Don’t do this:**

```
package com.company.apipart; //No API name!

public class ApiClass {};
```

**Don’t do this:**

```
package com.company.product.apiname.detail1.detail2; //Difficult to find and import classes!

public class ApiClass {};
```

### 1.2.4. **Consider** marking implementation-only packages by including “internal” in the package name

**Rationale:**

<[Simplicity](https://theamiableapi.com/2011/09/07/api-design-best-practice-keep-it-simple/), [Consistency](https://theamiableapi.com/2011/09/14/api-design-best-practice-consistency/), [Safety](https://theamiableapi.com/2011/10/12/api-design-best-practice-make-it-safe/), and [Evolution](https://theamiableapi.com/2011/10/18/api-design-best-practice-plan-for-evolution/). We want to help developers quickly skip implementation packages when searching for APIs. We also want to warn them when trying to import from off-limits implementation packages. We recommend the word “internal” placed either at or just below the level of the API. Using “impl” serves the same purpose, but “internal” sounds better and should be used for consistency.

You can use the refactoring feature of a modern IDE to rename implementation packages in existing code. Import statements will be adjusted in all source files. Changing the name of implementation packages should have no impact on external clients.

**Do this:**

```
package com.company.product.api;
package com.company.product.api.internal; //good
```

**Do this:**

```
package com.company.product.api1;
package com.company.product.api2;
package com.company.product.internal; //better
```

**Don’t do this:**

```
package com.company.product.api;
package com.company.product.impl;
```

**Don’t do this:**

```
package com.company.product.api;
package com.company.product.other.stuff;
```

### 1.2.5. **Avoid** composite names

**Rationale:**

[Consistency](https://theamiableapi.com/2011/09/14/api-design-best-practice-consistency/). Composite package names are hard to read because the Java language specification only allows lowercase characters in package names. While underscores are allowed, they are very rarely used in practice, their usage being inconsistent both with the standard Java library names and with the mixed case naming conventions of the other Java identifiers. We ask you to avoid composite package names entirely. Use a single word whenever possible or a well-chosen abbreviation. Remember that we always use fully qualified package names and there is often enough information to give otherwise ambiguous terms a more precise meaning. Consider also that the fully qualified package name can easily become quite long.

**Do this:**

```
com.company.process.designer
```

**Do this:**

```
com.company.bpm.designer
```

**Don’t do this:**

```
com.company.businessprocessmanagement.processdesigner
```

**Don’t do this:**

```
com.company.business_process_management.process_designer
```

### 1.2.6. **Avoid** using the same name for both package and class inside the package

**Rationale:**

[Naming](https://theamiableapi.com/2011/09/21/api-design-best-practice-naming/). It is generally not a good idea to give the same name to two different things. It becomes outright confusing when it is not clear what the API is. Is it the whole package or just the class?

**Do this:**

```
package com.company.product.scheduler; //OK. Scheduler is a package

public class TaskList {};
```

**Do this:**

```
package com.company.product.timing; 

public class Scheduler {}; //OK. Scheduler is a class
```

**Don’t do this:**

```
package com.company.product.scheduler; 

public class Scheduler {};  // What is a scheduler?
```

### 1.2.7. **Avoid** using “api” in package names

**Rationale:**

[Simplicity](https://theamiableapi.com/2011/09/07/api-design-best-practice-keep-it-simple/) and [Consistency](https://theamiableapi.com/2011/09/14/api-design-best-practice-consistency/). Callers import mostly APIs. The consistent use of the word “api” would make it redundant, as shown in the example below. This is why none of the core Java APIs has “api” in its package name. For consistency, we should follow the same convention.

**Do this:**

```
//consistent avoidance of the word "api"

import java.sql.*
import javax.naming.*
import javax.jms.*
import com.ibm.websphere.sca.*
```

**Don’t do this:**

```
//consistent, but redundant use of the word "api"

import java.sql.api.*
import javax.naming.api.*
import javax.jms.api.*
import com.ibm.websphere.sca.api.*
```

**Don’t do this:**

```
//inconsistent use of the word "api"

import java.sql.*
import javax.naming.*
import javax.jms.*
import com.ibm.websphere.sca.api.*
```

### 1.2.8. **Do not** use marketing, project, organizational unit or geographic location names

**Rationale:**

[Naming](https://theamiableapi.com/2011/09/21/api-design-best-practice-naming/). This is a simple application of the “do not use names which can change or may loose their meaning” rule. It deservers its place here only because such mistakes are very frequent in package names.

**Don’t do this:**

```
package com.company.borg;        //Internal R&D project name
package com.company.chicago;     //Team location
package com.company.livelink;    //Brand name (Marketing)
```

### 1.2.9. **Do not** use uppercase characters in package names

**Rationale:**

### 1.3.1. **Provide** a package overview (package.html) for each package

**Rationale:**

[Consistency](https://theamiableapi.com/2011/09/14/api-design-best-practice-consistency/) in [Documentation](https://theamiableapi.com/2011/11/01/api-design-best-practice-write-helpful-documentation/). Javadoc is the standard format for Java reference documentation. Developers expect to find Javadoc description of every single Java construct, including packages. Unlike classes and methods which can be documented using javadoc comments inserted directly into Java source files, you need to create a separate package-info.java (or package.html) file and place it into the package directory. Omitting to provide Javadoc documentation for packages is one of the most common Java documentation mistakes.

### 1.3.2. **Follow** standard Javadoc conventions

**Rationale:**

[Consistency](https://theamiableapi.com/2011/09/14/api-design-best-practice-consistency/) in [Documentation](https://theamiableapi.com/2011/11/01/api-design-best-practice-write-helpful-documentation/). Simply follow the [Javadoc conventions](http://www.oracle.com/technetwork/java/javase/documentation/index-137868.html) established by Oracle (Sun).

### 1.3.3. **Begin** with a short, one sentence summary of the API

**Rationale:**

[Consistency](https://theamiableapi.com/2011/09/14/api-design-best-practice-consistency/) in [Documentation](https://theamiableapi.com/2011/11/01/api-design-best-practice-write-helpful-documentation/). The Javadoc tool automatically copies the first sentence of the provided package overview to the package summary. Write a clear and informative initial sentence which can stand on its own. Focus on the purpose and intended use of the package. Developers use package summaries to decide which packages are relevant to their current task and which packages can be safely skipped.

### 1.3.4. **Provide** enough details to help deciding if and how to use the API

**Rationale:**

[Simplicity](https://theamiableapi.com/2011/09/07/api-design-best-practice-keep-it-simple/) and [Documentation](https://theamiableapi.com/2011/11/01/api-design-best-practice-write-helpful-documentation/). Developers will look at a package overview primarily to decide whether the API is worth further investigation. Consequently, a good package overview starts by answering in less than half a page of text the following questions: 1) Can this API solve my problem? 2) What will the solution look like, roughly?

**Do this:**

```
package com.company.video.converter;

/**
 * Provides utilities for converting MPEG-1 or MPEG-2 Video, in NTSC or PAL format,
 * into Internet media formats. The following output formats are supported:
 *
 *   - Flash Video (F4V)
 *   - QuickTime multimedia file format (MOV and QT)
 *   - Windows Media Video (WMV)
 *
 *  You can merge several video clips into one and clip multiple segments from a single video.
 *  You can clip, rotate, and flip.
 *  You can adjust brightness, contrast and saturation.
 *  You can add subtitles.
 *
 *  The MPEG input module produces a proprietary intermediate video format (VideoStream)
 *  and you pipe various modules together using VideoStream to produce the desired output.
**/
```

### 1.3.5. **Indicate** the entry points (main classes or methods) of the API

**Rationale:**

[Documentation](https://theamiableapi.com/2011/11/01/api-design-best-practice-write-helpful-documentation/). Callers rarely need all types from a package. Finding out where to start can be challenging when working with large packages. Nobody likes wasting time inspecting types they won’t need. Indicate (and also link to) the most important or most frequently used type(s) callers should explore first in the Javadoc package overview.

### 1.3.6. **Include** sample code for the main, most fundamental use case

**Rationale:**

[Documentation](https://theamiableapi.com/2011/11/01/api-design-best-practice-write-helpful-documentation/). A line of code is worth a thousand words. When encountering an unknown API, the developer’s first question is how to get it working. Sample code for the simplest, most fundamental use case included in the package overview section of the Javadoc answers this essential question.

### 1.3.7. **Include** a link to the Developer Guide

See item [1.3.8](#138-include-a-link-to-the-cookbook) below.

### 1.3.8. **Include** a link to the Cookbook

**Rationale:**

As we discussed in [“Writing helpful documentation”](https://theamiableapi.com/2011/11/01/api-design-best-practice-write-helpful-documentation/) , reference documentation like the Javadoc is rarely sufficient. For all but the simplest of APIs, we need other forms of documentation, such as developer guide or cookbook. These are best kept separate from the frequently-referenced Javadoc, but their content linked to from where it is appropriate. At a very minimum, package overviews should contain links to these materials. Links to relevant sections can be also provided from class and method overviews where needed.

### 1.3.9. **Indicate** related APIs

_Details coming soon…_

### 1.3.10. **Include** the API version number

**Rationale:**

[Evolution](https://theamiableapi.com/2011/10/18/api-design-best-practice-plan-for-evolution/). A change in minor version number indicates backwards-compatible API extensions and enhancements, while a change in the major version number indicates an extensive API overhaul which may include non-backwards compatible changes. Modifications are typically required in existing clients to make them work with a new major API version, hence the importance of including the @version tag indicating the version number into the package overview section of the Javadoc.

### 1.3.11. **Indicate** deprecated API versions with the @deprecated tag

_Details coming soon…_

### 1.3.12. **Consider** including a copyright notice

**Rationale:**

Both the API itself and the associated documentation, including the Javadoc, represent intellectual property which need legal protection in multiple jurisdictions. Including the appropriate copyright notice into the package overview section of the Javadoc may be a legal requirement. Talk to your manager or contact the legal department of your organization for advice and to obtain an approved copyright notice.

### 1.3.13. **Avoid** lengthy package overviews

_Details coming soon…_

### 1.3.14. **Do not** include implementation packages into published Javadoc

_Details coming soon…_

## 2. Type Design Checklist

### 2.1.1. **Ensure** each type has a single, well-defined purpose

_Details coming soon…_

### 2.1.2. **Ensure** types represent domain concepts, not design abstractions

_Details coming soon…_

### 2.1.3. **Limit** the number of types

**Rationale:**

[Simplicity](https://theamiableapi.com/2011/09/07/api-design-best-practice-keep-it-simple/). Each additional type increases the size of APIs, making them harder to learn and remember. The conceptual complexity of use cases and scenarios also increases when more types are used. The overall quality of APIs suffers, unless the concrete, measurable benefits added by the type compensate for the increased size and complexity. Everything else being equal, fewer types are better.

**Do this:**

```
package com.company.product.api;

//This class requires complex configuration and initialization
public class ApiClass { 

    //Default constructor
    public ApiClass() {}

    //Setters and getters
    public void setOption1(Object option1) {}
    public void setOption2(Object option2) {}

    //Initialize after correctly configured
    public void initialize() throws Exception {}
}
```

**Don’t do this:**

```
package com.company.product.api;

//This class requires complex configuration and initialization
public class ApiClass {}
public class ApiClassBuilder {}
public class ApiClassBuilderFactory {}
```

**Don’t do this:**

```
package com.company.product.api;

public class ValueHolder {
    public Object getValue() {}
    public void setValue(Object value) {}
}

public class ApiClass {

    /**
        Does stuff.
        @param result Contains the result of the operation
        @return True if success, False otherwise
    */
    public boolean doStuff(ValueHolder result) {}
}
```

**Don’t do this:**

```
package com.company.product.api;

public class Id {

     public Id(String id) {}
}

public class ApiClass {

    public Object lookupById(Id id) {}
}
```

### 2.1.4. **Limit** the size of types

_Details coming soon…_

### 2.1.5. **Follow** consistent design patterns when designing related types

_Details coming soon…_

### 2.1.6. **Favor** multiple (private) implementations over multiple public types

_Details coming soon…_

### 2.1.7. **Favor** interfaces over class inheritance for expressing simple commonality in behavior

**Rationale:**

[Simplicity](https://theamiableapi.com/2011/09/07/api-design-best-practice-keep-it-simple/) and [Evolution](https://theamiableapi.com/2011/10/18/api-design-best-practice-plan-for-evolution/). You can significantly simplify APIs by capturing common behavior to allow for uniform processing across different types. Commonalities are expressed in Java using either interfaces or class inheritance. Interfaces and class inheritance have different advantages and disadvantages and it is very important to know when to use which.

Abstract superclasses provide more powerful support for expressing commonalities and we talk about these in [item 2.1.8](#218-favor-abstract-classes-over-interfaces-for-decoupling-api-from-implementation). Unfortunately, Java limits you a single superclass per type, which prevents you from capturing several commonalities relating to different aspects in a complex API. Even if you don’t need to capture multiple aspects of common behavior in the first relese of the API, such requirements may appear in successive API releases. Adding new interfaces to existing classes is relatively easy. Modifying the class inheritance hierarchy without breaking backward compatibility can be quite difficult. It is this potential need to express several new comonalities in the future which makes interfaces a better choice for expressing simple commonalities.

### 2.1.8. **Favor** abstract classes over interfaces for decoupling API from implementation

**Rationale:**

[Safety](https://theamiableapi.com/2011/10/12/api-design-best-practice-make-it-safe/) and [Evolution](https://theamiableapi.com/2011/10/18/api-design-best-practice-plan-for-evolution/). Abstract classes can be used everywhere Java interfaces can with the limitation that a class can only extend a single (abstract) superclass while it can implement multiple interfaces. In most APIs Java’s lack of support for multiple class inheritance is not an issue because the interfaces are not intended to be implemented by clients. In these cases abstract classes are the better design choice.

In Java you cannot prevent clients from implementing public interfaces. When you use an interface for a method parameter type, the compiler only guarantees that it implements the correct method signatures; you can never be sure it also implements the correct behavior. You are left with only two choices: either you perform extensive runtime checks or blindly trust the implementation, risking serious consequences, such as data corruption. Unlike interfaces, abstract classes can have code to enforce constraints on behavior. For example, you can prevent clients from extending abstract classes, check preconditions, verify postconditions, and guarantee invariants as illustrated by the examples bellow. You cannot do any of these with interfaces.

**Do this:**

```
public abstract class ApiClass {

    //Package scope constructor prevents clients
    //from extending this class
    ApiClass() {}
}
```

**Do this:**

```
public abstract class ApiClass {

    //This public method guarantees preconditions and postconditions
    //for any abstract method implementation
    public final Collection find(String query) {
        if(query == null) {
            throw new NullPointerException("Null query parameter");
        }
        if(query.isEmpty()) {
            throw new IllegalArgumentException("Empty query parameter");
        }
        Collection result = findImplementation(query);
        if(result == null) {
            result = new ArrayList();
        }
        return result;
    }

    protected abstract Collection findImplementation(String query);
}
```

**Do this:**

```
public abstract class ApiClass {

    /*
    * Garanteed invariant:
    *   value != null &&
    *   value != this.lastModified &&
    *   value.before(new Date()) &&
    *   oldValue.before(newValue) &&
    *   value unchanged => class unchanged
    */
    public final Date getLastModified() {return (Date)lastModified.clone();}

    public final void anyMutatorMethod(Param param) {
        anyMutatorMethodImplementation(param);
        updateLastModified();
    } 

    protected final void updateLastModified() {
        Date newValue = new Date();
        if(lastModified.before(newValue)) {
            lastModified = newValue;
        }
    }

    protected abstract void anyMutatorMethodImplementation(Param param); 

    private Date lastModified = new Date();
}
```

**Exceptions:**

You should use abstract classes only when they are not intended for client extension. You _may_ provide abstract classes for client extension, but you should not force clients to extend them, since Java does not support multiple inheritance. Use Java interfaces instead and provide abstract classes which implement them for convenience. You can see this approach used in the Java collections library, which contains pairs like [Collection](http://download.oracle.com/javase/6/docs/api/java/util/Collection.html) and [AbstractCollection](http://download.oracle.com/javase/6/docs/api/java/util/AbstractCollection.html), [List](http://download.oracle.com/javase/6/docs/api/java/util/List.html) and [AbstractList](http://download.oracle.com/javase/6/docs/api/java/util/AbstractList.html), and so on. Clients are encouraged to extend the abstract classes and even when they decide to implement the interfaces directly, they can use the provided abstract classes as guidance for a correct implementation.

For simple interfaces, such as callbacks, there is no need to provide corresponding abstract classes.

Of course, whenever possible, concrete classes are preferable to abstract classes.

### 2.1.9. **Favor** enumeration types over constants

_Details coming soon…_

### 2.1.10. **Consider** generic types

**Rationale:**

[Simplicity](https://theamiableapi.com/2011/09/07/api-design-best-practice-keep-it-simple/) and [Safety](https://theamiableapi.com/2011/10/12/api-design-best-practice-make-it-safe/). Generic types are safer and easier to use than types which require casts in client code. In addition to increased complexity, casts may throw ClassCastException at runtime, introducing an additional failure mode. When you design new API types, try to make sure that they can be used without such casts. When the main difference between API types is the type of data they operate on, it may be possible to replace them with a single, generic type. For safety, consider restricting the permissible value of the type parameter of the generic parameter.

**Exceptions:**

Because generics are implemented by type erasure in Java, there are some restrictions on what you can do with a type parameter within a generic type. For example, it is not possible to create a new instance of the type indicated by the generic type parameter because this type is unknown to the Java virtual machine at runtime:

```
public class NaiveGenericFactory<T> {

    public T createInstance() {
        return new T(); //Compiler error: Type parameter T cannot be instantiated directly
   }
}
```

As a rule, only methods known at compile time (methods implemented by all possible type parameter values) can be called from within a generic type. This is one more reason why restricting the generic type parameter is useful. If the type parameter is not restricted, only the methods defined for all reference types can be used.

### 2.1.11. **Consider** placing constraints on the generic type parameter

_Details coming soon…_

### 2.1.12. **Consider** using interfaces to achieve similar effect to multiple inheritance

_Details coming soon…_

### 2.1.13. **Avoid** designing for client extension

_Details coming soon…_

### 2.1.14. **Avoid** deep inheritance hierarchies

_Details coming soon…_

### 2.1.15. **Do not** use public nested types

_Details coming soon…_

### 2.1.16. **Do not** declare public or protected fields

_Details coming soon…_

### 2.1.17. **Do not** expose implementation inheritance to the client

_Details coming soon…_

### 2.2.1. **Use** a noun or a noun phrase

_Details coming soon…_

### 2.2.2. **Use** PascalCasing

**Rationale:**

[Consistency](https://theamiableapi.com/2011/09/14/api-design-best-practice-consistency/). This is a long-established Java convention.

### 2.2.3. **Capitalize** only the first letter of acronyms

**Rationale:**

[Consistency](https://theamiableapi.com/2011/09/14/api-design-best-practice-consistency/). PascalCasing is the Java capitalization convention for type names and acronyms should be considered words, not individual letters. This way of capitalizing acronyms is widely used in SDK type names like [AclEntry](http://download.oracle.com/javase/6/docs/api/java/security/acl/AclEntry.html), [CipherSpi](http://download.oracle.com/javase/6/docs/api/javax/crypto/CipherSpi.html), [Clob](http://download.oracle.com/javase/6/docs/api/java/sql/Clob.html), [HttpRetryException](http://download.oracle.com/javase/6/docs/api/java/net/HttpRetryException.html), or [XmlSchema](http://download.oracle.com/javase/6/docs/api/javax/xml/bind/annotation/XmlSchema.html).

That being said, this is one of those cases where the Java SDK itself is inconsistent, a fact best illustrated with the type name [HttpURLConnection](http://download.oracle.com/javase/6/docs/api/java/net/HttpURLConnection.html). For a long time, there was no established Java convention for capitalizing acronyms and each team chose either one or the other. If you judge it based on how many times it appears in SDK names, it looks like all uppercase is the way to go. However, this is not the case. Capitalizing only the first letter is now the Java convention, being used consistently across the newer SDK packages. HttpURLConnection perfectly illustrates the superiority of the mixed case approach: HttpUrlConnection works just fine, HTTPURLConnection is rather hard to read.

**Do this:**

```
package com.company.product.api;

public interface HtmlDocument {}
public class XmlConfigurator {}
public class MtomInputStream {}
public class OtdsException {}
```

**Don’t do this:**

```
package com.company.product.api;

public interface HTMLDocument {}
public class XMLConfigurator {}
public class MTOMInputStream {}
public class OTDSException {}
```

**Exceptions:**

When adding a new type to an existing package which _consistently_ uses all uppercase acronyms, maintaining the consistency of the package should take precedence. For example, the [javax.xml.soap](http://download.oracle.com/javase/6/docs/api/javax/xml/soap/package-summary.html) SDK package uses the all uppercase SOAP acronym everywhere in names like SOAPHeader, SOAPElement or SOAPException. If a new type were needed for this package, it should have “SOAP” and not “Soap” in its name.

### 2.2.4. **Use** accurate names for purpose of the type

**Rationale:**

[Naming](https://theamiableapi.com/2011/09/21/api-design-best-practice-naming/), [Behavior](https://theamiableapi.com/2011/09/27/api-design-best-practice-specify-the-behavior/) and [Safety](https://theamiableapi.com/2011/10/12/api-design-best-practice-make-it-safe/). Callers make a series of assumptions based on type names. For example, if you call a type a Set, callers will assume that it cannot contain duplicate elements. If you call it an Image, they will assume that its content can be represented using popular image formats like JPEG or PNG. Callers will look for a method which returns the amount of money in an Account. If reasonable assumptions about the purpose, use, and behavior of a type are incorrect, the name is inaccurate. Ask people who did not participate in the API design but are familiar with the intended application domain to review your type names for accuracy.

### 2.2.5. **Reserve** the shortest, most memorable name for the most frequently used type

_Details coming soon…_

### 2.2.6. **End** the name of all exceptions with the word “Exception”

**Rationale:**

[Consistency](https://theamiableapi.com/2011/09/14/api-design-best-practice-consistency/). This is a long-established Java convention.

### 2.2.7. **Use** singular nouns (Color, not Colors) for naming enumerated types

**Rationale:**

[Consistency](https://theamiableapi.com/2011/09/14/api-design-best-practice-consistency/). This is a long-established Java convention.

### 2.2.8. **Consider** longer names

**Rationale:**

[Naming](https://theamiableapi.com/2011/09/21/api-design-best-practice-naming/). Having descriptive names is more important than having short, compact names. Single-word names are nice, but if you cannot find one which accurately describes a type, consider longer composite names. Resist the temptation of using abbreviations. Spell out all the words. Long names are fine in Java.

**Do this:**

```
package com.company.product.api;

/**
 * Cluster event raised in a
 * [split-brain](http://en.wikipedia.org/wiki/Split-brain_%28Computing%29) situation, when network connection
 * between the servers in a cluster is lost, and the servers
 * continue to function forming independent clusters.
 */
public class SplitBrainClusterEvent extends ClusterEvent {}
```

**Don’t do this:**

```
package com.company.product.api;

public class SplitEvent extends ClusterEvent {} //What is a SplitEvent?
```

**Don’t do this:**

```
package com.company.product.api;

public class SbcEvent extends ClusterEvent {} //What is a SbcEvent?
```

### 2.2.9. **Consider** ending the name of derived class with the name of the base class

_Details coming soon…_

### 2.2.10. **Consider** starting the name of an abstract class with the word “Abstract”

**Rationale:**

[Consistency](https://theamiableapi.com/2011/09/14/api-design-best-practice-consistency/) and [Naming](https://theamiableapi.com/2011/09/21/api-design-best-practice-naming/). Starting the name of abstract classes with the word “Abstract” is an established Java convention with many examples in the Java SDK. However, whether you should start an abstract class name with “Abstract” or not in your API depends on the purpose of the class. If you include abstract classes for callers to extend, you should follow this naming convention, as shown in the first example. However, when you use abstract classes in method signatures (see [item 2.1.8](https://theamiableapi.com/2012/01/16/java-api-design-checklist#cl.item.2.1.8).), whether these classes are abstract or not is much less relevant. In such cases, abstract classes should be named like concrete classes or interfaces, without including “Abstract” in the class name, as shown in the second example.

**Do this:**

```
package com.company.product.api;

/**
 * Defines method signatures implemented by all authentication
 * handlers.
 */
public interface AuthenticationHandler {}

/**
 * Extend this class to implement your custom authentication handler.
 */
public abstract class AbstractAuthenticationHandler implements AuthenticationHandler {}
```

**Do this:**

```
package com.company.product.api;

/**
 * Defines the common interface and behavior of all
 * triggers.
 */
public abstract class Trigger {}

/**
 * Implements a trigger which fires daily at the specified time
 */
public class DailyTrigger extends Trigger {

    public DailyTrigger(int hour, int minute, int second) {}
}

/**
 * Schedules jobs for execution at a later time
 */
public class Scheduler {

    public void scheduleJob(Job job, Trigger trigger, String description) {}
}
```

**Don’t do this:**

```
package com.company.product.api;

/**
 * Defines the common interface and behavior for all
 * triggers.
 */
public abstract class AbstractTrigger {}

/**
 * Schedules jobs for execution at a later time
 */
public class Scheduler {

    public void scheduleJob(Job job, AbstractTrigger trigger, String description) {}
}
```

### 2.2.11. **Avoid** abbreviations

_Details coming soon…_

### 2.2.12. **Avoid** generic nouns

_Details coming soon…_

### 2.2.13. **Avoid** synonyms

_Details coming soon…_

### 2.2.14. **Avoid** type names used in related APIs

_Details coming soon…_

### 2.2.15. **Do not** use names which differ in case alone

_Details coming soon…_

### 2.2.16. **Do not** use prefixes

_Details coming soon…_

### 2.2.17. **Do not** prefix interface names with “I”

_Details coming soon…_

### 2.2.18. **Do not** use types names used in Java core packages

**Rationale:**

[Naming](https://theamiableapi.com/2011/09/21/api-design-best-practice-naming/). Fully qualified Java names (using package names) exist to prevent _accidental_ name collisions, not to encourage intentional reuse of names for unrelated types. It can be especially confusing when a widely recognized name is borrowed from the [core Java API](http://download.oracle.com/javase/6/docs/api/index.html?overview-summary.html), which consists of all packages with names starting with java.* and javax.*. Before using a name, check if it is already used in one of these packages. If it is, find another name for your type.

**Don’t do this:**

```
package com.company.product.api;

public class ApiClass extends ContentHandler { //[java.net.ContentHandler?](http://download.oracle.com/javase/6/docs/api/java/net/ContentHandler.html)

public Container getContainer(){} //[java.awt.Container?](http://download.oracle.com/javase/6/docs/api/java/awt/Container.html)

public void saveDocument(Document doc) {} //[javax.swing.text.Document?](http://download.oracle.com/javase/6/docs/api/javax/swing/text/Document.html)

public String readContent() throws ProtocolException {} //[java.net.ProtocolException?](http://download.oracle.com/javase/6/docs/api/java/net/ProtocolException.html)

public void registerListener(NotificationListener listener) {} //[javax.management.NotificationListener?](http://download.oracle.com/javase/6/docs/api/javax/management/NotificationListener.html)  
}

```

### 2.3.1. **Minimize** implementation dependencies

_Details coming soon…_

### 2.3.2. **List** public methods first

#### Rationale:

[Simplicity](https://theamiableapi.com/2011/09/07/api-design-best-practice-keep-it-simple/). Implementation details are of no interest to callers. It is easier to ignore private or protected methods and fields if public methods are listed first in source code. The practice of listing (private) fields first, while widespread in implementation code, should not be used in public APIs.

### 2.3.3. **Declare** implementation methods private

_Details coming soon…_

### 2.3.4. **Define** at least one public concrete class which extends a public abstract class

#### Rationale:

For the same reasons why you should provide at least one concrete public implementing class for every interface. See [item.2.4.1](#214-limit-the-size-of-types)

### 2.3.5. **Provide** adequate defaults for the basic usage scenarios

_Details coming soon…_

### 2.3.6. **Design** classes with strong invariants

_Details coming soon…_

### 2.3.7. **Group** stateless, accessor and mutator methods together

_Details coming soon…_

### 2.3.8. **Keep** the number of mutator methods at a minimum

_Details coming soon…_

### 2.3.9. **Consider** providing a default no-parameter constructor

#### Rationale:

[Simplicity](https://theamiableapi.com/2011/09/07/api-design-best-practice-keep-it-simple/). The simplest way to instantiate types in Java is with a no-parameter default constructor. Developers often prefer to instantiate and experiment with individual types before wiring them together. The ability to defer connecting types is also useful in scenarios like lazy initialization, reflection, custom serialization, persisting objects in relational databases, or caching. Constructors which take instances of other types as parameters have the disadvantage of imposing a strict ordering on type instantiation and can make certain scenarios harder to implement.

#### Exceptions:

Default constructors may be unnecessary if you need to set all the missing parameter value(s) before you can do anything else with the class. This shows that there is a strong dependency between the types by design and adding a default constructor does not eliminate it. On the contrary, it may be better to make the existence of such dependencies obvious by _not_ providing a default constructor. For situations where you should not provide any constructors see [item 2.3.15](#2315-consider-preventing-class-instantiation-by-not-providing-a-public-constructor).

### 2.3.10. **Consider** overriding equals(), hashCode() and toString()

#### Rationale:

[Behavior](https://theamiableapi.com/2011/09/27/api-design-best-practice-specify-the-behavior/). Every class inherits these methods from [java.lang.Object](http://download.oracle.com/javase/6/docs/api/java/lang/Object.html) and callers expect the behavior to match to intended use of the class. Not surprisingly, the inherited behavior is often different from what callers expect. Consider the [java.net.URL class](http://download.oracle.com/javase/6/docs/api/java/net/URL.html) as an example. Callers expect toString() to return the string representation of the URL, not the default combination of class name and instance identifier. Callers would be also quite puzzled if two instances of the URL class pointing to the same resource were not equal to each other, which requires equals() to be overridden. And every time equals() is overriden, hashCode() should be overriden as well to ensure consistent behavior. Forgetting to override these inherited methods is a common API design mistake.

### 2.3.11. **Consider** implementing Comparable

#### Rationale:

[Simplicity](https://theamiableapi.com/2011/09/07/api-design-best-practice-keep-it-simple/). The Comparable interface makes it simpler for callers to order (sort) instances of a class. Objects which implement Comparator can be stored in ordered Java collections like [SortedMap](http://download.oracle.com/javase/6/docs/api/java/util/SortedMap.html) or [SortedSet](http://download.oracle.com/javase/6/docs/api/java/util/SortedSet.html) without providing a custom [Comparator](http://download.oracle.com/javase/6/docs/api/java/util/Comparator.html). From the [perspective of the caller](https://theamiableapi.com/2011/08/29/considering-the-perspective-of-the-caller/), there are no disadvantages of implementing Comparable; every class for which a meaningful natural (default) ordering exist can implement it.

#### Do this:

```
import com.company.product.api.ApiClass;
import java.util.*;

//Code fragment if ApiClass implements Comparable
List<ApiClass> list = new ArrayList<ApiClass>();
list.add(new ApiClass("B"));
list.add(new ApiClass("A"));
list.add(new ApiClass("D"));
list.add(new ApiClass("C"));
Collections.sort(list);  //list ordered A, B, C, D
```

#### Don’t do this:

```
import com.company.product.api.ApiClass;
import java.util.*;

//Needed if ApiClass does not implement Comparable
class ClientDefinedApiClassComparator
        implements Comparator<ApiClass> {

    public int compare(ApiClass o1, ApiClass o2) {
        String s1 = o1.getName().toUpperCase();
        String s2 = o2.getName().toUpperCase();
        return s1.compareTo(s2);
    }
}

//Code fragment if ApiClass does not implement Comparable
List<ApiClass> list = new ArrayList<ApiClass>();
list.add(new ApiClass("B"));
list.add(new ApiClass("A"));
list.add(new ApiClass("D"));
list.add(new ApiClass("C"));
Collections.sort(list, new ClientDefinedApiClassComparator());  //list ordered A, B, C,
```

### 2.3.12. **Consider** implementing Serializable

#### Rationale:

[Simplicity](https://theamiableapi.com/2011/09/07/api-design-best-practice-keep-it-simple/). [Java serialization](http://java.sun.com/developer/technicalArticles/Programming/serialization/) is useful for persisting class instances or transferring them across process and class loader boundaries using Java RMI. From the [perspective of the caller](https://theamiableapi.com/2011/08/29/considering-the-perspective-of-the-caller/), there are no disadvantages of implementing [java.io.Serializable](http://download.oracle.com/javase/6/docs/api/java/io/Serializable.html), even if you don’t anticipate the need for it. If you implement Serializable, always make sure (test) that all the class’ internal state is serialized correctly.

#### Do this:

```
import com.company.product.api.ApiClass;
import java.rmi.RemoteException;

//Code fragment when ApiClass implements Serializable
try {
    ApiClass result = clientDefinedRemoteCall();
    result.apiMethod();
    //and so on
}
catch (RemoteException e){}
```

#### Don’t do this:

```
import com.company.product.api.ApiClass;
import java.rmi.RemoteException;

/Needed if ApiClass does not implement Serializable
class ClientDefinedSerializableClass implements Serializable {

    private Serializable field1;
    private Serializable field2;

    public ClientDefinedSerializableClass(ApiClass a) {
        field1 = a.getField1();
        field2 = a.getField2();
    }

    public ApiClass toApiClass() {
        return new ApiClass(field1, field2);
    }
}

//Code fragment when ApiClass does not implement Serializable
try {
    ClientDefinedSerializableClass remoteResult = clientDefinedRemoteCall();
    ApiClass result = remoteResult.toApiClass();
    result.apiMethod();
    //and so on
}
catch (RemoteException e){}
```

### 2.3.13. **Consider** making classes re-entrant

_Details coming soon…_

### 2.3.14. **Consider** declaring the class as final

#### Rationale:

[Safety](https://theamiableapi.com/2011/10/12/api-design-best-practice-make-it-safe/). You should either explicitly design for client extension or prevent it by declaring classes as final. Safe extension requires careful design: you need to think about which methods should be protected and which ones private; you need to think about which methods can be overridden and which ones cannot; you need to add additional error handling code to deal with the errors which may only appear in client implementations; you need to write additional tests. If you are not willing to spend the effort, ensure the safety of the API by declaring the class as final.

### 2.3.15. **Consider** preventing class instantiation by not providing a public constructor

#### Rationale:

[Simplicity](https://theamiableapi.com/2011/09/07/api-design-best-practice-keep-it-simple/), [Safety](https://theamiableapi.com/2011/10/12/api-design-best-practice-make-it-safe/), and [Behavior](https://theamiableapi.com/2011/09/27/api-design-best-practice-specify-the-behavior/). From the [perspective of the caller](https://theamiableapi.com/2011/08/29/considering-the-perspective-of-the-caller/) constructors are only useful when instances created are at least partially functional. For example, there is no point in creating instances of [java.sql.ResultSet](http://download.oracle.com/javase/6/docs/api/java/sql/ResultSet.html) representing the result of SQL queries without executing the queries themselves. You should indicate that properly initialized instances can be only obtained using the appropriate method calls by declaring the constructors package scoped, thus making them inaccessible to callers.

### 2.3.16. **Consider** using custom types to enforce strong preconditions as class invariants

_Details coming soon…_

### 2.3.17. **Consider** designing immutable classes

#### Rationale:

[Behavior](https://theamiableapi.com/2011/09/27/api-design-best-practice-specify-the-behavior/) and [Safety](https://theamiableapi.com/2011/10/12/api-design-best-practice-make-it-safe/). Instances of immutable classes cannot be modified. The internal state is fixed for the lifetime of the object. The core Java library contains several immutable classes, including [String](http://download.oracle.com/javase/6/docs/api/java/lang/String.html) and [BigInteger](http://download.oracle.com/javase/6/docs/api/java/math/BigInteger.html). Immutable classes are easier to use and less error prone than mutable classes. For example, immutable objects require no synchronization, being inherently thread safe. You can also share immutable instances between client code and implementation code, meaning that you can safely store immutable object references passed as method parameters and you can safely return references to internal immutable objects. If you are using mutable objects, you need to store and/or return a copy; otherwise the client may inadvertently change the state of the internal object. Compare for example the Java [String](http://download.oracle.com/javase/6/docs/api/java/lang/String.html) (immutable) and [Date](http://download.oracle.com/javase/6/docs/api/java/util/Date.html) (mutable) classes.

You make a class immutable by removing all methods which can change the internal state (for example, all set methods) and by declaring all internal fields as final. Instead of changing the class itself, methods should return a new instance of the class when required. There are obvious performance implications of doing this, so you won’t be able to make all API classes immutable. However, it is a good rule of thumb to make API classes immutable _unless_ there is a good reason why you should make them mutable. Even when you cannot make the whole class immutable, it is useful to make as much of the internal state immutable as possible.

#### Do this:

```
import com.company.product.api.ApiClass;

public class ClientClass {

    private ApiClass clientField;

    public ApiClass getClientField() {
        return clientField;     //Safe, ApiClass immutable
    }

    public void getClientField(ApiClass value) {
        clientField = value;    //Safe, ApiClass immutable
    }
}
```

#### Don’t do this:

```
import com.company.product.api.ApiClass;

public class ClientClass {

    private ApiClass clientField;

    public ApiClass getClientField() {
        return new ApiClass(clientField);    //Copy needed, ApiClass mutable
    }

    public void setClientField(ApiClass value) {
        clientField = new ApiClass(value);   //Copy needed, ApiClass mutable
    }
}
```

### 2.3.18. **Avoid** static classes

_Details coming soon…_

### 2.3.19. **Avoid** using Cloneable

_Details coming soon…_

### 2.3.20. **Do not** add instance members to static classes

_Details coming soon…_

### 2.3.21. **Do not** define public constructors for public abstract classes clients should not extend

#### Rationale:

[Safety](https://theamiableapi.com/2011/10/12/api-design-best-practice-make-it-safe/). Concrete API classes not designed for extensions should be declared as final to prevent clients from extending them (see [item 2.3.14](#2314-consider-declaring-the-class-as-final)). Certain abstract API classes aren’t designed for client extension either (see [item 2.1.8](https://theamiableapi.com/2012/01/16/java-api-design-checklist#cl.item.2.1.8)), but in Java it is illegal to declare a class both abstract and final. Fortunately, there is another way for preventing clients from extending abstract classes: by making all constructors package scoped. While clients can work around this technical limitation by placing their own classes in the API package, it is enough to make it clear that what they are doing is unsupported and unsafe.

### 2.3.22. **Do not** require extensive initialization

_Details coming soon…_

[![Creative Commons Licence](https://i0.wp.com/i.creativecommons.org/l/by-sa/2.5/ca/88x31.png)](http://creativecommons.org/licenses/by-sa/2.5/ca/)  
This work is licensed under a [Creative Commons Attribution-ShareAlike 2.5 Canada License](http://creativecommons.org/licenses/by-sa/2.5/ca/).