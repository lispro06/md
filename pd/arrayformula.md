# Arryaformual 함수 규칙 및 사용 불가 함수나 수식을 사용 가능한 형태로 변경하는 노하우

1. 규칙 1
 - 전체 범위(배열 포함)에서 연산을 수행하는 함수는 사용할 수 없다.
 
 sum(sumproduct), min, max 등은 배열을 포함해 행 전체, 열 전체를 연산하는 함수이므로 열벡터를 연산하는 arrayformula 로는 표현할 수 없습니다.
 
 sum 의 경우는 각 열의 배열을 더하기로 표현하여 사용합니다.
 
 SUM(A:C) -> ARRAYFORMULA(A1:A+B1:B+C1:C)
 
2. 규칙 2
 - IF함수로 행단위 비교가 가능하며 해당 결과에 따라 행 단위 연산이 가능하다.
 
 비교 연산을 각 행마다 수식을 입력할 필요 없이 가장 상위열에만 입력하면 두 수중 큰 값을 출력하는 배열을 출력할 수 있다.
 
 IF(A1>B1,A1,B1:D1) -> IF(A1:A>B1:B,A1:A,B1:D)
 
 
3. 규칙 3
 - QUERY를 이용할 경우 조건에 맞는 행을 1개만 반환하는 경우만 고려되어 중복 데이터 처리가 안 되므로 1개만 출력하는 조건은 VLOOKUP으로 처리한다.
 
 1행의 연산은 기본적으로 1개 데이터만 출력하도록 기대된다. 2개 이상이 나오면 아래 데이터가 덮어 씌어지거나 순서가 밀어진다.
 
 QUERY 결과가 다중인 상태로 데이터를 배열을 만들어 VLOOUP으로 원하는 데이터의 순서 번째를 출력하게 한다.
 
* 특정 데이터 배열에서 가, 나, 다의 에 해당하는 데이터 중 B 컬럼의 데이터를 각 행에 출력할 때 사용

 A | B | C | D
 ------------ | ------------- | ------------ | -------------
 가 | 1 | 2 | 3
 나 | 4 | 5 | 6
 다 | 7 | 8 | 9
 
 QUERY("영역","SELECT B WHERE A='가') -> ARRAYFORMULA(VLOOKUP(A1:A,QUERY("영역","SELECT A, B, C, D"),1,0))
 
 
4. 규칙 4
 - 구글 스크립트를 이용해 사용자 정의 함수를 사용할 경우 정의 수식을 이용한다. 
 - 연속되는 행에서 사용자 정의 함수를 과도하게 호출하게 되는 상황에 대처할 수 있다.

* 원본 함수
```javascript
function eval(s) {
return process;
}
```

* 호출 함수
```javascript
function doEval( formula ) {
if (typeof formula.map === "function") {
return formula.map(doEval);
}else if(formula){
return eval(formula);
}
}
```
