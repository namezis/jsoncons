### jsoncons::jsonpointer::add

Adds a `json` value.

#### Header
```c++
#include <jsoncons_ext/jsonpointer/jsonpointer.hpp>

template<class Json>
jsonpointer_errc add(const Json& root, typename Json::string_view_type path, const Json& value); 
```

#### Return value

On success, a value-initialized [jsonpointer_errc](jsonpointer_errc.md). 

On error, a [jsonpointer_errc](jsonpointer_errc.md) error code 

### Examples

#### Add a member to a target location that does not already exist

```c++
#include <jsoncons/json.hpp>
#include <jsoncons_ext/jsonpointer/jsonpointer.hpp>

using namespace jsoncons;

int main()
{
    json target = json::parse(R"(
        { "foo": "bar"}
    )");

    auto ec = jsonpointer::add(target, "/baz", json("qux"));
    if (ec == jsonpointer::jsonpointer_errc())
    {
        std::cout << target << std::endl;
    }
    else
    {
        std::cout << make_error_code(ec).message() << std::endl;
    }
}
```
Output:
```json
{"baz":"qux","foo":"bar"}
```

#### Add an element to the second position in an array

```c++
#include <jsoncons/json.hpp>
#include <jsoncons_ext/jsonpointer/jsonpointer.hpp>

using namespace jsoncons;

int main()
{
    json target = json::parse(R"(
        { "foo": [ "bar", "baz" ] }
    )");

    auto ec = jsonpointer::add(target, "/foo/1", json("qux"));
    if (ec == jsonpointer::jsonpointer_errc())
    {
        std::cout << target << std::endl;
    }
    else
    {
        std::cout << make_error_code(ec).message() << std::endl;
    }
}
```
Output:
```json
{"foo":["bar","qux","baz"]}
```

#### Add a value to the end of an array

```c++
#include <jsoncons/json.hpp>
#include <jsoncons_ext/jsonpointer/jsonpointer.hpp>

using namespace jsoncons;

int main()
{
    json target = json::parse(R"(
        { "foo": [ "bar", "baz" ] }
    )");

    auto ec = jsonpointer::add(target, "/foo/-", json("qux"));
    if (ec == jsonpointer::jsonpointer_errc())
    {
        std::cout << target << std::endl;
    }
    else
    {
        std::cout << make_error_code(ec).message() << std::endl;
    }
}
```
Output:
```json
{"foo":["bar","baz","qux"]}
```

#### Add an object member to a location that already exists

```c++
#include <jsoncons/json.hpp>
#include <jsoncons_ext/jsonpointer/jsonpointer.hpp>

using namespace jsoncons;

int main()
{
    json target = json::parse(R"(
        { "foo": "bar", "baz" : "abc"}
    )");

    auto ec = jsonpointer::add(target, "/baz", json("qux"));
    if (ec == jsonpointer::jsonpointer_errc())
    {
        std::cout << target << std::endl;
    }
    else
    {
        std::cout << make_error_code(ec).message() << std::endl;
    }
}
```
Output:
```json
{"baz":"qux","foo":"bar"}
```

#### Add a value to a location in an array that exceeds the size of the array

```c++
#include <jsoncons/json.hpp>
#include <jsoncons_ext/jsonpointer/jsonpointer.hpp>

using namespace jsoncons;

int main()
{
    json target = json::parse(R"(
        { "foo": [ "bar", "baz" ] }
    )");

    auto ec = jsonpointer::add(target, "/foo/3", json("qux"));
    if (ec == jsonpointer::jsonpointer_errc())
    {
        std::cout << target << std::endl;
    }
    else
    {
        std::cout << make_error_code(ec).message() << std::endl;
    }
}
```
Output:
```
Index exceeds array size
```
