---
title: '[Clean Code] 4. 주석'
date: 2022-08-11 10:24:14
category: 'development'
draft: false
---


![](.\images\cleancode.jpg)

- 클린 코드 책을 읽으며 내용을 요약 정리한 글입니다.

# 4. 주석


- 프로그래밍 언어를 치밀하게 사용해 의도를 표현할 능력이 있다면 주석은 거의 필요하지 않다.
- 주석은 오래될수록 코드에서 멀어지고 주석을 유지 보수하는것은 현실적으로 불가능하다.
- 부정확한 주석은 아예 없는 주석보다 훨씬 더 나쁘다.


## 주석은 나쁜 코드를 보완하지 못한다.

- 코드에 주석을 추가하는 일반적인 이유는 코드 품질이 나쁘기 때문이다.
- 표현력이 풍부하고 깔끔하며 주석이 거의 없는 코드가, 복잡하고 어수선하며 주석이 많이 달린 코드보다 훨씬 좋다.

## 코드로 의도를 표현하라.

## 좋은 주석

### 법적인 주석

- 회사가 적립한 구현 표준에 맞춰 법적인 이유로 특정 주석을 넣는 경우
    - 저작권 정보, 소유권 정보

### 정보를 제공하는 주석

- 기본적인 정보를 주석으로 제공하는 경우. 그럴지라도 함수 이름에 정보를 담는 편이 더 좋다.

### 의도를 설명하는 주석

- 때때로 주석은 구현을 이해하게 도와주는 선을 넘어 결정에 깔린 의도까지 설명한다.

    ```java{1}
    // 스레드를 대량 생성하는 방법으로 어떻게든 경쟁 조건을 만들려 시도한다. 
    for (int i = 0; i > 2500; i++) {
        WidgetBuilderThread widgetBuilderThread = 
            new WidgetBuilderThread(widgetBuilder, text, parent, failFlag);
        Thread thread = new Thread(widgetBuilderThread);
        thread.start();
    }
    ```

### 결과를 경고하는 주석

- 다른 프로그래머에게 결과를 경고할 목적으로 사용하는 경우
    - 예를 들어, 특정 테스트 케이스를 꺼야 하는 이유를 설명하는 주석

    ```java{1}
    // 여유 시간이 충분하지 않다면 실행하지 마십시오.
    public void _testWithReallyBigFile() {

    }
    ```

### TODO 주석

- '앞으로 할 일'을 `//TODO` 주석으로 남겨두면 편하다.
    - 더 이상 필요 없는 기능을 삭제하라는 알림, 누군가에게 문제를 봐달라는 요청, 더 좋은 이름을 떠올려달라는 부탁, 앞으로 발생할 이벤트에 맞춰 코드를 고치라는 주의 등에 유용하다.

    ```java{1}
    // TODO-MdM 현재 필요하지 않다.
    // 체크아웃 모델을 도입하면 함수가 필요 없다.
    protected VersionInfo makeVersion() throws Exception {
        return null;
    }
    ```

### 중요성을 강조하는 주석

```java{2,3}
String listItemContent = match.group(3).trim();
// 여기서 trim은 정말 중요하다. trim 함수는 문자열에서 시작 공백을 제거한다.
// 문자열에 시작 공백이 있으면 다른 문자열로 인식되기 때문이다. 
new ListItemWidget(this, listItemContent, this.level + 1);
return buildList(text.substring(match.end()));
```

### 공개 API에서 Javadocs

- 설명이 잘 된 공개 API는 참으로 유용하고 만족스럽다. 공개 API를 구현한다면 반드시 훌륭한 Javadocs 작성을 추천한다. 


## 나쁜 주석

- 대다수의 주석이 이 범주에 속한다. 
- 일반적으로 대다수 주석은 허술한 코드를 지탱하거나, 엉성한 코드를 변명하거나, 미숙한 결정을 합리화하는 등 프로그래머가 주절거리는 독백에서 크게 벗어나지 못한다.

### 주절거리는 주석

- 특별한 이유없이 마지못해 주석을 단다면 시간낭비다. 주석을 달기로 결정했다면 충분한 시간을 들여 최고의 주석을 달도록 노력한다.

### 같은 이야기를 중복하는 주석

- 주석이 코드 내용을 중복한다.

### 오해할 여지가 있는 주석

### 의무적으로 다는 주석

- 모든 함수에 Javadocs를 넣거나 모든 변수에 주석을 달아야 한다는 규칙은 잘못된 정보를 제공할 수 있다.

### 이력을 기록하는 주석

- 지금은 소스 코드 관리 시스템이 있으니 전혀 필요없다.

### 있으나 마나 한 주석

```java
/*
 * 기본 생성자
 */
protected AnnualDateRule() {

}
```

### 함수나 변수로 표현할 수 있다면 주석을 달지 마라

```java
// 전역 목록 <smodule>에 속하는 모듈이 우리가 속한 하위 시스템에 의존하는가?
if (module.getDependSubsystems().contains(subSysMod.getSubSystem()))
```

주석을 없애고 다시 표현하면 다음과 같다.

```java
ArrayList moduleDependencies = smodule.getDependSubSystems();
String ourSubSystem = subSysMod.getSubSystem();
if (moduleDependees.contains(ourSubSystem))
```

### 위치를 표시하는 주석

```java
// Actions /////////////////////////////////////////////
```

- 이런 주석은 가독성만 낮추므로 제거해야 마땅하다. 특히 뒷부분에 슬래시로 이어지는 잡음은 제거하는 편이 좋다. 
- 너무 자주 사용하지 않는다면 배너는 눈에 띄며 주위를 환기한다. 그러므로 반드시 필요할 때 아주 드몰게 사용하는 편이 좋다.


### 닫는 괄호에 다는 주석

- 작고 캡슐화된 함수에는 잡음일 뿐이다.

### 공로를 돌리거나 저자를 표시하는 주석

- 이러한 정보는 소스 관리 시스템에 있다.

### 주석으로 처리한 코드

```java{3,4,5}
InputStreamResponse response = new InputStreamResponse();
response.setBody(formatter.getResultStream(), formatter.getByteCount());
// InputStream resultsStream = formatter.getResultStream();
// StreamReader reader = new StreamReader(resultsStream);
// response.setContent(reader.read(formatter.getByteCount()));

```

- 주석으로 처리된 코드는 다른 사람들이 지우기를 주저한다. 이유가 있어 남겨놓았으리라고, 중요하니까 지우면 안된다고 생각한다. 그래서 쓸모 없는 코드가 점차 쌓여간다.
- 주석으로 처리할 필요 없다. 그냥 삭제해라.

### 전역 정보

- 시스템의 전반적인 정보를 기술하지 마라.
- 포트 기본값 설정하는 코드가 변해도 아래 주석이 변하리라는 보장은 없다.

```java
/**
 * 적합성 테스트가 동작하는 포트: 기본값은 <b>8082</b>.
 *
 * @param fitnessePort
 */
public void setFitnessePort(int fitnessePort) {
    this.fitnewssePort = fitnessePort;
}
```