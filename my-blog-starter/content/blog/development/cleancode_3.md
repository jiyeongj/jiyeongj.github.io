---
title: '[Clean Code] 3. 함수'
date: 2022-07-27 16:54:14
category: 'development'
draft: false
---


![](.\images\cleancode.jpg)

- 클린 코드 책을 읽으며, 내용을 요약 정리한 글입니다.

# 3. 함수

> 어떤 프로그램이든 가장 기본적인 단위가 함수다.

### 작게 만들어라!

- 함수를 만드는 첫째 규칙은 '작게!'다. 함수를 만드는 둘째 규칙은 '더 작게!'다.

#### 블록과 들여쓰기

- if 문/else 문/while 문 등에 들어가는 블록은 한 줄이어야 한다.
- 바깥을 감싸는 함수가 작아질 뿐 아니라, 블록 안에서 호출하는 함수 이름을 적절히 짓는다면, 코드를 이해하기도 쉬워진다.
- 함수에서 들여쓰기 수준은 1단이나 2단을 넘어서면 안 된다.


### 한 가지만 해라!

> 함수는 한 가지를 해야 한다. 그 한가지를 잘 해야 한다. 그 한가지만을 해야 한다.

지정된 함수 이름 아래에서 추상화 수준이 하나인 단계만 수행한다면 그 함수는 한 가지 작업만 한다.

### 함수 당 추상화 수준은 하나로!

- 함수가 '한 가지' 작업만 하려면 함수 내 모든 문장의 추상화 수준이 동일해야 한다.
- 한 함수 내에 추상화 수준을 섞으면 코드를 읽는 사람이 헷갈린다.
- **위에서 아래로 코드 읽기 : 내려가기 규칙**
  - 코드는 위에서 아래로 이야기처럼 읽혀야 좋다. 한 함수 다음에는 추상화 수준이 한 단계 낮은 함수가 온다. 즉, 위에서 아래로 프로그램을 읽으면 함수 추상화 수준이 한 번에 한 단계씩 낮아진다.

### Switch 문

- 다형성을 이용하여 사용한다.
   - 추상 팩토리에 Switch 문을 사용한다.

   ```java
   public abstract class Employee {
        public abstract boolean isPayday();
        public abstract Money calculatePay();
        public abstract void deliverPay(Money pay);
    }
    -----------------
    public interface EmployeeFactory {
        public Employee makeEmployee(EmployeeRecord r) throws InvalidEmployeeType; 
    }
    -----------------
    public class EmployeeFactoryImpl implements EmployeeFactory {
        public Employee makeEmployee(EmployeeRecord r) throws InvalidEmployeeType {
            switch (r.type) {
                case COMMISSIONED:
                    return new CommissionedEmployee(r) ;
                case HOURLY:
                    return new HourlyEmployee(r);
                case SALARIED:
                    return new SalariedEmploye(r);
                default:
                    throw new InvalidEmployeeType(r.type);
            } 
        }
    }
   ```

### 서술적인 이름을 사용하라!

- 함수 이름을 정할 때는 여러 단어가 쉽게 읽히는 명명법을 사용한다.
- 서술적인 이름을 사용하면 개발자 머릿속에서도 설계가 뚜렷해지므로 코드를 개선하기 쉬워진다.
- 이름을 붙일 때는 일관성이 있어야 한다. 모듈 내에서 함수 이름은 같은 문구, 명사, 동사를 사용한다.


### 함수 인수

- 함수에서 이상적인 인수 개수는 0개(무항)다. 최선은 입력 인수가 없는 경우이며, 차선은 입력 인수가 1개인 경우이다.

#### 많이 쓰는 단항 형식

- 인수에 질문을 던지는 경우
    - `boolean fileExists("MyFile")`
- 인수를 뭔가로 반환해 결과를 반환하는 경우
    - `InputStream fileOpen("MyFile")`
- 이벤트 함수
    - `passwordAttemptFailedNtimes(int attempts)`

위 세가지 경우가 아니라면 단항 함수는 가급적 피한다.

#### 플래그 인수

- 플래그 인수는 추하다. 함수로 bool 값을 넘기는 것은 함수가 한꺼번에 여러가지를 처리한다고 공표하는 것과 마찬가지다.

#### 이항 함수

- 인수가 2개인 함수는 인수가 1개인 함수보다 이해하기 어렵다.
- 이항 함수가 적절한 경우도 있다.
   - `Point p = new Point(0, 0)`
- 이항함수가 무조건 나쁜건 아니지만, 그만큼 위험히 따른다는 사실을 이해하고 가능하면 단항 함수로 바꾸도록 노력해야 한다.

#### 삼항 함수

- 인수가 3개인 함수는 인수가 2개인 함수보다 훨씬 더 이해하기 어렵다.
- 삼항 함수를 만들 때는 신중히 고려하라.

#### 인수 객체

- 인수가 2-3개 필요하다면 일부를 독자적인 클래스 변수로 선언할 가능성을 짚어본다.
   ```java
   Circle makeCircle(double x, double y, double radius);
   Circle makeCircle(Point center, double radius);
   ```
   - x,y를 인자로 넘기는 것 보다 Point를 넘기는 것이 낫다.

#### 동사와 키워드

- 단항 함수는 함수와 인수가 동사/명사 쌍을 이뤄야 한다.
    - `writeFiled(name)`
- 함수 이름에 키워드(인수 이름)을 추가한다. 인수 순서를 기억할 필요가 없어진다.
    - `assertExpectedEqualsActual(expected, actual)`


### 부수 효과를 일으키지 마라!

```java
public class UserValidator {
	private Cryptographer cryptographer;
	public boolean checkPassword(String userName, String password) { 
		User user = UserGateway.findByName(userName);
		if (user != User.NULL) {
			String codedPhrase = user.getPhraseEncodedByPassword(); 
			String phrase = cryptographer.decrypt(codedPhrase, password); 
			if ("Valid Password".equals(phrase)) {
				Session.initialize();
				return true; 
			}
		}
		return false; 
	}
}
```

- `Session.Initialize()` 호출은 함수명과 맞지 않는 부수효과이다.
- `checkPassword` 함수는 세션을 초기화해도 괜찮은 특정 상황에서만 호출이 가능하다. 

#### 출력 인수

- 객체 지향 언어에서는 출력 인수를 사용할 필요가 거의 없다.
    - 이전 방식
      ```java
      public void appendFooter(StringBuffer report) 
      ```
    - 권장 방식
      ```java
      report.appendFooter()
      ```
- 일반적으로 출력 인수는 피해야 한다.
- 함수에서 상태를 변경해야 한다면 함수가 속한 객체 상태를 변경하는 방식을 택한다.

### 명령과 조회를 분리하라!

- 함수는 객체 상태를 변경하거나, 객체 정보를 반환하거나 둘 중 하나다. 둘 다 하면 혼란을 초래한다.
   - `public boolean set(String attribute, String value)`
   - 값을 설정하고 성공/실패 여부를 반환하고 있다.
- 명령과 조회를 분리하여 혼란을 주지 않도록 해야한다.
    ```java
    if (attributeExists("username"))
    {
        setAttribute("username","unclebob");
        ...
    }
    ```

### 오류 코드보다 예외를 사용하라!

- 오류 코드를 반환하는 방식 보다 예외를 사용하면 오류 처리 코드가 원래 코드에서 분리되므로 코드가 깔끔해진다.

#### Try/Catch 블록 뽑아내기

- try/catch 블록을 별도 함수로 뽑아내는 것이 좋다.
    - 정상 동작과 오류 처리 동작을 분리한다.
    
    ```java
    public void delete(Page page) {
        try {
            deletePageAndAllReferences(page);
        } catch (Exception e) {
            logError(e);
        }
    }

    private void deletePageAndAllReferences(Page page) throws Exception { 
        deletePage(page);
        registry.deleteReference(page.name); 
        configKeys.deleteKey(page.name.makeKey());
    }

    private void logError(Exception e) { 
        logger.log(e.getMessage());
    }
    ```


### 반복하지 마라!

- 중복은 소프트웨어에서 모든 악의 근원이다. 많은 원칙과 기법이 중복을 없애거나 제어할 목적으로 나왔다.

### 구조적 프로그래밍

- 다익스트라의 구조적 프로그래밍 원칙
    - 모든 함수와 함수 내 모든 블록에 입구와 출구가 하나만 존재해야 한다.
    - 함수는 return문이 하나여야 하고, 루프 안에서 break나 continue를 사용해선 안되며 goto는 절대로 안된다.
- 함수가 작다면 위 규칙은 별 이익을 제공하지 못한다.
- 함수를 작게 만든다면 간혹 return, break, continue를 여러 차례 사용해도 괜찮다.

### 함수를 어떻게 짜죠?

- 처음에는 길고 복잡하다. 하지만, 단위 테스트 케이스도 만든다.
- 그 다음 코드를 다듬고, 함수를 만들고, 이름을 바꾸고, 중복을 제거한다. 이 와중에도 코드는 항상 단위 테스트를 통과한다.
- 처음부터 탁 짜지지는 않는다.

### 결론

- 함수를 작성하는 것의 목표는 시스템이라는 이야기를 풀어가는 데 있다.
