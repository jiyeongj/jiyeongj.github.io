---
title: '주니어 개발자의 오픈소스 기여하기 - (2)'
date: 2022-04-30 19:19:19
category: 'development'
draft: false
---

## 1. 버그 발견 ..! 

때는 2019년 말.. 진행중이던 프로젝트에서 [Akka.NET](https://github.com/akkadotnet/akka.net)을 사용중이었다. Akka.Cluster로 서비스가 구성되어 있었는데 서비스가 죽어버리는... 현상이 있어서 원인을 파악하기 위한 업무를 진행했었다. 여러가지 상황으로 테스트해보며 여러 버그를 발견했다. 아래는 그 중 하나이다.

1. Akka.Cluster로 SeedNode, NonSeedNode1, NonSeedNode2를 구성한다.
2. NonSeetNode2를 종료하고 실행하는 과정을 반복한다.
3. 10회 이상 반복하면 `StackOverflowException`과 함께 SeedNode가 다운되었다..!

위 내용으로 GitHub에 이슈 등록을 했던 경험을 공유하고자 한다.

## 2. 유사한 이슈가 없는지 확인하기

먼저 올리기 전에 유사한 이슈가 없는지 확인해 보아야 한다. 이미 존재한다면 코멘트 등으로 부가 설명을 남기는 것이 좋다. 

나는 유사한 이슈는 없었기에 새로운 이슈로 등록했다.

## 3. 버그 이슈 등록하기

버그 이슈를 등록할 때는 버그 재현이 쉽도록 **실행 환경, 사용하는 버전, 테스트 예제, 스크린샷** 등을 첨부하면 좋다. 프로젝트마다 요구하는 템플릿이 있는 경우도 있다.
테스트 예제는 **가장 간단한 예제**를 작성하여 제공하는 것이 좋다.

![](.\images\akka_issue1.png)

나는 위와 같이 남겼다.

현재 사용중인 버전, 실행환경, 예제의 GitHub 링크와 상황 시나리오를 설명해주었다. 또한 예제 프로그램의 버그 상황을 재현하기 쉽도록 batch 파일을 만들어서 batch 파일에서 NonSeedNode2.exe를 반복 실행하도록 예제를 구성하여 만들어주었다.

![](.\images\akka_issue2.png)

그리고 `StackOverflowException` 호출 스택 캡쳐 화면도 첨부했다.

![](.\images\akka_issue3.png)

그 후에 위처럼 답변이 달렸다. 디테일하게 버그 리포트를 작성해줘서 고맙다는 말과 함께 test.bat 파일을 돌려보아도 재현 할 수 없었다는 이야기였다!:sweat_smile: log level과 configuration을 바꾸고 다시 올려달라고...

![](.\images\akka_issue4.png)

그래서 추천한 대로 configuration을 수정하고 예제를 업데이트하였다. 그리고 상황을 재현하여 SeedNode가 죽어버리는 그 순간을 gif로 만들어 올려주었다 !


![](.\images\akka_issue5.png)

그랬더니 드디어 버그 재현에 성공했고 문제가 무엇인지 알았다고 답변을 받았다. 얼마 지나 버그 픽스 되었고 이슈도 종료되었다. :thumbsup: [이슈 링크](https://github.com/akkadotnet/akka.net/issues/4083)


## 4. 이슈는 언제나 자세하게


이 이후에도 Akka.NET에 여러 질문 및 버그 리포트를 남겼었다. 그 때마다 이런 경험들이 도움이 되었다. 

버그 리포팅을 하는 경우 이처럼 **최대한 자세하게** 내가 겪고 있는 증상을 기록해야 한다. 기술적으로 **충분히 분석하고 이해**한 다음 정리하여 보고하는 것이 중요하다 !

