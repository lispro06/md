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

5. 규칙 5
 - 영역이 들어간 함수 중에 INDEX와 FILTER는 사용할 수 없으나, MATCH와 VLOOKUP 은 사용 가능하다.
 
정확한 색인 방법에 대한 연구가 부족하여 명확한 이유를 작성하긴 어렵다.

다중 조건 사용을 위해 QUERY 와 MATCH(CONCAT을 이용해 A와 B를 합쳐서 다중 조건 처리 시키는 편법이 있음) 사용이 가능하다.

QUERY는 결과 배열을 출력하지만, MATCH는 결과 행만 반환하므로 MATCH를 이용할 경우 VLOOKUP을 이용해야 한다.

데이터가 있는 영역 A2:D19에서 색인 값 F4:F를 QUERY와 MATCH를 이용해 찾는 방법이다.

MATCH는 숫자를 반환하기 때문에 데이터 영역(A:D)에서 A 컬럼이 숫자여야 한다.

```
=ARRAYFORMULA(IFERROR(VLOOKUP(F4:F,QUERY(A2:D19,"SELECT C, D",0),2,0)))
```

```
=ARRAYFORMULA(IFERROR(VLOOKUP(MATCH(F4:F,C1:C,0),A2:D19,4)))
```


## 결론
1. ARRAYFORMULA 는 압축된 수식을 풀어서 행단위 연산이 가능하게 서술한다.
2. VLOOUP이 엑셀이나 구글 스프레드시트에서나 필수 색인 함수이다.
3. ARRAYFORMULA 는 함수가 아니라 햄수다.(e. g 감성의 갬성 표현과 유사한 언어 유희)
