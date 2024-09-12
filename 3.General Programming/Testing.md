
# Attention singleton
 In tern of state, generally test method should be independent of each other.
 But when we use a singleton object state gotta remain between call because it's a global object.


# Stub

## How Stubs Work

Stubs work by intercepting calls made by the SUT to its dependencies and returning predefined responses. 
These responses can be static or dynamic, depending on the requirements of the test scenario. 
For example, if a function in the SUT makes a call to an external API to fetch user data, a stub could intercept this call and return a hardcoded set of user data instead of making an actual network request.

## Types of Stubs

1. **Static Stubs**: Return fixed data every time they are called. They are useful when the test does not require varying responses.
2. **Dynamic Stubs**: Can return different data based on the input parameters or state. They are useful for simulating more realistic interactions between the SUT and its dependencies.