# Modules

## Abstract

- Used to provide the interface to a reusable piece of code.
- A tool for replacing the header include method.
- Improves compiling performance.

## Problems with the Header Files

- Need to be cautious to avoid multiple includes of the same header.
- Need to be cautious to include headers in the correct order.
- The code becomes excessively long after preprocessing.
- Recompiled every time it's included.
- Modifying headers triggers cascading recompilation of multiple sources.

## The Benefits of Modules

- The import order of modules is not important.
- Modules are compiled only once.
- Modifying module code doesn't trigger recompilation of other code unless the interface is modified.
- Macros defined within a module are **not exported** to the outside.

## Module Interface Files

- Defines the interface for the functionality provided by a module.
- Usually have `.ixx` or `.cppm` as an extension.

```cpp
// mymodule.ixx

/*
    Module Declaration
        - A module interface starts with a declaration stating that the file is defining a module.
        - The module name follows C++ identifier naming conventions and may include dots.
        - But dots cannot appear at the beginning or end and cannot occur consecutively.
*/
export module com.examples.mymodule;

/*
    Import Declaration
        - Declaration section for referencing other modules or C++ headers.
        - To reference another module, specify the identifier of the target module.
        - To reference a C++ header, provide the header name.
*/
import com.examples.anothermodule;
import <string>;
import "ThirdPartyHeader.h";

/*
    Export declaration
        - It is necessary to specify which entities to expose.
        - The export keyword is used to expose entities.
        - Entities without the export keyword can only be referenced within the module.
*/
export class MyModule{ };
export void myModuleFunc(){ }
```

- When importing a header, all entities within the header are implicitly exported.
    - Macros defined in the header become visible to client code.
- Also when importing a header, the header is transformed into a module by the compiler.
- This ensures that the header is compiled only once, providing similar effects to precompiled header files.
- But C headers cannot use the `import` statement.
- Instead, they must be referenced using the `#include` statement in the ***global module fragment***.

```cpp
// Start of the global module fragment
module;

// Include C header files
#include <cstddef>
...

// End of the global module fragment (module declaration)
export module com.examples.mymodule;

...
```

- Almost anything can be exported from a module.
- When a namespace is exported, all entities inside it are automatically exported.

```cpp
export namespace MyModules
{
    // Exported
    class MyModule1{};

    // Exported
    class MyModule2{};

    // Exported
    class MyModule3{};
}
```

- Export a whole block of declarations using an ***export block***.

```cpp
export
{
    // Exported
    class MyModule1{};

    // Exported
    class MyModule2{};

    // Exported
    class MyModule3{};
}
```

## Module Implementation Files

- The implementation of a module can be written in a separate implementation file.
- Usually have `.cpp` as an extension.
- Separating the implementation is entirely flexible, unlike when using headers.

```cpp
// MyModule.ixx

export module com.examples.mymodule;

export class MyModule
{
public:
    int getValue() const;
    void setValue(int newValue);

private:
    int value{};
};
```

```cpp
// MyModule.cpp

// Module declaration, but without the export keyword
module com.examples.mymodule;

int MyModule::getValue() const
{
    return value;
}

void MyModule::setValue(int newValue)
{
    value = newValue;
}
```

- The module declaration in the implementation implicitly includes the import declarations from the interface.

## Importing Modules

```cpp
import com.examples.mymodule;

int main()
{
    MyModule myModule{};
    return 0;
}
```

## The Compilation Process of Modules

- A module interface is composed solely of entity signatures, regardless of the location of the implementation.
    - class definitions, function prototypes, enums, and so on
- Hence, changes in the implementation do not affect the code referencing the module.
    - Inline functions and template definitions are exceptions.


## Visibility vs. Reachability

- When importing a specific module, it does not recursively import the modules imported in its interface file.
- This is referred to as being not ***visible***.

```cpp
// MyModule.ixx

export module com.examples.mymodule;

import <string>;

export class MyModule
{
public:
    std::string name;
};
```

```cpp
// main.cpp

import com.examples.mymodule;

int main()
{
    // OK
    MyModule myModule{};

    // Compile error, std::string is not visible
    const std::string &name{ myModule.name }; 

    return 0;
}
```

- However, even if it's not *visible*, it is ***reachable***.
- This means that, although you may not able to resolve the `std::string` type, you can still access its fields, methods, and so on.

```cpp
// main.cpp

import com.examples.mymodule;

int main()
{
    // OK
    MyModule myModule{};

    // OK
    const auto &name{ myModule.name }; 

    // OK
    const size_t nameLength{ myModule.name.length() };

    return 0;
}
```

- To make a specific module visible that you imported in the module interface, use the `export import` statement to forward it.

```cpp
// MyModule.ixx

export module com.examples.mymodule;

// Import, and export
export import <string>;

export class MyModule
{
public:
    std::string name;
};
```

```cpp
// main.cpp

import com.examples.mymodule;

int main()
{
    // OK
    MyModule myModule{};

    // OK
    const std::string &name{ myModule.name }; 

    return 0;
}
```

## Module Partitions

- When a module becomes large, it can be divided into several sub-modules for conceptual separation.
- Module partitions are not exposed to the outside of the module.
- When utilizing module partitions, a single module is divided into a primary interface and interface partitions.
    - A module always has only one such primary module interface file.
    - That¡¯s the interface file containing the export module name declaration.

```cpp
// Car.ixx

export module car;

// Import partitions; specify the partition name prefixed with a colon
import :engine;
import :wheel;

export class Car
{
public:
	Engine engine{ };
	Wheel wheel{ };
};
```

```cpp
// Car.Engine.ixx

// Module partition declaration
export module car:engine;

export class Engine{};
```

```cpp
// Car.Wheel.ixx

export module car:wheel;

export class Wheel {};
```