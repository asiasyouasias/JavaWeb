# 스프링 부트(Spring boot)
- 스프링은 기존 기술의 복잡성을 크게 줄인 프레임워크이지만, 그럼에도 불구하고 스프링을 사용하기 위해서는 여러가지 사항들을 설정해줘야 한다.

- 하지만 스프링부트를 사용하면 복잡한 설정 정보를 간략하게 줄일 수 있다.

- 스프링 부트는 스프링으로 애플리케이션을 만들 때에 필요한 설정들을 간편하게 처리해주는 별도의 프레임워크이다.

- 톰캣과 같은 웹 애플리케이션(WAS)가 자체 내장되어 있다.
  - 기존에 배포를 할 때는 별도의 외장 웹 서버를 설치하고 프로젝트를 War파일로 빌드하여 배포를 진행했는데, 이러한 방식은 번거롭다는 단점을 가진다. 반면 스프링 부트는 자체적인 웹서버를 내장하고 있어, 빠르고 간편하게 배포를 진행할 수 있습니다.
  - 또한 스프링 부트를 사용하면 독립적으로 실행 가능한 Jar파일로 프로젝트를 빌드할 수 있어, 클라우드 서비스 및 도커와 같은 가상화 환경에 빠르게 배포할 수 있습니다.
  
- 빌드 구성을 단순화 하기 위한 Spring Boot Starter 제공
  - 스프링 부트에서 스타터(stater) 설정을 자동화해주는 모듈을 의미합니다. 
	- 프로젝트에서 설정해야 하는 다양한 의존성을 사전에 미리 정의해서 제공합니다. 
	- 프로젝트에서 설정해야 하는 다수의 의존성들을 스타터가 이미 포함하고 있기 때문에 스타터에 대한 의존성만 추가하면 프로젝트를 쉽게진행할 수 있습니다.
	
- XML 설정 없이 단순 자바 수준의 설정 방식 제공
	- 스프링 부트는 XML에 설정을 작성할 필요 없이 자바 코드로 설정할 수 있습니다. 
	- XML은 문법이틀리거나 선언이 선언을 잘못하면 원인을 찾기 힘듭니다. 
	- 자바 코드는 컴파일러의 도움을 받기 때문에 오타 등의 설정 정보 오류를 미리 알 수 있습니다. 
	- 또한 클래스 단위로 설정하기 때문에 쉽게 관리할 수 있습니다.

## 자바 설치하기
- [JDK다운로드](https://www.oracle.com/java/technologies/downloads)

## STS(Spring Tool Suite)

- STS(Spring Tool Suite)는 이클립스기반의 스프링에 최적화된 IDE입니다.

- [다운로드](https://spring.io/tools)

- 다운로드된 jar파일을 더블클릭하면 압축이 풀린다.

![image](https://github.com/to7485/Web1500/assets/54658614/c7c8da21-40a8-4153-a437-f32d596d8942)


## 프로젝트 생성하기

- File > New > Spring Starter Project

![image](https://github.com/to7485/Web1500/assets/54658614/a5f7e8de-4c78-462f-b85d-d93a901c8ab8)

- 프로젝트를 만들기 위해 다음과 같은 창이 뜹니다.

![image](https://github.com/to7485/Web1500/assets/54658614/daa65764-c2e7-43b2-8cbf-c98f2c80522b)

  - Name : 프로젝트명(여기에 입력한 명칭이 Artifact로 완성이 되는데, ArtifactId는 변경이 가능합니다.
  - Group,Description,Package를 입력합니다. 일반적으로 Package는 일반적으로 GroupId + artifactId를 합친 형태로 많이 설정합니다.
  - Maven Vs Gradle(빌드툴, 스프링을 할 때 Maven을 써봤으니 이번에는 Gradle을 사용해보자)
	- Maven은 태그로 이루어져 있어서 항상 열고 닫고를 해야 한다.
	- Gradle은 마침표를 찍어서 객체 형태로 관리할 수 있다.(설정파일 안에서 조건식도 쓸 수 있다고함)
  - Packaging : 배포할때 프로젝트를 어떤방식으로 포장할 것인지 설정
	- War로 하면 화면이 잘 안나오는 경우가 있다.


  - 기본적인 라이브러리들을 의존성 주입을 해놓고 시작할 수 있다.

![image](https://github.com/to7485/Web1500/assets/54658614/16678837-f075-45bd-8fa3-0a92591b29c2)

  - Spring Boot DevTools : 애플리케이션 개발시 유용한 기능들을 제공하는 모듈, 개발 생산성을 향상시키는데 도움을 줄 수 있다.
    - Automatic Restart : classpath에 있는 파일이 변경될 때마다 애플리케이션을 자동으로 재시작해준다.<br> 개발자가 소스 수정 후 애플리케이션을 재실행하는 과정을 줄일 수 있으므로 생산성을 향상시킬 수 있다.
    - Live Reload : 정적 자원(html, css, js) 수정 시 새로고침 없이 바로 적용할 수 있다.
    - Property Defaults : Thymeleaf는 기본적으로 성능을 향상시키기 위해서 캐싱 기능을 사용한다.<br> 하지만 개발하는 과정에서 캐싱 기능을 사용한다면 수정한 소스가 제대로 반영되지 않을 수 있기 때문에 cache의 기본값을 false로 설정할 수 있다.


spring java reconcile -> yes

![image](https://github.com/to7485/Web1500/assets/54658614/1e0078e1-c0b6-43bb-8444-697a5dbd406b)

## 스프링부트 프로젝트 구성

![image](https://github.com/to7485/Web1500/assets/54658614/1a15af1b-5cdc-4a19-a975-6308fcdc0ca3)

1. src/main/java : 서버단 Java파일
2. test/main/java : 단위 테스트 Java파일
3. src/main/resources : 설정 파일 및 View단
4. src/main/resources/static : css,js,image 등 정적 파일 경로
5. src/main/resources/templates : html 파일 경로
6. build.gradle : 라이브러리 의존성 관리
	- ```
		plugins {
		id 'java'
		id 'org.springframework.boot' version '3.1.0'
		id 'io.spring.dependency-management' version '1.1.0'
		}
		
		group = 'com.korea'
		version = '0.0.1-SNAPSHOT'
		
		java {
			sourceCompatibility = '11'
		}
		
		configurations {
			compileOnly {
				extendsFrom annotationProcessor
			}
		}
		
		repositories {
			mavenCentral()
		}
		
		dependencies {
		    implementation 'org.springframework.boot:spring-boot-starter-web'
		    implementation 'org.springframework.boot:spring-boot-starter-web-services'
		    compileOnly 'org.projectlombok:lombok'
		    developmentOnly 'org.springframework.boot:spring-boot-devtools'
		    annotationProcessor 'org.springframework.boot:spring-boot-configuration-processor'
		    annotationProcessor 'org.projectlombok:lombok'
		    testAnnotationProcessor 'org.projectlombok:lombok'
		    testImplementation 'org.projectlombok:lombok'
		    testImplementation 'org.springframework.boot:spring-boot-starter-test'
		}
		
		tasks.named('test') {
			useJUnitPlatform()
		}

8. application.yml : 서버 및 DB, 라이브러리 설정파일(properties로 관리를 해도 되지만 yml은 공통적인 부분을 많이 제거할 수 있다.)





















