---
title: '주니어 개발자의 오픈소스 기여하기 - (1)'
date: 2022-04-21 14:14:14
category: 'development'
draft: false
---

## 1. 오픈소스에 기여하는 방법

오픈소스에 기여하는 방법에도 여러 방법이 있다. 대표적인 방법 세 가지만 적어보자면 ..!

- 문서화(Documentation)에 기여하기
  - 영어 문서를 우리말로 번역하여 기여하는 것도 좋은 방법이다.
- 이슈 등록하기
  - 버그를 발견하여 이슈에 등록할 수 있다. 문서에 없는 내용을 질문하는 내용의 이슈를 올릴 수도 있다.
- 코드에 기여하기
  - 코드를 직접 작성하여 Pull Request을 이용해 코드에 기여할 수 있다.

## 2. 처음으로 오픈소스에 기여해 본 경험

때는 2018년 11월, 신입사원으로 입사한지 5개월차였던 때였다.
당시 진행중이던 프로젝트에서 [Roslyn Pad](https://github.com/roslynpad/roslynpad)를 사용하고 있었다.

> Roslyn Pad는 'Roslyn 및 Avalon Edit 기반 크로스 플랫폼 C# 편집기'이다.

### 이슈 등록하기

진행 중에 컨트롤을 Custom하는 기능이 동작하지 않아서 문의 이슈를 남겼던 적 있다. 
해당 내용이 궁금하다면 [링크](https://github.com/roslynpad/roslynpad/issues/221)

### 코드 기여하기

RoslynPad.exe를 **커맨드 상으로 실행할 때 특정 .csx 파일을 지정하여 실행하는 기능**이 필요했다. 
이것 또한 먼저 문의 이슈를 남겼다.

![](.\images\roslynpad_issue1.png)

내가 원하던 기능은 구현되어 있지 않다는 답변을 받게 되었고 그렇게 RoslynPad 코드에 기여하게 되었다.

코드에 기여하려면 아래와 같은 순서로 진행하면 된다.

> 1. RoslynPad Github 레포지터리 fork
> 2. fork한 내 레포지터리에서 git Clone
> 3. 수정하고 싶은 코드 수정 후 git add > git commit > git push
> 4. Pull Request 버튼 클릭
> 5. Pull Request 내용 작성

RoslynPad 레포지터리를 fork 한 뒤에 아래와 같이 Pull Request 내용을 작성하여 이슈를 등록하였다.

![](.\images\roslynpad_pr1.png)


내가 등록한 PR에 대해 두 명의 선생님들이 리뷰를 달아주셨다. `aelij`가 RoslynPad의 maintainer이다..!

![](.\images\roslynpad_pr2.png)


리뷰를 토대로 코드를 좀더 수정하여 무사히 Master에 Merge까지 성공하였다.

![](.\images\roslynpad_pr3.png)

Master에 Merge가 완료되고 수정해줘서 고맙다는 인사를 받을 수 있었고, 뿌듯함도 얻을 수 있었다. ✌🏻
회사에서 소통하는 개발자 외에 다른 커뮤니티의 개발자들과 소통하고 개발할 수 있는 것이 오픈 소스만의 매력이다.

해당 Pull Request 내용 및 코드가 궁금하다면 [링크](https://github.com/roslynpad/roslynpad/pull/223) 

다음 글은 버그에 대한 오픈소스 이슈 작성에 대해 써보려 한다 !!