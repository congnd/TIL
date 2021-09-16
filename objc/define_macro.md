### Embed pragma
```C
#define STRINGIFY(a) #a
#define DEFINE_DELETE_OBJECT(type)                      \
    type delete_ ## type ## _(int handle);                  \
    void delete_ ## type(int handle);                   \
    _Pragma( STRINGIFY( weak delete_ ## type ## _ = delete_ ## type) )
DEFINE_DELETE_OBJECT(foo);
```

When using `gcc -E` you will get this:
```
foo delete_foo_(int handle); void delete_foo(int handle);
#pragma weak delete_foo_ = delete_foo
```
