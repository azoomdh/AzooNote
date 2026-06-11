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
void safe_set (T* struct1, T* struct2) {
    free_struct(struct1);
    struct1 = struct2;
}
```

```c
sEmployee *e 
    buf __attribute__((cleanup(free_struct)))
    = malloc(sizeof(Employee));

Department *d  
    buf __attribute__((cleanup(free_struct)))
    = malloc(sizeof(Department));

e->dept = d;
d->manager = e;

safe_set(e, nullptr);
safe_set(d, nullptr);
```

```c
// GPT
#define safe_set(t1, t2) \
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

    // push back thường
    notSafe_pushback(listLog, log1)

    // xử lý ...
}
```

## leak thư viện thứ 3

(không làm được)

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

//# freeContext_unRegisterCallback ...

//# free_thread

//# free_struct
```

## leak bởi call back

code

```c
void foo() {
    Context* ctx = malloc(sizeof(Context));

    register_callback(on_event, ctx);

    // quên:
    // unregister_callback(...)
    // free(ctx)
}
```

```c
 Context* ctx 
     buf __attribute__((cleanup(freeContext_unRegisterCallback)))
     = malloc(sizeof(Context));
```

## leak do không tắt thread

dòng có chữ `free_thread`
