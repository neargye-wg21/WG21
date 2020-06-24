Document number: D????  
Project: Programming Language C++  
Audience: LEWGI, LEWG, LWG,  

Daniil Goncharov <neargye@gmail.com>  
Karaev Alexander <akaraevz@mail.ru>

Date: 2020-06-24

# Add Constexpr Modifiers to Functions to_chars and from_chars in \<charconv> Header

## I. Introduction and Motivation

There is currently no standard way to conversions between integers and strings and between floating-point numbers and strings in compile time.

`to_chars` and `from_chars` provide basic formatting and parsing functionality, locale-independent and no memory allocation, so they look like logical condats for constexpr string conversions. The paper proposes to make `to_chars` and `from_chars` functions usable in constexpr context.

Consider the simple example:

```cpp
// Maybe c++2b
int main() {
}
```

## II. Design Decisions

