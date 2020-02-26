# Comparison unbounded integer from different languages

## Python 3

* All integers are implemented as “long” integer objects of arbitrary size.
* <https://docs.python.org/3.8/library/functions.html#int> & <https://docs.python.org/3/c-api/long.html>
* Python represents all objects by C structures. The following data structure is responsible for all integer objects:

  ```c
  struct _longobject {
      PyObject_VAR_HEAD
      digit ob_digit[1];
  } PyLongObject;
  ```

* [Arithmetic Operators](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)

  | Operator | Result |
  |----------|--------|
  | x + y    | sum of x and y
  | x - y    | difference of x and y
  | x * y    | product of x and y
  | x / y    | quotient of x and y
  | x // y   | floored quotient of x and y
  | x % y    | remainder of x / y
  | -x       | x negated
  | +x       | x unchanged
  | x ** y   | x to the power y

* [Arithmetic Functions](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)

  | Functions | Result |
  |---------- |--------|
  | abs(x)       | absolute value or magnitude of x
  | int(x)       | x converted to integer
  | float(x)     | x converted to floating point
  | divmod(x, y) | the pair (x // y, x % y)
  | pow(x, y)    | x to the power y

* [Comparisons Operators](https://docs.python.org/3/library/stdtypes.html#comparisons)

  | Operator | Result |
  |----------|--------|
  | <        | strictly less than
  | <=       | less than or equal
  | >        | strictly greater than
  | >=       | greater than or equal
  | ==       | equal
  | !=       | not equal
  | is       | object identity
  | is not   | negated object identity

* [Bitwise Operators](https://docs.python.org/3/library/stdtypes.html#bitwise-operations-on-integer-types)

  | Operator | Result |
  |----------|--------|
  | x | y    | bitwise or of x and y
  | x ^ y    | bitwise exclusive or of x and y
  | x & y    | bitwise and of x and y
  | x << n   | x shifted left by n bits
  | x >> n   | x shifted right by n bits
  | ~x       | the bits of x inverted

* [Conversions Methods](https://docs.python.org/3/library/stdtypes.html#additional-methods-on-integer-types)

  | Methods | Result |
  |---------|--------|
  | to_bytes(length, byteorder, *, signed=False)  | Return an array of bytes representing an integer
  | from_bytes(bytes, byteorder, *, signed=False) | Return the integer represented by the given array of bytes
  | as_integer_ratio()                            | Return a pair of integers whose ratio is exactly equal to the original integer and with a positive denominator

* [Hash Functions](https://docs.python.org/3/library/stdtypes.html#hashing-of-numeric-types)

  | Functions | Result |
  |-----------|--------|
  | hash(x)   | Returns the hash code

## CSharp

* BigInteger Struct - type is an immutable type that represents an arbitrarily large integer whose value in theory has no upper or lower bounds.
* <https://docs.microsoft.com/en-us/dotnet/api/system.numerics.biginteger>

* [Constructors](https://docs.microsoft.com/en-us/dotnet/api/system.numerics.biginteger#constructors)

  | Constructor | Result |
  |-------------|--------|
  | BigInteger(Byte[])  | Initializes a new instance of the BigInteger structure using the values in a byte array
  | BigInteger(Decimal) | Initializes a new instance of the BigInteger structure using a Decimal value
  | BigInteger(Double)  | Initializes a new instance of the BigInteger structure using a double-precision floating-point value
  | BigInteger(Int32)   | Initializes a new instance of the BigInteger structure using a 32-bit signed integer value
  | BigInteger(Int64)   | Initializes a new instance of the BigInteger structure using a 64-bit signed integer value
  | BigInteger(Single)  | Initializes a new instance of the BigInteger structure using a single-precision floating-point value
  | BigInteger(UInt32)  | Initializes a new instance of the BigInteger structure using an unsigned 32-bit integer value
  | BigInteger(UInt64)  | Initializes a new instance of the BigInteger structure with an unsigned 64-bit integer value

* [Properties](https://docs.microsoft.com/en-us/dotnet/api/system.numerics.biginteger#properties)

  | Properties | Result |
  |------------|--------|
  | IsEven       | Indicates whether the value of the current BigInteger object is an even number
  | IsOne        | Indicates whether the value of the current BigInteger object is One
  | IsPowerOfTwo | Indicates whether the value of the current BigInteger object is a power of two
  | IsZero       | Indicates whether the value of the current BigInteger object is Zero
  | MinusOne     | Gets a value that represents the number negative one (-1)
  | One          | Gets a value that represents the number one (1)
  | Sign         | Gets a number that indicates the sign (negative, positive, or zero) of the current BigInteger object
  | Zero         | Gets a value that represents the number 0 (zero)

* [Arithmetic Operators](https://docs.microsoft.com/en-us/dotnet/api/system.numerics.biginteger#operators)

  | Operator | Result |
  |----------|--------|
  | x + y    | Adds the values of two specified BigInteger objects
  | x - y    | Returns the difference between two BigInteger numbers
  | x * y    | Returns the product of two BigInteger numbers
  | x / y    | Divides a specified BigInteger value by another specified BigInteger value by using integer division
  | x % y    | Returns the remainder after dividing one BigInteger by another and returns the result
  | -x       | Returns a BigInteger with the opposite sign
  | +x       | Returns the value of a BigInteger
  | --x      | Decrements a BigInteger value by 1
  | ++x      | Increments a BigInteger value by 1

* [Arithmetic Methods](https://docs.microsoft.com/en-us/dotnet/api/system.numerics.biginteger#methods)

  | Operator | Result |
  |----------|--------|
  | Abs(BigInteger)                               | Gets the absolute value of a BigInteger object
  | Add(BigInteger, BigInteger)                   | Adds two BigInteger values and returns the result
  | Divide(BigInteger, BigInteger)                | Divides one BigInteger value by another and returns the result
  | DivRem(BigInteger, BigInteger, BigInteger)    | Divides one BigInteger value by another, returns the result, and returns the remainder in an output parameter
  | GreatestCommonDivisor(BigInteger, BigInteger) | Finds the greatest common divisor of two BigInteger values
  | Log(BigInteger)                               | Returns the natural (base e) logarithm of a specified number
  | Log(BigInteger, Double)                       | Returns the logarithm of a specified number in a specified base
  | Log10(BigInteger)                             | Returns the base 10 logarithm of a specified number
  | Max(BigInteger, BigInteger)                   | Returns the larger of two BigInteger values
  | Min(BigInteger, BigInteger)                   | Returns the smaller of two BigInteger values
  | ModPow(BigInteger, BigInteger, BigInteger)    | Performs modulus division on a number raised to the power of another number
  | Multiply(BigInteger, BigInteger)              | Returns the product of two BigInteger values
  | Negate(BigInteger)                            | Negates a specified BigInteger value
  | Pow(BigInteger, Int32)                        | Raises a BigInteger value to the power of a specified value
  | Remainder(BigInteger, BigInteger)             | Performs integer division on two BigInteger values and returns the remainder
  | Subtract(BigInteger, BigInteger)              | Subtracts one BigInteger value from another and returns the result

* [Comparisons Operators](https://docs.microsoft.com/en-us/dotnet/api/system.numerics.biginteger#operators)

  | Operator | Result |
  |----------|--------|
  | <        | strictly less than: BigInteger < BigInteger, BigInteger < Int64, BigInteger < UInt64, Int64 < BigInteger, UInt64 < BigInteger.
  | <=       | less than or equal : BigInteger <= BigInteger, BigInteger <= Int64, BigInteger <= UInt64, Int64 <= BigInteger, UInt64 <= BigInteger.
  | >        | strictly greater than : BigInteger > BigInteger, BigInteger > Int64, BigInteger > UInt64, Int64 > BigInteger, UInt64 > BigInteger.
  | >=       | greater than or equal : BigInteger >= BigInteger, BigInteger >= Int64, BigInteger >= UInt64, Int64 >= BigInteger, UInt64 >= BigInteger.
  | ==       | equal : BigInteger & BigInteger, BigInteger == Int64, BigInteger == UInt64, Int64 == BigInteger, UInt64 == BigInteger.
  | !=       | not equal : BigInteger & BigInteger, BigInteger != Int64, BigInteger != UInt64, Int64 != BigInteger, UInt64 != BigInteger.

* [Bitwise Operators](https://docs.microsoft.com/en-us/dotnet/api/system.numerics.biginteger#operators)

  | Operator | Result |
  |----------|--------|
  | x | y    | Performs a bitwise Or operation on two BigInteger values
  | x ^ y    | Performs a bitwise exclusive Or (XOr) operation on two BigInteger values
  | x & y    | Performs a bitwise And operation on two BigInteger values
  | x << n   | Shifts a BigInteger value a specified number of bits to the left
  | x >> n   | Shifts a BigInteger value a specified number of bits to the right
  | ~x       | Returns the bitwise one's complement of a BigInteger value

* [Conversions Operators](https://docs.microsoft.com/en-us/dotnet/api/system.numerics.biginteger#operators)

  | Operator | Result |
  |----------|--------|
  | Explicit(BigInteger to Single)  | Defines an explicit conversion of a BigInteger object to a single-precision floating-point value
  | Explicit(BigInteger to UInt16)  | Defines an explicit conversion of a BigInteger object to an unsigned 16-bit integer value.This API is not CLS-compliant. The compliant alternative is Int32
  | Explicit(BigInteger to UInt32)  | Defines an explicit conversion of a BigInteger object to an unsigned 32-bit integer value.This API is not CLS-compliant. The compliant alternative is Int64
  | Explicit(BigInteger to UInt64)  | Defines an explicit conversion of a BigInteger object to an unsigned 64-bit integer value.This API is not CLS-compliant. The compliant alternative is Double
  | Explicit(Decimal to BigInteger) | Defines an explicit conversion of a Decimal object to a BigInteger value
  | Explicit(Double to BigInteger)  | Defines an explicit conversion of a Double value to a BigInteger value
  | Explicit(Single to BigInteger)  | Defines an explicit conversion of a Single value to a BigInteger value
  | Explicit(BigInteger to Byte)    | Defines an explicit conversion of a BigInteger object to an unsigned byte value
  | Explicit(BigInteger to Decimal) | Defines an explicit conversion of a BigInteger object to a Decimal value
  | Explicit(BigInteger to Double)  | Defines an explicit conversion of a BigInteger object to a Double value
  | Explicit(BigInteger to Int16)   | Defines an explicit conversion of a BigInteger object to a 16-bit signed integer value
  | Explicit(BigInteger to Int32)   | Defines an explicit conversion of a BigInteger object to a 32-bit signed integer value
  | Explicit(BigInteger to Int64)   | Defines an explicit conversion of a BigInteger object to a 64-bit signed integer value
  | Explicit(BigInteger to SByte)   | Defines an explicit conversion of a BigInteger object to a signed 8-bit value.This API is not CLS-compliant. The compliant alternative is Int16
  | Implicit(Byte to BigInteger)    | Defines an implicit conversion of an unsigned byte to a BigInteger value
  | Implicit(Int16 to BigInteger)   | Defines an implicit conversion of a signed 16-bit integer to a BigInteger value
  | Implicit(Int32 to BigInteger)   | Defines an implicit conversion of a signed 32-bit integer to a BigInteger value
  | Implicit(Int64 to BigInteger)   | Defines an implicit conversion of a signed 64-bit integer to a BigInteger value
  | Implicit(SByte to BigInteger)   | Defines an implicit conversion of an 8-bit signed integer to a BigInteger value.This API is not CLS-compliant. The compliant alternative is BigInteger(Int32)
  | Implicit(UInt16 to BigInteger)  | Defines an implicit conversion of a 16-bit unsigned integer to a BigInteger value.This API is not CLS-compliant. The compliant alternative is Implicit(Int32 to BigInteger)
  | Implicit(UInt32 to BigInteger)  | Defines an implicit conversion of a 32-bit unsigned integer to a BigInteger value.This API is not CLS-compliant. The compliant alternative is Implicit(Int64 to BigInteger)
  | Implicit(UInt64 to BigInteger)  | Defines an implicit conversion of a 64-bit unsigned integer to a BigInteger value.This API is not CLS-compliant. The compliant alternative is Double

* [Conversions Methods](https://docs.microsoft.com/en-us/dotnet/api/system.numerics.biginteger#methods)

  | Methods | Result |
  |---------|--------|
  | ToByteArray()                                               | Converts a BigInteger value to a byte array
  | ToString()                                                  | Converts the numeric value of the current BigInteger object to its equivalent string representation
  | ToString(IFormatProvider)                                   | Converts the numeric value of the current BigInteger object to its equivalent string representation by using the specified culture-specific formatting information
  | ToString(String)                                            | Converts the numeric value of the current BigInteger object to its equivalent string representation by using the specified format
  | ToString(String, IFormatProvider)                           | Converts the numeric value of the current BigInteger object to its equivalent string representation by using the specified format and culture-specific format information
  | TryParse(String, BigInteger)                                | Tries to convert the string representation of a number to its BigInteger equivalent, and returns a value that indicates whether the conversion succeeded
  | TryParse(String, NumberStyles, IFormatProvider, BigInteger) | Tries to convert the string representation of a number in a specified style and culture-specific format to its BigInteger equivalent, and returns a value that indicates whether the conversion succeeded
  | Parse(String)                                               | Converts the string representation of a number to its BigInteger equivalent
  | Parse(String, IFormatProvider)                              | Converts the string representation of a number in a specified culture-specific format to its BigInteger equivalent
  | Parse(String, NumberStyles)                                 | Converts the string representation of a number in a specified style to its BigInteger equivalent
  | Parse(String, NumberStyles, IFormatProvider)                | Converts the string representation of a number in a specified style and culture-specific format to its BigInteger equivalent

* [Compares Methods](https://docs.microsoft.com/en-us/dotnet/api/system.numerics.biginteger#methods)

  | Methods | Result |
  |---------|--------|
  | CompareTo(BigInteger) | Compares this instance to a second BigInteger and returns an integer that indicates whether the value of this instance is less than, equal to, or greater than the value of the specified object
  | CompareTo(Int64)      | Compares this instance to a signed 64-bit integer and returns an integer that indicates whether the value of this instance is less than, equal to, or greater than the value of the signed 64-bit integer
  | CompareTo(Object)     | Compares this instance to a specified object and returns an integer that indicates whether the value of this instance is less than, equal to, or greater than the value of the specified object
  | CompareTo(UInt64)     | Compares this instance to an unsigned 64-bit integer and returns an integer that indicates whether the value of this instance is less than, equal to, or greater than the value of the unsigned 64-bit integer
  | Equals(BigInteger)    | Returns a value that indicates whether the current instance and a specified BigInteger object have the same value
  | Equals(Int64)         | Returns a value that indicates whether the current instance and a signed 64-bit integer have the same value
  | Equals(Object)        | Returns a value that indicates whether the current instance and a specified object have the same value
  | Equals(UInt64)        | Returns a value that indicates whether the current instance and an unsigned 64-bit integer have the same value

* [Hash Methods](https://docs.microsoft.com/en-us/dotnet/api/system.numerics.biginteger#methods)

  | Methods | Result |
  |---------|--------|
  | GetHashCode() | Returns the hash code for the current BigInteger object

## Java

* Immutable arbitrary-precision integers.
* <https://docs.oracle.com/javase/7/docs/api/java/math/BigInteger.html>
* Arithmetic Operators omitted.
* Bitwise Operators omitted.

* [Constructors](https://docs.oracle.com/javase/7/docs/api/java/math/BigInteger.html)

  | Constructor | Result |
  |-------------|--------|
  | BigInteger(byte[] val)                               | Translates a byte array containing the two's-complement binary representation of a BigInteger into a BigInteger
  | BigInteger(int signum, byte[] magnitude)             | Translates the sign-magnitude representation of a BigInteger into a BigInteger
  | BigInteger(int bitLength, int certainty, Random rnd) | Constructs a randomly generated positive BigInteger that is probably prime, with the specified bitLength
  | BigInteger(int numBits, Random rnd)                  | Constructs a randomly generated BigInteger, uniformly distributed over the range 0 to (2<sup>numBits</sup> - 1), inclusive
  | BigInteger(String val)                               | Translates the decimal String representation of a BigInteger into a BigInteger
  | BigInteger(String val, int radix)                    | Translates the String representation of a BigInteger in the specified radix into a BigInteger

* [Arithmetic Methods](https://docs.oracle.com/javase/7/docs/api/java/math/BigInteger.html)

  | Methods | Result |
  |---------|--------|
  | abs()                                     | Returns a BigInteger whose value is the absolute value of this BigInteger
  | add(BigInteger val)                       | Returns a BigInteger whose value is (this + val)
  | divide(BigInteger val)                    | Returns a BigInteger whose value is (this / val)
  | divideAndRemainder(BigInteger val)        | Returns an array of two BigIntegers containing (this / val) followed by (this % val)
  | gcd(BigInteger val)                       | Returns a BigInteger whose value is the greatest common divisor of abs(this) and abs(val)
  | isProbablePrime(int certainty)            | Returns true if this BigInteger is probably prime, false if it's definitely composite
  | max(BigInteger val)                       | Returns the maximum of this BigInteger and val
  | min(BigInteger val)                       | Returns the minimum of this BigInteger and val
  | mod(BigInteger m)                         | Returns a BigInteger whose value is (this mod m)
  | modInverse(BigInteger m)                  | Returns a BigInteger whose value is (this<sup>-1</sup> mod m)
  | modPow(BigInteger exponent, BigInteger m) | Returns a BigInteger whose value is (this<sup>exponent</sup> mod m)
  | multiply(BigInteger val)                  | Returns a BigInteger whose value is (this * val)
  | negate()                                  | Returns a BigInteger whose value is (-this)
  | nextProbablePrime()                       | Returns the first integer greater than this BigInteger that is probably prime
  | pow(int exponent)                         | Returns a BigInteger whose value is (this<sup>exponent</sup>)
  | remainder(BigInteger val)                 | Returns a BigInteger whose value is (this % val)
  | signum()                                  | Returns the signum function of this BigInteger
  | subtract(BigInteger val)                  | Returns a BigInteger whose value is (this - val)

* [Static Arithmetic Methods](https://docs.oracle.com/javase/7/docs/api/java/math/BigInteger.html)

  | Static methods | Result |
  |----------------|--------|
  | probablePrime(int bitLength, Random rnd) | Returns a positive BigInteger that is probably prime, with the specified bitLength.

* [Bitwise Methods](https://docs.oracle.com/javase/7/docs/api/java/math/BigInteger.html)

  | Methods | Result |
  |---------|--------|
  | and(BigInteger val)    | Returns a BigInteger whose value is (this & val)
  | andNot(BigInteger val) | Returns a BigInteger whose value is (this & ~val)
  | bitCount()             | Returns the number of bits in the two's complement representation of this BigInteger that differ from its sign bit
  | bitLength()            | Returns the number of bits in the minimal two's-complement representation of this BigInteger, excluding a sign bit
  | clearBit(int n)        | Returns a BigInteger whose value is equivalent to this BigInteger with the designated bit cleared
  | flipBit(int n)         | Returns a BigInteger whose value is equivalent to this BigInteger with the designated bit flipped
  | getLowestSetBit()      | Returns the index of the rightmost (lowest-order) one bit in this BigInteger (the number of zero bits to the right of the rightmost one bit)
  | not()                  | Returns a BigInteger whose value is (~this)
  | or(BigInteger val)     | Returns a BigInteger whose value is (this | val)
  | setBit(int n)          | Returns a BigInteger whose value is equivalent to this BigInteger with the designated bit set
  | shiftLeft(int n)       | Returns a BigInteger whose value is (this << n)
  | shiftRight(int n)      | Returns a BigInteger whose value is (this >> n)
  | testBit(int n)         | Returns true if and only if the designated bit is set
  | xor(BigInteger val)    | Returns a BigInteger whose value is (this ^ val)

* [Conversions Methods](https://docs.oracle.com/javase/7/docs/api/java/math/BigInteger.html)

  | Methods | Result |
  |---------|--------|
  | valueOf(long val)   | Returns a BigInteger whose value is equal to that of the specified long
  | intValue()          | Converts this BigInteger to an int
  | longValue()         | Converts this BigInteger to a long
  | toByteArray()       | Returns a byte array containing the two's-complement representation of this BigInteger
  | toString()          | Returns the decimal String representation of this BigIntege
  | toString(int radix) | Returns the String representation of this BigInteger in the given radix

* [Compares Methods](https://docs.oracle.com/javase/7/docs/api/java/math/BigInteger.html)

  | Methods | Result |
  |---------|--------|
  | compareTo(BigInteger val) | Compares this BigInteger with the specified BigInteger
  | equals(Object x)          | Compares this BigInteger with the specified Object for equality

* [Hash Methods](https://docs.oracle.com/javase/7/docs/api/java/math/BigInteger.html)

  | Methods | Result |
  |---------|--------|
  | hashCode() | Returns the hash code for this BigInteger
