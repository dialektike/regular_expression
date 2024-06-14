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
