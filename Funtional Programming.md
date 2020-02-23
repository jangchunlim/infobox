### 함수형 프로그래밍이 필요한 이유
기존과 다르게 사고함으로써 더욱 유연한 문제해결이 가능해진다.

### 명령형 프로그래밍 VS 선언형(함수형) 프로그래밍
* 명령형 프로그래밍: 프로그래밍의 상태와 상태를 변경시키는 구문의 관점에서 연산을 설명하는 방식  
  - 절차지향 프로그래밍: 수행되어야 할 연속적인 계산 과정을 포함하는 방식 (C, C++)  
  - 객체지향 프로그래밍: 객체들의 집합으로 프로그램의 상호작용을 표현 (C++, Java, C#)  
* 선언형 프로그래밍: 어떤 방법으로 해야 하는지(How)를 나타내기보다 무엇(What)과 같은지를 설명하는 방식  
  - 함수형 프로그래밍: 순수 함수를 조합하고 소프트웨어를 만드는 방식 (클로저, 하스켈, 리스프)  

### 

### 명령형과 선언형 자바스크립트 코드 비교

<pre><code>
// 명령형
function double (arr) {
  let results = []
  for (let i = 0; i < arr.length; i++){
    results.push(arr[i] * 2)
  }
  return results
}

function add (arr) {
  let result = 0
  for (let i = 0; i < arr.length; i++){
    result += arr[i]
  }
  return result
}


// 선언형
function double (arr) {
  return arr.map((item) => item * 2)
}

function add (arr) {
  return arr.reduce((prev, current) => prev + current, 0)
}

</pre></code>
