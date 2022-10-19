# 38장 브라우저의 렌더링 과정
- 파싱(구문 분석) : 문자열을 토큰으로 만들고 이를 파스 트리로 생성하는 일련의 과정
- 렌더링 : html, css, js 문서를 파싱하여 브라우저에 시각적으로 출력하는 것.
## HTML 파싱와 DOM 생성
1. 바이트(10110101111) 
   - meta charset="UTF-8" 인코딩 방식으로 문자열로 변환
2. 문자(<html><head><meta charset="UTF-8">...</html>)
3. 토큰
```
{
    startTag : 'html',
    contents:{...},
    endTag:'html'
}
```
4. 노드( html head meta link body ...)
5. DOM : HTML 문서를 파싱한 결과물
    - 노드들의 부자관계를 반영한 트리 자료구조를 생성.

## CSS 파싱과 CSSOM 생성
- CSS를 파싱하여 CSSOM을 생성

## 렌더 트리 생성
- HTML과 CSS 를 파싱하여 DOM과 CSSOM을 생성
- 그리고 렌더 트리로 결합(이때 렌더링 되지 않는 display:none되는 노드들을 포함안됨)

## script 태그의 async/defer 어트리뷰트
- 자바스크립트 파싱에 의해 DOM 생성이 중단되는 문제를 해결하기 위해 추가됨
- async 어트리뷰트 : javascript 로드는 비동기, 실행은 동기
- defer 어트리뷰트 : js 로드 비동기, 실행은 dom 생성후 즉시 실행됨.