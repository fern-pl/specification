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

## Symbol Properties

Fern has an assortment of utility functions defined for symbols automatically to make working with them more fluid, they may also be defined specially to be used with the `->` operator if a comptime available function returns `alias` and takes in an `alias` as the first generic parameter.

| Property | Evaluates |
|----------|-----------|
| `isType` | Is this symbol a type? |
| `isClass` | Is this symbol a class? |
| `isStruct` | Is this symbol a struct? |
| `isTagged` | Is this symbol a tagged? |
| `isTuple` | Is this symbol a tuple? |
| `isModule` | Is this symbol a module? |
| `isGlob` | Is this symbol a glob? |
| `isAlias` | Is this symbol an alias? |
| `isAliasSeq` | Is this symbol an alias sequence? `isAlias && isArray` |
| `isFunction` | Is this symbol a function? |
| `isDelegate` | Is this symbol a delegate? |
| `isLambda` | Is this symbol a lambda? |
| `isCtor` | Is this symbol a constructor? |
| `isDtor` | Is this symbol a destructor? |
| `isSCtor` | Is this symbol a static constructor? |
| `isSDtor` | Is this symbol a static destructor? |
| `isUnittest` | Is this symbol a unittest? |
| `isField` | Is this symbol a field? |
| `isLocal` | Is this symbol a local? |
| `isParameter` | Is this symbol a parameter? |
| `isVariable` | Is this symbol a variable? `isField \|\| isLocal \|\| isParameter` |
| `isExpression` | Is this symbol an expression? |
| `isLiteral` | Is this symbol a literal? |
| `isArray` | Is this symbol an array? |
| `isDynamicArray` | Is this symbol a dynamic array? |
| `isStaticArray` | Is this symbol a static array? |
| `isAssociativeArray` | Is this symbol an associative array? |
| `isString` | Is this symbol a string? |
| `isWideString` | Is this symbol a wide string? `dstring` or `wstring` |
| `isSigned` | Is this symbol of a signed type? |
| `isIntegral` | Is this symbol an integral? |
| `isFloating` | Is this symbol a floating-point? |
| `isNumeric` | Is this symbol an integral or floating-point? |
| `isByRef` | Is this symbol passed by reference? `isRef \|\| isClass \|\| isKHeap` |
| `isVector` | Is this symbol a vector? `isKXMM \|\| isKYMM \|\| isKZMM` |
| `isPublic` | Is this symbol publicly visible? |
| `isPrivate` | Is this symbol privately visible? |
| `isInternal` | Is this symbol internally visible? |
| `isSafe` | Is this symbol safe? |
| `isSystem` | Is this symbol system? |
| `isTrusted` | Is this symbol trusted? |
| `isStatic` | Is this symbol static? |
| `isGlobal` | Is this symbol global? |
| `isTransient` | Is this symbol transient? |
| `isAtomic` | Is this symbol atomic? |
| `isBitfield` | Is this symbol a bitfield? This alters the size field of symbols, and thus will result in it being in bits. |
| `isPure` | Is this symbol pure? |
| `isConst` | Is this symbol const? |
| `isRef` | Is this symbol ref? |
| `isKHeap` | Is this symbol a heap kind? |
| `isKStack` | Is this symbol a stack kind? |
| `isKScalar` | Is this symbol a scalar kind? |
| `isKFloat` | Is this symbol a floating-point kind? |
| `isKXMM` | Is this symbol a 128-bit vector kind? |
| `isKYMM` | Is this symbol a 256-bit vector kind? |
| `isKZMM` | Is this symbol a 512-bit vector kind? |
| `isKReadOnly` | Is this symbol a readonly kind? This may, but won't always, happen with `const` symbols. |
| `isKDefault` | Is this symbol of the default kind? Does not indicate that this was set to default! |
| `isNested` | Is this symbol nested in one or more other non-module symbols? |
| `isPrimitive` | Is this symbol a primitive type? `!isAggregate && !isArray` |
| `isBuiltin` | Is this symbol not an aggregate? |
| `hasDepth` | Does this symbol have any depth? Pointers and arrays. |
| `hasBody` | Is this symbol a function with instructions? |
| `hasDataAllocations` | Does this symbol have any variables contained within it? |
| `isEnum` | Is this symbol an enum? All members must be `const` and the type must be a `tagged`. |
| `getOverloads(string)` | Get all overloads of the given function by name in this symbol. |
| `getChild(string)` | Gets the child of this symbol with the given name. |
| `getParent(string)` | Gets the parent of this symbol with the given name. |
| `getAttribute(string)` | Gets the attribute of this symbol with the given name. |
| `getField(string)` | Gets the field of this symbol with the given name. |
| `getFunction(string)` | Gets the function of this symbol with the given name. |
| `getInherit(string)` | Gets the inherit of this symbol with the given name. |
| `getAlias(string)` | Gets the child alias of this symbol with the given name. |
| `hasChild(string)` | Does this symbol have a child with the given name? |
| `hasParent(string)` | Does this symbol have a parent with the given name? |
| `hasAttribute(string)` | Does this symbol have an attribute with the given name? |
| `hasField(string)` | Does this symbol have a field with the given name? |
| `hasFunction(string)` | Does this symbol have a function with the given name? |
| `hasInherit(string)` | Does this symbol have an inherit with the given name? |
| `hasAlias(string)` | Does this symbol have a child alias with the given name? |
| `hasChild(Symbol)` | Is the given symbol a child of this symbol? |
| `hasParent(Symbol)` | Is the given symbol a parent of this symbol? |
| `hasAttribute(Symbol)` | Is the given symbol an attribute of this symbol? |
| `hasField(Symbol)` | Is the given symbol a field of this symbol? |
| `hasFunction(Symbol)` | Is the given symbol a function of this symbol? |
| `hasInherit(Symbol)` | Does this symbol inherit the given symbol? |
| `hasAlias(Symbol)` | Is the given symbol an alias child of this symbol? |

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
    Symbol[string] attributes;

    string identifier;
    Symbol parent;
    # May not be accessed on primitive types, as there is no module available.
    Module module;
]
```

```
Type : Symbol [
    Type[string] inherits;
    Variable[] fields;
    Function[] functions;
    ubyte[] data;
    size_t size;
    size_t align;
    # For pointer and arrays, how deeply nested they are.
    uint depth;

    string type;
]
```

```
# This is also used for delegates, lambdas, ctors, dtors, and unittests.
Function : Symbol [
    Variable[] parameters;
    # This will include the return and parameters as the first locals.
    Variable[string] locals;
    size_t align;

    Symbol return;
    string type;
]
```

```
# This is also used for locals, parameters, expressions, and literals.
Variable : Symbol [
    Type type;
    ubyte[] data;
    size_t size;
    size_t align;
    size_t offset;
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