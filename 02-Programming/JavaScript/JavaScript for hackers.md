# The basics
- ### Hexadecimal 
	-  just for Strings
	- Example
```js
'\x61'//a
"\x61"//a
`\x61`//a
function a(){}
\x61()//fails
```
- call the function because hex escapes <span style="color: red;">aren`t</span> allowed there
- ## Unicode
```javascript
'\u0061'//a
"\u0061"//a f
`\u0061`//a
function a(){}
\u0061()//correctly calls the function
```
- Some important points about this form of Unicode escapes is you must
- Specify four hexadecimal characters, for example \u61 is not allowed.
- Can use them in strings and identifiers, but unlike standard Unicode escapes you are not restricted to four hexadecimal characters. To use these Unicode escapes you use \u{} inside the curly braces you, specify a hex Unicode code point.
```javascript
'\u{61}'//a
"\u{000000000061}"//a
`\u{0061}`//a
function a(){}
\u{61}()//correctly calls the function
\u{3134a}=123//unicode character "3134a" is allowed as a variable
```
## Octal
- Octal escapes use base 8 and can only be used strings. There is no prefix with octal escapes; yousimply use a backslash followed by a base 8 number. If you try to use a number outside the octal ==range the JavaScript engine will just return the number==
```js
'\141'//a
"\8"//number outside the octal range so 8 is returned
function a(){
	alert(1)
}
'\141'() // not work
```
- <span style="color: red;">Octal escapes aren’t allowed in template strings.</span>
## Eval and escape
```js 
// Using a hex escape with an eval call
eval('\x61=123')//a = 123
// Using a unicode escapes with an eval call
eval('\\u0061=123')

	eval('\\u0061=123')//unicode escape using "a" assignment
eval('\\u\x30061=123')//hex encode the first zero
eval('\\u\x300\661=123')//octal encode the 6
```
## Strings
- There are three forms of strings, you have **single quoted, double-quoted and template strings**
```js
// Single character escape sequences

'\b'//backspace
'\f'//form feed
'\n'//new line
'\r'//carriage return
'\t'//tab
'\v'//vertical tab
'\0'//null
'\''//single quote
'\"'//double quote
'\\'//backslash
```
- You can also escape any character that isn’t part of an escape sequence, and they are treated as the actual character, for example
```javascript
"\H\E\L\L\O"//HELLO

// Interestingly you can use a backslash at the end of line to continue it onto the next line
'salar khalili \
in ha dar khat chap mishe'

// Using backslash in property names
let foo = {
'bar \
': "baz"
};
```
- Double and single qoute does not support  multi-Line with out \
```js
x = `a\
b\
c`;
x==='abc'//true
x = `a
b
c`;
x==='abc 
//false
```
-  Using placeholder expressions in template strings
```js
`${7*7}`
// Nested template string expressions!
`${`${`${`${7*7}`}`}`}`//49
```
## call and apply 
- Call is a property of every function that allows you to call it and change the “this value” of the function in the first argument and any subsequent arguments are passed to that function. For example:
```js
function x(){
console.log(this.bar);//baz
}
let foo={bar:"baz"}
x.call(foo);

//Using call() with null
function x() {
console.log(arguments[0]);//1
console.log(arguments[1]);//2
console.log(this);//[object Window]
}
x.call(null, 1, 2)
```
- If you use the “use strict” directive “this” will be null instead of
window:
```js
function x() {
"use strict";
console.log(arguments[0]);//1
console.log(arguments[1]);//2
console.log(this);//null
}
x.call(null, 1, 2)
```
- The apply function is pretty much exactly the same as the call function with one important difference, you can supply an **array** of arguments in the second argument
```js
//Using apply() with null
function x() {
console.log(arguments[0]);//1
console.log(arguments[1]);//2
console.log(this);//[object Window]
}
x.apply(null, [1, 2])
```
# JavaScript without parentheses
- ##  Calling functions without parentheses
- can use valueOf when you want a particular object to return a primitive value such as a number. Generally you’d use it with an object literal to make your object interact with other primitives to perhaps perform addition or subtraction
```javascript
let obj = {valueOf(){return 1}};
obj+1//2

```