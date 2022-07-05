---
title: "Multipart form data"
date: 2022-06-29T11:34:00+09:00
categories:
  - Memo
tags:
  - HTTP
  - multipart form data
# header:
#   teaser: /assets/images/code.jpg
---

## Multipart form data

The ENCTYPE attribute of `<form>` tag specifies the method of encoding for the form data. It is one of the two ways of encoding the HTML form. It is specifically used when file uploading is required in HTML form. It sends the form data to server in multiple parts because of large size of file.

```html
<form action="login.php" method="post" 
    enctype="multipart/form-data">
</form>
```

## Multipart request handling in Spring

Multipart requests consist of sending data of many different types separated by a boundary as part of a single HTTP method call. Generally, we can send complicated JSON, XML, or CSV data, as well as transfer multipart file(s) in this request.

### Using `@ModelAttribute` when sending an employee's data consisting of a name and file using a form

- create an Employee abstraction to store the form data:

```java
public class Employee {
    private String name;
    private MultipartFile document;
}
```

- generate the form using Thymeleaf:
- declare the enctype as multipart/form-data in the view.

```javascript
<form action="#" th:action="@{/employee}" th:object="${employee}" method="post" enctype="multipart/form-data">
    <p>name: <input type="text" th:field="*{name}" /></p>
    <p>document:<input type="file" th:field="*{document}" multiple="multiple"/>
    <input type="submit" value="upload" />
    <input type="reset" value="Reset" /></p>
</form>
```

- create a method that accepts the form data, including the multipart file:
- consumes attribute value is set to multipart/form-data
- `@ModelAttribute` has captured all the form data into the Employee POJO, including the uploaded file

```java
@RequestMapping(path = "/employee", method = POST, consumes = { MediaType.MULTIPART_FORM_DATA_VALUE })
public String saveEmployee(@ModelAttribute Employee employee) {
    employeeService.save(employee);
    return "employee/success";
}
```

### Using `@RequestPart`

- This annotation associates a part of a multipart request with the method argument, which is useful for sending complex multi-attribute data as payload, e.g., JSON or XML.
- Let's create a method with two arguments, first of type Employee and second as MultipartFile. Furthermore, we'll annotate both of these arguments with @RequestPart:

```java
@RequestMapping(path = "/requestpart/employee", method = POST, consumes = { MediaType.MULTIPART_FORM_DATA_VALUE })
public ResponseEntity<Object> saveEmployee(@RequestPart Employee employee, @RequestPart MultipartFile document) {
    employee.setDocument(document);
    employeeService.save(employee);
    return ResponseEntity.ok().build();
}
```

- to see this annotation in action, we'll create the test using MockMultipartFile:

```java
@Test
public void givenEmployeeJsonAndMultipartFile_whenPostWithRequestPart_thenReturnsOK() throws Exception {
    MockMultipartFile employeeJson = new MockMultipartFile("employee", null,
      "application/json", "{\"name\": \"Emp Name\"}".getBytes());

    mockMvc.perform(multipart("/requestpart/employee")
      .file(A_FILE)
      .file(employeeJson))
      .andExpect(status().isOk());
}
```

### Using `@RequestParam`

- Another way of sending multipart data is to use `@RequestParam`. This is especially useful for simple data, which is sent as key/value pairs along with the file:

```java
@RequestMapping(path = "/requestparam/employee", method = POST, consumes = { MediaType.MULTIPART_FORM_DATA_VALUE })
public ResponseEntity<Object> saveEmployee(@RequestParam String name, @RequestPart MultipartFile document) {
    Employee employee = new Employee(name, document);
    employeeService.save(employee);
    return ResponseEntity.ok().build();
}
```

- Let's write the test for this method to demonstrate:

```java
@Test
public void givenRequestPartAndRequestParam_whenPost_thenReturns200OK() throws Exception {
    mockMvc.perform(multipart("/requestparam/employee")
      .file(A_FILE)
      .param("name", "testname"))
      .andExpect(status().isOk());
}
```

## Testing a Spring Multipart POST Request

- how to test a multipart POST request in Spring using MockMvc.

### Testing a Multipart POST Request

- a simple endpoint in our REST controller:

```java
@PostMapping(path = "/upload")
public ResponseEntity<String> uploadFile(@RequestParam("file") MultipartFile file) {
    return file.isEmpty() ?
      new ResponseEntity<String>(HttpStatus.NOT_FOUND) : new ResponseEntity<String>(HttpStatus.OK);
}
```

- First, let's autowire the WebApplicationContext in our unit test class:
- Now, let's write a method to test the multipart POST request defined above:
- we're defining a hello.txt file using MockMultipartFile constructor
- we're building the mockMvc object using the webApplicationContext object.
- use the MockMvc#perform method to call the REST Endpoint and pass it the file object.
- check the status code 200 to verify our test case.

```java
@Autowired
private WebApplicationContext webApplicationContext;

@Test
public void whenFileUploaded_thenVerifyStatus() 
  throws Exception {
    MockMultipartFile file 
      = new MockMultipartFile(
        "file", 
        "hello.txt", 
        MediaType.TEXT_PLAIN_VALUE, 
        "Hello, World!".getBytes()
      );

    MockMvc mockMvc 
      = MockMvcBuilders.webAppContextSetup(webApplicationContext).build();
    mockMvc.perform(multipart("/upload").file(file))
      .andExpect(status().isOk());
}
```

