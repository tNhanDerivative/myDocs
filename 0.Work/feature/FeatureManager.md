
# Feature list

```cpp
using FeaturePtr = std::shared_ptr<FeatureIF>;
using Features = std::list<std::pair<FeatureDesc, FeaturePtr>>;
Features _features;
```

In the `FeatureManager` class, there are several operations that involve adding or removing features from the `_features` container, such as `addFeature`, `removeFeature`, and the loop in `init` that iterates over `_features` and potentially removes elements.

The choice of using `std::list` for the `_features` member in the `FeatureManager` class is likely due to the need for efficient insertion and removal of elements from the middle of the container. Unlike `std::vector`, which is optimized for contiguous memory access and efficient random access, `std::list` is a doubly-linked list that allows for constant-time insertion and removal of elements at any position.

If the primary operations on `_features` involve iterating over the elements or accessing them by index, `std::vector` might be a better choice. However, if the frequent operations involve inserting or removing elements from the middle of the container, `std::list` is a more appropriate choice to maintain efficient performance.

However, it's worth noting that `std::list` has some drawbacks compared to `std::vector`, such as higher memory overhead due to the additional pointers required for the linked list structure, and potentially slower iteration due to non-contiguous memory access. Therefore, the choice of using `std::list` for `_features` is likely a trade-off between efficient insertion/removal and potentially slower iteration or higher memory usage.

# init()
```cpp
  using FeaturePtr = std::shared_ptr<FeatureIF>;
```
1. `if (isBuiltIn(it->first.name) || shared::featureInstalled(it->first.name))`  it attempts to initialize the feature by calling `it->second->init()`, where `it->second` is the `FeaturePtr` (shared pointer to the `FeatureIF` object).
2. If the initialization of the feature fails (`it->second->init()` returns false), it logs a message indicating the failure and removes the feature from the `_features` list using `_features.erase(it)`. The `continue` statement is used to move to the next iteration of the loop.
3. After iterating over all the features, it sets the `currentState_` to `State::Initialized`

