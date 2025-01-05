---
aliases:
  - building from source
---
# Build

```sh
 git clone https://github.com/gabime/spdlog.git
 cd spdlog && mkdir build && cd build
 cmake .. && cmake --build .
```

build with Debug option

```sh
 cd spdlog && mkdir build
 cmake -B build/default -DCMAKE_BUILD_TYPE=Debug -H.
 cmake --build build/default -j8
```

#build
#building
#library
#source

---