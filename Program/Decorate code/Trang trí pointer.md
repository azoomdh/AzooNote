```cpp
int x =10;

int* pointer1;

pointer1 = &x;

cout<< *pointer1 << endl;
```

<br/>

```cpp
int x =10;

PointerType(int) pointer1;

pointer1 = PointerAddress(x)

cout<< PointerValue(pointer1) << endl; 
```

<br/>

nhờ GPT

```cpp
#define PointerType(T)    T*
#define PointerAddress(x) (&(x))
#define PointerValue(x)   (*(x))
#define PointerNull(T)    ((T*)0)
```

<br/>

ví dụ

```cpp
PointerType(PointerType(int)) pointer1;

PointerValue(PointerValue(pointer1)) = 50;

int a= 60;
pointer1 = &&a;
// trông xấu, chỉ dùng được với pointer 1 cấp

// C không có class = không dùng pointer1.pointerValue().pointerValue();
```
