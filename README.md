# CMake build system for Blossom-V with patches


Vladimir Kolmogorov published 
[a C++ implementation](https://pub.ista.ac.at/~vnk/software.html#BLOSSOM5) of his
*Blossom-V* algorithm for solving minimum-cost perfect matching in graphs.

[Vladimir Kolmogorov. "Blossom V: A new implementation of a minimum cost perfect matching algorithm." In Mathematical Programming Computation (MPC), July 2009, 1(1):43-67.](https://pub.ista.ac.at/~vnk/papers/blossom5.pdf)

That code is **not** under a free license, but available for evaluation and research purposes.
A commercial version is available at <https://xip.uclb.com/product/BlossomV>.
Notably, redistribution of the code is not permitted, thus this repository only
includes a patch file with our changes.

We also include a `CMakeLists.txt` for seamless integration into CMake based projects
that will automatically download and patch the source.

Note that the GEOM module is not compiled, it was not necessary for our uses.
We'll happily accept well-tested pull requests though.

## Patch contents

- Wrap everything in `namespace BlossomV` to avoid namespace pollution
- Use `int64_t` for the cost type `REAL` instead of the original `int`.
    - Change some `int*` weight params that should be `REAL*` for compatibility with non-int `REAL`
    - Instantiate `MinCost` and `DualMinCost` for `int64_t`

## Usage

```
add_subdirectory(path/to/blossom5-cmake)
target_link_libraries(your_target PRIVATE Blossom5::Blossom5)
```
By default, Blossom-V is built as static libray, but the `BUILD_SHARED_LIBS` cmake option
allows creating a shared library instead.

**Note: Be sure to obey the terms of the Blossom-V license!**

## License

The contents of this repository (excluding Blossom-V itself), e.g., our contributions,
are available under the [**Unlicense**](https://unlicense.org/) -- i.e., public domain,
do with it whatever you like :-)
We'd be very happy about a brief email if you do use it though :-)

## Contact

Author: Martin Heistermann <martin.heistermann@unibe.ch>

