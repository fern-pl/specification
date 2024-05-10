# Metaprogramming

> Locals are commonly referred to as variables, which in Fern are specifically variables that are local to a function (ie: not a field or parameter variable. )

All symbol structure members may be accessed from any symbol using the `->` operator, to see such structures view the section on symbol formats!

`rt.symbol` may be used to access internal symbol structures, but not to directly access full symbol structures at once.

```
auto foo = 1;
string bar = "abc";

void main()
{
    writeln(foo->size); // 4
    writeln(foo->type->identifier); // int
    writeln(foo->type->name); // int

    writeln(foo->size == bar->size); // false
    writeln(bar.length); // 3
    writeln(bar->type == string); // true
    writeln(bar->glob == foo->glob); // true
}
```

## Symbol Attributes & Formats

| Symbol Attribute | Definition |
|-------------|------------|
| `type` | Structure of data with or without instance presence - `struct`, `class`, `tagged` or `tuple`. |
| `struct` | A product-type aggregate passed by-value. |
| `class` | A product-type aggregate passed by-reference. |
| `tagged` | A sum-type aggregate passed by-value with a `tag`. |
| `tuple` | A sum-type aggregate passed by-value with arbitrary types. |
| `module` | Top or domain level scope with no instance presence. |
| `function` | Executable code scope taking parameters and returning a return type. |
| `delegate` | Dynamic executable code scope taking parameters and returning a return type from an address. |
| `lambda` | Special inline format of `delegate`. |
| `ctor` | Scope constructor, namely used for `type` and `module`. |
| `dtor` | Scope destructor, namely used for `type` and `module`. |
| `unittest` | Scope independent executable code taking no parameters and not returning anything. Executes synchronously and may not be called. |
| `field` | Data that exists and persists outside of an execution scope. |
| `local` | Data that exists and persists only inside of an execution scope. |
| `parameter` | Local declarations in a function signature which require arguments. |
| `expression` | Code which may not function without an existing statement to modify, like `1 + 1` |
| `literal` | Value known to the compiler before execution. |
| `glob` | The global scope of the entire program, containing all of its symbols. |
| `alias` | A symbol pointing to another symbol. |

This is implementation defined, but generally symbols have or store the same information as the following formats internally:

```
Symbol [ 
    Glob glob;
    SymAttr symattr;
    string name;
    Symbol[] parents;
    Symbol[] children;
    Symbol[] attributes;

    string identifier;
]
```

```
Type : Symbol [
    Type[] inherits;
    Variable[] fields;
    Function[] functions;
    ubyte[] data;
    size_t size;
    size_t align;
    // For pointer and arrays, how deeply nested they are.
    uint depth;

    string type;
]
```

```
# This is also used for delegates, lambdas, ctors, dtors, and unittests.
Function : Symbol [
    Variable[] parameters;
    // This will include the return and parameters as the first locals.
    Variable[string] locals;
    size_t align;

    Symbol return;
    string type;
]
```

```
# This is also used for locals, parameters, expressions, and literals.
Variable : Symbol [
    ubyte[] data;
    size_t size;
    size_t align;
    size_t offset;

    Symbol type;
]
```

```
Alias : Symbol [
    union
    {
        Symbol single;
        Symbol[] many;
    }

    // Always "alias"
    string type;
]
```

```
Module : Symbol [
    Symbol[] imports;
    Type[] types;
    Variable[] fields;
    Function[] functions;
]
```

```
# This is used to store global information about the program.
Glob : Symbol [
    Symbol[string] symbols;
    Module[string] modules;
    Type[string] types;
    Field[string] fields;
    Function[string] functions;
    Alias[string] aliases;
    Function[] unittests;
]
```