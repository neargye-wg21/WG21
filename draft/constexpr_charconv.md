Document number: D????  
Project: Programming Language C++  
Audience: LEWGI, LEWG, LWG,  

Daniil Goncharov <neargye@gmail.com>  
Karaev Alexander <akaraevz@mail.ru>

Date: 2020-06-24

# Add Constexpr Modifiers to Functions to_chars and from_chars in \<charconv> Header

## I. Introduction and Motivation

There is currently no standard way to conversions between integers and strings and between floating-point numbers and strings in compile time.

`to_chars` and `from_chars` provide basic formatting and parsing functionality, locale-independent, no-throws and no memory allocation, so they look like logical candidate for constexpr string conversions. The paper proposes to make `to_chars` and `from_chars` functions usable in constexpr context.

Consider the simple example:

```cpp
// Maybe c++2b
int main() {
}
```

## II. Design Decisions

The discussion is based on the implementation of `to_chars` and `from_chars` from [Microsoft/STL](https://github.com/microsoft/STL), because it implements the algorithm integers and floating-point numbers.

Based on [Microsoft/STL](https://github.com/microsoft/STL) version sience 2020-01-09 commit <https://github.com/microsoft/STL/commit/7ad0d63987668a91538408d120d6a2699f6a2abe>.

### Integers

During testing, the possibility of implementing the following changes were made to the original algorithm:

* `std::memcpy` will replased to copy in a loop. To maintain performance in a real implementation, can be used `std::char_traits<char>::copy`.
* Some variables needed to be initialized by default. In C++20 we adopted [P1331](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2019/p1331r2.pdf), which eliminates these changes.

### Floating-point

// TODO:

## III. Conclusions

`to_chars` and `from_chars` are the basic building block for formatting, mark them `constexpr` we will add a standard way to work with formatting in compile time.

## IV. Proposed Changes relative to N4861

All the additions to the Standard are marked with <font color='green'>green</font>.

### A. Modifications to "20.19.1 Header \<charconv> synopsis" [charconv.syn]

<font color='green'>constexpr<font> to_chars_result to_chars(char* first, char* last, see below value, int base = 10);
<font color='green'>constexpr<font> to_chars_result to_chars(char* first, char* last, bool value, int base = 10) = delete;
<font color='green'>constexpr<font> to_chars_result to_chars(char* first, char* last, float value);
<font color='green'>constexpr<font> to_chars_result to_chars(char* first, char* last, double value);
<font color='green'>constexpr<font> to_chars_result to_chars(char* first, char* last, long double value);
<font color='green'>constexpr<font> to_chars_result to_chars(char* first, char* last, float value, chars_format fmt);
<font color='green'>constexpr<font> to_chars_result to_chars(char* first, char* last, double value, chars_format fmt);
<font color='green'>constexpr<font> to_chars_result to_chars(char* first, char* last, long double value, chars_format fmt);
<font color='green'>constexpr<font> to_chars_result to_chars(char* first, char* last, float value, chars_format fmt, int precision);
<font color='green'>constexpr<font> to_chars_result to_chars(char* first, char* last, double value, chars_format fmt, int precision);
<font color='green'>constexpr<font> to_chars_result to_chars(char* first, char* last, long double value, chars_format fmt, int precision);

<font color='green'>constexpr<font> from_chars_result from_chars(const char* first, const char* last, see below & value, int base = 10);
<font color='green'>constexpr<font> from_chars_result from_chars(const char* first, const char* last, float& value, chars_format fmt = chars_format::general);
<font color='green'>constexpr<font> from_chars_result from_chars(const char* first, const char* last, double& value, chars_format fmt = chars_format::general);
<font color='green'>constexpr<font> from_chars_result from_chars(const char* first, const char* last, long double& value, chars_format fmt = chars_format::general);

### D. Modify to "17.3.2 Header \<version> synopsis" [version.syn]

<font color='green'>#define __cpp_lib_to_chars _DATE OF ADOPTION_</font>

## V. Revision History

Revision 0:

* Initial proposal

## VI. Acknowledgements

Thanks to Antony Polukhin for reviewing the paper and providing valuable feedback.

## VII. References

* [N4861] Working Draft, Standard for Programming Language C++. Available online at <https://github.com/cplusplus/draft/releases/download/n4861/n4861.pdf>
* Microsoft's C++ Standard Library <https://github.com/microsoft/STL>
* [neargye] Proof of concept for `to_chars` and `from_chars` functions <https://github.com/Neargye/cstring-constexpr-proposal>
