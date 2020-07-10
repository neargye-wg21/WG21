Document number: D????
Project: Programming Language C++
Audience: LEWGI, LEWG, LWG,

Daniil Goncharov <neargye@gmail.com>
Karaev Alexander <akaraevz@mail.ru>

Date: 2020-07-10

# Add Constexpr Modifiers to Functions to_chars and from_chars in \<charconv> Header

## I. Introduction and Motivation

There is currently no standard way to make conversion between numbers and strings *at compile time*.

`to_chars` and `from_chars` are fundamental blocks for parsing and formatting being locale-independent and non-throwing without memory allocation, so they look like natural candidates for constexpr string conversions. The paper proposes to make `to_chars` and `from_chars` functions usable in constexpr context.

Consider the simple example:

```cpp

constexpr std::optional<int> to_int(std::string_view s) {
    int value;

    if (auto [p, err] = std::from_chars(s.begin(), s.end(), value); err == std::errc{}) {
        return value;
    } else {
        return std::nullopt;
    }
}

static_assert(to_int("42") == 42);
static_assert(to_int("foo") == std::nullopt);
```

## II. Design Decisions

The discussion is based on the implementation of `to_chars` and `from_chars` from [Microsoft/STL](https://github.com/microsoft/STL), because it implements algorithm for integers and floating-point numbers.

During testing, the following changes were made to the original algorithm to make the implementation possible:

### Integers

* Replace `std::memcpy` with a raw loop. To maintain performance in a real implementation, one can use `std::char_traits<char>::copy`.
* Zero initialize some variables. In C++20 we adopted [P1331](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2019/p1331r2.pdf), that eliminates these changes.

### Floating-point

* Replace `std::memcpy`, `std::memmove`, `std::memset` with a raw loop if `is_constant_evaluated`.
* Zero initialize some variables. In C++20 we adopted [P1331](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2019/p1331r2.pdf), that eliminates these changes.
* In the Microsoft/STL implementation not use `union` or `reinterpret_cast`, but only used `bit_cast` which constexpr in C++20.
* Most conversion tables were already global inline constexpr constants, so no big changes were needed here. (Было всего 2 static constexpr таблицы внутри функций, но они были маленькие, не думаю что это большое изменение)

## III. Conclusions

`to_chars` and `from_chars` are basic building blocks for string formatting, so marking them `constexpr` provides a standard way for compile-time formatting.

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
