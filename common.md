### Пример `POST` запроса (через curl) для отправки на сервер json:   
```bash
curl -X POST "http://address:port/api/v1/users" 
  -H "accept: application/json" 
  -H "Authorization: Token c70cb671-3974-45ca-b6eb-57ccaabb964a" 
  -H "Content-Type: application/json-patch+json" 
  -d "{ "name": "John", "city": "New York", "age": 27, "email": "john@example.com" }"
```
Циклический вариант исполнения через `bash`:
```bash
#!/bin/bash
for (( i=1; i <= 10; i++ ))
do
curl -X POST "http://address:port/api/v1/users" 
  -H "accept: application/json" 
  -H "Authorization: Token c70cb671-3974-45ca-b6eb-57ccaabb964a" 
  -H "Content-Type: application/json-patch+json" 
  -d "{ "name": "John", "city": "New York", "age": 27, "email": "john@example.com" }"
done
```
