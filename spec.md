# TurboScript Language Specification

Version 1.0

July, 2017

This Specification available under the Open Web Foundation Final Specification Agreement Version 1.0 ("OWF 1.0") as of October 1, 2012. The OWF 1.0 is available at http://www.openwebfoundation.org/legal/the-owf-1-0-agreements/owfa-1-0.

## Table of Contents

* [Introduction](#Introduction)
* [Basic Concepts](#Basic-concepts)
* [Lexical elements](#Lexical-elements)
    * [Comments](#Comments)
    * [Tokens](#Tokens)
    * [Semicolons](#Semicolons)
    * [Identifiers](#Identifiers)
    * [Keywords](#Keywords)
    * [Operators and Delimiters](#Operators-and-Delimiters)
    * [Integer literals](#Integer-literals)
    * [Floating­point literals](#Floating­point-literals)
    * [String literals](#String-literals)
* [Constants](#Constants)
* [Variables](#Variables)
* [Block scope variables](#Block-scope-variables)
* [Types](#Types)
    * [Boolean type](#Boolean-type)
    * [Numeric types](#Numeric-types)
    * [String type](#String-type)
    * [Array type](#Array-type)
    * [Struct type](#Struct-type)
    * [Class type](#Class-type)
    * [Pointer type](#Pointer-type)
    * [Function type](#Function-type)
    * [Interface type](#Interface-type)
    * [Channel type](#Channel-type)
    * [Generic types](#Generic-types)
    * [SIMD 128bit type](#SIMD-128bit-type)
* [Blocks](#Blocks)
* [Declarations and scope](#Declarations-and-scope)
* [Module](#Module)
    * [Import Declarations](#Import-Declarations)
    * [Declarations and Dynamic Linking](#Dynamic-Linking)


# <a name="Introduction"/> Introduction

TurboScript designed to fill missing features of JavaScript which offered by WebAssembly. TurboScript's primary goals is to leverage features of WebAssembly while progamming in JavaScript like language. This specification provide a complete picture of what TurboScript is and not. There is no intention to replace JavaScript or TypeScript with TurboScript. Initial version of language will concentrate on parallel programming and SIMD features targeting post-MVP WebAssembly. Since language mainly targeting parallel programming features, integration with WebGL Next and WebGPU are in radar. 

TurboScript intented to work closely with JavaScript therefore JavaScript is an integral part of TurboScript. TurboScript project can be composed of JavaScript and WebAssembly. Since JavaScript is a dyanamic language JavaScript modules in TurboScript can be without types but any intraction with TurboScript modules need to be strongly typed. That is any function exporting from JavaScript module need to be strongly typed. 

There are some difference between concept of JavaScript and TurboScript. 1) variables and members cannot be undefined therefore no `undefined` type. default value will be `null`. 2) TurboScript doesnot support closure functions. 3) TurboScript's runtime memory is managed by programmer, ie. there is no garbage collector in TurboScript. 

# <a name="Basic-concepts"/> Basic concepts

TurboScript stands for hassel free parallel programming in web using JavaScript dialect with additional type information on top of JavaScript syntax and parallel programming features like shared memory, context sharing workers, SIMD, channels etc... TurboScript may look like TypeScript but it is not TypeScript.

# <a name="Lexical-elements"> Lexical elements

## <a name="Comments"> Comments

Comments serve as program documentation. There are two forms:
1. Line comments start with the character sequence `//` and stop at the end of the line.
2. General comments start with the character sequence `/* and stop with the first subsequent
  character sequence */`.
A comment cannot start inside a string literal, or inside a comment. A general comment containing no newlines acts like a space. Any other comment acts like a newline.

## <a name="Tokens"> Tokens

Tokens form the vocabulary of the TurboScript language. There are four classes: identifiers, keywords, operators and delimiters, and literals. White space, formed from spaces (U+0020), horizontal tabs (U+0009), carriage returns (U+000D), and newlines (U+000A), is ignored except as it separates tokens that would otherwise combine into a single token. Also, a newline or end of file may trigger the insertion of a semicolon. While breaking the input into tokens, the next token is the longest sequence of characters that form a valid token.

## <a name="Semicolons"> Semicolons

The formal grammar uses semicolons `;` as terminators in a number of productions. TurboScript programs may omit most of these semicolons using the following two rules:
1. When the input is broken into tokens, a semicolon is automatically inserted into the token stream immediately after a line's final token if that token is
    * an identifier
    * an integer, floating­point or string literal
    * one of the keywords break, continue, fallthrough, or return 
    * one of the operators and delimiters ++, --, ), ], or }
2. To allow complex statements to occupy a single line, a semicolon may be omitted before a closing ")" or "}".
To reflect idiomatic use, code examples in this document elide semicolons using these rules.

## <a name="Identifiers"> Identifiers

Identifiers name program entities such as variables and types. An identifier is a sequence of one or more letters and digits. The first character in an identifier must be a letter.

## <a name="Keywords"> Keywords
The following keywords are reserved and may not be used as identifiers.

```typescript
break           case                catch               class  
const           continue            debugger            default
channel         delete              do                  else
enum            export              extends             false
finally         for                 function            if
import          in                  instanceof          new
null            return              super               switch
sizeof          select              struct              select
this            throw               true                try
typeof          var                 void                while
```
## <a name="Operators-and-Delimiters"> Operators and Delimiters

The following character sequences represent operators, delimiters, and other special tokens:

```typescript
 +      &       +=      &=      &&      ==      !=      (       )  
 -      |       -=      |=      ||      <       <=      [       ]
 *      ^       *=      ^=      <-      >       >=      {       }
 /      <<      /=      <<=     ++      =       :=      ,       ; 
 %      >>      %=      >>=     --      !       ...     .       :
        &^              &^=
```

## <a name="Integer-literals"> Integer literals

Integers can be expressed in decimal (base 10), hexadecimal (base 16), octal (base 8) and binary (base 2).

* Decimal integer literal consists of a sequence of digits without a leading 0 (zero).
* Leading 0 (zero) on an integer literal, or leading 0o (or 0O) indicates it is in octal. Octal integers can include only the digits 0-7.
* Leading 0x (or 0X) indicates hexadecimal. Hexadecimal integers can include digits (0-9) and the letters a-f and A-F.
* Leading 0b (or 0B) indicates binary. Binary integers can include digits only 0 and 1.

```typescript
0, 117 and -345 (decimal, base 10)
015, 0001 and -0o77 (octal, base 8) 
0x1123, 0x00111 and -0xF1A7 (hexadecimal, "hex" or base 16)
0b11, 0b0011 and -0b11 (binary, base 2)
```

## <a name="Floating­point-literals"> Floating­point literals

A floating­point literal is a decimal representation of a floating­point constant. It has an integer part, a decimal point, a fractional part, and an exponent part. The integer and fractional part comprise decimal digits; the exponent part is an e or E followed by an optionally signed decimal exponent. One of the integer part or the fractional part may be elided; one of the decimal point or the exponent may be elided.

```typescript
float_lit = decimals "." [ decimals ] [ exponent ] |
            decimals exponent |
            "." decimals [ exponent ] .
decimals  = decimal_digit { decimal_digit } .
exponent  = ( "e" | "E" ) [ "+" | "-" ] decimals .
```

## <a name="String-literals"> String literals

A string literal represents a string variable obtained from concatenating a sequence of characters. There are two forms: primitive string literals and template literals.

Template literals are string literals allowing embedded expressions. multi-line strings and string interpolation features are possible with them

## <a name="Constants"> Constants

Constants are block-scoped, much like variables defined using the let statement. The value of a constant cannot change through re-assignment, and it can't be redeclared.

## <a name="Variables"> Variables

The `var` statement declares a variable, optionally initializing it to a value.

```typescript
var varname1 [= value1] [, varname2 [= value2] ... [, varnameN [= valueN]]];
```

## <a name="Block-scope-variables"> Block scope variables

The `let` statement declares a block scope local variable, optionally initializing it to a value.

```typescript
let var1 [= value1] [, var2 [= value2]] [, ..., varN [= valueN]];
```

# <a name="Types"> Types

A type determines the set of values and operations specific to values of that type. Types may be named or unnamed. Named types are specified by a (possibly qualified) type name; unnamed types are specified using a type literal, which composes a new type from existing types.

## <a name="Boolean-type"> Boolean type

A boolean type represents the set of Boolean truth values denoted by the predeclared constants true and false. The predeclared boolean type is boolean.

## <a name="Numeric-types"> Numeric types

Type      | Alias | Description
----------|-------------|-------------
`i8`   | `int8`, `sbyte`         | signed  8-bit integers (-128 to 127)
`u8`    | `uint8`, `byte`         | unsigned  8-bit integers (0 to 255)
`i16`   | `int16`, `short`         | signed 16-bit integers (-32768 to 32767)
`u16`  | `uint16`, `ushort`         | unsigned 16-bit integers (0 to 65535)
`i32`     | `int32`, `int`         | signed 32-bit integers (-2147483648 to 2147483647)
`u32`    | `uint32`, `uint`         | unsigned 32-bit integers (0 to 4294967295)
`i64`    | `int64`, `long`         | signed 64-bit integers (-9223372036854775808 to 9223372036854775807)
`u64`   | `uint64`, `ulong`         | unsigned 64-bit integers (0 to 18446744073709551615)
`f32`   | `float32`, `float`         | IEEE-754 32-bit floating-point numbers
`f64`  | `float64`, `double`         | IEEE-754 64-bit floating-point numbers

## <a name="String-type"> String type
A string type represents the set of string values. A string value is a (possibly empty) sequence of bytes. The predeclared string type is string. The length of a string `s` can be discovered using the built­in instance getter `s.length`. The length is a compile­time constant if the string is a constant. A string's elements can be accessed by integer indices 0 through s.length - 1. It is illegal to take the address of such an element; if s[i] is the i'th byte of a string, &s[i] is invalid.

## <a name="Array-type"> Array type
An array is a numbered sequence of elements of a single type, called the element type. The number of elements is called the length and is never negative.
The length is part of the array's type; it must evaluate to a non­negative constant representable by a value of type `int64`. The length of array `a` can be discovered using the instance getter `a.length`. The elements can be addressed by integer indices 0 through `a.length -1`. Array types are always one­ dimensional but may be composed to form multi­dimensional types.

## <a name="Struct-type"> Struct type

A struct is a sequence of named elements, called fields, each of which has a name and a type. Field names must be specified explicitly (IdentifierList). Within a struct, field names must be unique.

```typescript
struct GenericRobot {
    readonly name = "GENERIC_ROBOT"
    readonly id = 1
    serialNumber:uint32
}
```
```typescript
struct Dog {
    GenericRobot
    name = "DOG"
}
```

## <a name="Class-type"> Class type

A class is similar to struct, inside class methods can be defined along with fields.

## <a name="Pointer-type"> Pointer type
A pointer type denotes the set of all pointers to variables of a given type, called the base type of the pointer. The value of an uninitialized pointer is `null`.

## <a name="Function-type"> Function type

A function type denotes the set of all functions with the same parameter and result types. The value of an uninitialized variable of function type is `null`.

## <a name="Interface-type"> Interface type

An interface specifies properties and method set of an unknown implementation. implementations can be completely isolated, invoking unimplemented methods or properties raise compile time error.

```typescript
interface Robot {
    name:string
    id:uint32
    serialNumber:uint32

    wake()
    eat()
    sleep()
    walk()
    run()
    reproduce()
    die()
}
```

## <a name="Channel-type"> Channel type

A channel provides a mechanism for concurrently executing functions to communicate by sending and receiving values of a specified element type. The value of an uninitialized channel is `null`.

```
ChannelType = ( "chan" | "chan" "<-" | "<-" "chan" ) ElementType .
```

The optional <- operator specifies the channel direction, send or receive. If no direction is given, the channel is bidirectional. A channel may be constrained only to send or only to receive by conversion or assignment.

```typescript
chan T          // can be used to send and receive values of type T
chan<- float64  // can only be used to send float64s
<-chan int      // can only be used to receive ints
```

The <- operator associates with the leftmost chan possible:

```typescript
chan<- chan int    // same as chan<- (chan int)
chan<- <-chan int  // same as chan<- (<-chan int)
<-chan <-chan int  // same as <-chan (<-chan int)
chan (<-chan int)
```

A new, initialized channel value can be made using the new keyword, which takes the channel type and an optional capacity as arguments:

```typescript
class RGBA {
    constructor(
        public r:float64,
        public g:float64,
        public b:float64,
        public a:float64){
    }
}
channel color:RGBA4 = new channel<RGBA>(4)
let rgba = new RGBA(1.0, 1.0, 1.0, 1.0)
console.log(rgba)
color <- rgba
console.log(rgba)
rgba <- color
console.log(rgba)
```

## <a name="Generic-types"/> Generic types

```typescript
class Foo<T> {
    value:T;
    constructor(value:T){
        this.value = value;
    }
    getValue():T {
        return this.value;
    }
}

export function testI32(value:int32):int32 {
    let instance = new Foo<int32>(value);
    return instance.getValue();
}

export function testI64(value:int32):int32 {
    let value2 = value as int64;
    let instance = new Foo<int64>(value2);
    return instance.getValue() as int32;
}

export function testF32(value:float32):float32 {
    let instance = new Foo<float32>(value);
    return instance.getValue();
}

export function testF64(value:float64):float64 {
    let instance = new Foo<float64>(value);
    return instance.getValue();
}
```

## <a name="SIMD-128bit-type"> SIMD 128bit type

```typescript
let int8_x16:simd128<int8>;
let int16_x8:simd128<int16>;
let int32_x4:simd128<int32>;
let int64_x2:simd128<int64>;
let float32_x4:simd128<float32>;
let float64_x2:simd128<float64>;
```

```typescript
class RGBA_x2 {
    constructor(
        public r:simd128<float64> = new simd128<float64>()
        public g:simd128<float64> = new simd128<float64>()
        public b:simd128<float64> = new simd128<float64>()
        public a:simd128<float64> = new simd128<float64>()){       
    }
}
let rgba = new RGBA_x2();
console.log(rgba)   // {r:[0.0, 0.0], g:[0.0, 0.0], b:[0.0, 0.0], a:[0.0, 0.0]}

class RGBA32_x4 {
    constructor(
        public r:simd128<float32> = new simd128<float32>()
        public g:simd128<float32> = new simd128<float32>()
        public b:simd128<float32> = new simd128<float32>()
        public a:simd128<float32> = new simd128<float32>()){       
    }
}
let rgba = new RGBA32_x4();
console.log(rgba)   // {r:[0.0, 0.0, 0.0, 0.0], g:[0.0, 0.0, 0.0, 0.0], b:[0.0, 0.0, 0.0, 0.0], a:[0.0, 0.0, 0.0, 0.0]}
```

# <a name="Blocks"> Blocks

A block is a possibly empty sequence of declarations and statements within matching brace brackets.
```
Block = "{" StatementList "}" .
StatementList = { Statement ";" } .
```
In addition to explicit blocks in the source code, there are implicit blocks:

1. Each "if", "for", and "switch" statement is considered to be in its own implicit block. 
2. Each clause in a "switch" or "select" statement acts as an implicit block.

# <a name="Declarations-and-scope"> Declarations and scope

A declaration binds identifier to a constant, type, variable, function, label, or package. Every identifier in a program must be declared. No identifier may be declared twice in the same block.

# <a name="Label-scopes"> Label scopes

Labels are declared by labeled statements and are used in the "break", "continue", and "goto" statements. It is illegal to define a label that is never used. In contrast to other identifiers, labels are not block scoped and do not conflict with identifiers that are not labels. The scope of a label is the body of the function in which it is declared and excludes the body of any nested function.

# <a name="Predeclared-identifiers"> Predeclared identifiers

The following identifiers are implicitly declared in the global block:

```
Types:
        boolean byte simd128 error float32 float64
        int int8 int16 int32 int64 string
        uint uint8 uint16 uint32 uint64 uintptr
Constants:
        true false
Zero value:
        null
Functions:
        alignof instanceof new sizeof typeof  
```

# <a name="Uniqueness-of-identifiers"> Uniqueness of identifiers

Given a set of identifiers, an identifier is called unique if it is different from every other in the set. Two identifiers are different if they are spelled differently, or if they appear in different modules. Otherwise, they are the same.

# <a name="Constant-declarations"> Constant declarations

A constant declaration binds identifiers (the names of the constants) to the values of constant expressions.

```
const tinyDouble = 1.0
const tinyInt = 1
const longFloat:float32 = 64912.8273, logInt:int64 = 8915238172653
```

# <a name="Type-declarations"> Type declarations

A type declaration binds an identifier, the type name, to a new type that has the same underlying type as an existing type, and operations defined for the existing type are also defined for the new type. The new type is different from the existing type.

# <a name="Variable-declarations"> Variable declarations

A variable declaration creates one or more variables, binds corresponding identifiers to them, and gives each a type and an initial value.

# <a name="Function-declarations"> Function declarations

A function declaration binds an identifier, the function name, to a function.
```
FunctionDecl = "function" FunctionName ( Function | Signature )
FunctionName = identifier
Function     = Signature FunctionBody
FunctionBody = Block
```

# <a name="Method-declarations"> Method declarations

A method is a function with a receiver. A method declaration binds an identifier, the method name, to a method, and associates the method with the receiver's base type.
```
MethodDecl   = "function" Receiver MethodName ( Function | Signature )
Receiver     = Parameters
```
# <a name="Class-declarations"> Class declarations

A class is combinations of struct and methods.

# <a name="Expressions"> Expressions

An expression specifies the computation of a value by applying operators and functions to operands.

# <a name="Operands"> Operands

# <a name="Qualified-identifiers"> Qualified identifiers

# <a name="Function-literals"> Function literals

# <a name="Primary-expressions"> Primary expressions

# <a name="Selectors"> Selectors

# <a name="Method-expressions"> Method expressions

# <a name="Method-values"> Method values

# <a name="Index-expressions"> Index expressions

# <a name="Calls"> Calls

# <a name="Operators"> Operators

# <a name="Arithmetic-operators"> Arithmetic operators

# <a name="Comparison-operators"> Comparison operators

# <a name="Logical-operators"> Logical operators

# <a name="Address-operators"> Address operators

# <a name="Receive-operator"> Receive operator

# <a name="Conversions"> Conversions

# <a name="Constant-expressions"> Constant expressions

# <a name="Order-of-evaluation"> Order of evaluation

# <a name="Statements"> Statements

# <a name="Terminating-statements"> Terminating statements

# <a name="Empty-statements"> Empty statements

# <a name="Labeled-statements"> Labeled statements

# <a name="Expression-statements"> Expression statements

# <a name="Send-statements"> Send statements

# <a name="Send-statements"> Send statements

# <a name="IncDec-statements"> IncDec statements

# <a name="Assignments"> Assignments

# <a name="If-statements"> If statements

# <a name="Switch-statements"> Switch statements

# <a name="For-statements"> For statements

# <a name="Worker-statements"> Worker statements

# <a name="Select-statements"> Select statements

# <a name="Return-statements"> Return statements

# <a name="Break-statements"> Break statements

# <a name="Continue-statements"> Continue statements

# <a name="Goto-statements"> Goto statements

# <a name="Built­in-functions"> Built­in functions


# <a name="Module"/> Module
Modules are fundamental components of TurboScript. Each TurboScript file is a module and modules can be combined to single wasm module. Worker modules always compiled to separate .wasm modules.

```typescript
// main.tbs
from "sevenzip" import {SevenZipReader} 
var sevenZipReader:SevenZipReader;

start function main():void{
    sevenZipReader = new SevenZipReader();
}
```

```typescript
// sevenzip.tbs
export class SevenZipReader {
    constructor(){

    }
}
```

# <a name="3.1">3.1 Import Declarations
Import declarations are used to import entities from other modules and provide bindings for them in the current module.

An import declaration of the form
```typescript
from "./components/awesome-component" import * as awesome
```
imports the module with the given name and creates a local binding for the module itself. The local binding is classified as a value (representing the module instance) and a namespace (representing a container of types and namespaces).

An import declaration of the form
```typescript
from "./module" import {_funcName, _className, _varName}
```
imports a given module and creates local bindings for a specified list of exported members of the module. The specified names must each reference an entity in the export member set of the given module. The local bindings have the same names and classifications as the entities they represent unless as clauses are used to that specify different local names:
```typescript
from "./module" import {_funcName as a, _className as b, _varName as c}
```

An import declaration of the form
```typescript
from "./components/awesome-component" import awesome
```
is exactly equivalent to the import declaration
```typescript
from "./components/awesome-component" import {default as awesome}
```

# <a name="3.2"/>3.2 Declarations and Dynamic Linking
Declarations defines symbols in the AST without implementations details. 
These symbols must be dynamically linked at runtime.
```typescript
declare module console {
    function log(value:int32):void
}

declare function test(){
}

declare class CommonObject {
    constructor(){}
}

start function main() {
    let obj = new CommonObject();
}
```

Above declaration will import `{ console: { log: value => {} } }` to WebAssembly
implementation of the import is up to the user.

# <a name="5">5 Literals

# <a name="5.1">5.1 Numeric literals

```typescript
// Integers literals
let _uint = 1; //default integer number type is uint32
let _int = -1; //default negative integer number type is int32
let _int64 = 1l //number+l represent 64bit integers
let _int64 = -1l //-number+l represent 64bit unsigned integers

//Floating point literals
let _float64 = 1.0 //default floating point number type is float64
let _float32 = 1.0f //number.fraction+f represent 32bit floating point numbers
```

# <a name="9"/>9 Class

##### Example 1
```typescript
class GenericRobot {
    
    name = "GENERIC_ROBOT"
    id = 1

    constructor(public serialNumber:uint32){
    }

    wake(){/*implementation*/}
    eat(){/*implementation*/}
    sleep(){/*implementation*/}
    walk(){/*implementation*/}
    run(){/*implementation*/}
    reproduce(){/*implementation*/}
    die(){/*implementation*/}
}

class Dog extends GenericRobot {
    name = "DOG"
    bark(){/*implementation*/}
}

class Bird extends GenericRobot {
    name = "BIRD"
    fly(){/*implementation*/}
}

function main() {
    let robots = [
        new GenericRobot(0),
        new Dog(1),
        new Bird(2),
    ]

    robots.forEach(robot:Robot => {
        robot.wake();
        console.log(robot.name);
        console.log(robot.id);
        console.log(robot.serialNumber);
        robot.sleep();
    });
}
```

##### Example 2
```typescript
//common.wasm
class Ray {
    origin:Vector3D
    direction:Vector3D
}
interface Shape {
    bbox:Box
    intersect(r:Ray):HitInfo
}
```
```typescript
//intersectable.wasm
class Triangle {
    bbox:Box
    points:Vector3D[3]
    normals:Vector3D[3]
    textureCoord:Vector2D[3]
    intersect(r:Ray):HitInfo{/*implementation*/}
}
class Cube {
    bbox:Box
    min:Vector3D
    max:Vector3D
    intersect(r:Ray):HitInfo{/*implementation*/}
}
class Sphere {
    bbox:Box
    radius:float64
    center:Vector3D
    intersect(r:Ray):HitInfo{/*implementation*/}
}
```

```typescript
//intersector.wasm
function intersect(thing:Shape, ray:Ray):HitInfo {
    return thing.intersect(ray)
}
```

```typescript
//main.wasm
declare class Triangle {}
declare function intersect(thing:Shape, ray:Ray):HitInfo

function main() {
    let shapes:Shape[] = [
        new Triangle,
        new Sphere,
        new Cube
    ]
    let ray = new Ray()
    shapes.forEach(shape => intersect(shape, r))
}
```

# <a name="13"/>13 Worker

```typescript
function filterKernel(in:float64[], out:float64[], start:int32, end:int32) {
    for(let i:int32 = start; i < end; i++){
        out[i] = in[i];
    }
}

async function applyFilter(in:RGBA[]): RGBA[] {
    let length = in.length;
    let numCpu = process.NUM_CPU;
    let blockSize = length / process.NUM_CPU;
    let ch = new RGBA[](length);
    channel color:RGBA4 = new channel<i32>(numCpu)
    for(let i = 0; i < numCpu; i++) {
        let start = i * blockSize;
        worker filterKernel(in, out, start, start + blockSize);
    }
    for (i = 0; i < numCpu; i++) {
        <-ch
        console.log('Show some progress...')
    }
    return out;
}

class RGBA {
    constructor(
        public r:float64 = 0.0,
        public g:float64 = 0.0,
        public b:float64 = 0.0,
        public a:float64 = 0.0){
    }
}

async function main() {
    let i = 0;
    let colors = new RGBA[](1024 * 1024);
    let result = await applyFilter(colors);
}
```
