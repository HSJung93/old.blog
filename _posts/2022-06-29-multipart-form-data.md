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
### Multipart form data

The ENCTYPE attribute of `<form>` tag specifies the method of encoding for the form data. It is one of the two ways of encoding the HTML form. It is specifically used when file uploading is required in HTML form. It sends the form data to server in multiple parts because of large size of file.

```html
<form action="login.php" method="post" 
    enctype="multipart/form-data">
</form>
```

### Multipart request handling in Spring

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


