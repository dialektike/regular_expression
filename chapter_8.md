# 8장 역참조 사용하기

이번 장에서는 하위 표현식(Subexpression)'에서 중요한 사용법 중 하나인 역참조(backreferences)를 알아보겠습니다.

## 역참조 이해하기

역참조를 언제 사용하는지 예제를 살펴보자.5장에서는 단어 하나 하나가 여러 번 일치하는 경우를 살펴봤습니다. `HTML` 개발자가 헤더 텍스트를 정의하고자 한다고 가정해 봅시다.`HTML`에는 헤더 태그가 `<H1>`, `<H2>`, `<H3>`, `<H4>`, `<H5>`, `<H6>`, 총 6개가 있습니다. 다음 예제에서 1부터 6까지의 단계에 상관없이 헤더를 찾아야 합니다.

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

이때 탐욕스러운 수량자인 `.*`을 쓰는 것보다 `.*?`과 같은 게으른 수량자를 쓰는 편이 좋을 수 있습니다.

```re
grep '<[Hh][1-6]>.*?<\/[Hh][1-6]>' ex/8_1.html
```

그러나 문제는 다음과 같은 예제에서는 위의 패턴이 적절하게 작동하지 않습니다.

```html
<BODY>
    <H1>Welcome to my Homepage</H1>
    Content is divided into two sections: <BR>
    <H2>ColdFusion</H2>
    Information about Macromedia ColdFusion.
    <H2>Wireless</H2>
    Information about Bluetooth, 802.11, and more.
    <H2>This is not valid HTML</H3>
</BODY>
```

이것은 `ex/8_2.html`에 들어 있습니다. 다음과 같이 확인하면 됩니다.

```bash
bat ex/8_2.html
```

윗 예제의 마지막 줄인 `<H2>This is not valid HTML</H3>`이 앞에서 사용했던 패턴으로도는 걸러낼 수 없습니다. 문제는 윗 패턴으로는 시작 태그와 종료 태그가 다른 것을 거를 수 없습니다. 이때 역참조가 유용해 집니다.

## 역참조로 찾기

앞의 태그 문제는 나중에 다시 해결해 보겠습니다. 우선 역참고를 잘 이해하기 위해서 다음 예제를 살펴봅시다.

```txt
This is a block of of text, several words here are are repeated, and and they should not be.
```

이것은 `ex/8_3.txt`에 들어 있습니다. 다음과 같이 확인하면 됩니다.

```bash
bat ex/8_3.txt
```

이 파일에서 반복해서 나오는 문자, 예를 들어 'of'와 같은 것을 찾으려고 합니다. 이런 단어를 실수로 두 번 입력한 것을 찾으려고 합니다. 두 번 입력한 것을 찾으려고 한다면 우선 먼저 입력한 단어가 무엇인지 반드시 알고 있어야 합니다. 우선 다음 코드를 실행해 봅시다.

참고로 `rg`에서 역참조를 사용하기 위해서는 `--pcre2`을 옵션으로 선택해야 합니다.

```re
rg '[ ]+(\w+)[ ]+\1' ex/8_3.txt --pcre2
```

만약 `grep`을 사용하고자 한다면 `-E`을 옵션으로 선택해야 합니다. 이 옵션은 `Interpret pattern as an extended regular expression`을 하기 위한 옵션입니다. `man grep`을 참고하세요.

```re
grep '[ ]+(\w+)[ ]+\1' ex/8_3.txt -E
```

이 두 코드를 실행하면 정확하게 반복되는 단어 3개를 찾아내는 것을 알 수 있습니다. 여기서 주목할 부분은 `(\w+)`입니다. 이는 나중에 일치한 부분을 사용할 수 있도록 하위 표현식으로 표시해 놓은 것입니다. 패턴 마지막 부분에 있는 `\1`이 앞에서 표시한 하위 표현식을 참조하게 됩니다. 만약 괄호로 감싼 하위 표현식이 없다면 작동하지 않습니다.

여기서 `1`은 패턴에서 처음 사용한 하위 표현식과 일치한다는 뜻입니다. 당연히 `\2`라면 두 번째 사용한 하위 표현식과 일치한다는 뜻입니다. 역참조는 변수와 비슷하게 생각해도 된다고 합니다. 앞의 패턴은 빈칸 사이에 있는 단어와 같은 단어가 반복해서 나오는 패턴을 말합니다.

이제 앞의 해결하지 못한 `ex/8_2.html` 사례에 적용해 봅시다.

```re
rg '<[Hh]([1-6])>.*?<\/[Hh]\1>' ex/8_2.html --pcre2
```

```re
grep '<[Hh]([1-6])>.*?<\/[Hh]\1>' ex/8_2.html -E
```

이렇게 하면 `<H2>This is not valid HTML</H3>`을 선택하지 않게 됩니다.

## 정리해 보자

- 하위 표현식: 괄호`(,)`를 사용해 문자 집합이나 표현식을 묶어서 정의한다. 7장에서처럼 반복해 작업하는데 사용할 수 있고 패턴 안에서 참조할 때 사용할 수 있다. 이런 참조를 '역 참조'라고 한다.
