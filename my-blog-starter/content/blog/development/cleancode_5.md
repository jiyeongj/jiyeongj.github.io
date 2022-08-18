---
title: '[Clean Code] 5. 형식 맞추기'
date: 2022-08-18 9:59:14
category: 'development'
draft: false
---


![](.\images\cleancode.jpg)

- 클린 코드 책을 읽으며, 내용을 요약 정리한 글입니다.

# 5. 형식 맞추기

- 프로그래머라면 형식을 깔끔하게 맞춰 코드를 짜야 한다. 코드 형식을 맞추기 위한 간단한 규칙을 정하고 그 규칙을 확실히 따라야 한다.

## 형식을 맞추는 목적

- 코드 형식은 중요하다 !
- 오랜 시간이 지나 원래 코드의 흔적을 더 이상 찾아보기 어려울 정도로 코드가 바뀌어도 맨 처음 잡아놓은 구현 스타일과 가독성 수준은 유지보수 용이성과 확장성에 계속 영향을 미친다.
- 원래 코드는 사라질지라도 개발자의 스타일과 규율은 사라지지 않는다.

## 적절한 행 길이를 유지하라

- 500줄을 넘지 않고도 대부분 200줄 정도인 파일로도 커다란 시스템을 구축할 수 있다.
- 일반적으로 큰 파일보다 작은 파일이 이해하기 쉽다.

### 신문 기사처럼 작성하라

- 소스 파일도 신문기사와 비슷하게 이름은 간단하면서도 설명이 가능하게 짓는다.
- 소스 파일 첫 부분은 고차원 개념과 알고리즘을 설명한다. 아래로 내려갈수록 의도를 세세하게 묘사한다. 마지막에는 가장 저차원 함수와 세부 내역이 나온다.

### 개념은 빈 행으로 분리하라

- 빈 행은 새로운 개념을 시작한다는 시각적 단서다.

```java
// 빈 행을 넣지 않을 경우
package fitnesse.wikitext.widgets;
import java.util.regex.*;
public class BoldWidget extends ParentWidget {
	public static final String REGEXP = "'''.+?'''";
	private static final Pattern pattern = Pattern.compile("'''(.+?)'''",
		Pattern.MULTILINE + Pattern.DOTALL);
	public BoldWidget(ParentWidget parent, String text) throws Exception {
		super(parent);
		Matcher match = pattern.matcher(text); match.find(); 
		addChildWidgets(match.group(1));}
	public String render() throws Exception { 
		StringBuffer html = new StringBuffer("<b>"); 		
		html.append(childHtml()).append("</b>"); 
		return html.toString();
	} 
}
```

```java
// 빈 행을 넣을 경우
package fitnesse.wikitext.widgets;

import java.util.regex.*;

public class BoldWidget extends ParentWidget {
	public static final String REGEXP = "'''.+?'''";
	private static final Pattern pattern = Pattern.compile("'''(.+?)'''", 
		Pattern.MULTILINE + Pattern.DOTALL
	);
	
	public BoldWidget(ParentWidget parent, String text) throws Exception { 
		super(parent);
		Matcher match = pattern.matcher(text);
		match.find();
		addChildWidgets(match.group(1)); 
	}
	
	public String render() throws Exception { 
		StringBuffer html = new StringBuffer("<b>"); 
		html.append(childHtml()).append("</b>"); 
		return html.toString();
	} 
}
```

### 세로 밀집도

- 줄바꿈이 개념을 분리한다면 세로 밀집도는 연관성을 의미한다.
- 서로 밀집한 코드 행은 세로로 가까이 놓여야 한다.

### 수직 거리

- 함수나 변수가 정의된 코드를 찾으려 상속 관계를 줄줄이 거슬러 올라간 경험이 있을것이다...😂
- 서로 밀접한 개념은 세로로 가까이 둬야 한다.
- 연관성이 깊은 두 개념이 멀리 떨어져 있으면 코드를 읽는 사람이 소스 파일과 클래스를 여기저기 뒤지게 된다.
- **변수 선언**
    - 변수는 사용하는 위치에 최대한 가까이 선언한다.
- **인스턴스 변수**
    - 인스턴스 변수는 클래스 맨 처음에 선언한다.
    - 변수 선언을 어디서 찾을지 모두가 알고 있어야 한다.
- **종속 함수**
    - 한 함수가 다른 함수를 호출한다면 두 함수는 세로로 가까이 배치한다.
    - 가능하다면 호출하는 함수를 호출되는 함수보다 먼저 배치한다.
- **개념적 유사성**
    - 어떤 코드는 개념적인 친화도가 높기 때문에 서로 끌어당긴다.
    - 친화도가 높을수록 코드를 가까이 배치한다.
        ```java
        public class Assert {
            static public void assertTrue(String message, boolean condition) {
                if (!condition) 
                    fail(message);
            }

            static public void assertTrue(boolean condition) { 
                assertTrue(null, condition);
            }

            static public void assertFalse(String message, boolean condition) { 
                assertTrue(message, !condition);
            }
            
            static public void assertFalse(boolean condition) { 
                assertFalse(null, condition);
            } 
        ...
        ```

### 세로 순서

- 일반적으로 함수 호출 종속성은 아래 방향으로 유지한다. 호출되는 함수를 호출하는 함수보다 나중에 배치한다.
- 그러면 소스 코드 모듈이 고차원에서 저차원으로 자연스럽게 내려간다.

## 가로 형식 맞추기

- 짧은 행이 바람직하다. (120자 정도 행 길이를 제안한다.)

### 가로 공백과 밀집도

- 가로로는 공백을 사용해 밀접한 개념과 느슨한 개념을 표현한다.
- 할당 연산자를 강조하기 위해 앞뒤에 공백을 준다.
- 함수 이름과 이어지는 괄호 사이에는 공백을 넣지 않는다.

```java
private void measureLine(String line) { 
	lineCount++;
	
	// 할당 연산자 좌우로 공백
	int lineSize = line.length();
	totalChars += lineSize; 
	
	// 함수이름과 괄호 사이에는 공백을 없앰으로써 함수와 인수의 밀접함을 보여준다
	lineWidthHistogram.addLine(lineSize, lineCount);
	recordWidestLine(lineSize);
}
```

### 가로 정렬

- 선언문과 할당문을 별도로 정렬하지 않는다.

```java
public class FitNesseExpediter implements ResponseSender {
	private		Socket		      socket;
	private 	InputStream 	  input;
	private 	OutputStream 	  output;
	private 	Reques		      request; 		
	private 	Response 	      response;	
	... 
```

### 들여쓰기

- 범위(Scope)로 이뤄진 계층을 표현하기 위해 코드를 들여쓴다.
- 들여쓰기한 파일은 구조가 한눈에 들어온다. 변수, 생성자 함수, 접근자 함수, 메서드가 금방 보인다.
- **들여쓰기 무시하기**
    - 간단한 if 문, 짧은 while 문, 짧은 함수에서 들여쓰기 규칙을 무시하고픈 유혹이 생기더라도 들여쓰기로 범위를 제대로 표현한 코드가 좋다.

```java
public class CommentWidget extends TextWidget {
	public static final String REGEXP = "^#[^\r\n]*(?:(?:\r\n)|\n|\r)?";
	
	public CommentWidget(ParentWidget parent, String text){
		super(parent, text);
	}
	
	public String render() throws Exception {
		return ""; 
	} 
}
```

### 가짜 범위

- 빈 while 문이나 for 문을 피해야 한다. 피하지 못할 때는 빈 블록을 올바로 들여쓰고 괄호로 감싼다.

## 팀 규칙

- 팀에 속한다면 자신이 선호해야 할 규칙은 바로 팀 규칙이다.
- 좋은 소프트웨어 시스템은 읽기 쉬운 문서로 이뤄진다.
- 스타일은 일관적이고 매끄러워야 한다.



