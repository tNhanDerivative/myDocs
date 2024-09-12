
## Register
method `void Register()` register a factory (which is a function)
and that factory will return an object of type T
We have a lot of type so we templated it

input parameter of `Register()` is a function that return a `share_ptr` of type T.
We alias that type as `Generator`:
```cpp
  using Generator = std::function<std::shared_ptr<T>()>;
```

## Resolve
We want at any time create instances of

1. Looks up the generator function for the requested type in the serviceMap_.
2. If found, it attempts to cast the stored std::any to the appropriate generator type (G).
3. Calls the generator function with any provided parameters to create an instance of the requested type.
4. Returns the created instance as a std::shared_ptr.

This process effectively "resolves" the dependency by creating and returning an instance of the requested type, using the registered generator function. It handles both parameterized and non-parameterized types, and includes error handling for cases where the type is not registered or there's a type mismatch.