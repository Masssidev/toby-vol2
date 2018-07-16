# 스프링 모듈
스프링 프레임워크는 20개의 작은 모듈로 세분화되어 있다. 스프링을 사용하는 목적과 환경, 사용 기술에 따라 필요한 모듈을 선택해 사용해야 한다. 또, 모듈 사이의 의존관계도 이해할 필요가 있다.

모듈의 의존관계는 필수 의존관계와 선택 의존관계가 있다. 모듈 A가 모듈 B에 대해서 필수 의존관계를 갖고 있다면 모듈 A를 사용할 때는 모듈 B를 반드시 포함시켜야 한다. 
반면에 선택 의존관계를 가진 모듈도 있다. 모듈 A가 모듈 C에 대해 선택 의존관계를 갖고 있다면 모듈 A를 사용하기 위해 모듈 C가 반드시 필요한 것은 아니지만, 
모듈 A의 특정 기능을 사용할 때는 모듈 C가 필요할 수도 있다는 뜻이다. 따라서 선택적인 의존관계에 있는 모듈에 대해서는 어떤 경우에 필요한지 알고 있어야 한다.
<hr/>

### 스프링 모듈의 종류와 특징
#### 스프링 모듈 이름
스프링 모듈은 jar 확장자를 가진 파일이다. 모듈 파일의 이름은 명명 규칙에 따라 두 가지로 만들어져 있다. 스프링 모듈은 기본적으로 OSGi의 모듈 명명 규칙을 따라서 
다음과 같이 패키지 이름과 모듈 버전으로 구성된다.
```
org.springframework.core-3.0.7.RELEASE.jar
        모듈 패키지 이름 - 모듈 버전
```
스프링 모듈은 OSGi의 모듈 조건을 충족하는 OSGi 번들이기도 하다. 따라서 OSGi 플랫폼에 바로 가져다 사용할 수 있다. 물론 OSGi가 아닌 일반 JavaEE나 JavaSE 
환경에서도 아무런 문제 없이 사용할 수 있다.

Maven의 명명 규칙을 따라 만들어진 스프링 모듈 파일도 있다. Maven이나 Ivy를 통해 접근할 수 있는 Maven Central 리포지토리에서는 다음과 같은 Maven 스타일의 
모듈 이름을 가진 파일을 찾을 수 있다. Maven 모듈 이름에는 패키지를 사용하지 않기 때문에 이름이 단순한 편이다.
```
spring-core-3.0.7.RELEASE.jar
  모듈 이름 - 모듈 버전
```
> 이 두 개의 파일은 이름은 다르지만 그 내용은 완전히 동일하다.
#### 스프링 모듈 추가
스프링 모듈을 프로젝트에 추가하는 방법은 두 가지가 있다. 스프링 배포판에 포함된 모듈을 직접 추가하는 방법과 Maven이나 Ivy의 의존 라이브러리 관리 기능을 이용해 모듈 리포지토리에서 자동으로 추가되게 하는 방법이다.
##### 수동 추가
스프링 모듈을 얻을 수 있는 가장 손쉬운 방법은 스프링 배포판을 이용하는 것이다. 스프링 프레임워크 배포판은 http://www.springsource.com/download/community에서 다운로드할 수 있다. 배포판 파일의 압축을 풀고 dist 폴더를 열어보면 스프링 모듈을 찾을 수 있다.

스프링 배포판에 포함된 모듈 파일은 OSGi 호환 모듈 이름을 갖고 있다. 사용할 기능에 따라 적절한 모듈파일을 프로젝트에 포함시켜주면 된다. 수동으로 모듈을 추가할 때는 모듈 의존관계에 따라서 필요한 의존모듈을 빼먹지 않도록 주의해야 한다.
##### Maven/Ivy 자동 추가
Maven을 이용해 의존 라이브러리를 관리한다면 pom.xml 파일의 의존정보 설정만으로 필요한 스프링 모듈을 가져올 수 있다.

스프링 모듈은 Maven Central 리포지토리에 등록되어 있다. 따라서 리포지토리를 따로 지정하지 않아도 스프링 모듈을 가져올 수 있다. 이때는 Maven 스타일의 모듈 이름을 사용해서 스프링 모듈을 가져와야 한다.
* core 모듈에 대한 Maven 의존 라이브러리 선언 항목
```
<dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-core</artifactId>
        <version>3.0.4.RELEASE</version>
</dependency>
```
gruopId는 항상 동일하다. artifactId는 스프링 모듈의 Maven 이름으로 지정해주면 된다.

OSGi 이름을 가진 모듈을 사용하려면 스프링 소스에서 제공하는 Maven 리포지토리를 다음과 같이 지정해줘야 한다.
```
<repository>
        <id>com.springsource.repository.bundles.release</id>
        <name>springSource Enterprise Bundle Repository - springSource Bundel Releases</name>
        <url>http://repository.springsource.com/maven/bundles/release</url>
</repository>
```
스프링소스 리포지토리를 지정했다면 다음과 같이 OSGi 이름을 가진 모듈을 가져올 수 있다.
```
<dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-core</artifactId>
        <version>3.0.4.RELEASE</version>
</dependency>
```
Maven Central에 등록된 모듈을 사용할 때와 다른 점은 artifactId에 OSGi 모듈 이름을 사용하는 것이다.

Maven을 이용하면 전이적 의존관계 관리기능이 적용되므로 필수 의존관계에 있는 모듈이 자동으로 추가된다. 따라서 모든 모듈을 일일이 지정할 필요가 없다.
#### 스프링 모듈 목록
모듈 이름 | OSGi 모듈 이름 | Maven 모듈 이름 | 주요 기능
:--------:|:------------:|:--------------:|:----------
AOP | org.springframework.aop | spring-aop | AOP
ASM | org.springframework.asm | spring-asm | ASM 재패키징
Aspects | org.springframework.aspects | spring-adpects | AspectJ 지원
Beans | org.springframework.beans | spring-beans | 빈 팩토리
Context | org.springframework.context | spring-context | 애플리케이션 컨텍스트
Context.Support |org.springframework.context.support | spring-context-support | 컨텍스트 부가 기능
Core | org.springframework.core | spring-core | 공통 기능
Expression | org.springframework.expression | spring-expression | SpEL
Instrument | org.springframework.instrument | spring-instrument | 스프링 Java Agent
Instrument.Tomcat | org.springframework.instrument.tomcat | spring-instrument-tomcat | 톰캣 클래스 로더
JDBC | org.springframework.jdbc | spring-jdbc | JDBC
JMS | org.springframework.jms | spring-jms | JMS
ORM | org.springframework.orm | spring-orm | ORM(Hibernate, JPA 등)
OXM | org.springframework.oxm | spring-oxm | OXM
Test | org.springframework.test | spring-test | 테스트
Transaction | org.springframework.transaction | spring-tx | 트랜잭션
Web | org.springframework.web | spring-web | 웹 공통
Web.Portlet | org.springframework.web.portlet | spring-webmvc-portlet | 포틀릿
Web.Servlet | org.springframework.web.servlet | spring-webmvc | 서블릿
Web.Struts | org.springframework.web.struts | spring-struts | 스트럿츠 1 지원
<hr/>

### 스프링 모듈의 의존관계
Maven이나 Ivy의 의존 라이브러리 관리 기능을 이용한다면 전이적인 의존관계를 따라서 모든 필수 의존 라이브러리가 자동으로 추가된다. 
#### 모듈별 의존관계
##### ASM 모듈
* ASM 모듈은 엄밀히 말해 스프링 모듈이 아니다. ASM 모듈은 클래스 바이트코드 조작 및 분석 프레임워크인 ASM(http://asm.ow2.org/)을 재패키징한 모듈이다. ASM 프레임워크는 스프링뿐 아니라 다른 많은 프레임워크와 라이브러리에서도 사용된다. 그런데 각기 다른 버전의 ASM을 사용하는 경우가 있어서 의존관계에 문제가 생기기도 한다. 그래서 스프링은 다른 ASM을 사용하는 프레임워크와의 충돌을 방지하기 위해 스프링이 사용하는 버전의 ASM 프레임워크를 org.springframework.asm 패키지로 재패키징해서 독립적인 모듈로 제공한다.
* ASM 모듈이 의존하는 다른 모듈은 없다. ASM 모듈은 모든 스프링 프레임워크에서 필요로 하는 필수 모듈이다.
##### Core 모듈
* Core 모듈은 대부분의 모듈에서 필요로 하는 공통 기능을 갖고 있는 핵심 모듈이다. 스프링이 사용하는 주요 타입, 애노테이션, 컨버터, 상수, 유틸리티 클래스 등을 제공한다.
* Core 모듈의 필수 라이브러리는 없다. 클래스 메타정보를 읽는 기능에서 선택적으로 ASM 모듈에 의존하고 있을 뿐이다. Core는 모든 스프링 프로젝트에 반드시 포함시켜야 하는 필수 모듈이다.
##### Beans 모듈
* Beans는 스프링 DI 기능의 핵심인 빈 팩토리와 DI 기능을 제공하는 모듈이다. 빈 메타정보, 빈 리더, 빈 팩토리의 구현과 프로퍼티 에디터가 포함되어 있다. 애플릿이나 모바일 같은 제한된 환경에서 순수하게 스프링의 DI 기술만 적용하고 싶다면, Beans 모듈까지만 적용하고 BeanFactory API를 사용할 수 있다.
* Beans는 ASM, Core 두 개의 필수 의존모듈을 갖는다.
##### AOP 모듈
* AOP는 스프링의 프록시 AOP 기능을 제공하는 모듈이다. 프록시 기반 AOP를 만들 때 필요한 어드바이스, 포인트컷, 프록시 팩토리빈, 자동 프록시 생성기 등을 제공한다.
* 스프링의 AOP는 DI에 기반을 두고 있다. 따라서 AOP 모듈은 DI 기능을 제공하는 Beans 모듈에 의존한다. 물론 전이적 의존관계에 따라서 Beans가 의존하는 ASM과 Core에도 의존한다.
##### Expression 모듈
* Expression은 스프링 표현식 언어(SpEL) 기능을 지원하는 모듈이다.
* Expression은 Core 모듈에만 의존한다. Core 모듈과 함께 사용한다면 스프링 애플리케이션 밖에서 순수한 표현식 언어 라이브러리로 이용할 수도 있다.
##### Context 모듈
* Context는 애플리케이션 컨텍스트 기능을 제공하는 모듈이다. 애플리케이션 컨텍스트를 만드는 데 필요한 대부분의 기능을 포함해서 빈 스캐너, 자바 코드에 의한 설정 기능, EJB 지원 기능, 포매터, 로드타임 위빙, 표현식, JMX, JNDI, 리모팅, 스케줄링, 스크립트 언어 지원, 검증기 등의 애플리케이션 컨테이너로서의 주요한 기능을 담고 있다. 단순한 DI 프레임워크가 아니라 본격적인 엔터프라이즈 애플리케이션 프레임워크로 사용하기 위해 반드시 필요한 모듈이다.
* Context는 AOP와 그 상위 의존모듈을 필요로 한다. 또 Expression도 Context의 필수 의존모듈이다.
* Context의 로드타임 위빙(LTW) 기능을 사용하는 경우에는 Instrument 모듈이 선택적으로 필요하다.
##### Context.Support 모듈
* Context.Support는 Context처럼 자주 사용되는 기능은 아니지만, 경우에 따라 애플리케이션 컨텍스트에서 필요로 하는 부가기능을 담은 모듈이다. EhCache, 메일 추상화 서비스, CommonJ와 Quartz 스케줄링 그리고 FreeMarker, JasperReports, Velocity 팩토리 기능을 제공한다. 이런 특별한 기능을 사용하지 않는다면 Context.Support 모듈은 필요 없다. 단, 스프링 MVC가 Context.Support에 의존하기 때문에 스프링 MVC를 사용한다면 필수로 추가해야 한다. 그 외의 웹기술을 사용하는 경우라면 포함시키지 않을 수도 있다.
* Context.Support는 기본적으로 Context 모듈에 의존한다. Quartz의 JobStroe 기능을 활용하는 경우에 선택적으로 JDBC와 Transaction 모듈이 필요하다.
##### Transaction 모듈
* Transaction은 스프링의 데이터 액세스 추상화의 공통 기능을 담고 있는 모듈이다. DataAccessException 예외 계층구조와 트랜잭션 추상화 기능, 트랜잭션 동기화 저장소 그리고 JCA 지원 기능을 포함하고 있다. 스프링의 데이터 액세스 기술을 사용하는 데 반드시 필요한 모듈이다.
* Transaction의 필수 의존모듈은 Context다.
##### JDBC 모듈
* JDBC는 JDBC 템플릿을 포함한 JDBC 지원 기능을 가진 모듈이다. JdbcTemplate 등의 JDBC 지원 오브젝트 외에도 스프링이 직접 제공하는 DataSource 구현 클래스들이 제공된다.
* JDBC는 Transaction 모듈에 의존한다.
##### ORM 모듈
* ORM은 하이버네이트, JPA, JDO, iBatis와 같은 ORM에 대한 스프링의 지원 기능을 갖고 있는 모듈이다.
* ORM은 JDBC에 의존한다. ORM 모듈에서 JDBC 모듈의 DataSource나 트랜잭션 매니저 등을 활용하기 때문이다. OpenSessionInViewFilter와 같은 일부 기능은 Web 모듈에 선택적으로 의존한다.
##### Web 모듈
* Web은 스프링 웹 기술의 공통적인 기능을 정의한 모듈이다. 스프링 MVC 외에도 스프링이 직접 지원하는 스트럿츠, JSF 등을 적용할 때도 필요하다. 또한 Caucho, HttpInvoker, JAX-RPC, JAX-WS 등의 리모팅 기능도 포함하고 있다. 기본적인 바인딩, 컨텍스트 로더, 필터, 멀티파트, 메시지 컨버터 기능도 제공한다.
* Web 모듈은 Context 모듈을 필수 모듈로 갖고 있다. XML을 사용하는 메시지 컨버터 기능에는 OXM 모듈이 필요할 수 있다.
##### Web.Servlet 모듈
* Web.Servlet은 스프링 MVC 기능을 제공하는 모듈이다. 전통적인 스프링 MVC와 최신 @MVC 기능이 모두 포함되어 있다.
* Web.Servlet의 필수 의존모듈은 Web과 Context.Support다. XML을 이용한 뷰나 메시지 컨버터 등을 사용할 때는 선택적으로 OXM 모듈을 필요로 한다.
##### Web.Portlet 모듈
* Web.Portlet은 Portlet 개발에 사용하는 스프링 모듈이다. Web.Portlet의 필수 의존모듈은 Web.Servlet이다.
##### Web.Struts 모듈
* Web.Struts는 스트럿츠 1.x를 지원하는 모듈이다. 스트럿츠 1.x를 스프링의 웹 프레젠테이션 계층으로 사용할 때 유용한 기능을 제공한다.
* Web.Struts는 Web 모듈에만 의존한다.
##### JMS 모듈
* JMS는 스프링의 JMS 지원 기능을 사용할 때 필요한 모듈이다.
* JMS는 Transaction 모듈에 의존한다.
##### Aspects 모듈
* Aspects는 스프링이 제공하는 AspectJ AOP 기능을 사용할 때 필요한 모듈이다. 스프링이 직접 제공하는 AspectJ로 만든 기능은 @Configurable을 이용한 도메인 오브젝트 DI 기능, JPA 예외 변환기, AspectJ 방식의 트랜잭션 기능 등이 있다.
* Aspects의 필수 의존모듈은 없다. JPA 지원 기능을 사용할 때는 ORM, 트랜잭션 지원 기능을 사용할 때는 Transaction 모듈에 선택적으로 의존한다.
##### Instrument 모듈
* Instrument는 스프링의 로드타임 위버(LTW) 기능을 적용할 때 필요한 모듈이다. LTW 기능을 제공하는 Context 모듈에도 선택적으로 사용된다. JVM의 -javaagent 옵션을 사용해 자바에이전트로도 사용된다.
* Instrument는 다른 의존모듈이 없다.
##### Instrument.Tomcat 모듈
* Instrument.Tomcat은 애플리케이션이 아니라 톰캣 서버의 클래스 로더로 사용하는 모듈이다. 다른 모듈에 대한 의존관계는 전혀 없다. 자세한 사용 방법은 스프링 레퍼런스의 로드타임 위빙 항목을 참고.
##### Test 모듈
* Test는 스프링의 테스트 지원 기능을 가진 모듈이다. 테스트 컨텍스트 프레임워크나 목 오브젝트 등을 이용해 테스트를 만들 때 사용한다. 테스트용 모듈이기 때문에 운영 중에는 사용되지 않는다.



