# giả raii

(GPT) GCC __attribute__((cleanup)) (gần giống RAII)

```c
// đây là thư viện

void free_ptr(void *p) {
    free(*(void**)p);
}
```

```c
// đây là file khác
void foo() {
    char *buf __attribute__((cleanup(free_ptr)))
        = malloc(100);

    // tự free khi rời scope

}
```

# list

## vòng tham chiếu

ví dụ

```c
Employee *e = malloc(sizeof(Employee));
Department *d = malloc(sizeof(Department));

e->dept = d;
d->manager = e;

// bên dưới là leak
e = NULL;
d = NULL;
```

code

```c
// đây là thư viện

// các hàm free bên trên

// free_struct ...

void set (T* struct1, T* struct2) {
    free_struct(struct1);
    struct1 = struct2;
}
```

```c
sEmployee *e = malloc(sizeof(Employee));
Department *d = malloc(sizeof(Department));

e->dept = d;
d->manager = e;

set(e, nullptr);
set(d, nullptr);
```

tương tự

```c
// GPT

#define set(t1, t2) \
    do {            \
        free(t1);   \
        (t1) = (t2);\
    } while (0)
```

## biến lưu dữ liệu tăng liên tục

chèn dữ liệu ở "lần gọi hàm mới" : dòng có chữ `free_collection`

chèn dữ liệu ở "vòng lặp mới" : 

```c
auto listLog;

while(true){
	auto listLog_Temp = ...
    
    // xóa rồi pushBack
    safe_PushBack(listLog, listLog_Temp);
    
    // xử lý ...
}
```

## leak thư viện thứ 3

## đóng file, socket, mutex, connect-to-db, gpu-mem

```c
// đây là thư viện

void free_ptr(void *p) {
    free(*(void**)p);
}

//# void free_socket...

//# void free_dbConnect ...

//# void free_collection...

//# void free_resource ... (??? nếu có thể phân biệt tài nguyên)
```

## leak bởi call back

## leak do không tắt thread
