<img src="https://capsule-render.vercel.app/api?type=waving&color=gradient&customColorList=1&height=200&section=header&text=Chapter38.%20%EB%B8%8C%EB%9D%BC%EC%9A%B0%EC%A0%80%EC%9D%98%20%EB%A0%8C%EB%8D%94%EB%A7%81%20%EA%B3%BC%EC%A0%95&fontSize=48" width="100%" alt="">

```파싱(parsing)```<br>
프로그래밍 언어의 문법에 맞게 작성된 텍스트 문서를 읽어 들여 실행하기 위해 텍스트 문서의 문자열을 토큰으로 분해(어휘분석)하고, 토큰에 문법적 의미와 구조를 반영하여 트리 구조의 자료구조인 파스트리를 생성하는 일련과정

<br>

```렌더링(rendering)```<br>
HTML,CSS, JS로 작성된 문서를 파싱하여 브라우저에 시각적으로 출력하는 것을 말한다.

<br>

1. 브라우저는 HTML, CSS, js, 이미지, 폰트 파일 등 렌더링에 필요한 리소스를 요청하고 서버로부터 응답 받는다.
2. 브라우저 렌더링 엔진은 서버로부터 응답된 HTML과 CSS를 파싱하여 DOM과 CSSOM을 생성하고 이들을 결합하여 렌더 트리를 생성한다.
3. 브라우저의 js엔진은 서버로부터 응답된 js를 파싱하여 AST를 생성하고, 바이트코드로 변환하여 실행한다. 이때 js는 DOM API를 통해 DOM이나 CSSOM을 변경할 수 있다. 변경된 DOM과 CSSOM은 다시 렌더 트리로 결합된다.
4. 렌더 트리를 기반으로 HTML 요소의 레이아웃(위치와 크기)을 계산하고 브라우저 화면에 HTML 요소를 페인팅한다.

# **38.1 요청과 응답**