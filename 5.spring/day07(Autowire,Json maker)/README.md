# 의존 자동 주입
- 의존 대상을 설정 코드에서 직접 주입하기 않고 스프링이 자동으로 의존하는 빈 객체를 주입해주는 기능도 있다. 이를 자동 주입이라 한다.
- 스프링에서 의존 자동 주입을 설정하려면 @Autowired 애노테이션이나 @Resource 애노테이션을 사용하면 된다. 스프링에서는 주고 @Autowired를 많이 사용한다.

## @Autowired 애노테이션을 이용한 의존 자동 주입
- 자동 주입 기능을 사용하면 스프링이 알아서 의존 객체를 찾아서 주입한다.
- 자동 주입 기능을 사용하는 방법은 의존을 주입할 대상에 @Autowired 애노테이션을 붙이기만 하면 된다.

## Ex_날짜_SpringAutowire 프로젝트 생성하기
- com.korea.auto
- pom.xml파일 복사, resources 패키지 복사, xml 파일 제거하기

## dao패키지에 DeptDAO클래스 만들기
```java
package dao;


public class DeptDAO {

	//확인을 하는 용도
	public DeptDAO() {
		System.out.println("--deptdao의 생성자---");
	}
  
  사실상 DeptDAO는 일반적인 클래스이고 항상 @Bean을 통해서 객체를 만들었다.
  
}

```
Servlet_Context에서도 자동탐색으로 컨트롤러 객체를 만들어보자.

## Servlet_Context 코드 수정하기
```java
package mvc;

import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.EnableWebMvc;
import org.springframework.web.servlet.config.annotation.ResourceHandlerRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;


@Configuration
@EnableWebMvc
@ComponentScan(basePackages = {"com.korea.auto","dao"})
컨트롤러 뿐만 아니라 컴포넌트라는 요소를 전부 탐색해서 객체로 만들어준다.
@Component
ㄴ@Repository
ㄴ@Service
ㄴ@Controller
public class Servlet_Context implements WebMvcConfigurer {

	@Override
	public void addResourceHandlers(ResourceHandlerRegistry registry) {
		registry.addResourceHandler("/resources/**").addResourceLocations("/resources/");
	}

	/*
	 * @Bean public InternalResourceViewResolver resolver() {
	 * InternalResourceViewResolver resolver = new InternalResourceViewResolver();
	 * resolver.setViewClass(JstlView.class); resolver.setPrefix("/WEB-INF/views/");
	 * resolver.setSuffix(".jsp"); return resolver; }
	 */
	 
}

```

![image](https://github.com/to7485/Web1500/assets/54658614/3f2ef563-0716-4d04-b2ef-6ecf5648f12e)

## controller 패키지에 TestController클래스 생성하기
```java
package controller;

import org.springframework.stereotype.Controller;

@Controller
public class TestController {

	public TestController() {
		System.out.println("--TestController의 생성자--");
	}
}
```
- Servlet_Context에 패키지 경로 잘 잡아주기

```java
package mvc;

@Configuration
@EnableWebMvc
@ComponentScan(basePackages = {"controller","dao"})
```
- 실행해보면 콘솔에 생성자가 두개 다 잘 호출된 것을 볼 수 있다.

![image](https://github.com/to7485/Web1500/assets/54658614/a0ab0e02-aff3-4e16-a041-fed3a84992e4)

## vo패키지에 DeptVO클래스 만들기

```java
package vo;

import lombok.Getter;
import lombok.Setter;

@Setter
@Getter
public class DeptVO {

	private int deptno;
	private String dname,loc;
}

```
- 이전에 DAO를 살펴보면 sqlSession객체가 필요했다.
- SqlSession도 라이브러리가 있으면 @Autowired로 받을 수 있다.

## DeptDAO에 코드 추가하기
```java
package dao;

import org.apache.ibatis.session.SqlSession;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Repository;

@Repository
public class DeptDAO {
	
	@Autowired
	SqlSession sqlSession;

	//확인을 하는 용도
	public DeptDAO() {
		System.out.println("--deptdao의 생성자---");
	}
}
```
이제 sqlSession을 갖고 있는 DeptDAO를 컨트롤러가 참조를 해야 한다.<br>
그런데 자동생성을 해서 setter,생성자 주입이 불가능하다.<br>

## TestController에 코드 추가하기
```java
package controller;

import org.springframework.stereotype.Controller;

import dao.DeptDAO;

@Controller
public class TestController {
	
	DeptDAO dept_dao; 아무런 정보도 들어있지 않은 dao객체

	public TestController() {
		System.out.println("--TestController의 생성자--");
	}
}

```

## DeptDAO클래스에 코드 추가하기
```java
package dao;

import org.apache.ibatis.session.SqlSession;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Repository;

@Repository("dept_dao")//dept_dao : 현재 DeptDAO를 자동생성하기 위한 별칭
public class DeptDAO {
	
	@Autowired
	SqlSession sqlSession;

	//확인을 하는 용도
	public DeptDAO() {
		System.out.println("--deptdao의 생성자---");
	}
}

```

## TestController에서 자동주입으로 DAO 받기
```java
package controller;

import org.springframework.stereotype.Controller;

import dao.DeptDAO;

@Controller
public class TestController {
	
	@Autowired
	DeptDAO dept_dao;//@Repository("dept_dao") 객체이름과 별칭을 같게 만들어준다.

	public TestController() {
		System.out.println("--TestController의 생성자--");
	}
}
```

자동으로 만들게 되면 Context_3_dao 파일이 필요가 없어진다.

## DeptDAO에서 부서목록 조회메서드 만들기
```java
package dao;

import java.util.List;

import org.apache.ibatis.session.SqlSession;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Repository;

import vo.DeptVO;

@Repository("dept_dao")//dept_dao : 현재 DeptDAO를 자동생성하기 위한 별칭
public class DeptDAO {
	
	@Autowired
	SqlSession sqlSession;

	//확인을 하는 용도
	public DeptDAO() {
		System.out.println("--deptdao의 생성자---");
	}
	
	public List<DeptVO> selectList(){
		List<DeptVO> list = sqlSession.selectList("d.dept_list");
		return list;
	}
}

```

## dept.xml 매퍼에 쿼리문 작성하기
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="d">

	<select id="dept_list" resultType="dept">
		select * from dept
	</select>

</mapper>
```

## mybatis-config.xml 매핑 맞춰주기
```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN" "HTTP://mybatis.org/dtd/mybatis-3-config.dtd">

<configuration>
	<settings>
		<setting name="cacheEnabled" value="false" />
		<setting name="useGeneratedKeys" value="true" />
		<setting name="defaultExecutorType" value="REUSE" />
	</settings>
	
	<typeAliases>
		  <typeAlias type="vo.DeptVO" alias="dept"/>
	</typeAliases>
	
	<mappers>
		<mapper resource="config/mybatis/mapper/dept.xml" />
	</mappers>
</configuration>
```

## TestController에서 list받아주고 포워딩하기
```java
@RequestMapping(value= {"/","list.do"})
public String deptList(Model model) {
	List<DeptVO> list = dept_dao.selectList();
	model.addAttribute("list",list);
	return "/WEB-INF/views/dept_list.jsp";
}
```
## views 폴더에 dept_list.jsp 만들기
```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title>Insert title here</title>
	</head>
	<body>
		<table border="1" align="center">
			<tr>
				<th>부서번호</th>
				<th>부서명</th>
				<th>위치</th>
			</tr>
			<c:forEach var="vo" items="${list }">
				<tr>
					<td>${vo.deptno }</td>
					<td>${vo.dname }</td>
					<td>${vo.loc }</td>
				</tr>
			</c:forEach>
		</table>
	</body>
</html>
```
# JSON 형식 사용해보기

## Ex_날짜_JsonMaker
- com.korea.json
- pom.xml, resources 패키지 복사하기

## jackson 라이브러리 다운로드받기

![image](https://github.com/to7485/Web1500/assets/54658614/9b457b31-3446-41f1-bcf5-cbe6f8e84c4b)



```
<!-- https://mvnrepository.com/artifact/com.fasterxml.jackson.core/jackson-databind -->
<!-- Java Object를 JSON으로 변환하거나 JSON을 Java Object로 변환하는데 사용하는 라이브러리 -->
<!-- jackson-databind 라이브러리는 jackson-core 및 jackson-annotation 라이브러리의 의존성을 포함 -->
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-databind</artifactId>
    <version>2.9.8</version>
</dependency>
```

## vo 패키지에 PersonVO 만들기
```java
package vo;

import lombok.Getter;
import lombok.Setter;

@Setter
@Getter
public class PersonVO {

	private String name, addr;
	private int age;
}

```

## JsonMakerController 클래스 만들기
```
package com.korea.visit;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;

import vo.PersonVO;

@Controller
public class JsonMakerController {

	@RequestMapping("/vo_to_json.do")
	@ResponseBody
	public PersonVO voJson() {
		PersonVO p = new PersonVO();
		p.setAddr("인천시 부평구");
		p.setAge(30);
		p.setName("홍길동");
		return p;
	}
	
	
}

```









