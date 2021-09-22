<style>
table, th, td {
  border: 1px solid black;
  border-collapse: collapse;
  text-align: left;
  padding: 10px;
  border-spacing: 0px;
}
</style>
Document number: P2417R1  
Project: Programming Language C++  
Audience: LWG, LEWG  

Daniil Goncharov <neargye@gmail.com>

Date: 2021-09-22

# A more constexpr bitset

## I. Changelog

Revision 1:

* Add design decisions and proof of concept

Revision 0:

* Revised [P1251] after [discussed in Cologne](https://github.com/cplusplus/papers/issues/58#issuecomment-513194409), because the original author is no longer active and non-responsive.

## II. Motivation

`constexpr` `bitset` will allow to naturally use them as flags-mask in `constexpr`/`consteval` functions. It's add, without limitations, new high-level and more user-friendly class for bit mask in embedded developing.

As of N4762 only the default constructor, the constructor accepting an `unsigned long long` and `operator[]` of `bitset` are marked as `constexpr`. With the adoption of [P0980](https://wg21.link/p0980), there is no reason the rest of the class cannot be made `constexpr`.
The lack of `constexpr` for most member functions was probably due to the nontrivial destructor of `bitset::reference`. Now it is possible to mark the destructor and therefore the rest of `bitset` as `constexpr` instead of requiring trivial destructability of `bitset::reference` and risking potential ABI breaks in certain implementations.

## III. Proposed Changes

Mark every member function except iostream operators. Make all of `bitset::reference` constexpr.

## IV. Design Decisions

The discussion is based on the implementation of `bitset` from [Microsoft/STL](https://github.com/microsoft/STL).

During testing, the following changes were made to the implementation possible:

* Replace `std::memcpy`, `std::memset` with loop.
* Replace reinterpret_cast and _Bitsperbyte in bitset::count with loop with std::popcount.

To keep performance in a real implementation, one should use `std::is_constant_evaluated` or `if consteval`.

<div style="page-break-after: always; visibility: hidden">
\pagebreak
</div>

### Testing

All the corresponding [tests](https://github.com/Neargye/bitset-constexpr-proposal/tree/master/test) were *constexprified* and checked at compile-time and run-time.
The modified version passes [set tests from Microsoft/STL](https://github.com/microsoft/STL/blob/main/tests/tr1/tests/bitset/test.cpp) and [LLVM/libc++](https://github.com/llvm/llvm-project/tree/main/libcxx/test/std/utilities/template.bitset) tests.

### Other implementations

In `libstdc++` and `libc++` there is nothing in the implementation of `constexpr` `bitset` that goes beyond the existing capabilities of C++23.

## V. Impact on the Standard

This proposal is a pure library addition.

## VI. Proposed wording

### A. Modifications to "20.9 Bitsets" [bitset]

All the additions to the Standard are marked with <font color='green'>green</font>.

#### Change 20.9.1 of N4892 to the following:

<pre>
#include &lt;string&gt;
#include &lt;iosfwd&gt; // for istream (29.7.1), ostream (29.7.2), see 29.3.1

namespace std {
template&lt;size_t N&gt; class bitset;

// 20.9.4, bitset operators
template&lt;size_t N&gt;
  <font color='green'>constexpr</font> bitset&lt;N&gt; operator&(const bitset&lt;N&gt;&, const bitset&lt;N&gt;&) noexcept;

template&lt;size_t N&gt;
  <font color='green'>constexpr</font> bitset&lt;N&gt; operator|(const bitset&lt;N&gt;&, const bitset&lt;N&gt;&) noexcept;

template&lt;size_t N&gt;
  <font color='green'>constexpr</font> bitset&lt;N&gt; operator^(const bitset&lt;N&gt;&, const bitset&lt;N&gt;&) noexcept;

template&lt;class charT, class traits, size_t N&gt;  
  basic_istream&lt;charT, traits&gt;&
    operator&gt;&gt;(basic_istream&lt;charT, traits&gt;& is, bitset&lt;N&gt;& x);

template&lt;class charT, class traits, size_t N&gt;  
  basic_ostream&lt;charT, traits&gt;&
    operator&lt;&lt;(basic_ostream&lt;charT, traits&gt;& os, const bitset&lt;N&gt;& x);
}
</pre>

#### Change 20.9.2.1 of N4892 to the following:

<pre>
namespace std {
  template&lt;size_t N&gt; class bitset {
  public:
    // bit reference
    class reference {
      friend class bitset;
      <font color='green'>constexpr</font> reference() noexcept;

    public:
      <font color='green'>constexpr</font> reference(const reference&) = default;
      <font color='green'>constexpr</font> ~reference();
      <font color='green'>constexpr</font> reference& operator=(bool x) noexcept;            // for b[i] = x;
      <font color='green'>constexpr</font> reference& operator=(const reference&) noexcept;  // for b[i] = b[j];
      <font color='green'>constexpr</font> bool operator~() const noexcept;                  // flips the bit
      <font color='green'>constexpr</font> operator bool() const noexcept;                   // for x = b[i];
      <font color='green'>constexpr</font> reference& flip() noexcept;                       // for b[i].flip();
    };

    // 20.9.2.2, constructors
    constexpr bitset() noexcept;
    constexpr bitset(unsigned long long val) noexcept;
    template&lt;class charT, class traits, class Allocator&gt;
      <font color='green'>constexpr</font> explicit bitset(
        const basic_string&lt;charT, traits, Allocator&gt;& str,
        typename basic_string&lt;charT, traits, Allocator&gt;::size_type pos = 0,
        typename basic_string&lt;charT, traits, Allocator&gt;::size_type n
          = basic_string&lt;charT, traits, Allocator&gt;::npos,
        charT zero = charT('0'),
        charT one = charT('1'));
    template&lt;class charT&gt;
      <font color='green'>constexpr</font> explicit bitset(
        const charT* str,
        typename basic_string&lt;charT&gt;::size_type n = basic_string&lt;charT&gt;::npos,
        charT zero = charT('0'),
        charT one = charT('1'));

    // 20.9.2.3, bitset operations
    <font color='green'>constexpr</font> bitset&lt;N&gt;& operator&=(const bitset&lt;N&gt;& rhs) noexcept;
    <font color='green'>constexpr</font> bitset&lt;N&gt;& operator|=(const bitset&lt;N&gt;& rhs) noexcept;
    <font color='green'>constexpr</font> bitset&lt;N&gt;& operator^=(const bitset&lt;N&gt;& rhs) noexcept;
    <font color='green'>constexpr</font> bitset&lt;N&gt;& operator&lt;&lt;=(size_t pos) noexcept;
    <font color='green'>constexpr</font> bitset&lt;N&gt;& operator&gt;&gt;=(size_t pos) noexcept;
    <font color='green'>constexpr</font> bitset&lt;N&gt;& set() noexcept;
    <font color='green'>constexpr</font> bitset&lt;N&gt;& set(size_t pos, bool val = true);
    <font color='green'>constexpr</font> bitset&lt;N&gt;& reset() noexcept;
    <font color='green'>constexpr</font> bitset&lt;N&gt;& reset(size_t pos);
    <font color='green'>constexpr</font> bitset&lt;N&gt;  operator~() const noexcept;
    <font color='green'>constexpr</font> bitset&lt;N&gt;& flip() noexcept;
    <font color='green'>constexpr</font> bitset&lt;N&gt;& flip(size_t pos);

    // element access
    constexpr bool operator[](size_t pos) const;        // for b[i];
    <font color='green'>constexpr</font> reference operator[](size_t pos);       // for b[i];

    <font color='green'>constexpr</font> unsigned long to_ulong() const;
    <font color='green'>constexpr</font> unsigned long long to_ullong() const;
    template&lt;class charT = char,
              class traits = char_traits&lt;charT&gt;,
              class Allocator = allocator&lt;charT&gt;&gt;
      <font color='green'>constexpr</font> basic_string&lt;charT, traits, Allocator&gt;
        to_string(charT zero = charT('0'), charT one = charT('1')) const;

    <font color='green'>constexpr</font> size_t count() const noexcept;
    <font color='green'>constexpr</font> constexpr size_t size() const noexcept;
    <font color='green'>constexpr</font> bool operator==(const bitset&lt;N&gt;& rhs) const noexcept;
    <font color='green'>constexpr</font> bool operator!=(const bitset&lt;N&gt;& rhs) const noexcept;
    <font color='green'>constexpr</font> bool test(size_t pos) const;
    <font color='green'>constexpr</font> bool all() const noexcept;
    <font color='green'>constexpr</font> bool any() const noexcept;
    <font color='green'>constexpr</font> bool none() const noexcept;
    <font color='green'>constexpr</font> bitset&lt;N&gt; operator&lt;&lt;(size_t pos) const noexcept;
    <font color='green'>constexpr</font> bitset&lt;N&gt; operator&gt;&gt;(size_t pos) const noexcept;
  };

  // 20.9.3, hash support
  template&lt;class T&gt; struct hash;
  template&lt;size_t N&gt; struct hash&lt;bitset&lt;N&gt;&gt;;
}
</pre>

### B. Modify to "17.3.2 Header \<version> synopsis" [version.syn]

<font color='green'>#define __cpp_lib_constexpr_bitset _DATE OF ADOPTION_ // also in \<bitset> </font>

<div style="page-break-after: always; visibility: hidden">
\pagebreak
</div>

## VII. Acknowledgements

Thanks to Morris Hafner for the work done on the original version of the paper [P1251].

## VIII. References

* [n4892] Working Draft, Standard for Programming Language C++. Available online at <https://github.com/cplusplus/draft/releases/download/n4892/n4892.pdf>
* [P1251] Morris Hafner: A more constexpr bitset <https://wg21.link/p1251>
* Proof of concept for `constexpr` `bitset` <https://github.com/Neargye/bitset-constexpr-proposal>
* [P0784] L. Dionne, R. Smith, N. Ranns, D.Vandevoorde: More constexpr containers <https://wg21.link/p0784>
* [P0980] L. Dionne: Making std::string constexpr <https://wg21.link/p0980>
