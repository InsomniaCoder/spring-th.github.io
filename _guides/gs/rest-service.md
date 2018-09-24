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

ค่าพารามิเตอร์ `name` จะถูกแทนค่าเริ่มต้น "World" และจะแสดงใน response:
```
{"id":1,"content":"Hello, User!"}
```
<br/>

## คุณจะต้องมีอะไรบ้าง
- เวลาประมาณ 15 นาที
- Text Editor หรือ IDE ที่คุณชื่นชอบ
- [JDK 1.8](http://www.oracle.com/technetwork/java/javase/downloads/index.html) หรือใหม่กว่า
- [Gradle 4+](http://www.gradle.org/downloads) หรือ [Maven 3.2+](https://maven.apache.org/download.cgi)
- คุณสามารถ import โค้ดเข้าไปใน IDE ของคุณ:
  - [Spring Tool Suite (STS)](https://spring.io/guides/gs/sts)
  - [InteliJ IDEA](https://spring.io/guides/gs/intellij-idea/)
<br/>
<br/>

## ทำให้สำเร็จได้อย่างไร
คำแนะนำส่วนใหญ่ของ Spring, คุณสามารถเริ่มต้นจนกระทั่งสำเร็จเป็นขั้นเป็นตอน หรือคุณจะข้ามขั้นตอนเริ่มต้นที่คุณรู้อยู่แล้วไป ทั้งสองวิธี คุณจะจบที่โค้ดที่ทำงานได้

**เริ่มจากต้น**, ไปที่ สร้างด้วย Gradle<br>
**ข้ามพื้นฐาน**, ทำขั้นตอนต่อไปนี้:
- Download และ unzip source repository จากคำแนะนำ หรือ clone โดยใช้ Git:<br>
`git clone https://github.com/spring-guides/gs-rest-service.git`
- cd ไปที่ `gs-rest-service/initial`
- กระโดดไปที่ [สร้างคลาสแทน resource](https://spring.io/guides/gs/rest-service/#initial)

**เมื่อคุณสำเร็จ** คุณสามารถตรวจสอบโค้ดที่คุณสร้างกับโค้ดใน<br>
`gs-rest-service/complete`

## สร้างด้วย Gradle 
ก่อนอื่นคุณจะต้องตั้งค่า build script เบื้องต้น คุณสามารถใช้ build system อะไรก็ได้ตามที่คุณถนัดเมื่อ build แอพพลิเคชันด้วย Spring, แต่โค้ดของคุณจำเป็นต้องทำงานได้กับ Gradle และ Maven รวมอยู่ที่นี่แล้ว ถ้าคุณไม่คุ้นเคยกับทั้งสอง สามารถอ่าน สร้างโปรเจ็ค Java ด้วย Gradle หรือสร้างโปรเจ็ค Java ด้วย Maven


### สร้างโค้งสร้างโปรเจ็ต
ในโครงสร้างของโปรเจ็คที่คุณเลือก สร้างตามโครงสร้างย่อยดังนี้ ตัวอย่างเช่นใช้คำสั่ง `mkdir -p src/main/java/hello` บนระบบ *nix:

```
└── src
    └── main
        └── java
            └── hello
```

### สร้างไฟล์ Gradle build
ด้านล่างนี้เป็น [ไฟล์ Gradle build เบื้องต้น](initial Gradle build file)

`build.gradle`

{% highlight gradle %}
buildscript {
    repositories {
        mavenCentral()
    }
    dependencies {
        classpath("org.springframework.boot:spring-boot-gradle-plugin:2.0.5.RELEASE")
    }
}

apply plugin: 'java'
apply plugin: 'eclipse'
apply plugin: 'idea'
apply plugin: 'org.springframework.boot'
apply plugin: 'io.spring.dependency-management'

bootJar {
    baseName = 'gs-rest-service'
    version =  '0.1.0'
}

repositories {
    mavenCentral()
}

sourceCompatibility = 1.8
targetCompatibility = 1.8

dependencies {
    compile("org.springframework.boot:spring-boot-starter-web")
    testCompile('org.springframework.boot:spring-boot-starter-test')
}
{% endhighlight %}

[Spring Boot gradle plugin](https://docs.spring.io/spring-boot/docs/current/gradle-plugin/reference/html) มีหลากหลายคุณสมบัติที่สะดวก เช่น:
- รวบรวมไฟล์ jar ทั้งหมดบน classpath และ build เป็นไฟล์เดียว, runnable "uber-jar" ซึ่งทำให้มีความสะดวกที่จะ execute และ transport servic ของคุณ
- ค้นหา `public static void main()` method เพื่อระบุว่าเป็นคลาส runnable
- มี build-in dependency resolver ที่ตั้งค่าเวอร์ชันให้เข้ากันได้กับ Spring Boot dependency ต่างๆ คุณสามารถเขียนทับทุกๆ เวอร์ชันที่คุณต้องการ แต่จะเลือกค่าเริ่มต้นเป็นเวอร์ชันที่เลือกของ Spring Boot

## สร้างด้วย Maven
ก่อนอื่นคุณจะต้องตั้งค่า build script เบื้องต้น คุณสามารถใช้ build system อะไรก็ได้ตามที่คุณถนัดเมื่อ build แอพพลิเคชันด้วย Spring, แต่โค้ดของคุณจำเป็นต้องทำงานได้กับ Maven รวมอยู่ที่นี่แล้ว ถ้าคุณไม่คุ้นเคยกับทั้งสอง สามารถอ่าน สร้างโปรเจ็ค Java ด้วย Maven

### สร้างโค้งสร้างโปรเจ็ต
ในโครงสร้างของโปรเจ็คที่คุณเลือก สร้างตามโครงสร้างย่อยดังนี้ ตัวอย่างเช่นใช้คำสั่ง `mkdir -p src/main/java/hello` บนระบบ *nix:

```
└── src
    └── main
        └── java
            └── hello
```

`pom.xml`

{% highlight xml %}
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>org.springframework</groupId>
    <artifactId>gs-rest-service</artifactId>
    <version>0.1.0</version>

    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.0.5.RELEASE</version>
    </parent>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
        <dependency>
            <groupId>com.jayway.jsonpath</groupId>
            <artifactId>json-path</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>

    <properties>
        <java.version>1.8</java.version>
    </properties>


    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>

    <repositories>
        <repository>
            <id>spring-releases</id>
            <url>https://repo.spring.io/libs-release</url>
        </repository>
    </repositories>
    <pluginRepositories>
        <pluginRepository>
            <id>spring-releases</id>
            <url>https://repo.spring.io/libs-release</url>
        </pluginRepository>
    </pluginRepositories>
</project>
{% endhighlight %}

[Spring Boot gradle plugin](https://docs.spring.io/spring-boot/docs/current/gradle-plugin/reference/html) มีหลากหลายคุณสมบัติที่สะดวก เช่น:
- รวบรวมไฟล์ jar ทั้งหมดบน classpath และ build เป็นไฟล์เดียว, runnable "uber-jar" ซึ่งทำให้มีความสะดวกที่จะ execute และ transport servic ของคุณ
- ค้นหา `public static void main()` method เพื่อระบุว่าเป็นคลาส runnable
- มี build-in dependency resolver ที่ตั้งค่าเวอร์ชันให้เข้ากันได้กับ Spring Boot dependency ต่างๆ คุณสามารถเขียนทับทุกๆ เวอร์ชันที่คุณต้องการ แต่จะเลือกค่าเริ่มต้นเป็นเวอร์ชันที่เลือกของ Spring Boot


## สร้างด้วย IDE
- อ่าน import โปรเจคนี้ใน [Spring Tool Suite](https://spring.io/guides/gs/sts/) อย่างไร
- อ่านทำงานกับโปรเจคนี้ใน [IntelliJ IDEA](https://spring.io/guides/gs/intellij-idea) ได้อย่างไร

## สร้างคลาสแทน Resource
ณ ตอนนี้ คุณได้สร้างโปรเจคและ build system แล้ว ต่อไปคุณจะได้สร้าง Web Service

เริ่มต้นกระบวนการด้วยการคิดเกี่ยวกับการติดต่อกับ Service

ซึ่ง Service จะจัดการกับ GET request สำหรับ `/greeting`, ด้วยตัวเลือกพารามิเตอร์ `name` ใน request string, `GET` request ควรจะ return เป็น `200 OK` ด้วย JSON ใน body ที่เป็นตัวแทนของ greeting มันควรจะมีหน้าตาแบบนี้:

{% highlight json %}
{
    "id": 1,
    "content": "Hello, World!"
}
{% endhighlight %}

ซึ่ง `id` เป็นฟิลด์ที่เฉพาะสำหรับ greeting และ `content` เป็นตัวอักษรที่แทนที่สำหรับ greeting

เพื่อที่จะ model ตัวแทนของ greeting คุณจะต้องสร้างคลาสแทน resource ด้วย Pain Old Java Object ด้วย field, construtor และ accessors สำหรับ `id` และ `content` ข้อมูล

`src/main/java/hello/Greeting.java`

{% highlight java %}
package hello;

public class Greeting {

    private final long id;
    private final String content;

    public Greeting(long id, String content) {
        this.id = id;
        this.content = content;
    }

    public long getId() {
        return id;
    }

    public String getContent() {
        return content;
    }
}
{% endhighlight %}

> ในขั้นตอนต่อไปคุณจะได้เห็นว่า Spring ใช้ Jaskson JSON library เพื่อแปลง instance ของ type `Greeting` ไปเป็น JSON โดยอัตโนมัต

ต่อไปคุณจะสร้าง Resource Controller ที่จะบริการการ greeting

## สร้าง Resource Controller
ในวิธีการการสร้าง RESTful Web Service ของ Spring, HTTP request จะถูกจัดการโดย Controller ซึ่งเป็นส่วนประกอบที่ง่ายที่ระบุด้วย `@RestController` annotation และคลาส `GreetingController` ด้านล่างจะจัดการกับ `GET` request สำหรับ `/greeting` โดยการ return instance ใหม่ของคลาส `Greeting`

`src/main/java/hello/GreetingController.java`

{% highlight java %}
package hello;

import java.util.concurrent.atomic.AtomicLong;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class GreetingController {

    private static final String template = "Hello, %s!";
    private final AtomicLong counter = new AtomicLong();

    @RequestMapping("/greeting")
    public Greeting greeting(@RequestParam(value="name", defaultValue="World") String name) {
        return new Greeting(counter.incrementAndGet(),
                            String.format(template, name));
    }
}
{% endhighlight %}

Controller นี้จะกระชับและง่าย แต่มันมีสิ่งที่เกิดขึ้นมากมายภายใน ลองมาดูทีละขั้นคอน

`@RequestMapping` annotatoin จะเป็นตัวที่ทำให้แน่ใจว่ารองรับ HTTP request ที่ `/greeting` โดยที่ผูกกับ method `greeting()`

> จากตัวอย่างด้านบนไม่ได้ระบุว่าเป็น `GET` หรือ `PUT` หรือ `POST` และอื่นๆ เพราะว่า `@RequestMapping` จะรองรับ method เหล่านั้นเป็นค่าเริ่มต้น ใช้ `@RequestMapping(method = GET)` เพื่อเจาะจงว่าจะใช้ method ใด

`@RequestParam` จะผูกค่าของพารามิเตอร์ query string `name` กับพารามิเตอร์ `name` ของ `greeting()` method ถ้าพารามิเตอร์ `name` ไม่มีอยู่ใน query string ตอน request จะใช้ค่าตั้งต้นเป็น `defaultValue` เป็น "World" แทน

implementation ของ method body ที่ถูกสร้างขึ้นและ return เป็นวัตถุ `Greeting` ใหม่ ด้วยแอทริบิวท์ `id` และ `content` ขึ้นอยู่กับค่าถัดไปจาก `counter` และรูปแบบที่ให้มาด้วย `name` ที่ใช้โดย greeting `template`

จุดที่แตกต่างอย่างสำคัญกับ MVC Controller แบบเดิมและ RESTful web service controller คือวิธีที่สร้าง HTTP response body แทนที่จะขึ้นอยู่กับ view technology เพื่อทำ server-side randering ของ greeting data เป็น HTML, RESTful web service controller นี้จะ populate data และ response เป็น Greeting object ซึ่งข้มูล object จะถูกเขียนโดยตรงเป็น HTTP respone ในรูปแบบ JSON

โค้ดนี้ใช้ `@RestController` annotation ใหม่ของ Spring 4 
