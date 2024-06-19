# 6장 역참조 사용하기

이번 장에서는 하위 표현식(Subexpression)'에서 중요한 사용법 중 하나인 역참조(backreferences)를 알아보겠습니다.

## 역참조 이해하기

역참조를 언제 사용하는지 예제를 살펴보자.5장에서는 단어 하나 하나가 여러 번 일치하는 경우를 살펴봤습니다. `HTML` 개발자가 헤더 텍스트를 정의하고자 한다고 가정하자. `HTML`에는 헤더 태그가 `<H1>`, `<H2>`, `<H3>`, `<H4>`, `<H5>`, `<H6>`, 총 6개가 있습니다. 다음 예제에서 1부터 6까지의 단계에 상관없이 헤더를 찾아야 합니다.

```html
<BODY>
<H1>Welcome to my Homepage</H1>
Content is divided into two sections: <BR>
<H2>ColdFusion</H2>
Information about Macromedia ColdFusion.
<H2>Wireless</H2>
Information about Bluetooth, 802.11, and more.
</BODY>
```

이것은 `ex/8_1.html`에 들어 있습니다. 다음과 같이 확인하면 됩니다.

```bash
bat ex/8_1.html
```

여기서 우선 `H1`만 찾으려면 다음과 같이 하면 됩니다.

```re
rg '<[Hh]1>.*<\/[Hh]1>' ex/8_1.html
```

실행 결과는 다음과 같습니다.

```bash
rg '<[Hh]1>.*<\/[Hh]1>' ex/8_1.html

2:    <H1>Welcome to my Homepage</H1>
```

```re
rg '<[Hh][1-6]>.*<\/[Hh][1-6]>' ex/8_1.html
```

## 정리해 보자

- 하위 표현식: 괄호`(,)`를 사용해 일부 표현식을 한데 묶는데 사용한다. 이를 쓰는 목적은 정확하게 반복하는 패턴 영역을 결정하거나 `or` 조건을 적절하게 사용하고자 할 때 사용한다.
