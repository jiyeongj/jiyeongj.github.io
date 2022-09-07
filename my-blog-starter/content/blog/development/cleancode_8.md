---
title: '[Clean Code] 8. 경계'
date: 2022-09-07 09:26:14
category: 'development'
draft: false
---


![](.\images\cleancode.jpg)

- 클린 코드 책을 읽으며 내용을 요약 정리한 글입니다.

# 8. 경계

- 시스템에 들어가는 외부 코드(패키지, 오픈소스)를 우리 코드에 깔끔하게 통합해야 한다.
- 소프트웨어 경계를 깔끔하게 처리하는 기법과 기교를 살펴보자.

## 외부 코드 사용하기

- `java.util.Map` 예시
    - Map 사용자라면 누구나 Map을 지우거나 추가할 수 있다.
- Map을 깔끔하게 사용한 코드
    ```java
    public class Sensors {
        private Map sensors = new HashMap();
        
        public Sensor getById(String id) {
            return (Sensor)sensors.get(id);
        }
    }
    ```
    - Sensor 클래스 안에서 객체 유형을 관리하고 변환한다.
    - Sensor 클래스는 프로그램에 필요한 인터페이스만 제공한다.
    - Map과 같은 경계 인터페이스를 이용할 때는 이를 이용하는 클래스나 클래스 계열 밖으로 노출되지 않도록 주의한다.
    - Map 인스턴스를 공개 API의 인수로 넘기거나 반환값으로 사용하지 않는다.

## 경계 살피고 익히기

- 외부 코드를 사용할 때 우리 자신을 위해 우리가 사용할 코드를 테스트하는 편이 바람직하다.
- 우리쪽 코드를 작성해 외부 코드를 호출하는 대신 먼저 간단한 테스트 케이스를 작성해 외부 코드를 익힌다. (짐 뉴커크는 이를 **학습 테스트**라 부른다.)

## log4j 익히기

- 화면에 "hello"를 출력하는 테스트 케이스를 작성한다.
    ```java
    @Test
    public void testLogCreate() {
        Logger logger = Logger.getLogger("MyLogger");
        logger.info("hello");
    }
    ```
- 위 테스트 케이스를 돌렸더니 Appender가 필요하다는 오류가 발생한다. 문서를 좀 더 읽어보니 ConsoleAppender라는 클래스가 있어서 ConsoleAppender를 생성한 후 테스트케이스를 다시 돌린다.
    ```java
    @Test
    public void testLogAddAppender() {
        Logger logger = Logger.getLogger("MyLogger");
        ConsoleAppender appender = new ConsoleAppender();
        logger.addAppender(appender);
        logger.info("hello");
    }
    ```
- 이번엔 Appender에 출력 스트림이 없다는 사실을 발견한다. 구글 검색 후 다시 시도한다.
    ```java
    @Test
    public void testLogAddAppender() {
        Logger logger = Logger.getLogger("MyLogger");
        logger.removeAllAppenders();
        logger.addAppender(new ConsoleAppender(
            new PatternLayout("%p %t %m%n"),
            ConsoleAppender.SYSTEM_OUT));
        logger.info("hello");
    }
    ```
- 성공했다. 하지만 ConsoleAppender에게 콘솔에 쓰라고 알려야 하다니 뭔가 수상하다. 그래서 onsoleAppender.SYSTEM_OUT 인수를 제거했더니 문제가 없다. 하지만 PatternLayout을 제거하니 출력 스트림이 없다는 오류가 또 뜬다.
문서를 자세히 읽어보니 기본 ConsoleAppender 생성자는 '설정되지 않은 상태'란다. 당연하지도 유용하지도 않은 log4j 버그이거나 일관성 부족으로 여겨진다.
- 좀 더 구글을 뒤지고, 문서를 읽어보고 테스트를 돌린 끝에 테스트 케이스 몇 개로 표현했다.
    ```java
    public class LogTest {
        private Logger logger;
        
        @Before
        public void initialize() {
            logger = Logger.getLogger("logger");
            logger.removeAllAppenders();
            Logger.getRootLogger().removeAllAppenders();
        }
        
        @Test
        public void basicLogger() {
            BasicConfigurator.configure();
            logger.info("basicLogger");
        }
        
        @Test
        public void addAppenderWithStream() {
            logger.addAppender(new ConsoleAppender(
                new PatternLayout("%p %t %m%n"),
                ConsoleAppender.SYSTEM_OUT));
            logger.info("addAppenderWithStream");
        }
        
        @Test
        public void addAppenderWithoutStream() {
            logger.addAppender(new ConsoleAppender(
                new PatternLayout("%p %t %m%n")));
            logger.info("addAppenderWithoutStream");
        }
    }
    ```
- 나머지 프로그램은 log4j 경계 인터페이스를 몰라도 된다.

## 학습 테스트는 공짜 이상이다.

- 학습 테스트는 투자하는 노력보다 얻는 성과가 더 크다. 패키지 새 버전이 나온다면 학습 테스트를 돌려 차이가 있는지 확인한다.
- 학습 테스트는 패키지가 예상대로 도는지 검증한다. 통합한 이후라고 하더라도 패키지가 우리 코드와 호환되리라는 보장은 없다.
- 패키지 새 버전이 나오면 새 버전이 우리 코드와 호환되지 않으면 학습 테스트가 이 사실을 곧바로 밝혀낸다.
- 실제 코드와 동일한 방식으로 인터페이스를 사용하는 테스트 케이스가 필요하다.

## 아직 존재하지 않는 코드를 사용하기

- 아는 코드와 모르는 코드를 분리하는 경계
- Adapter 패턴으로 API 사용을 캡슐화해 API가 바뀔 때 수정할 코드를 한곳으로 모았다.

## 깨끗한 경계

- 소프트웨어 설계가 우수하다면 변경하는데 많은 투자와 재작업이 필요하지 않다.
- 통제하지 못하는 코드를 사용할 때는 너무 많은 투자를 하거나 향후 변경 비용이 지나치게 커지지 않도록 각별히 주의해야 한다.
- 경계에 위치하는 코드는 깔끔히 분리한다.
- 외부 패키지를 호출하는 코드를 가능한 줄여 경계를 관리한다.
- 새로운 클래스로 경계를 감싸거나 Adapter 패턴을 사용해 우리가 원하는 인터페이스를 패키지가 제공하는 인터페이스로 변환한다.

