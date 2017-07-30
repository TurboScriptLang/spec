# TurboScript Language Specification

Version 1.0

July, 2017

This Specification available under the Open Web Foundation Final Specification Agreement Version 1.0 ("OWF 1.0") as of October 1, 2012. The OWF 1.0 is available at http://www.openwebfoundation.org/legal/the-owf-1-0-agreements/owfa-1-0.

## Table of Contents

* [1 Introduction](#1)
* [2 Basic Concepts](#2)
* [3 Module](#3)
    * [3.1 Import Declarations](#3.1)
    * [3.2 Declarations and Dynamic Linking](#3.2)
* [4 Types](#4)
    * [4.1 Numeric Types](#4.1)
* [5 Literals](#5)
    * [5.1 Numeric Literals](#5.1)
* [6 Interface](#6)
* [7 Struct](#7)
* [8 Receving functions](#8)
* [9 Class](#9)
* [11 SIMD (128bit)](#11)
* [13 Worker](#13)


# <a name="1"/>1 Introduction

TurboScript designed to fill missing featrures of JavaScript which offered by WebAssembly. TurboScript's primary goals is to leverage features of WebAssembly while progamming in JavaScript like language. This specification provide a complete picture of what TurboScript is and not. There is no intention to replace JavaScript or TypeScript with TurboScript. Initial version of language will concentrate on parallel programming and SIMD features targeting post-MVP WebAssembly. Since language mainly targeting parallel programming features, integration with WebGL Next and WebGPU are in radar. 

TurboScript intented to work closely with JavaScript therefore JavaScript is an integral part of TurboScript. TurboScript project can be composed of JavaScript and WebAssembly. Since JavaScript is a dyanamic language JavaScript modules in TurboScript can be without types but any intraction with TurboScript modules need to be strongly typed. That is any function exporting from JavaScript module need to be strongly typed. 

There are some difference between concept of JavaScript and TurboScript. 1) variables and members cannot be undefined therefore no `undefined` type. default value will be `null`. 2) TurboScript doesnot support closure functions. 3) TurboScript's runtime memory is managed by programmer, ie. there is no garbage collector in TurboScript. 

# <a name="2"/>2 Basic Concepts

TurboScript stands for hassel free parallel programming in web using JavaScript dialect with additional type information on top of JavaScript syntax and parallel programming features like shared memory, context sharing workers, SIMD, channels etc... TurboScript may look like TypeScript but it is not TypeScript.

# <a name="Lexical-elements"> Lexical elements

# <a name="Comments"> Comments
# <a name="Tokens"> Tokens
# <a name="Semicolons"> Semicolons
# <a name="Identifiers"> Identifiers

# <a name="Keywords"> Keywords
The following keywords are reserved and may not be used as identifiers.

```TypeScript
break             case              catch             class  
const             continue          debugger          default  
delete            do                else              enum  
export            extends           false             finally  
for               function          if                import  
in                instanceof        new               null  
return            super             switch            this  
throw             true              try               typeof  
var               void              while             with
struct            channel           select
```
# <a name="Operators and Delimiters"> Operators and Delimiters

# <a name="Integer literals"> Integer literals

# <a name="Floating­point literals"> Floating­point literals

# <a name="Rune literals"> Rune literals

# <a name="String literals"> String literals

# <a name="Constants"> Constants

# <a name="Variables"> Variables

# <a name="Types"> Types

# <a name="Method sets"> Method sets

# <a name="Boolean types"> Boolean types

# <a name="String types"> String types

# <a name="Array types"> Array types

# <a name="Struct types"> Struct types

# <a name="Class types"> Class types

# <a name="Pointer types"> Pointer types

# <a name="Function types"> Function types

# <a name="Interface types"> Interface types

# <a name="Channel types"> Channel types

# <a name="Blocks"> Blocks

# <a name="Declarations and scope"> Declarations and scope

# <a name="Label scopes"> Label scopes

# <a name="Predeclared identifiers"> Predeclared identifiers

# <a name="Exported identifiers"> Exported identifiers

# <a name="Uniqueness of identifiers"> Uniqueness of identifiers

# <a name="Constant declarations"> Constant declarations

# <a name="Type declarations"> Type declarations

# <a name="Variable declarations"> Variable declarations

# <a name="Function declarations"> Function declarations

# <a name="Method declarations"> Method declarations

# <a name="Class declarations"> Class declarations

# <a name="Expressions"> Expressions

# <a name="Operands"> Operands

# <a name="Qualified identifiers"> Qualified identifiers

# <a name="Function literals"> Function literals

# <a name="Primary expressions"> Primary expressions

# <a name="Selectors"> Selectors

# <a name="Method expressions"> Method expressions

# <a name="Method values"> Method values

# <a name="Index expressions"> Index expressions

# <a name="Calls"> Calls

# <a name="Operators"> Operators

# <a name="Arithmetic operators"> Arithmetic operators

# <a name="Comparison operators"> Comparison operators

# <a name="Logical operators"> Logical operators

# <a name="Address operators"> Address operators

# <a name="Receive operator"> Receive operator

# <a name="Conversions"> Conversions

# <a name="Constant expressions"> Constant expressions

# <a name="Order of evaluation"> Order of evaluation

# <a name="Statements"> Statements

# <a name="Terminating statements"> Terminating statements

# <a name="Empty statements"> Empty statements

# <a name="Labeled statements"> Labeled statements

# <a name="Expression statements"> Expression statements

# <a name="Send statements"> Send statements

# <a name="Send statements"> Send statements

# <a name="IncDec statements"> IncDec statements

# <a name="Assignments"> Assignments

# <a name="If statements"> If statements

# <a name="Switch statements"> Switch statements

# <a name="For statements"> For statements

# <a name="Worker statements"> Worker statements

# <a name="Select statements"> Select statements

# <a name="Return statements"> Return statements

# <a name="Break statements"> Break statements

# <a name="Continue statements"> Continue statements

# <a name="Goto statements"> Goto statements

# <a name="Built­in functions"> Built­in functions


# <a name="3"/>3 Module
Modules are fundamental components of TurboScript. Each TurboScript file is a module and modules can be combined to single wasm module. Worker modules always compiled to separate .wasm modules.

```turboscript
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

# <a name="4">4 Types

# <a name="4.1">4.1 Numeric types

Type      | Alias | Description
----------|-------------|-------------
`int8`   | sbyte         | signed  8-bit integers (-128 to 127)
`uint8`    | byte         | unsigned  8-bit integers (0 to 255)
`int16`   | short         | signed 16-bit integers (-32768 to 32767)
`uint16`  | ushort         | unsigned 16-bit integers (0 to 65535)
`int32`     | int         | signed 32-bit integers (-2147483648 to 2147483647)
`uint32`    | uint         | unsigned 32-bit integers (0 to 4294967295)
`int64`    | long         | signed 64-bit integers (-9223372036854775808 to 9223372036854775807)
`uint64`   | ulong         | unsigned 64-bit integers (0 to 18446744073709551615)
`float32`   | float         | IEEE-754 32-bit floating-point numbers
`float64`  | double         | IEEE-754 64-bit floating-point numbers

# <a name="4.2">4.2 Array type
An array is a numbered sequence of elements of a single type, called the element type. The number of elements is called the length and is never negative.
The length is part of the array's type; it must evaluate to a non­negative constant representable by a value of type `int64`. The length of array `a` can be discovered using the instance getter `a.length`. The elements can be addressed by integer indices 0 through `a.length -1`. Array types are always one­ dimensional but may be composed to form multi­dimensional types.

# <a name="4.3">4.3 Pointer type
A pointer type denotes the set of all pointers to variables of a given type, called the base type of the pointer. The value of an uninitialized pointer is `null`.

# <a name="4.4">4.3 String type
A string type represents the set of string values. A string value is a (possibly empty) sequence of bytes. The predeclared string type is string. The length of a string `s` can be discovered using the built­in instance getter `s.length`. The length is a compile­time constant if the string is a constant. A string's elements can be accessed by integer indices 0 through s.length - 1. It is illegal to take the address of such an element; if s[i] is the i'th byte of a string, &s[i] is invalid.

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

# <a name="6"/>6 Interface

An interface specifies properties and method set of an unknown implementation. implementations can be completely isolated, invoking unimplemented methods or properties raise compiler time error.

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
# <a name="7"/>7 Struct
```typescript
struct GenericRobot {
    readonly name = "GENERIC_ROBOT"
    readonly id = 1
    serialNumber:uint32
}
```
Semicolumns are accepted
```typescript
struct GenericRobot {readonly name = "GENERIC_ROBOT";readonly id = 1;required serialNumber:uint32;}
```
```typescript
struct Dog {
    GenericRobot
    name = "DOG"
}
```
# <a name="8"/>8 Receving functions
```typescript
function [this:Dog] bark() {/*implementation*/}
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

# <a name="10"/>10 Generic Types and Functions

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

# <a name="11"/>11 SIMD (128bit)
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

# <a name="12"/>12 Channel
```typescript
class RGBA {
    constructor(
        public r:float64,
        public g:float64,
        public b:float64,
        public a:float64){       
    }
}
channel color:RGBA4 = new channel<RGBA>
let rgba = new RGBA(1.0, 1.0, 1.0, 1.0);
console.log(rgba)
color <- rgba
console.log(rgba)
rgba <- color
console.log(rgba)
```

# <a name="13"/>13 Worker

```typescript
worker {
    export function filterKernel(in:float64[], out:float64[], start:int32, end:int32) {
        for(let i:int32 = start; i < end; i++){
            out[i] = in[i];
        }
    }
}

async function applyFilter(in:RGBA[]): RGBA[] {
    let length = in.length;
    let numCpu = process.NUM_CPU;
    let blockSize = length / process.NUM_CPU;
    let out = new RGBA[](length);
    for(let i = 0; i < numCpu; i++) {
        let start = i * blockSize;
        worker filterKernel(in, out, start, start + blockSize);
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
