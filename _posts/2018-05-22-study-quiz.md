---
layout: post
title: "Quiz"
categories:
  - Study
tags:
---

<br>

문제 풀이
====

### 1. RestFul 아키텍쳐에 대해서 설명해주세요.
 RestFul 아키텍쳐란 자원을 URI로 표현하고 HTTP 메소드로 자원에 대한 행위를 표현하는 것입니다.  
최근 Node.js로 라우팅할때 사용했던 Rest 구조의 일부 입니다.  
해당 id(자원)에 대한 CRUD(행위)를 표현해 구조를 잡았습니다.  

해당 id에 해당하는 글을 가져올때
> GET /posts/{id}

새 글을 등록할때
> POST /posts

해당 id에 해당하는 글을 수정할때
> PUT /posts/{id}

해당 id에 해당하는 글을 삭제할때
> DELETE /posts/{id}

GET, POST, PUT, DELETE 는 각각 기능을 수행할 HTTP 메소드 이름이고 그 뒤는 자원을 표현하는 URI입니다.  
아래에 해당하는 Route에 걸려 수행하게됩니다.  

```javascript
// Read
router.get("/:id", function(req, res){
  ...
});
// Create
router.post("/", function(req, res){
  ...
});
// Update
router.put("/:id", function(req, res){
  ...
});
// Delete
router.delete("/:id", function(req, res){
  ...
});
```

참조 : <http://meetup.toast.com/posts/92>

### 2. 다익스트라 최단경로 알고리즘에 대해서 설명하고 수도코드를 작성해주세요.

풀지 못하였습니다. 그렇지만 [다익스트라 위키백과](https://namu.wiki/w/%EB%8B%A4%EC%9D%B5%EC%8A%A4%ED%8A%B8%EB%9D%BC%20%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98 "다익스트라 위키백과")에 좋은 해답이 있어서  읽어봤습니다.

### 3. 제3, BCNF 정규화에 대해서 설명해주세요.
 3차 정규화는 제 2 정규형에 기본키를 제외한 모든 속성에 이행적 함수적 종속을 제거하는 것을 뜻합니다.  
이행적 함수적 종속이란 a -> b 이고 b -> c 일떄 a -> c 인것을 뜻합니다.  

예를들어 아래와 같이 기본키가 주문번호와 고객번호인 테이블에서  
주문번호 -> 고객번호 이고 고객번호 -> 주소  
라는 이행적 함수적 종속관계가 성립되기 때문에  

<주문>
주문번호|고객번호|주소
----|----|----
A345|100|서울
D347|200|부산
A210|300|광주

아래와 같이 테이블을 분리함으로서 제 3 정규형을 만들수 있습니다.  

<주문>
주문번호|고객번호
----|----
A345|100
D347|200
A210|300
B230|200

<고객>
고객번호|주소
----|----
100|서울
200|부산
300|광주

----

 BCNF는 제 3 정규형이면서 모든 결정자가 후보키인 정규형입니다.  
결정자란 종속관계 a -> b 에서 종속자 b를 결정하는 a를 뜻합니다.  
이런 모든 결정자들이 기본키가 될 수 있는 후보키가 되면 BCNF를 만족시킵니다.  

예를들어 아래와 같이 학번과 과목명이 기본키인 수강 테이블에서  
담당교수 -> 과목명 
이라는 종속관계가 발생하는데, 이때 결정자인 담당교수는 후보키가 아니기때문에  

<수강>
학번|과목명|담당교수
----|----|----
46|데이터베이스|홍길동
47|네트워크|유관순
48|인공지능|윤봉길
49|데이터베이스|홍길동
47|데이터베이스|이순신
49|네트워크|유관순

아래와 같이 담당교수를 분리해내면 BCNF를 만족합니다.  

<수강>
학번|담당교수
----|----
46|홍길동
47|유관순
48|윤봉길
49|홍길동
47|이순신
49|유관순

<교수>
담당교수|과목명
----|----
홍길동|데이터베이스
이순신|데이터베이스
윤봉길|인공지능
유관순|네트워크

참조 : 정보처리기사 교재

### 4. 정렬되어 있는 임의의 수의 배열이 있다. 해당 배열에 속해있지 않은 특정한 값 n과 가장 가까운 값을 찾기위한 알고리즘을 수도코드로 작성해주세요.  

의사코드가 익숙치 않아 javascript로 작성한점 양해 부탁드립니다.  

```javascript
// 임의의 정렬된 배열
var arr = [1, 23, 221, 424, 2345, 55555];
// 주어진 값
var PROVIDED_NUM = 6666;

function findNum(num, arr){
  // 거리차가 저장될 변수
  var distance = getDist(arr[0], num);
  // 결과값으로 출력될 가까운 수가 저장될 변수
  var nearNum = 0;
  for (var i = 1; i < arr.length; i++){
    var newDist = getDist(arr[i], num);
    if(distance > newDist){
      distance = newDist;
      nearNum = arr[i];
    };
  }
  return nearNum;
}
// 두 수의 거리를 구하는 함수
function getDist(point1, point2){
  return Math.abs(point1 - point2);
}
// 거리 구하기
console.log("근접한수 : " + findNum(PROVIDED_NUM, arr));

```

처음에 위와 같이 작성했으나 비교할 양이 많아지면 성능상 문제가 생길것 같아  
이진탐색 알고리즘을 응용하여 다시 작성하였습니다.

```javascript
function findNum2(num, arr){
  var first = 0;
  var last = arr.length - 1;
  var mid = 0;
  var lowNum = arr[first];
  var highNum = arr[last];

  while(first <= last){
    mid = Math.floor((first + last) / 2);
    if (num == arr[mid]){
      return arr[mid];
    }
    else if(num < arr[mid]){
      last = mid - 1;
      highNum = arr[last];
    }
    else {
      first = mid + 1;
      lowNum = arr[first];
    }
  }
  return getDist(num, lowNum) < getDist(num, highNum) ? lowNum : highNum;
}

// 거리 구하기2
console.log("findNum2 : " + findNum2(PROVIDED_NUM, arr));
```

참조 : <https://ko.wikipedia.org/wiki/%EC%9D%B4%EC%A7%84_%EA%B2%80%EC%83%89_%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98>

### 5. https와 http의 차이점에 대해서 설명해주세요.
 http는 웹 서버와 브라우저 사이에 정보를 전송하기 위한 통신 규약입니다.  
https는 http 프로토콜에 Secure Socket Layer(SSL)를 이용해 모든 통신 내용을 암호화 하는 보안 통신망입니다.  
따라서 https는 http에 비해 보안성이 뛰어납니다. 하지만 암호화 하는 과정을 거치기 때문에 속도가 느린 단점이 있습니다.  

참조 : <http://dany-it.tistory.com/96>

### 6. 아래 정의에 따라 line l(t)가 plane pl과 만나는 포인트를 위한 t를 구하는 수식을 작성해주세요.
l(t) = p + tv, v는 3 차원 벡터, t는 임의의 상수, p는 임의의 포인트, l은 line을 의미.
pl = nq + d = 0, n은 노멀벡터, q는 임의의 포인트, pl은 plane을 의미. 여기서 d는 중요하지 않으므로 자세한 건 생략.

풀지 못하였습니다.

### 7. 다음코드에서 콘솔이 찍히는 값과 해당값이 어떻게 나왔는지 설명해주세요.  
```javascript
console.log(1, Person());
console.log(2. person); // '.'을 ','라 생각하고 풀었습니다.
var person = 1001;
var Person = function(age, height){
	return age + “:”+height;
}
function Person(){
	console.log(SubPerson());
	var SubPerson = function(){
		return 10000;
	}

function SubPerson(){
	return 20000;
	}
	return SubPerson();
}
person = Person(10,100);
console.log(3, person);
```

 javascript 는 실행시 호이스팅의 과정을 거치게 되는데, 호이스팅이란   
var 로 선언된 변수와 선언적 함수가 위로 끌어올려지는 것을 뜻합니다.  
따라서 위 문제는 호이스팅 이후 아래와 같은 코드가 될것이라 예상했습니다.  
또한, javascript는 오버로딩이 되지 않고 덮어쓰여지기 때문에 마지막으로 할당한 값이 실행됩니다.  
추가로 es6 이후 let를 사용하면 호이스팅을 막을 수 있는것으로 알고 있습니다.  

```javascript
var person = undefined;
var Person = undefined;
function Person(){
  var SubPerson = undefined;
  function SubPerson(){
	  return 20000;
	} // -> SubPerson()1
	console.log(SubPerson()); // -> SubPerson()1 이 실행됨
	SubPerson = function(){
		return 10000;
	} // -> SubPerson()2
	return SubPerson();
} // -> Person()1

console.log(1, Person()); // -> Person()1 이 실행됨, return으로 SubPerson()2 의 값을 받아옴
console.log(2, person); // 값이 할당되지 않았기 때문에 undefined
person = 1001; // -> person1
Person = function(age, height){
	return age + ":" +height;
} // -> Person()2
person = Person(10,100); // -> person2 
console.log(3, person); // 마지막으로 할당된 person2가 실행됨. 따라서 Person()2 가 실행됩니다.

// 결과
// 20000
// 1 10000
// 2 undefined
// 3 "10 : 100"
```
