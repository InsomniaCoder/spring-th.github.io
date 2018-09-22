---
layout: page
name: rest-service
title: สร้าง RESTful Web Service 
categories: /rest-service/
---

บทความนี้จะนำคุณไปสู่กระบวนการสร้าง "Hello World" [RESTful Web Service](https://spring.io/understanding/REST) ด้วย Spring

## อะไรที่คุณกำลังจะสร้าง
คุณจะสร้าง service ที่จะรับ HTTP GET request ดังนี้:

```
http://localhost:8080/greeting
```

และ response ด้วย JSON ที่แทนข้อความ greeting:

```
{"id":1,"content":"Hello, World!"}
```

คุณสามารถปรับแก้ไขข้อความ greeting ด้วย option พารามิเตอร์ `name` ใน query string:

```
http://localhost:8080/greeting?name=User
```

พารามิเตอร์ 