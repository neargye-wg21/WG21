<style>
table, th, td {
  border: 1px solid black;
  border-collapse: collapse;
  text-align: left;
  padding: 10px;
  border-spacing: 0px;
}
</style>
Document number: P????R0  
Project: Programming Language C++  
Audience: EWG  

Daniil Goncharov <neargye@gmail.com>  
Antony Polukhin <antoshkka@gmail.com>

Date: 2023-12-21

# Type and constexpr of `__func__`

## I. Introduction and Motivation

This paper is intended to close the [CWG2362](https://github.com/cplusplus/papers/issues/1378) and [CWG1962](https://github.com/cplusplus/papers/issues/1375).

The definition of `__func__` in 9.5.1 [[dcl.fct.def.general]](https://eel.is/c++draft/dcl.fct.def.general#8) paragraph 8 is:
```cpp
  static const char __func__[] = "function-name";
```

CWG1962 asks that the type of func be `const char*`, but this can break the existing behavior. `sizeof(__func__)` will return pointer size rather than the size of the array. This breaks code that does compile time transformations of the `__func__` value and uses sizeof to compute the length of the function name.

CWG2362 asks asks for the ability to use func in a `constexpr` function.
Modern versions of the MSVС, Сlang and GСС already allow the use `__func__` in context expressions.

This code example is built on GCC 9.2 but not on GCC 7.2.
```cpp
int main () {
  // error: the value of __func__ is not usable in a constant expression
  constexpr char c = __func__[0];
}
```

This code example does not build on either GCC 9.2 or GCC 7.2 or GCC 13.2.
```cpp
int main () {
  // error: the value of 'myFunc' is not usable in a constant expression
  static const char myFunc[] = "main";
  constexpr char c = myFunc[0];
}
```

It is proposed to explicitly note the possibility of using a `__func__` in a contextual context.

## II. Design Decisions

For fix CWG2362 it is suggested to add a modifier `constexpr` to `__func__`.

CWG1962 It is suggested not to fix it so as not to break the current behavior.

## III. Proposed wording relative to N4917

All the additions to the Standard are marked with <font color='green'>green</font>.

### A. Modifications to "9.5.1 In general" [dcl.fct.def.general] paragraph 8

<pre>
static <font color='green'>constexpr</font> const char __func__[] = "function-name";
</pre>

## IV. Revision History

Revision 0:

* Initial proposal

## V. References

* [N4917] Working Draft, Standard for Programming Language C++. Available online at <https://github.com/cplusplus/draft/releases/download/n4917/n4917.pdf>