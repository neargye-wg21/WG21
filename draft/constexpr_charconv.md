Document number: D????  
Project: Programming Language C++  
Audience: LEWGI, LEWG, LWG,

Daniil Goncharov <neargye@gmail.com>  
Karaev Alexander <akaraevz@mail.ru>

Date: 2020-11-17

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

### `constexpr std::format` and reflection

In C++20 we adopted `constexpr std::string`, so we can already build strings at compile-time:

```cpp
static_assert(std::string("Hello, ") + "world" + "!" == "Hello, world");
```

The only non-constexpr dependency of `std::format` is `std::to_chars` so with current proposal we could mark `std::formatter<T>::parse` and even `std::formatter<T>::format` with `constexpr`.

```cpp
// C++23?
static_assert(std::format("The answer is {}", 42) == "The answer is 42");
```

This can be very useful in context of reflection, i.e. to generate unique member names:

```cpp
for (std::size_t i = 0; i < sizeof...(Ts); i++) {
    constexpr std::string member_name = std::format("field_{}", i);
    // use member_name
}
```

By the way, it's possible even without `constexpr std::format`

### No standard way to parse integer from string at compile-time

There are too many ways to convert string-like object to number - `atol`, `sscanf`, `stoi`, `strto*l`, `istream` and the best C++17 alternative - `from_chars`. However, none of them are `constexpr`. This leads to numerous hand-made `constexpr int detail::parse_int(const char* str)` in libraries:
- TODO
- TODO
- TODO TODO TODO TODO TODO

## II. Design Decisions

The discussion is based on the implementation of `to_chars` and `from_chars` from [Microsoft/STL](https://github.com/microsoft/STL), because it implements algorithm for integers and floating-point numbers.

During testing, the following changes were made to the original algorithm to make the implementation possible:
* Add constexpr modifiers to all functions
* Replace internal assert-like macro with simple assert (`_Adl_verify_range`, `_STL_ASSERT`, `_STL_INTERNAL_CHECK`)
* Replace `static constexpr` variables inside function scope with `constexpr`
* Replace `std::memcpy`, `std::memmove`, `std::memset` with constexpr equivalents: `third_party::trivial_copy`,`third_party::trivial_move`, `third_party::trivial_fill`. To keep performance in a real implementation, one should use`std::is_constant_evaluated`
* Replace `__float_to_bits`, `__double_to_bits`, `_Bit_cast` with `third_party::bit_cast`
* Replace `_BitScanForward`, `_BitScanReverse` with `third_party::bit_scan_forward`, `third_party::bit_scan_reverse`

### Testing

All the corresponding [tests](https://github.com/microsoft/STL/tree/master/tests/std/tests/P0067R5_charconv) were *constexprified* and checked at compile-time and run-time.
The modified version passes full [set tests from Microsoft/STL](https://github.com/microsoft/STL/tree/master/tests/std/tests/P0067R5_charconv) test and some constexpr set tests.

### Floating-point and Tables

Microsoft/STL implemented using the Ryu algorithm, and the implementation is already done as a header only, tables are stored as constexpr arrays in the header <https://github.com/microsoft/STL/blob/2b4cf99c044176637497518294281046439a1bcc/stl/inc/xcharconv_ryu_tables.h>.
Implementation does not use `union` nor `reinterpret_cast`, only `bit_cast` wich added in C++20 as `constexpr`.

For some other STL implementations, storing large tables in headers may be a problem, for example for `float-128`. Perhaps this problem can be leveled by modules or not using tables at the compilation stage used `std::is_constant_evaluated`.

## III. Conclusions

`to_chars` and `from_chars` are basic building blocks for string conversions, so marking them `constexpr` provides a standard way for compile-time parsing and formatting.

## IV. Proposed Changes relative to N4861

All the additions to the Standard are marked with <font color='green'>green</font>.

### A. Modifications to "20.19.1 Header \<charconv> synopsis" [charconv.syn]

<font color='green'>constexpr</font> to_chars_result to_chars(char* first, char* last, see below value, int base = 10);

to_chars_result to_chars(char* first, char* last, bool value, int base = 10) = delete;

<font color='green'>constexpr</font> to_chars_result to_chars(char* first, char* last, float value);

<font color='green'>constexpr</font> to_chars_result to_chars(char* first, char* last, double value);

<font color='green'>constexpr</font> to_chars_result to_chars(char* first, char* last, long double value);

<font color='green'>constexpr</font> to_chars_result to_chars(char* first, char* last, float value, chars_format fmt);

<font color='green'>constexpr</font> to_chars_result to_chars(char* first, char* last, double value, chars_format fmt);

<font color='green'>constexpr</font> to_chars_result to_chars(char* first, char* last, long double value, chars_format fmt);

<font color='green'>constexpr</font> to_chars_result to_chars(char* first, char* last, float value, chars_format fmt, int precision);

<font color='green'>constexpr</font> to_chars_result to_chars(char* first, char* last, double value, chars_format fmt, int precision);

<font color='green'>constexpr</font> to_chars_result to_chars(char* first, char* last, long double value, chars_format fmt, int precision);


<font color='green'>constexpr</font> from_chars_result from_chars(const char* first, const char* last, see below & value, int base = 10);

<font color='green'>constexpr</font> from_chars_result from_chars(const char* first, const char* last, float& value, chars_format fmt = chars_format::general);

<font color='green'>constexpr</font> from_chars_result from_chars(const char* first, const char* last, double& value, chars_format fmt = chars_format::general);

<font color='green'>constexpr</font> from_chars_result from_chars(const char* first, const char* last, long double& value, chars_format fmt = chars_format::general);

### D. Modify to "17.3.2 Header \<version> synopsis" [version.syn]

<font color='green'>#define __cpp_lib_to_chars _DATE OF ADOPTION_</font>

## V. Revision History

Revision 0:

* Initial proposal

## VI. Acknowledgements

Thanks to Antony Polukhin for reviewing the paper and providing valuable feedback.

## VII. References

* [N4861] Working Draft, Standard for Programming Language C++. Available online at <https://github.com/cplusplus/draft/releases/download/n4861/n4861.pdf>
* Microsoft's C++ Standard Library <https://github.com/microsoft/STL>, commit 2b4cf99c044176637497518294281046439a1bcc
* Proof of concept for `to_chars` and `from_chars` functions <https://github.com/Neargye/charconv-constexpr-proposal>
