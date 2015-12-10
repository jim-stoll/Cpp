Cpp Extension for StarUML 2
============================
This extension for StarUML(http://staruml.io) support to generate Cpp code from UML model.



### UMLPackage
* converted to folder.

### UMLClass

* converted to _Cpp Class_. (as a separate `.h` file)
* `visibility` to one of modifiers `public`, `protected`, `private`. If visibility is not setted, consider as `protected`.
* `isFinalSpecialization` and `isLeaf` property to `final` modifier.
* Default constructor is generated.
* All contained types (_UMLClass_, _UMLInterface_, _UMLEnumeration_) are generated as inner type definition.
* TemplateParameter to _Cpp Template_.
* A class with the stereotype of 'noclass' will be written as a bare C-style file (both '.h' and '.cpp'), with no class declaration

### UMLAttribute

* converted to _Cpp Field_.
* `visibility` property to one of modifiers `public`, `protected`, `private`. If visibility is not setted, consider as `protected`.
* `name` property to field identifier.
* `type` property to field type.
* `multiplicity` property to vector type.
* `isStatic` property to `static` modifier.
* `isLeaf` property to `final` modifier.
* `defaultValue` property to initial value.
* Documentation property to JavaDoc comment.
* A 'public' `visibility` attribute with the stereotype 'extern' will be declared as 'extern', and will occur in the generated '.h' file before the class declaration

### UMLOperation

* converted to _Cpp Methods_.
* `visibility` to one of modifiers `public`, `protected`, `private`. If visibility is not setted, consider as `protected`.
* `name` property to method identifier.
* `isAbstract` property to `virtual` modifier. (TODO need options to create pure-virtual function or virtual function)
* `isStatic` property to `static` modifier.
* _UMLParameter_ to _Cpp Method Parameters_.
* _UMLParameter_'s name property to parameter identifier.
* _UMLParameter_'s type property to type of parameter.
* _UMLParameter_ with `direction` = `return` to return type of method. When no return parameter, `void` is used.
* _UMLParameter_ with `isReadOnly` = `true` to `const` modifier of parameter.

### UMLInterface

* converted to _Cpp Class_.  (as a separate `.h` file)
* `visibility` property to one of modifiers `public`, `protected`, `private`. If visibility is not setted, consider as `protected`.
* all method will treated as pure virtaul.

### UMLEnumeration
| Weekdays      |
| ------------- |
| Monday        |
| Tuesday       |
| Saturday      |

converts

```c
/* Test header @ toori67
 * This is Test
 * also test
 * also test again
 */
#ifndef (_WEEKDAYS_H)
#define _WEEKDAYS_H

enum Weekdays { Monday,Tuesday,Saturday };

#endif //_WEEKDAYS_H
```

* converted to _Cpp Enum_.  (as a separate `.h` file)
* `visibility` property to one of modifiers `public`, `protected`, `private`. If visibility is not setted, consider as `protected`.
* _UMLEnumerationLiteral_ to literals of enum.

### UMLAssociationEnd

* converted to _Cpp Field_.
* `visibility` property to one of modifiers `public`, `protected`, `private`. If visibility is not setted, consider as `protected`.
* `name` property to field identifier.
* `type` property to field type.
* If `multiplicity` is one of `0..*`, `1..*`, `*`, then collection type (`std::vector<T>` ) is used.
* `defaultValue` property to initial value.

### UMLArtifactInstance Dependency

* _UMLDependency_ to a _UMLArtifactInstance_ - target `name` property is converted to '#include' in source element's generated '.cpp' file.
  * if Dependency has no stereotype, or is not stereotyped 'hinclude', the '#include' is put into the source element's '.cpp' file
  * if Dependency is stereotyped with 'hinclude', then the '#include' is put into the source element's '.h' file
* target _UMLArtifactInstance_ `name` property is appended with '.h' in the generated '#include'

### Stereotyped UMLDependency

* _UMLDependency_ target `name` property is converted to '#include' in source element's generated '.cpp' file.
  * if Dependency has no stereotype, or is not stereotyped 'hinclude', the '#include' is put into the source elment's '.cpp' file
  * if Dependency is stereotyped with 'hinclude', then the '#include' is put into the source element's '.h' file
* target element's `name` property is appended with '.h' in the generated '#include'

### UMLGeneralization & UMLInterfaceRealization

* converted to _Cpp Inheritance_ (` : `).
* Allowed for _UMLClass_ to _UMLClass_, and _UMLClass_ to _UMLInterface_.



C++ Reverse Engineering
------------------------

1. Click the menu (`Tools > C++ > Reverse Code...`)
2. Select a folder containing C# source files to be converted to UML model elements.
3. `CppReverse` model will be created in the Project.

Belows are the rules to convert from C++ source code to UML model elements.

### C++ Namespace

* converted to _UMLPackage_.

### C++ Class

* converted to _UMLClass_.
* Class name to `name` property.
* Type parameters to _UMLTemplateParameter_.
* Access modifier `public`, `protected` and  `private` to `visibility` property.
* `abstract` modifier to `isAbstract` property.
* Constructors to _UMLOperation_ with stereotype `<<constructor>>`.
* All contained types (_UMLClass_, _UMLInterface_, _UMLEnumeration_) are generated as inner type definition.


### C++ Field (to UMLAttribute)

* converted to _UMLAttribute_ if __"Use Association"__ is __off__ in Preferences.
* Field type to `type` property.

    * Primitive Types : `type` property has the primitive type name as string.
    * `T[]`(array) or its decendants: `type` property refers to `T` with multiplicity `*`.
    * `T` (User-Defined Types)  : `type` property refers to the `T` type.
    * Otherwise : `type` property has the type name as string.

* Access modifier `public`, `protected` and  `private` to `visibility` property.
* `static` modifier to `isStatic` property.
* Initial value to `defaultValue` property.

### C++ Field (to UMLAssociation)

* converted to (Directed) _UMLAssociation_ if __"Use Association"__ is __on__ in Preferences and there is a UML type element (_UMLClass_, _UMLInterface_, or _UMLEnumeration_) correspond to the field type.
* Field type to `end2.reference` property.

    * `T[]`(array) or its decendants: `reference` property refers to `T` with multiplicity `*`.
    * `T` (User-Defined Types)  : `reference` property refers to the `T` type.
    * Otherwise : converted to _UMLAttribute_, not _UMLAssociation_.

* Access modifier `public`, `protected` and  `private` to `visibility` property.

### C++ Method

* converted to _UMLOperation_.
* Type parameters to _UMLTemplateParameter_.
* Access modifier `public`, `protected` and  `private` to `visibility` property.
* `static` modifier to `isStatic` property.
* `abstract` modifier to `isAbstract` property.


### C++ Enum

* converted to _UMLEnumeration_.
* Enum name to `name` property.
* Type parameters to _UMLTemplateParameter_.
* Access modifier `public`, `protected` and  `private` to `visibility` property.

---

Licensed under the MIT license (see LICENSE file).
