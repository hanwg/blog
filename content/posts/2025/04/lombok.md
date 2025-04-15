---
title: "Boost your Java development productivity with Lombok! 🚀"
date: 2025-04-06
tags:
  - SoftwareEngineering
---
**𝗪𝗵𝗮𝘁 𝗶𝘀 𝗶𝘁?**

Lombok is a Java library which auto generates boilerplate code through the use of annotations.
It can generate code for getters/setters, `𝚎𝚚𝚞𝚊𝚕𝚜()`, `𝚑𝚊𝚜𝚑𝙲𝚘𝚍𝚎()`, `𝚝𝚘𝚂𝚝𝚛𝚒𝚗𝚐()` and even constructors/builders.

<br>

**𝗪𝗵𝘆 𝘂𝘀𝗲 𝗶𝘁?**

It eliminates boilerplate code which leads to...
- Improve readability since there is less clutter and also makes pull requests easier to review.
- Increased productivity and frees up your time to focus on core business logic.

<br>

**An example**

No Lombok:
```java
public class User {
  private String name;
  
  public String getName() {
    return name;
  }
  public void setName(String name) {
    this.name = name;
  }
  
  @Override
  public int hashCode() {
    // hashCode implementation
  }
  
  @Override
  public boolean equals(Object o) {
    // equals implementation
  }
  
  @Override
  public String toString() {
    return "name: " + name;
  }
}
```

<br>

With Lombok:
```java
@Data
public class User {
  private String name;
}
```
- All the boilerplate code is replaced with just a `@Data` annotation.
- If you need to add new attributes, you don't need to change the `equals()`, `hashCode()` and `toString()` methods as Lombok will handle them for you!

<br>

**How to get started?**

Note:
The setup below is for maven projects.
At the time of writing, the latest of Lombok is `1.18.38`.

Add the Lombok dependency in your `pom.xml`:
```xml
<dependency>
		<groupId>org.projectlombok</groupId>
		<artifactId>lombok</artifactId>
		<version>${lombok.version}</version>
		<scope>provided</scope>
</dependency>
```

After adding the Lombok dependency, you might get a prompt from your IDE to enable annotation processing (If you don't get the prompt, you will have to follow your IDE specific instructions to enable). Enable annotation processing.

Add the following plugin in your `pom.xml` so that maven build can use the Lombok generated code.
```xml
<plugin>
	<groupId>org.apache.maven.plugins</groupId>
		<artifactId>maven-compiler-plugin</artifactId>
		<version>3.14.0</version>
		<configuration>
			<annotationProcessorPaths>
				<path>
					<groupId>org.projectlombok</groupId>
					<artifactId>lombok</artifactId>
					<version>${lombok.version}</version>
    			</path>
			</annotationProcessorPaths>
		</configuration>
</plugin>
```