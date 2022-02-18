


> Written with [StackEdit](https://stackedit.io/).
# Restful API + @RestController + @PathVariable + @RequestBody
### **@RestController**

Khác với `@Controller` là sẽ trả về một template.

`@RestController` trả về dữ liệu dưới dạng JSON.
```java
@RestController
@RequestMapping("/api/v1")
public class RestApiController{

    @GetMapping("/todo")
    public List<Todo> getTodoList() {
        return todoList;
    }
}
```
Các đối tượng trả về dưới dạng Object sẽ được **Spring Boot** chuyển thành JSON.

Các đối tượng trả về rất đa dạng, bạn có thể trả về `List`, `Map`, v.v.. **Spring Boot** sẽ convert hết chúng thành JSON, mặc định sẽ dùng Jackson converter để làm điều đó.

Nếu bạn muốn API tùy biến được kiểu dữ liệu trả về, bạn có thể trả về đối tượng `ResponseEntity` của **Spring** cung cấp. Đây là đối tượng cha của mọi response và sẽ wrapper các object trả về. Cái này bạn xem tiếp phần dưới sẽ rõ.

### **@RequestBody**

Vì bây giờ chúng ta xây dựng API, nên các thông tin từ phía **Client** gửi lên **Server** sẽ nằm trong `Body`, và cũng dưới dạng `JSON` luôn.

```java
@RestController
@RequestMapping("/api/v1")
public class RestApiController {

    List<Todo> todoList = new CopyOnWriteArrayList<>();

    @PostMapping("/todo")
    public ResponseEntity addTodo(@RequestBody Todo todo) {
        todoList.add(todo);
        // Trả về response với STATUS CODE = 200 (OK)
        // Body sẽ chứa thông tin về đối tượng todo vừa được tạo.
        return ResponseEntity.ok().body(todo);
    }

}
```
Tất nhiên là **Spring Boot** sẽ làm giúp chúng ta các phần nặng nhọc, nó chuyển chuỗi JSON trong request thành một Object Java. bạn chỉ cần cho nó biết cần chuyển JSON thành Object nào bằng Annotation `@RequestBody`
### **@PathVariable**

`RESTful API` là một tiêu chuẩn dùng trong việc thết kế các thiết kế API cho các ứng dụng web để quản lý các resource.

Và với cách thống nhất này, thì sẽ có một phần thông tin quan trọng sẽ nằm ngay trong chính URL của api.

Ví dụ:

1.  URL tạo To-do: [https://loda.me/todo](https://loda.me/). Tương ứng với HTTP method là POST
2.  URL lấy thông tin To-do số 12: [https://loda.me/todo/12](https://loda.me/). Tương ứng với HTTP method là GET
3.  URL sửa thông tin To-do số 12: [https://loda.me/todo/12](https://loda.me/). Tương ứng với HTTP method là PUT
4.  URL xoá To-do số 12: [https://loda.me/todo/12](https://loda.me/). Tương ứng với HTTP method là DELETE

Ngoài thông tin trong `Body` của request, thì cái chúng ta cần chính là cái con số 12 nằm trong URL. Phải lấy được con số đó thì mới biết được đối tượng To-do cần thao tác là gì.

`@PathVariable` tham chiến.

```java
@RestController
@RequestMapping("/api/v1")
public class RestApiController {

    /*
    phần path URL bạn muốn lấy thông tin sẽ để trong ngoặc kép {}
     */
    @GetMapping("/todo/{todoId}")
    public Todo getTodo(@PathVariable(name = "todoId") Integer todoId){
        // @PathVariable lấy ra thông tin trong URL
        // dựa vào tên của thuộc tính đã định nghĩa trong ngoặc kép /todo/{todoId}
        return todoList.get(todoId);
    }
}
```
