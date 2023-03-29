## Annotation In Spring MVC

같은 리소스라도 다른 메서드를 매핑하면 다른 작업을 수행하게 할 수 있다.

### @RequestMapping

HTTP 요청에 대한 URL 경로 설정에 사용되며 특히 Controller에 사용하여 해당 컨트롤러에 공통적으로 사용되는 리소스 일부를 명시할 수 있다.
Get, Post, Patch, Delete 등을 method 속성 값으로 받아 해당 요청을 수행할 수 있고, 직접 Mapping 앞에 메서드를 명시하여 처리할 수도 있다.

### @GetMapping

HTTP GET 요청 메서드를 처리하는 애너테이션이다.

### @PostMapping

HTTP POST 요청 메서드를 처리하는 애너테이션이다.

### @PatchMapping

HTTP PATCH 요청 메서드를 처리하는 애너테이션이다.

### @DeleteMapping

HTTP Delete 요청 메서드를 처리하는 애너테이션이다.

### @PathVariable

URI에 명시되는 파라미터를 {parameter} 형식으로 추출하여 @PathVariable 애너테이션을 통해 컨트롤러에서 사용할 수 있다.

#### @RequestParam

?key=value 형태로 작성된 URL에서 **쿼리** 를 통해 key 값에 해당하는 값을 추출하여 매핑한다. 
즉 PathVariable는 변수 값을 추출하고 RequestParam은 쿼리 파라미터를 추출하여 파라미터로 사용한다.

### @RequestBody

HTTP 요청 파일을 자바 객체로 변환하여 컨트롤러의 메서드 매개변수로 전달할 수 있게 하는 애너테이션이다.
```
// json
{
  "name": "John",
  "age": 30
}
```

```
// User 객체 형식의 변수로 전달 됨.

@PostMapping("/users")
public ResponseEntity<User> createUser(@RequestBody User user) {
  userService.createUser(user);
  return ResponseEntity.ok(user);
}

```
### @ExceptionHandler

예외 처리를 위한 애너테이션으로, 예외 타입을 지정하여 해당 예외가 발생시 지정한 메서드를 통해 처리할 때 사용한다.
```
// UserNotFound 예외가 발생했을 때 실행하는 메서드 
  @ExceptionHandler(UserNotFoundException.class)
  @ResponseStatus(HttpStatus.NOT_FOUND)
  public void handleUserNotFoundException() {
    // Handle UserNotFoundException when thrown from this controller
  }
```

### @ResponseStatus

HTTP 응답 코드를 지정하기 위한 애너테이션으로, 개발자 마음대로 응답 코드를 수정해서 반환할 수 있다.

```   
/* /question 리소스를 공통으로 가지는 요청을 처리하는 컨트롤러에 RequestMapping을 붙였고, 
* /list/{category}에 대한 GET요청을 매핑하며, {category} 파라미터를 변수로 받고, URL의 query 내 page 값을 받아 변수로 사용한다.
*/
@RequestMapping("/question")
@RequiredArgsConstructor
@Controller
public class QuestionController {
   @GetMapping("/list/{category}")
    public String list(Model model, @RequestParam(value = "page", defaultValue = "0") int page,
                       @RequestParam(value = "kw", defaultValue = "") String kw,
                       @PathVariable(required = false) String category,
                       @RequestParam(defaultValue = "id", value = "sort") String sort) {
        Page<Question> paging = this.questionService.getList(page, kw, category, sort);
        model.addAttribute("paging", paging);
        model.addAttribute("kw", kw);
        return "question_list";
    }
    }
```