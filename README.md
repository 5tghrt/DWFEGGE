# SLIMCPP
# Simple long integer math library for C++
SLIMCPP is C++ header-only library that implements long integers that exceed maximum size of native type supported by a specific compiler by 2-4 times. All classes, methods and functions were not created or designed to work with huge numbers, for which there are specialized mathematical libraries. In some cases, it is necessary to temporarily perform calculations with precision exceeding the maximum supported size of integers, and then return the result to its native size again. The conditions are ideal for SLIMCPP.
The main features:
* easy to use: the library is header-only
* small: consists of few header files, there is no dependencies
* speed: use intrinsics if supported
* cross-platform: supports MSVC, GCC and CLANG C++17 compilers

The library implements two main classes:
```c++
namespace slim
{
template<typename native_t = uintmax_t, uint_t size = 2>
class long_uint_t; // unsigned integer
template<typename native_t = uintmax_t, uint_t size = 2>
class long_int_t;  // signed integer

} // namespace slim
```
where native_t may be one of base unsigned type and size must by power of two.
## Implementation
* [long_int.h](include/long_int.h) - long signed integer class
* [long_uint.h](include/long_uint.h) - long unsigned integer class
* [long_math.h](include/long_math.h) - crossplatform helper classes and functions
* [long_math_gcc.h](include/long_math_gcc.h) - GCC, CLANG helper classes and functions
* [long_math_msvc.h](include/long_math_msvc.h) - MSVC helper classes and functions
## Integration
The library implements four predefined types: uint128_t, uint256_t, int128_t, int256_t. You can use them in you project by include code below:
```c++
#include <slimcpplib/long_int.h>  // Include all integers support
// or
#include <slimcpplib/long_uint.h> // Include only unsigned integers support

namespace your_namespace
{
    using uint128_t = slim::uint128_t;
    using uint256_t = slim::uint256_t;
    using int128_t  = slim::int128_t;
    using int256_t  = slim::int256_t;
    using namespace slim::literals;

} // namespace your_namespace
```
## Constant declaration:
```c++
constexpr auto uo = 03766713523035452062041773345651416625031020_ui128;  // octal unsigned integer
constexpr auto ud = 338770000845734292534325025077361652240_ui128;       // decimal unsigned integer
constexpr auto uh = 0xfedcba9876543210fedcba9876543210_ui128;            // hexadecimal unsigned integer

constexpr auto io = -03766713523035452062041773345651416625031020_si128; // octal signed integer
constexpr auto id = -338770000845734292534325025077361652240_si128;      // decimal signed integer
constexpr auto ih = -0xfedcba9876543210fedcba9876543210_si128;           // hexadecimal signed integer
```
also supported (') separator for integer literals:
```c++
constexpr auto u = 0xfedcba98'76543210'fedcba98'76543210_ui128;          // hexadecimal unsigned integer
```
## Construction:
```c++
const uint128_t u;               // construct uninitialized unsigned integer
const uint128_t u = 1U;          // construction from unsigned integer
const int128_t  s = -1;          // construction from signed integer
const uint128_t u = 10000_ui128; // construction from long unsigned integer
const int128_t  u = -10000_i128; // construction from long signed integer
const uint128_t u = true;        // construction from boolean value
```
## Operators
* long_uint_t type supports following operators:
` ==, !=, <, <=, >, >=, <<=, <<, >>=, >>, +=, +, ++, -=, -, --, *=, *, /=, /, %=, %, ~, &=, &, |=, |, ^=, ^`

* long_int_t type supports following operators:
`==, !=, <, <=, >, >=, +=, +, ++, -=, -, --, *=, *, /=, /, %=, %`
## MulDiv
The library implements the muldiv method for faster calculation of the following expressions: (a * b / c). It can be used with signed and unsigned integers.
```c++
template<typename type_t>
constexpr type_t muldiv(const type_t& value, const type_t& multiplier, const type_t& divider) noexcept;
```
## Limitations
* Although all methods and functions are defined using the constexpr qualifier, due to the limitations of C++ 17, working completely at compile time is only possible for code without instrinsics, since there is no implementation of [std::is_constant_evaluated()](https://en.cppreference.com/w/cpp/types/is_constant_evaluated) in the standard before C++ 20.
* The design of long integers tries to completely repeat the behavior of native integers, but still differs. For example, the propagation of integer types always occurs from a signed integer to an unsigned integer, and an implicit conversion from a larger integer to a smaller integer does not cause a warning, but a compilation error.
