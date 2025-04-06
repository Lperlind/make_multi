# Make Multi
Allocate multiple things in one procedure call. This reduces the number of errors you need to handle when allocating data.

The code is all auto-generated and can support up to 20 separate allocations.

## Usage
Check it out on godbolt: https://godbolt.org/z/4qGPcqGer
```odin
Foo :: struct {
    a, b, c: int
}

// NOTE(lucas): allocate an int slice, byte slice and a pointer to foo in one call
base_ptr, int_slice, byte_slice, foo_ptr, err := make_multi(
    Make_Multi([]int) { len = 20 }, // will have natural alignment
    Make_Multi([]byte) { len = 200, alignment = 64 }, // custom alignment
    Make_Multi(^Foo) { alignment = 4096 }, // you can have pointers too, which also can be aligned
    context.allocator
)
// NOTE(lucas): only one error check needed for all allocations, good for
// when you can't use an arena or an allocator that is trivial to free data with
if err != nil {
    return
}
// NOTE(lucas): free everything, again great when you cannot use an arena
free(base_ptr, context.allocator)

```
