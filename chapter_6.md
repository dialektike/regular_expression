# 6장 위치 찾기

이번 장에서는 텍스트 영역 내에 있는 특정 위치에 있는 텍스트를 찾아내야 할 때가 있습니다. 이때 그 텍스트의 위치를 찾는 것이 필요합니다.

## 경계 지정하기

아래 글에서 `cat`을 선택하고 싶습니다.

```text
The cat scattered his food all over the room.
```

이것은 `ex/6_1.txt`에 들어 있습니다. 다음과 같이 확인하면 됩니다.

```bash
bat ex/6_1.txt
```

아주 단순하게 `cat`을 찾으는 패턴을 이용해 봅시다.

```bash
rg 'cat' ex/6_1.txt
```

윗 코드 실행 결과는 예상한 것과는 다르게 'cat' 뿐만 아니라 'scattered' 안에 있는 'cat'도 일치한다고 알려 줍니다. 이건은 당연한 결과입니다. 왜냐하면 `cat` 패턴은 'cat' 이 있는 모은 부분에 일치하는 것이기 때문입니다.

이럴 때는 경계(boundaries)를 사용하거나 패턴 앞이나 뒤에 특정한 위치 혹은 경계를 나타내는 메타 문자를 사용해야 합니다.

## 단어 경계를 지정하기

`\b`을 사용하여 단어 경계를 지정할 수 있습니다. `\b`을 단어 앞에 사용하면 해당 단어로 시작하는 곳을 찾고 `\b`에 사용하면 해당 단어로 끝나는 것을 찾습니다. 다음 코드를 봅시다.

```bash
rg '\bcat\b' ex/6_1.txt
```

윗 코드는 `\b`을 `cat`에 앞뒤에 사용했습니다. 따라서 정확하게 `cat`만 일치한다고 알려 줍니다. 이 코드는 이 단어에 앞뒤에 빈칸이 있는 것을 찾게 됩니다. 왜냐하면 여기서는 빈칸이 단어와 단어를 구분하는 것이기 때문입니다. 그러나 `scattered`은 `cat`의 앞 글자는 `s`이고 뒷 글자는 `t`이기 때문에 일치하지 않게 됩니다.

앞에서처럼 정확하게 완전한 단어 한 개를 일치해 찾으려고 한다면, 해당 단어 앞 뒤에 모두 `\b`을 붙여야 합니다. 다음 예제를 봅시다.

```txt
The captain wore this cap and cape proudly as he sat listening to the recap of how his crew saved the men from a capsized vessel.
```

이것은 `ex/6_2.txt`에 들어 있습니다. 다음과 같이 확인하면 됩니다.

```bash
bat ex/6_2.txt
```

여기에서 다음과 같은 코드를 실행해 봅시다.

```bash
rg '\bcap' ex/6_2.txt
```

이 코드는 `cap` 앞에만 `\b`을 붙였기 때문에 `cap`로 시작하는 단어에 다 일치하게 됩니다. 그래서 총 4개의 단어와 일치하게 됩니다.

이번에는 뒤에 붙여봅시다.

```bash
rg 'cap\b' ex/6_2.txt
```

이때는 2개만 일치하게 됩니다.

참고로 `\b`는 실제로 문자와 일치라는 것이라 아니라 위치를 가리키기 때문에 앞에서 `\bcat\b`으로 찾은 문자열의 길이는 `5`가 아니라 `3`입니다.

만약 단어 경계와 일치와 일치시키지 않고 싶을 때는 `\B`를 사용합니다. 다음 문자열을 살펴봅시다.

```txt
Please enter the nine digit-id as it appears on your color - coded pass-key.
```

이것은 `ex/6_3.txt`에 들어 있습니다. 다음과 같이 확인하면 됩니다.

```bash
bat ex/6_3.txt
```

`\B`을 이용해 봅시다.

```bash
rg '\B-\B' ex/6_3.txt 
```

이 코드는 `-` 앞 뒤에

```bash
rg '\b-\b' ex/6_3.txt
```

`\b`을 조금 더 살펴 보자면 다음과 같습니다.

> Matches the empty string, but only at the beginning or end of a word. A word is defined as a sequence of word characters. Note that formally, \b is defined as the boundary between a \w and a \W character (or vice versa), or between \w and the beginning or end of the string. This means that r'\bat\b' matches 'at', 'at.', '(at)', and 'as at ay' but not 'attempt' or 'atlas'

윗 글에서 나온 사례를 정리해서 `\b`를 연습해 봅시다.

```txt
at at. (at), and as at ay attempt atlas
```

이것은 `ex/6_4.txt`에 들어 있습니다. 다음과 같이 확인하면 됩니다.

```bash
bat ex/6_4.txt
```

다음을 실행해 봅시다.

```bash
rg 'at\b' ex/6_4.txt
```

윗 코드를 실행하면 앞 글에서 언급된 것처럼 `at` `at.` `(at)`에 있는 `at`에만 일치하게 됩니다. 그러나 뒷 부분에 있는 `attempt`과 a`tlas`에 있는 `at`에는 일치하지 않습니다.

이번에는 `\B`을 을 조금 더 살펴 봅시다.

> Matches the empty string, but only when it is not at the beginning or end of a word. This means that r'at\B' matches 'athens', 'atom', 'attorney', but not 'at', 'at.', or 'at!'.

앞에서 한 것처럼 윗 글에서 나온 사례를 정리해서 `\b`를 연습해 봅시다.

```txt
athens atom attorney at at. at!
```

이것은 `ex/6_5.txt`에 들어 있습니다. 다음과 같이 확인하면 됩니다.

```bash
bat ex/6_5.txt
```

다음을 실행해 봅시다.

```bash
rg 'at\B' ex/6_5.txt
```

이번에는 `athens`, `atom`, `attorney` 안에있는 `at`에만 일치하게 됩니다. 즉 `at`으로 시작해서 `\w`과 일치하는 것이 나오는 것을 찾습니다.

## 문자열 경계 정의하기

문자열의 경계는 전체 문자열의 시작이나 마지막 부분과 패턴을 일치시키고자 할 때 사용한다. 문자열 경계는 메타 문자 가운데 케럿(`^`)으로 문자열의 시작을 달러 기호(`$`)로 문자열의 마지막을 나타낸다.

참고: 케럿(`^`) 문자는 대괄호(`[,]`)로 둘러싸인 집합 안에서는 여는 대괄호(`[`) 문자 바로 다음에 쓰면 부정을 뜻한다. 그러나 이 집합 밖에서는 패턴 시작에서 케럿(`^`) 문자를 사용하면 이는 문자열 시작을 뜻한다.

이제 아래 예제를 살펴 봅시다.

```xml
<?xml version="1.0" encodingi"UTF-8" ?>
‹wsdl:definitions targetNamespace="http://tips.cf"
xmlns: impl="http://tips.cf" xmlns: intf="http://tips.cf"
xm1ns: apachesoap="http://xml.apache.org/xml-soap"
```

이것은 `ex/6_6.txt`에 들어 있습니다. 다음과 같이 확인하면 됩니다.

```bash
bat ex/6_6.txt
```

이것이 `XML` 형식의 문서인지 아닌지를 검사하는 패턴을 만들어 봅시다. 올바른 `XML` 문서는 `<？xml`으로 시작하고 부가 속성을 포함한 다음, `?>`으로 끝납니다.

다음을 실행해 봅시다.

```bash
rg '<\?xml.*\?>' ex/6_6.txt
```

이것은 `<\?xml`은 `<\xml`과 일치하고. `\?>`은 `?>`과 일치하고 `.*`은 앞 둘 사이에 모든 글자, 즉 `.`이 여러개, 즉 `*`이 있는 패턴을 찾습니다.

실행 결과는 다음과 같습니다.

```bash
rg '<\?xml.*\?>' ex/6_6.txt

1:<?xml version="1.0" encodingi"UTF-8" ?>
```

적절하게 필요한 것을 찾는 것 같습니다. 그러나 다음과 같이 찾고자 하는 것이 2번째 줄에 있는 경우에는 문제가 발생할 수 있습니다.

```xml
This is bad, real bad!
<?xml version="1.0" encodingi"UTF-8" ?>
‹wsdl:definitions targetNamespace="http://tips.cf"
xmlns: impl="http://tips.cf" xmlns: intf="http://tips.cf"
xm1ns: apachesoap="http://xml.apache.org/xml-soap"
```

이것은 `ex/6_7.txt`에 들어 있습니다. 다음과 같이 확인하면 됩니다.

```bash
bat ex/6_7.txt
```

이 경우에도 위와 동일한 문자열을 찾아냅니다.

```bash
rg '<\?xml.*\?>' ex/6_7.txt

2:<?xml version="1.0" encodingi"UTF-8" ?>
```

그러나 문제는 이것이 2번째 줄에 있는 것을 찾아낸 것입니다. 우리는 지금 찾은 문자열이 첫 번째 줄에 들어 있는지를 알고 싶습니다. 이럴 때 필요한 것이 케럿(`^`) 문자입니다.

```bash
rg '^\s*<\?xml.*\?>' ex/6_6.txt

1:<?xml version="1.0" encodingi"UTF-8" ?>
```

조심할 점은 지금 여기서 사용하고 있는 `rg`, 즉 `ripgrep`은 입력된 텍스트의 줄마다 검사를 합니다. 아래 내용 참고

> ripgrep is a line-oriented search tool that recursively searches the current directory for a regex pattern.

그러므로 정확하게 찾으려면 `python`의 `re`을 사용해야 합니다. 앞의 패턴을 사용하기 위해서는 다음과 같이 하면 됩니다.

```python
import re
pattern = re.compile("^\s*<\?xml.*\?>")
pattern.search(temp)
```

그러면 이것을 "ex/6_6.txt"과 "ex/6_7.txt"에 적용해봅시다.

```python
import re

with open("ex/6_6.txt") as my_file:
    temp = my_file.read()
    pattern = re.compile("^\s*<\?xml.*\?>")
    pattern.search(temp)  
```

다음은 실행 결과 입니다.

```python
>>> import re
>>> 
>>> with open("ex/6_6.txt") as my_file:
...     temp = my_file.read()
...     pattern = re.compile("^\s*<\?xml.*\?>")
...     pattern.search(temp)  
... 
<re.Match object; span=(0, 39), match='<?xml version="1.0" encodingi"UTF-8" ?>'>
```

"ex/6_6.txt"에는 맨 앞 줄에 있는 것을 확인할 수 있습니다.

```python
import re

with open("ex/6_7.txt") as my_file:
    temp = my_file.read()
    pattern = re.compile("^\s*<\?xml.*\?>")
    pattern.search(temp)  
```

다음은 실행 결과 입니다.

```python
>>> import re
>>> 
>>> with open("ex/6_7.txt") as my_file:
...     temp = my_file.read()
...     pattern = re.compile("^\s*<\?xml.*\?>")
...     pattern.search(temp)  
... 
>>> 
```

"ex/6_7.txt"에서는 아무런 결과가 나오지 않고 있습니다. 즉 맨 앞 줄에 우리가 찾고자 하는 것이 없다는 것을 확인할 수 있습니다.

## 다중행 모드를 사용하기

대개 케럿(`^`)은 문자열의 시작과 일치하고, 달러 기호(`$`)로 문자열의 마지막과 일치합니다. 예외적으로 이 두 메타 문자의 동작을 바꾸는 방법이 있습니다. `(?m)`을 사용하면 다중행(multiline)을 지원하게 됩니다. 다중행 모드로 변경하면 강제로 정규 표현식 엔진이 줄바꿈 문자를 문자열 구분자로 인식합니다. 케럿(`^`)은 문자열의 시작 또는 줄바꿈 다음(새로운 행)에 나오는 문자열 시작과 일치하고, 달러 기호(`$`)로 문자열의 마지막과 줄바꿈 다음에 나오는 문자열의 마지막과 일치하게 됩니다. 앞에서 우리가 사용했던 `rg`, 즉 `ripgrep`과 같이 작동하게 됩니다. 만약 `python`에서 이를 적용해 봅시다.

```python
import re

with open("ex/6_7.txt") as my_file:
    temp = my_file.read()
    pattern = re.compile("(?m)^\s*<\?xml.*\?>")
    pattern.search(temp)  
```

실행 결과는 다음과 같습니다.

```python
>>> import re
>>> 
>>> with open("ex/6_7.txt") as my_file:
...     temp = my_file.read()
...     pattern = re.compile("(?m)^\s*<\?xml.*\?>")
...     pattern.search(temp)  
... 
<re.Match object; span=(23, 62), match='<?xml version="1.0" encodingi"UTF-8" ?>'>
```

앞에서 `(?m)`을 사용하지 않은 것과 확실하게 다르게 동작합니다. 여기서는 2번째 줄에 있다는 것을 찾고 있습니다.

**Note**: 어떤 정규 표현식은 케럿 기호(`^`)은 `\A`로, 달러 기호(`$`)은 `\Z`로 바꿔 사용할 수 있습니다. 이렇게 바꿔 사용할 경우에는 `(?m)`이 작동하지 않습니다.

## 정리해 보자

- `\b`: 단어의 경계를 지정, `\B`은 반대
- `^`: 문자열 시작
- `$`: 문자열 끝
