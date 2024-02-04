# Overload constructor 
Overload UART::Manager constructor with a no idx parameter.


# Compiletime polymorphism

Create Interface






# Recursive template instantiation technique

```cpp
template <int N> struct MultiplyByTwoImpl { static constexpr int value = N * 2; };
```
This is a template struct named `MultiplyByTwoImpl` that takes an integer `N` as a parameter. It has a single `constexpr` member variable `value` that is initialized to `N * 2`. This means that for any given `N`, `MultiplyByTwoImpl<N>::value` will be `N * 2`.

```cpp
template <> struct MultiplyByTwoImpl<0> { static constexpr int value = 0; };
```
This is a specialization of `MultiplyByTwoImpl` for the case where `N` is `0`. It overrides the `value` member variable to be `0`. This is the base case of the recursion.

```cpp
template <int N> struct MultiplyByTwoImpl : MultiplyByTwoImpl<N - 1> {};
```

This is the recursive case. For any `N` greater than `0`, `MultiplyByTwoImpl<N>` inherits from `MultiplyByTwoImpl<N - 1>`. This means that `MultiplyByTwoImpl<N>::value` will be `N * 2`, because it inherits the `value` from `MultiplyByTwoImpl<N - 1>`, which is `(N - 1) * 2`, and then adds `2` to it.

```cpp
template<int N> constexpr int multiply_by_2(N) { return MultiplyByTwoImpl<N>::value; }
```

This is a `constexpr` function template named `multiply_by_2` that takes an integer `N` as a parameter. It returns `MultiplyByTwoImpl<N>::value`, which is `N * 2` due to the inheritance chain described above.
So, if you call `multiply_by_2<5>()`, it will return `10`, because `MultiplyByTwoImpl<5>::value` is `10`.

```cpp
template <int N>
struct MultiplyByTwoImpl {
   static constexpr int value = N * 2;
};

// Base case
template <>
struct MultiplyByTwoImpl<0> {
   static constexpr int value = 0;
};

// Recursive case
template <int N>
struct MultiplyByTwoImpl : MultiplyByTwoImpl<N - 1> {};

template<int N>
constexpr int multiply_by_2(N) {
   return MultiplyByTwoImpl<T>::value;
}
```


# Generate method overloading at compile time

## Use wrap type
haven't find solution yet

first idea is to create a Wrap integer type to wrap a  integer
Then we can use that WrapInt to create overload Mehthod 
```cpp
template<int N>
class WrapInt : public WrapInt<N - 1> {
public:
   static constexpr const int value = N;
};

template<>
class WrapInt<0> {
public:
   static constexpr const int value = 0;
};

// Define the functions outside the class
static constexpr int multiply_by_2(WrapInt<1>) {
   return 2;
}

static constexpr int multiply_by_2(WrapInt<2>) {
   return 4;
}
```

## Use the Rvalue reference
[the approve answer Stackoverflow ](https://stackoverflow.com/questions/29800867/overloading-function-calls-for-compile-time-constants)
Is there any way to provide overloads that distinguish between these two value of the same type.
This is what a 'rvalue reference' is for. literal type is a rvalue.
```cpp
void foo(int&& a);
```

