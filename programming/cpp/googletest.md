---
title: Google Tet
---

## Google Test
Google testはTesterとMockの2つからなる。
Google MockとGoogle Test（Test Runner）のどちらか一方のみを使うこともできる。

## Google TestでProfileやdebugをする
google testをcmakeでbuildする場合に、デフォルトではdebug用のsymbolの出力がされない。
幾つかのprofilerやdebuggerではdebug symbolが必須なので、`-g`つきでビルドする方法を記す。

一番簡単な方法は、`CMakeLists.txt`を編集し、`-g`オプションをつける方法である。
Google Testのsourceを取得するたびに、`CMakeLists.txt`を書き換えるのは面倒なので、`cmake`実行時に`-g`を付加するのが理想的である。

* `CMAKE_BUILD_TYPE`をDEBUGに設定することで、DEBUG用のcompileオプションを付加できる
* `CMAKE_CXX_FLAGS_DEBUG`がdebug用のcompilerオプションで、上記の`CMAKE_BUILD_TYPE`がDEBUGの時にのみ利用されるオプションである。

```
cmake -D CMAKE_BUILD_TYPE="Debug" -D CMAKE_CXX_FLAGS_DEBUG="-g -Wall" .
```

## gitのrepositoryにsubmoduleとして追加

```sh
mkdir submodule
cd submodule
git submodule add https://github.com/google/googletest.git googletest
git commit -m "Add googltetest as git-submodule
```

CMakeを使っている場合は以下を追加する。

```cmake
#GTEST and GMOCK build
# SET(BUILD_GTEST ON CACHE BOOL "Use some options")
add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/submodule/googletest)

# include directory
include_directories(
    ${CMAKE_CURRENT_SOURCE_DIR}/submodule/googletest/googlemock/include
    ${CMAKE_CURRENT_SOURCE_DIR}/submodule/googletest/googletest/include
)
```

## Reference
* [GitHub - google/googletest: Google Test](https://github.com/google/googletest)