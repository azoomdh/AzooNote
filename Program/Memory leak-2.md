Có khá nhiều trường hợp bản chất không gắn với scope của hàm nên giả RAII không giải quyết được.

### 1. Đưa dữ liệu vào cache toàn cục

```c
void load_user()
{
    User *u = malloc(sizeof(User));

    cache_add(u);
}
```

`u` vẫn được cache giữ lại sau khi hàm kết thúc.

Nếu RAII tự `free(u)` khi thoát hàm thì cache sẽ chứa con trỏ treo (dangling pointer).

---

### 2. Tạo thread nền

```c
void start_worker()
{
    pthread_t tid;
    pthread_create(&tid, NULL, worker, NULL);
}
```

Thread tiếp tục chạy sau khi `start_worker()` kết thúc.

RAII không thể tự `pthread_join()` khi thoát hàm nếu mục tiêu là thread chạy lâu dài.

---

### 3. Đăng ký object vào container toàn cục

```c
void create_session()
{
    Session *s = malloc(sizeof(Session));

    session_manager_add(s);
}
```

Sau khi hàm kết thúc, `session_manager` vẫn giữ `s`.

RAII không biết khi nào session thực sự hết hạn.

---

### 4. Cấu trúc dữ liệu tự tăng mãi

```c
void process_request()
{
    log_store_add(new_log());
}
```

`log_store` có thể giữ hàng triệu log.

Không có leak theo nghĩa mất con trỏ, nhưng bộ nhớ tăng mãi (logical leak).

RAII không phát hiện được vì mọi object vẫn còn tham chiếu hợp lệ.

---

### 5. Vòng tham chiếu với refcount

```c
A -> B
B -> A
```

Cả hai đều có refcount = 1.

RAII của từng scope hoạt động đúng, nhưng hai object không bao giờ về 0 để giải phóng.

---

Điểm chung của các trường hợp này là tài nguyên vẫn còn được tham chiếu hợp lệ sau khi hàm kết thúc. RAII chỉ quản lý tốt tài nguyên có vòng đời trùng với scope; còn khi vòng đời thuộc về cache, manager, thread, event loop hoặc hệ thống khác thì phải có cơ chế quản lý vòng đời riêng.

<br/><hr/>
<br/>

1 file = 1 class

biến toàn cục = thuộc tính của class

biến cục bộ trong hàm = không được phép tồn tại

hàm dọn dẹp biến toàn cục

dùng safe_set với mọi biến

các hàm khác

<br/>

các biến nằm trong struct = thuộc tính của class
(để gọi bên ngoài đỡ nhầm lẫn)

```
typedef struct ProcessA_Context {
    int *val1;
    char *val2;
    // ...
};

ProcessA_Context processA_context =  // ???

void processA_cleanup() {
	free(processAContext.val1)
    free(processAContext.val2)
    // ...
}

void processA_start(){ }

void processA_stop() { }

void safe_set(int* val1, int* val1B){
	free(val1);
    val1=val1B;
}
```



<br/>
<hr/>
<br/>
