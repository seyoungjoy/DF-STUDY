<img src="https://capsule-render.vercel.app/api?type=waving&color=gradient&customColorList=1&height=200&section=header&text=Chapter24.%20%ED%81%B4%EB%A1%9C%EC%A0%80&fontSize=50">

# **24.1 렉시컬 스코프**
렉시컬 환경의 상위 스코프에 대한 참조는 **함수 정의가 평가되는 시점**에 함수가 정의된 환경(위치)에 의대 결정된다.

<br>

# **24.2 함수 객체의 내부 슬롯[[Environment]]**
함수는 자신의 내부 슬롯 [[Environment]]에 상위 스코프 참조를 저장한다.