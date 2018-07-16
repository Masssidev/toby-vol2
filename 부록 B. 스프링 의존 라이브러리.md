# 스프링 의존 라이브러리
스프링 3.0과 스프링 3.1에는 스프링 모듈 외에도 100여 개의 의존 라이브러리가 존재한다. 이 의존 라이브러리는 스프링 프레임워크를 빌드하고 테스트하는 데 필요한 라이브러리다. 
스프링 애플리케이션을 개발하고 운영할 때는 이 외에도 다양한 많은 라이브러리가 추가로 필요할 수 있다.

스프링의 의존 라이브러리는 스프링의 업데이트마다 새롭게 추가되거나 버전이 바뀔 수 있다. 최신 의존 라이브러리 정보는 스프링 배포판이나 Maven 리포지토리의 POM 정보를 참고.
<hr/>

### 의존 라이브러리의 종류와 특징
#### 의존 라이브러리 이름
의존 라이브러리는 스프링 개발팀이 만든 것이 아니므로 모듈의 이름에 일정한 패턴이 있지는 않다. 하지만 스프링 소스가 OSGi 호환 모듈로 재패키징한 모듈의 이름은 일정한 패턴이 있다.

최신 아파치 Commons 프로젝트(http://commons.apache.org)의 Logging 라이브러리 파일의 이름은 commons-logging-1.1.1.jar이다. Maven 리포지토리에 등록된 라이브러리 파일 이름도 동일하다.

그런데 스프링소스는 Commons Logging 라이브러리 파일을 OSGi 표준에 맞도록 메타정보를 추가해서 재패키징하고, OSGi 모듈의 명명 규칙을 따라 다음과 같은 이름의 파일로 만들어 제공한다.
```
com.springsource.org.apache.commons.logging-1.1.1.jar
공통 패키지 이름       모듈 이름               버전
```
OSGi의 명명 규칙을 따라 모듈의 기본 패키지 이름을 모듈 이름으로 사용하게 했을뿐만 아니라, 스프링소스가 OSGi 플랫폼에서 사용 가능한 모듈로 재패키징했음을 나타내도록 
항상 com.springsource라는 공통 패키지 이름이 추가되어 있다.

오픈소스 프레임워크나 라이브러리뿐 아니라 자바의 표준 API를 위해서도 OSGi 호환 모듈이 제공된다. 예를 들어 서블릿 API는 보통 servlet-api.jar라는 이름의 
파일로 되어 있다. 반면에 스프링소스가 제공하는 OSGi 호환 라이브러리 이름은 com.springsource.javax.servlet-2.5.0.jar이다. 앞의 com.springsource 부분을 
제외하면 모듈의 패키지 이름을 알 수 있으므로 어떤 라이브러리 파일인지 쉽게 확인할 수 있을 것이다. 표준 API는 보통 서버에서 제공되기 때문에 애플리케이션 모듈에 포함시킬 
필요는 없지만, 빌드 작업 중에 필요하기 때문에 애플리케이션 프로젝트에 포함시켜야 한다.

스프링 모듈은 파일의 이름이 달라도 내용은 동일하다. 반면에 스프링 소스에 의해서 OSGi 호환 라이브러리로 재패키징된 파일은 라이브러리의 공식 배포파일이나 Maven에 등록된 파일과 구성이 조금 다
다를 수 있다는 점에 주의해야 한다.
#### 의존 라이브러리 추가
스프링 프로젝트에 의존 라이브러리를 추가하는 방법은 스프링 모듈과 마찬가지로 수동으로 라이브러리 파일을 직접 넣어주는 방법과, Maven 또는 Ivy의 의존 라이브러리 
관리 기능을 사용해 자동으로 넣어주는 방법이 있다.
##### 수동 추가
스프링의 의존 라이브러리 파일은 각 라이브러리의 웹사이트에서 직접 다운로드 받을 수 있다. 라이브러리의 웹사이트나 다운로드 정보는 검색엔진 등을 통해 직접 찾아야 한다. 
이때는 스프링 버전에 따라서 호환 가능한 의존 라이브러리 버전이 맞는지 확인해야 한다. 버전이 일치하지 않으면 바르게 동작하지 않을 수도 있다.

스프링소스가 제공하는 OSGi 호환 라이브러리 파일을 사용할 경우에는 스프링소스의 리포지토리 검색 기능을 활용하면 된다.

스프링 소스 엔터프라이즈 번들 리포지토리의 검색 기능(http://ebr.springsource.com/repository/app/)을 이용해 라이브러리를 찾으면 된다.

※ 각 라이브러리의 의존정보는 해당 라이브러리의 문서나 POM 정보를 참고.

> 수동 추가의 경우는 이렇게 추가적으로 필요한 의존 라이브러리를 일일이 찾아서 넣어줘야 하는 번거로움이 있다.
##### 자동 추가
Maven이나 Ivy를 이용하는 경우에는 Maven의 디폴트 리포지토리인 Maven Central에서 Maven 스타일의 이름을 가진 파일을 가져오는 방법과 스프링소스가 제공하는 Maven 리포지토리에서 OSGi 호환 파일을 가져오는 방법이 있다.

디폴트 리포지토리를 이용하는 경우에는 Maven 검색 사이트(http://mvnrepository.com/)를 통해 손쉽게 의존 라이브러리 설정정보를 얻을 수 있다.

Commons Logging이라면 다음과 같은 ```<dependency>``` 태그를 pom.xml에 넣어주면 된다.
```
<dependency>
  <groupId>commons-logging</groupId>
  <artifactId>commons-logging</artifactId>
  <version>1.1.1</version>
</dependency>
```
스프링소스가 제공하는 OSGi 호환 라이브러리 Maven 리포지토리를 이용하려면 먼저 다음과 같은 추가 리포지토리 설정을 pom.xml에 넣어줘야 한다.
```
<repository>
  <id>com.springsource.repository.bundles.external</id>
  <name>SpringSource Enterprise Bundle Repository - External Bundle Releases</name>
  <usl>http://repository.springsource.com/maven/bundles/external</url>
</repository>
```
스프링소스 Maven 리포지토리에 등록된 Commons Logging은 다음과 같은 설정을 통해 가져올 수 있다. 각 라이브러리의 Maven 설정정보는 검색(http://ebr.springsource.com/repository/app/) 결과에서 찾을 수 있다.
```
<dependency>
  <groupId>org.apache.commons</groupId>
  <artifactId>com.springsource.org.apache.commons.logging</artifactId>
  <version>1.1.1</version>
</dependency>
```
가능하면 의존 라이브러리의 리포지토리는 하나로 통일하는 게 좋다. 하지만 스프링 소스의 OSGi 호환 라이브러리 리포지토리는 Maven Central보다 최신 버전의 업데이트가 느린 편이다. 따라서 스프링소스 리포지토리의 라이브러리를 사용하더라도 일부 라이브러리는 Maven Central 리포지토리에서 가져와야 할 수도 있다.

※ Ivy의 리포지토리 설정은 스프링소스 리포지토리의 FAQ(http://ebr.springsource.com/repository/app/faq)를 참고.

> 자동 의존 라이브러리 관리 기능을 사용할 경우 라이브러리의 의존정보를 참고해서 추가적인 의존 라이브러리도 포함해준다. 하지만 의존정보가 항상 완벽하진 않고, 프로젝트마다 선택적으로 추가하거나 제외시켜야 할 것도 있으므로 어떤 라이브러리가 자동으로 추가되는지 직접 확인해볼 필요가 있다.
<hr/>

### 모듈별 의존 라이브러리 의존관계
거의 대부분의 의존 라이브러리는 필수가 아니다. 따라서 모듈별 선택 라이브러리 중에서 적절한 라이브러리를 선택할 수 있어야 한다. 
#### 필수 라이브러리
* Commons Logging 1.1.1
  * com.spring source.org.apache.commons.logging-1.1.1.jar
    * 재패키징 모듈인 ASM과 자바 에이전트와 톰캣 클래스 로더로 사용되는 Instrument와 Instrument-Tomcat 모듈을 제외한 모든 스프링 모듈은 아파치 Commons 프로젝트의 Logging 라이브러리(http://commons.apache.org/logging/)를 사용해 로그를 남기도록 되어 있다. 따라서 Commons Logging은 모든 스프링 프로젝트에 포함시켜야 하는 필수 라이브러리다. Commons-Logging 대신 SLF4J(http://www.slf4j.org)를 사용한다면 jcl-over-slf4j로 대체할 수도 있다.
#### 모듈별 선택 라이브러리
선택 의존 라이브러리 중에서 패키지 이름이 javax로 시작하는 것은 자바의 표준 API 라이브러리다. 표준 API 중 일부는 서버에서 제공되기 때문에 빌드 중에는 필요하지만 서버에 배포할 때는 제외해야 하는 것들도 있다. 예를 들어 Servlet API는 코드에서 사용하는 경우 빌드 과정에는 필요하지만 서버에 배포할 웹 모듈 패키지에는 넣을 필요가 없다.
##### ASM 모듈
ASM 모듈은 단순히 ASM 라이브러리를 재패키징한 것이기 때문에 다른 의존 라이브러리는 없다.
##### Core 모듈
Core 모듈의 필수 라이브러리는 없다.
* AspectJ Weaver 1.6.8
  * com.springsource.org.aspectj.weaver-1.6.8.RELEASE.jar<br/>
    * AspectJTypeFilter 클래스에서 사용된다. 이 클래스를 사용할 경우, AspectJ 기능을 활용하는 다른 모듈이 함께 사용된다. Core 모듈의 선택 의존 라이브러리이긴 하지만 무시해도 좋다.
* JBoss VFS 2.1.0.GA
  * com.springsource.org.jboss.virtual-2.1.0.GA.jar
    * JBoss 서버가 제공하는 VFS(Virtual File System)에 대한 VFS 리소스 추상화 기능을 위해 사용된다. JBoss VFS의 리소스를 직접 활용해야 할 경우에 사용한다.
##### Beans 모듈
Beans 모듈은 필수 의존 라이브러리가 없다.
* CGLib 2.2.0/2.1.3
  * com.springsource.net.sf.cglib-2.2.0.jar
    * 프로토타입 빈의 DL 기능을 자동으로 추가해주는 메소드 주입(method injection)을 사용할 때 필요하다.
##### AOP 모듈
* AOP Alliance 1.0.0
  * com.springsource.org.aopalliance-1.0.0.jar
    * AOP 모듈의 필수 라이브러리다. 어드바이스를 만들 때 사용하는 Advice, MethodInterceptor 인터페이스 등이 정의되어 있다.
* Jamon API 2.4.0
  * com.springsource.com.jamonapi-2.4.0.jar
    * 애플리케이션 모니터 API인 Jamon API(http://jamonapi.sourceforge.net/)를 이용한 성능측정용 어드바이스인 JamonPerformanceMonitorInterceptor에 사용된다. AOP를 이용한 간단한 성능측정/모니터링 기능이 필요한 경우에 사용할 수 있다.
* GCLib 2.2.0/2.1.3
  * com.springsource.net.sf.cglib-2.2.0.jar
    * CGLib 기반의 프록시를 만들 때 사용한다. 인터페이스 대신 클래스 상속을 통한 클래스 프록시를 만들 수 있다.
* Commons Pool 1.5.3/1.3.0
  * com.springsource.org.apache.commons.pool-1.5.3.jar
    * AOP에서 사용되는 TargetSource 구현의 하나인 CommonsPoolTargetSource에서 사용된다. TargetSource를 풀링 방식으로 설정해서 사용할 수 있는 고급 사용자에게 필요한 기능이다. 일반적으로는 무시해도 좋다.
* AspectJ Weaver 1.6.8
  * com.springsource.org.aspectj.weaver-1.6.8.RELEASE.jar
    * AspectJ 스타일의 스프링 AOP를 사용할 경우에 필요하다. AspectJ의 문법과 애노테이션 등을 차용해서 스프링 AOP를 만들기 위해 필요한 것이지 AspectJ 컴파일러나 위빙 기능을 사용하기 위한 것은 아니다. AspectJ 애노테이션을 사용해 만든 어드바이스, 포인트컷 등을 쓰려면 추가해야 한다.
##### Expression 모듈
Expression 모듈은 의존 라이브러리가 없다.
##### Context 모듈
* BeanShell 2.0.0.b4
  * com.springsource.bsh-2.0.0.b4.jar
    * 스프링의 BeanShell 언어 지원 기능을 사용할 때 필요하다. 스프링은 BeanShell 스크립트를 실행하거나 빈으로 등록해서 사용할 수 있는 기능을 제공한다.
* Backport 3.0.0
  * com.springsource.edu.emory.mathcs.backport-3.0.0.jar
    * JSR-166 Backport를 이용한 스케줄링 기능(Executor, TaskExecutor 등)을 사용할 때 필요하다.
* JSR-250 Common Annotations 1.0.0
  * com.springsource.javax.annotation-1.0.0.jar
    * @Resource, @PostConstruct 같은 표준 애노테이션을 정의하고 있다. 애노테이션을 이용한 DI를 적용하려면 필요하다. 단, JDK 6나 JavaEE 1.5 이상에서는 기본적으로 제공되기 때문에 포함하지 않아도 된다.
* EJB 3.0.0
  * com.springsource.javax.ejb-3.0.0.jar
    * 스프링의 EJB 지원 기능에 필요하다. EJB 빈을 프록시를 통해 DI 해서 사용할 경우에 쓴다.
* JSR-330 DI for Java 1.0.0
  * com.springsource.javax.inject-1.0.0.jar
    * @Inject와 ```Provider<T>``` 같은 JSR-330 애노테이션을 이용해 DI 하는 경우에 필요하다.
* JMS 1.1.0
  * com.springsource.javax.jms-1.1.0.jar
    * 스프링의 JMS 지원 기능을 사용하는 경우에 필요하다.
* Java Persistence API 1.0.0
  * com.springsource.javax.persistence-1.0.0.jar
    * JPA 지원을 위한 클래스 로더에서 필요로 한다. JPA를 사용하지 않는다면 포함시킬 필요는 없다.
* JSR-303 Bean Validation 1.0.0.GA
  * com.springsource.javax.validation-1.0.0.GA.jar
    * JSR-303의 빈 검증기를 이용한 검증 기능을 적용할 때 필요하다. JSR-303은 웹 환경의 모델 검증에도 사용되지만 독자적으로 검증기를 만들어 적용할 수도 있다.
* JAX-WS 2.1.1
  * com.springsource.javax.xml.ws-2.1.1.jar
    * JAX-WS의 @WebServiceRef 애노테이션을 사용할 때 필요하다.
* GCLib 2.2.0/2.1.3
  * com.springsource.net.sf.gclib-2.2.0.jar
    * 스코프 프록시, JMX, 스크립팅 언어 지원 기능 등에 사용된다.
* AOP Alliance 1.0.0
  * com.springsource.org.aopalliance-1.0.0.jar
    * 스프링 이벤트, EJB, JMX, JNDI, 리모팅, 스크립팅 등 많은 기능에 사용한다. 반드시 포함시키도록 한다.
* AspectJ Weaver 1.6.8
  * com.springsource.org.aspectj.weaver-1.6.8.RELEASE.jar
    * 로드타임 위버 기능에 사용된다. LTW를 사용하지 않는다면 필요하지 않다.
* Groovy 1.6.5
  * com.springsource.org.codehaus.groovy-1.6.5.jar
    * Groovy 스크립팅 언어 지원 기능에 사용한다. Groovy를 이용해서 빈을 만들어서 사용하거나 Groovy 코드를 실행시켜야 할 때 필요하다.
* Joda Time 1.6.0
  * com.springsource.org.joda.time-1.6.0.jar
    * Joda(Java Date and time API http://joda-time.sourceforge.net/)를 이용한 포매터에 사용된다.
* JRuby 1.4.0
  * com.springsource.org.jruby-1.4.0.jar
    * JRuby 스크립팅 언어 지원 기능에 사용한다. JRuby를 이용해 만든 빈을 사용할 경우에 필요하다.
##### Context.Support 모듈
* CommonJ 1.1.0
  * com.springsource.commonj-1.1.0.jar
    * CommonJ를 이용한 스케줄링 기능을 사용할 때 필요하다.
* FreeMarker 2.3.15
  * com.springsource.freemarker-2.3.15.jar
    * FreeMarker 템플릿의 지원 기능에 필요하다. FreeMarker는 스프링 MVC의 뷰로도 사용하지만 독립적인 템플릿 엔진으로 활용할 수도 있다.
* JAF(JavaBeans Activation Framework) 1.1.0
  * com.springsource.javax.activation-1.1.0.jar
    * JavaMail을 이용한 메일 메시지 추상화에 필요하다.
* Java Mail 1.4.0
  * com.springsource.javax.mail-1.4.0.jar
    * JavaMail을 이용한 메일 메시지 추상화에 필요하다.
* Ehcache 1.6.2
  * com.springsource.net.sf.ehcache-1.6.2.jar
    * EhCacheFactoryBean을 이용해 EhCache 빈을 생성할 경우에 필요하다.
* JasperReports 2.0.5
  * com.springsource.net.sf.jasperreports-2.0.5.jar
    * JasperReport를 이용할 때 활용할 수 있는 편리한 기능을 제공해주는 JasperReportsUtils를 사용할 때 필요하다.
* Velocity 1.5.0
  * com.springsource.org.apache.velocity-1.5.0.jar
    * Velocity 템플릿 엔진을 사용하는 경우에 포함시켜야 한다.
* Commons Collection 3.2.1/3.2.0
  * com.springsource.org.apache.commons.collections-3.2.1.jar
    * Velocity를 위한 리소스 로더에 사용된다. Velocity를 사용하는 경우에 추가한다.
* Quartz 1.6.2
  * com.springsource.org.quartz-1.6.2.jar
    * Quartz를 이용한 스케줄링 기능을 사용할 때 필요하다.
##### Transaction 모듈
* AOP Alliance 1.0.0
  * com.springsource.org.aopalliance-1.0.0.jar
    * Transaction 모듈의 필수 라이브러리다. JPA의 예외자동 변환용 AOP와 트랜잭션 AOP에 필요하다.
* WebSphere UOW 6.0.2.17
  * com.springsource.com.ibm.websphere.uow-6.0.2.17.jar
    * 웹스피어 UOW 트랜잭션 매니저를 사용할 때 필요하다. 웹스피어에 특화된 트랜잭션 확장 기능을 사용할 수 있게 해준다.
* EJB 3.0.0
  * com.springsource.javax.ejb-3.0.0.jar
    * EJB 3의 @TransactionAttribute 애노테이션을 트랜잭션 속성을 부여하는 데 사용하기 위해 필요하다.
* Java Resource 1.5.0
  * com.springsource.javax.resource-1.5.0.jar
    * 스프링의 JCA 지원 기능을 사용할 때 필요하다.
* Java Transaction 1.1.0
  * com.springsource.javax.transaction-1.1.0.jar
    * JTA 트랜잭션 매니저를 사용할 때 필요하다.
##### JDBC 모듈
* C3P0 0.9.1.2
  * com.springsource.com.mchange.v2.c3p0-0.9.1.2.jar
    * C3P0는 스프링 애플리케이션에서 애용되는 애플리케이션 내장 DB 커넥션 풀의 하나다. C3P0를 DB 풀로 사용한다면 이미 추가됐을 것이다. Transaction 모듈에서는 C3P0NativeJdbcExtractor에서 사용한다. Native JDBC 오브젝트를 가져와야 하는 특별한 경우에 사용된다.
* Java Transaction 1.1.0
  * com.springsource.javax.transaction-1.1.0.jar
    * JTA Lob 지원 기능에 사용된다. JTA를 사용한다면 이미 JDBC의 의존모듈인 Transaction 모듈에서 포함시켰을 것이므로 신경 쓰지 않아도 된다.
* Derby 10.5.1000001.764942
  * com.springsource.org.apache.derby-10.5.1000001.764942.jar
    * 내장형 DB인 Derby를 사용할 경우에 필요하다. 스프링의 내장형 DB 추상화 기능에도 사용된다.
* H2 1.0.71
  * com.springsource.org.h2-1.0.71.jar
    * 내장형 DB인 H2를 사용할 경우에 필요하다.
* HSQLDB 1.8.0.9
  * com.springsource.org.hsqldb-1.8.0.9.jar
    * 내장형 DB인 HSQLDB를 사용할 경우에 필요하다.
##### ORM 모듈
* iBatis 2.3.4.726
  * com.springsource.com.ibatis-2.3.4.726.jar
    * iBatis를 이용해 DAO를 만들 때 필요하다.
* JDO 2.1.0
  * com.springsource.javax.jdo-2.1.0.jar
    * JDO를 이요해 DAO를 만들 때 필요하다.
* Java Persistence API 1.0.0
  * com.springsource.javax.persistence-1.0.0.jar
    * JPA를 이용해 DAO를 만들 때 필요하다.
* Servlet 2.5.0
  * com.springsource.javax.servlet-2.5.0.jar
    * 서블릿 환경에서 하이버네이트의 OpenSessionInViewFilter나 JPA의 OpenEntityManaverInViewFilter 기능을 이용할 때 필요하다.
* Java Transaction 1.1.0
  * com.springsource.javax.transaction-1.1.0.jar
    * Hibernate 3에서 사용하므로 반드시 추가해야 한다.
* TopLink Essentials 2.0.0.b41-beta2
  * com.springsource.oracle.toplink.essentials-2.0.0.b41-beta2.jar
    * JPA 구현 엔진으로 TopLink Essentials를 사용할 때 추가해준다.
* AOP Alliance 1.0.0
  * com.springsource.org.aopalliance-1.0.0.jar
    * Hibernate나 JPA의 템플릿/콜백을 대신해서 AOP 방식으로 세션과 엔티티 매니저를 바인딩할 경우에 필요하다. 기본적으로 트랜잭션 매니저를 사용하고 트랜잭션 AOP를 적용하고 있다면 ORM 모듈에서는 신경 쓰지 않아도 된다.
* OpenJPA 1.1.0
  * com.springsource.org.apache.openjpa-1.1.0.jar
    * JPA 구현 제품의 하나인 OpenJPA를 사용할 경우에 필요하다.
* Eclipse Persistence JPA 1.0.1
  * com.springsource.org.eclipse.persistence.jpa-1.0.1.jar
    * 이클립스의 JPA 구현 제품인 EclipseLink JPA를 사용할 때 추가한다.
* Hibernate 3.3.1.GA
  * com.springsource.org.hibernate-3.3.1.GA.jar
    * 스프링이 지원하는 하이버네이트의 최신 버전은 3.3.1.GA다. 하이버네이트를 이용한 DAO를 작성하거나 하이버네이트를 JPA 구현으로 사용할 경우에 모두 필요하다. 하이버네이트의 사용 방법에 따라서 추가적인 의존 라이브러리가 필요할 수 있다.
* Hibernate Annotation 3.4.0.GA
  * com.springsource.org.hibernate.annotations-3.4.0.GA.jar
    * 하이버네이트의 애노테이션을 이용한 ORM 설정을 사용하는 경우 필요하다.
* Hibernate EJB 3.4.0.GA
  * com.springsource.org.hibernate.ejb-3.4.0.GA.jar
    * 하이버네이트를 JPA 엔진으로 사용하는 경우에 필요하다.
##### Web 모듈
* Caucho 3.2.1
  * com.springsource.com.caucho-3.2.1.jar
    * Burlap/Hessian을 이용한 리모팅 기능을 사용할 때 필요하다.
* Java EL 1.0.0
  * com.springsource.javax.el-1.0.0.jar
    * Web 모듈에서 JSF 지원 기능에 필요하다.
* Java Faces 1.2.0.08
  * com.springsource.javax.faces-1.2.0.08.jar
    * JSF를 스프링의 웹 기술로 사용할 때 필요하다.
* Portlet 2.0.0
  * com.springsource.javax.portlet-2.0.0.jar
    * 포틀릿을 사용할 때 필요하다.
* Servlet 2.5.0
  * com.springsource.javax.servlet-2.5.0.jar
    * 서블릿 기반의 웹 모듈의 핵심 API다. 서블릿 API를 직접 코드에서 사용하지 않는다면, 서버에서 기본적으로 제공해주므로 신경 쓰지 않아도 좋다.
* JSP 2.1.0
  * com.springsource.javax.servlet.jsp-2.1.0.jar
    * 스프링의 태그 지원 기능에서 사용한다. 서버에서 지원되므로 대부분의 경우 포함시키지 않아도 된다.
* JAX-RPC 1.1.0
  * com.springsource.javax.xml.rpc-1.1.0.jar
    * JAX-RPC를 이용한 리모팅 기능을 만들 때 사용한다.
* XML SOAP 1.3.0
  * com.springsource.javax.xml.soap-1.3.0.jar
    * JAX-WS를 사용해 리모팅 기능을 만들 때 필요하다.
* JAX-WS 2.1.1
  * com.springsource.javax.xml.ws-2.1.1.jar
    * JAX-WS를 사용해 리모팅 기능을 만들 때 필요하다.
* AOP Alliance 1.0.0
  * com.springsource.org.aopalliance-1.0.0.jar
    * 모든 리모팅 기능에 필요하다.
* Axis 1.4.0
  * com.springsource.org.apache.axis-1.4.0.jar
    * Axis를 이용해 웹 서비스를 구현할 때 필요하다. 하지만 Axis보다는 JAX-WS를 사용하도록 권장한다. Axis 지원 기능은 앞으로 제거될 예정이다.
* Commons Fileupload 1.2.0
  * com.springsource.org.apache.commons.fileupload-1.2.0.jar
    * 웹에서 파일 업로드 기능을 사용할 때 필요하다.
* Commons HttpClient 3.1.0
  * com.springsource.org.apache.commons.httpclient-3.1.0.jar
    * REST 템플릿을 사용할 때 필요하다.
* Log4J 1.2.15
  * com.springsource.org.apache.log4j-1.2.15.jar
    * Log4jConfigListener를 이용해 애플리케이션 레벨의 Log4J 설정을 해줄 때 필요하다.
* Jackson Mapper 1.4.2
  * com.springsource.org.codehaus.jackson.mapper-1.4.2.jar
    * JSON 메시지 컨버터에 사용된다.
##### Web.Servlet 모듈
* iText 2.0.8
  * com.springsource.com.lowagie.text-2.0.8.jar
    * iText를 이용한 PDF 뷰를 만들 때 필요하다.
* Syndication 1.0.0
  * com.springsource.com.sun.syndication-1.0.0.jar
    * RSS/Atom 피드 뷰를 만들 때 필요하다.
* FreeMarker 2.3.15
  * com.springsource.freemarker-2.3.15.jar
    * FreeMarker 뷰를 만들 때 필요하다.
* Servlet 2.5.0
  * com.springsource.javax.servlet-2.5.0.jar
    * 코드에서 서블릿 API를 직접 사용하는 경우에 필요하다. 배포 패키지에는 포함시키지 않아도 된다.
* JSP 2.1.0
  * com.springsource.javax.servlet.jsp-2.1.0.jar
    * JSP/JSTL 뷰(InternalResourceView, JstlView)와 폼 태그에 사용된다. 코드에서 직접 참조하는 것이 아니라면 포함시키지 않아도 된다.
* JSTL 1.1.2
  * com.springsource.javax.servlet.jsp.jstl-1.1.2.jar
    * JSP/JSTL 뷰(InternalResourceView, JstlView)와 폼 태그에 사용된다.
* JExcepApi 2.6.6
  * com.springsource.jxl-2.6.6.jar
    * JExcel API를 이용해 엑셀 뷰를 만들 때 필요하다.
* JasperReports 2.0.5
  * com.springsource.net.sf.jasperreports-2.0.5.jar
    * JasperReports를 이용한 뷰를 만들 때 필요하다.
* POI 3.0.2.FINAL
  * com.springsource.org.apache.poi-3.0.2.FINAL.jar
    * POI를 이용한 엑셀 뷰를 만들 때 필요하다.
* Tiles 2.1.2.osgi
  * com.springsource.org.apache.tiles-2.1.2.osgi.jar
    * Tiles 뷰에 사용된다.
* Tiles Core 2.1.2.osgi
  * com.springsource.org.apache.tiles.core-2.1.2.osgi.jar
    * Tiles 뷰에 사용된다.
* Tiles JSP 2.1.2
  * com.springsource.org.apache.tiles.jsp-2.1.2.jar
    * Tiles 뷰에 사용된다.
* Tiles Servlet 2.1.2
  * com.springsource.org.apache.tiles.servlet-2.1.2.jar
    * Tiles뷰에 사용된다.
* Velocity 1.5.0
  * com.springsource.org.apache.velocity-1.5.0.jar
    * Velocity 뷰를 만들 때 필요하다.
* Velocity Tools View 1.4.0
  * com.springsource.org.apache.velocity.tools.view-1.4.0.jar
    * Velocity 뷰를 만들 때 필요하다.
* Jackson Mapper 1.4.2
  * com.springsource.org.codehaus.jackson.mapper-1.4.2.jar
    * JSON 뷰를 만들 때 필요하다.
##### Web.Portlet 모듈
* Portlet 2.0.0
  * com.springsource.javax.portlet-2.0.0.jar
    * 포틀릿 API를 사용하는 경우에 필요하다.
* Servlet 2.5.0
  * com.springsource.javax.servlet-2.5.0.jar
    * 서블릿 API를 사용하는 경우에 필요하다.
* Commons Fileupload 1.2.0
  * com.springsource.org.apache.commons.fileupload-1.2.0.jar
    * 포틀릿에 파일 업로드 기능을 넣을 때 필요하다.
##### Web.Struts 모듈
* Commons Beanutils 1.7.0/1.8.0
  * com.springsource.org.apache.commons.beanutils-1.7.0.jar
    * 스트럿츠 1과의 연동을 위해 필요하다.
##### JMS 모듈
* JMS 1.1.0
  * com.springsource.javax.jms-1.1.0.jar
    * JMS API를 사용하는 경우에 필요하다.
* Java Resource 1.5.0
  * com.springsource.javax.resource-1.5.0.jar
    * JMS 메시지 리스너를 등록해서 사용할 때 필요하다.
* AOP Alliance 1.0.0
  * com.springsource.org.aopalliance-1.0.0.jar
    * JmsInvokerClientInterceptor에 사용된다.
##### Aspects 모듈
* AspectJ Weaver 1.6.8
  * com.springsource.org.aspectj.weaver-1.6.8.RELEASE.jar
    * AspectJ AOP 기능을 사용하기 위해 반드시 추가해야 한다.
##### Instrument 모듈
Instrument 모듈은 의존 라이브러리가 없다.
##### Instrument.Tomcat 모듈
Instrument.Tomcat 모듈은 의존 라이브러리가 없다.
##### Test 모듈
테스트 모듈이 직접 의존하는 것은 JUnit과 같은 테스트 프레임워크뿐이다. 하지만 테스트 방법에 따라서 DBUnit과 같은 다양한 테스트용 라이브러리가 추가로 필요할 수 있다.
* JUnit 3.8.2
  * com.springsource.junit-3.8.2.jar
    * JUnit 3.8.2를 이용해 테스트를 만들 때 필요하다.
* JUnit 4.7.0
  * com.springsource.org.junit-4.7.0.jar
    * JUnit 4.7을 이용해 테스트를 만들 때 필요하다.
    
