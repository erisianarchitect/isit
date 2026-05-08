`isit` is a compile time checking library. It has a multitude of functions for performing compile time checks to ensure that your program will work as expected before you ever run it.

# Examples

### `const_assert`
```rust
#[repr(C)]
struct Fred {
    foo: u32,
    bar: u64,
    baz: bool,
}
const _: () = isit::const_assert(std::mem::offset_of!(Fred, bar) == 8);
```

### `const_assert_all`
```rust
#[repr(C)]
struct Fred {
    foo: u32,
    bar: u64,
    baz: bool,
}
const _: () = isit::const_assert_all([
    std::mem::size_of::<Fred>() == 24,
    std::mem::offset_of!(Fred, foo) == 0,
    std::mem::offset_of!(Fred, bar) == 8,
    std::mem::offset_of!(Fred, baz) == 16,
]);
```

### `const_assert_any`
```rust
#[repr(C)]
struct Fred {
    foo: u32,
    bar: usize,
    baz: bool,
}
const _: () = isit::const_assert_any([
    // 64-bit systems
    std::mem::size_of::<Fred>() == 24,
    // 32-bit systems
    std::mem::size_of::<Fred>() == 12,
    // 16-bit systems
    std::mem::size_of::<Fred>() == 8,
]);
```

### `const_assert_none`
```rust
#[repr(C)]
struct Fred {
    foo: u32,
    bar: u64,
    baz: bool,
}
const _: () = isit::const_assert_none([
    // align is 8, so none of these will be true.
    std::mem::align_of::<Fred>() == 1,
    std::mem::align_of::<Fred>() == 2,
    std::mem::align_of::<Fred>() == 4,
]);
```

### `assert_size`
```rust
#[repr(C)]
struct Fred {
    foo: u32,
    bar: u64,
    baz: bool,
}
const _: () = isit::assert_size::<Fred, 24>();
```

### `assert_not_size`
```rust
#[repr(C)]
struct Fred {
    foo: u32,
    bar: u64,
    baz: bool,
}
// If the type were not `C` layout, and were instead `Rust`
// layout, the size would end up being 16 instead of 24, and
// this check would fail.
const _: () = isit::assert_not_size::<Fred, 16>();
```

### `assert_same_size`
```rust
#[repr(C)]
struct Fred {
    foo: u32,
    bar: u64,
    baz: bool,
}
const _: () = isit::assert_same_size::<Fred, [u64; 3]>();
```

### `assert_different_size`
```rust
#[repr(C)]
struct Fred {
    foo: u32,
    bar: u64,
    baz: bool,
}
/// 

#[repr(C)]
struct NotFred {
    bar: u64,
    foo: u32,
    baz: bool,
}
const _: () = isit::assert_different_size::<Fred, NotFred>();
```

### `assert_compatible_size`
```rust
#[repr(C)]
struct Fred {
    foo: u32,
    bar: u64,
    baz: bool,
}
const _: () = isit::assert_compatible_size::<[u64; 1], Fred>();
const _: () = isit::assert_compatible_size::<[u64; 2], Fred>();
const _: () = isit::assert_compatible_size::<[u64; 3], Fred>();
```

### `assert_incompatible_size`
```rust
#[repr(C)]
struct Fred {
    foo: u32,
    bar: u64,
    baz: bool,
}
const _: () = isit::assert_incompatible_size::<Fred, [u64; 2]>();
```

### `assert_align`
```rust
#[repr(C)]
struct Fred {
    foo: u32,
    bar: u64,
    baz: bool,
}
const _: () = isit::assert_align::<Fred, 8>();
```

### `assert_not_align`
```rust
#[repr(C)]
struct Fred {
    foo: u32,
    bar: u64,
    baz: bool,
}
const _: () = isit::assert_not_align::<Fred, 4>();
const _: () = isit::assert_not_align::<Fred, 16>();
```

### `assert_same_align`
```rust
#[repr(C)]
struct Fred {
    foo: u32,
    bar: u64,
    baz: bool,
}
const _: () = isit::assert_same_align::<Fred, u64>();
```

### `assert_different_align`
```rust
#[repr(C)]
struct Fred {
    foo: u32,
    bar: u64,
    baz: bool,
}
const _: () = isit::assert_different_align::<Fred, u32>();
const _: () = isit::assert_different_align::<Fred, bool>();
```

### `assert_compatible_align`
```rust
#[repr(C)]
struct Fred {
    foo: u32,
    bar: u64,
    baz: bool,
}
const _: () = isit::assert_compatible_align::<Fred, u64>();
```

### `assert_incompatible_align`
```rust
#[repr(C)]
struct Fred {
    foo: u32,
    bar: u64,
    baz: bool,
}
const _: () = isit::assert_incompatible_align::<Fred, u32>();
const _: () = isit::assert_incompatible_align::<Fred, bool>();
```

### `assert_size_align`
```rust
#[repr(C)]
struct Fred {
    foo: u32,
    bar: u64,
    baz: bool,
}
const _: () = isit::assert_size_align::<Fred, 24, 8>();
```

### `assert_not_size_align`
```rust
#[repr(C)]
struct Fred {
    foo: u32,
    bar: u64,
    baz: bool,
}
const _: () = isit::assert_not_size_align::<Fred, 16, 4>();
```

### `assert_same_size_align`
```rust
#[repr(C)]
struct Fred {
    foo: u32,
    bar: u64,
    baz: bool,
}
const _: () = isit::assert_same_size_align::<Fred, [u64; 3]>();
```

### `assert_different_size_align`
```rust
#[repr(C)]
struct Fred {
    foo: u32,
    bar: u64,
    baz: bool,
}
const _: () = isit::assert_different_size_align::<Fred, [u32; 4]>();
```

### `assert_compatible_size_align`
```rust
#[repr(C)]
struct Fred {
    foo: u32,
    bar: u64,
    baz: bool,
}
const _: () = isit::assert_compatible_size_align::<[u32; 3], Fred>();
```

### `assert_incompatible_size_algin`
```rust
#[repr(C)]
struct Fred {
    foo: u32,
    bar: u64,
    baz: bool,
}
const _: () = isit::assert_incompatible_size_align::<Fred, u32>();
```

### `assert_pointer_size`
```rust
use std::ptr::NonNull;
#[repr(transparent)]
struct Foo {
    ptr: NonNull<u8>,
}
const _: () = isit::assert_pointer_size::<Foo>();
```

### `assert_not_pointer_size`
```rust
#[repr(C)]
struct Foo {
    a: bool,
    b: u16,
    c: bool,
    d: u16,
    e: bool,
}
const _: () = isit::assert_not_pointer_size::<Foo>();
```

### `assert_pointer_align`
```rust
#[cfg_attr(target_pointer_width = "64", repr(C, align(8)))]
#[cfg_attr(target_pointer_width = "32", repr(C, align(4)))]
#[cfg_attr(target_pointer_width = "16", repr(C, align(2)))]
struct Foo(usize);
const _: () = isit::assert_pointer_align::<Foo>();
```

### `assert_not_pointer_align`
```rust
#[repr(transparent)]
struct Foo {
    inner: u8,
}
const _: () = isit::assert_not_pointer_align::<Foo>();
```

### `assert_pointer_size_align`
```rust
use std::ptr::NonNull;
/// 

#[repr(transparent)]
struct Foo(NonNull<Foo>);
const _: () = isit::assert_pointer_size_align::<Foo>();
```

### `assert_not_pointer_size_align`
```rust
#[repr(transparent)]
struct Foo(u8);
const _: () = isit::assert_not_pointer_size_align::<Foo>();
```

### `assert_pointer_compatible_size_align`
```rust
#[repr(transparent)]
struct Foo(u8);
const _: () = isit::assert_pointer_compatible_size_align::<Foo>();
```

### `assert_pointer_incompatible_size_align`
```rust
#[repr(C, align(16))]
struct Fred {
    foo: u32,
    bar: u64,
    baz: bool,
}
const _: () = isit::assert_pointer_incompatible_size_align::<Fred>();
```

### `assert_t_niche`
```rust
use std::ptr::NonNull;
/// 

#[repr(transparent)]
struct Foo {
    ptr: NonNull<Foo>,
}
const _: () = isit::assert_t_niche::<Foo, NonNull<Foo>>();
```

### `assert_no_t_niche`
```rust
#[repr(transparent)]
struct Foo {
    foo: u32,
}
const _: () = isit::assert_no_t_niche::<Foo, u32>();
```

### `assert_t_niche_compatible`
```rust
use std::num::NonZero;
#[repr(transparent)]
struct Foo(NonZero<u8>);
const _: () = isit::assert_t_niche_compatible::<Foo, NonZero<u8>>();
const _: () = isit::assert_t_niche_compatible::<Foo, NonZero<u16>>();
```

### `assert_niche`
```rust
use std::ptr::NonNull;
/// 

#[repr(transparent)]
struct Foo(NonNull<Foo>);
const _: () = isit::assert_niche::<Foo>();
```

### `assert_no_niche`
```rust
#[repr(transparent)]
struct Foo(u64);
const _: () = isit::assert_no_niche::<Foo>();
```

### `assert_single_niche`
```rust
use std::ptr::NonNull;
/// 

#[repr(transparent)]
struct Foo(NonNull<Foo>);
const _: () = isit::assert_single_niche::<Foo>();
```
### `assert_pointer_niche`
```rust
use std::ptr::NonNull;
/// 

#[repr(transparent)]
struct Foo(NonNull<Foo>);
const _: () = isit::assert_pointer_niche::<Foo>();
```

### `assert_no_pointer_niche`
```rust
#[repr(transparent)]
struct Foo(usize);
const _: () = isit::assert_no_pointer_niche::<Foo>();
```

### `assert_pointer_niche_compatible`
```rust
use std::num::NonZero;
#[repr(transparent)]
struct Foo(NonZero<u128>);
const _: () = isit::assert_pointer_niche_compatible::<Foo>();
```

### `assert_u8_niche`
```rust
#[repr(u8)]
enum Fred {
    Foo,
    Bar,
    Baz,
}
const _: () = isit::assert_u8_niche::<Fred>();
```

### `assert_u8_size_align`
```rust
#[repr(transparent)]
struct Foo(u8);
const _: () = isit::assert_u8_size_align::<Foo>();
```

### `assert_u8_compatible`
```rust
struct Zst;
#[repr(transparent)]
struct U8(u8);
const _: () = isit::assert_u8_compatible::<Zst>();
const _: () = isit::assert_u8_compatible::<U8>();
```

### `assert_u8_incompatible`
```rust
#[repr(transparent)]
struct Foo(u16);
const _: () = isit::assert_u8_incompatible::<Foo>();
```

### `assert_zst`
```rust
struct Zst;
const _: () = isit::assert_zst::<Zst>();
const _: () = isit::assert_zst::<()>();
const _: () = isit::assert_zst::<[[u64; u16::MAX as usize]; 0]>();
```

### `assert_not_zst`
```rust
struct NonZst([u64; u16::MAX as usize]);
const _: () = isit::assert_not_zst::<NonZst>();
```

### `assert_uninhabited_zst`
```rust
enum Uninhabited {}
struct Holder<T>(T);
const _: () = isit::assert_uninhabited_zst::<Uninhabited>();
const _: () = isit::assert_uninhabited_zst::<(Uninhabited, Uninhabited)>();
const _: () = isit::assert_uninhabited_zst::<[Uninhabited; usize::MAX]>();
const _: () = isit::assert_uninhabited_zst::<Holder<Uninhabited>>();
const _: () = isit::assert_uninhabited_zst::<Holder<[Uninhabited; usize::MAX]>>();
const _: () = isit::assert_uninhabited_zst::<Holder<[[Uninhabited; usize::MAX]; usize::MAX]>>();
const _: () = isit::assert_uninhabited_zst::<Holder<[[[Uninhabited; usize::MAX]; usize::MAX]; usize::MAX]>>();
const _: () = isit::assert_uninhabited_zst::<Holder<(
    Holder<[[[Uninhabited; usize::MAX]; usize::MAX]; usize::MAX]>,
    Holder<[[[Uninhabited; usize::MAX]; usize::MAX]; usize::MAX]>,
)>>();
```

### `assert_not_uninhabited_zst`
```rust
struct Foo(u8);
const _: () = isit::assert_not_uninhabited_zst::<Foo>();
```