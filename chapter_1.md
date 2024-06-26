# 1장 정규 표현식 소개

‘정규 표현식(Regular Expression)’은 원하는 정보가 어디에 있는지 찾거나(검색), 정보를 찾은 뒤에 편집(치환)하는 것이다. 사실 아주 단순하게 이야기하자면, 정규 표현식을 사용하는 이유는 검색과 치환이 전부이다.
정규 표현식은 텍스트를 찾고 조작하는데 쓰는 문자열이다. 정규 표현식은 정규 표현 언어를 사용해 만든다.

여기서는 사용할 파일을 확인하기 위해 `bat`을 사용하고, 정규표현식을 연습하기 위해 `ripgrep`, `grep`을 사용합니다. `bat`과 `ripgrep`은 아래 링크를 참고하세요.

- `bat`: <https://github.com/sharkdp/bat>
- `ripgrep`: <https://github.com/BurntSushi/ripgrep>

참고로 `ripgrep`은 줄 단위로 정규표현식을 적용하지만, `grep`은 기본적으로 전체 단위로 처리합니다.
