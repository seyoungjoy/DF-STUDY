# 08장 제어문
## 8.1 블록문
- 코드블록 or 블록이라고 부름
- 0개 이상의 문을 `{}`중괄호로 묶은 것
- 블록문은 자체 종결성을 갖기 때문에 세미콜론을 붙이지 않아도 된다.

## 8.2 조건문
- boolean 값으로 평가될 수 있는 표현식

### 8.2.1 if...else 문
- 조건식의 평가 결과에 따라 실행할 코드 블록을 결정.

### 8.2.2 switch 문
- 다양한 조건식을 평가해야할 때 사용.

## 8.3 반복문
### 8.3.1 for 문
- 조건문이 거짓으로 평가될때까지 코드 블록을 반복 실행.

### 8.3.2 while 문
- 조건식이 참이면 코드 블록을 계속해서 반복 실행.
- for 문은 반복 횟수가 명확할때 주로 사용, while 문은 반복횟수가 불명확할 때 사용.

### 8.3.3 do... while 문
- 코드블록을 먼저 실행하고 조건식을 평가.

## 8.4 break 문
- 코드 블록 탈출

## 8.5 continue 문
- 코드 블록의 실행을 현 지점에서 중단하고 반복문의 증감식으로 이동.
