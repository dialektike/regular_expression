# 10장 조건 달기

정규표현식에서도 조건 처리를 할수 있습니다.

**주의할 점**: 모든 정규표현식에서 조건 처리를 지원하지는 않는다.

## 왜 조건을 다는가?

아래 예제를 살펴봅시다.

```txt
123-456-7890
(123)456-7890
(123)-456-7890
(123-456-7890
1234567890
123 456 7890
```

이것은 `ex/10_1.txt`에 들어 있습니다. 다음과 같이 확인하면 됩니다.

```bash
bat ex/10_1.txt
```

여기에 들어 있는 각각의 줄은 모두 미국 전화 번호 형식입니다. 여기서 1,2번째 줄만 적절한 번호 형식이라고 생각할 수 있습니다. 이를 토대로 패턴을 생각해 봅시다.

```re
rg '\(?\d{3}\)?-?\d{3}-\d{4}' ex/10_1.txt
```

```re
grep '\(?\d{3}\)?-?\d{3}-\d{4}' ex/10_1.txt -E
```

실행 결과는 다음과 같습니다.

```bash
rg '\(?\d{3}\)?-?\d{3}-\d{4}' ex/10_1.txt

1:123-456-7890
2:(123)456-7890
3:(123)-456-7890
4:(123-456-7890
```

```bash
grep '\(?\d{3}\)?-?\d{3}-\d{4}' ex/10_1.txt -E

123-456-7890
(123)456-7890
(123)-456-7890
(123-456-7890
```

이 코드는 우선 `(`이 없거나 하나인 경우 )

## 조건 사용하기

### 역참조 조건

```html
<!-- Nav bar -->
<TD>
    <IMG SRC="/images/spacer.gif">
    <IMG SRC="/images/spacer.gif">
    <A HREF="/home"><IMG SRC="/images/home.gif"></A>
    <A HREF="/search"><IMG SRC="/images/search.gif"></A>
    <A HREF="/help"><IMG SRC="/images/help.gif"></A>
</TD>
```

```re
rg '(<[Aa]|s+[^>]+>|s*)?<[Ii][Mm][Gg]|s+[^>1+>(?(1)|s*</[Aa]>)' ex/10_2.html --pcre2
```
