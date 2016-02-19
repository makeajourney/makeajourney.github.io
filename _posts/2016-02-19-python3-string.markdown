---
title: "파이썬3 문자열(String)"
layout: post
date: 2016-02-19
categories : python3
---


### 문자열 (String)  
* 유니코드 표준 지원  
* 불변 (immutable)  

<br>

### 인용부호(\'', \"")로 문자열 생성
1. 작은 따옴표(\')와 큰 따옴표(\") 사이에 문자들을 입력하여 문자열 생성.  
	작은 따옴표로 시작한 문자열은 작은 따옴표로 끝나며, 그 사이에 작은 따옴표가 들어가면 안된다.  
	큰 따옴표로 시작한 문자열은 큰 따옴표로 끝나며, 그 사이에 큰 따옴표가 들어가면 안된다.  
	이렇게 두 가지의 부호를 사용하는 이유는, 문자열 내에 작은 따옴표의 인용이 필요할 때 큰 따옴표로 문자열을 생성하여 사용할 수 있도록 하기 위함이다.  
	`"'Nay,' said the naysayer.`  
	`'The rare double quote in captivity: ".'`  
2. 세 개의 작은 따옴표 혹은 세 개의 큰 따옴표를 사용하여 문자열 표현.  
	여러 줄의 문자열의 표현에 적합.  
	```
	'''
	poem = ''' There was a Young Lady of Norway,
	Who casually sat in a doorway;
	When the door squeezed her flat,
	She exclaimed, "What of that?"
	This courageous Young Lady of Norway.'''
	```  
3. 위 인용부호들을 이용하여 빈 문자열 생성 가능.
	`base = ""`  
	`''`

<br>

### 다른 데이터 타입을 문자열로 변경  
`str()` 사용.  
`str(98.6)		// 결과는 '98.6'`  

<br>

### 이스케이프 문자 (\)  
백슬래시를 이용하여 특별한 의미를 갖는 문자의 입력이 가능하다.  
1. `\n` : 줄 바꿈  
2. `\t` : 탭  
3. `\"`, `\'` : 인용부호 \", \'  
4. `\\` : 백슬래시 \\  

<br>

### 문자열의 결합  
두 문자열을 합칠 때 `+` 연산자를 사용하여 결합이 가능하다.  
`'Release the kraken! ' + 'At once!' 	// 'Release the kraken! At once!'`  
여기서 주의할 점은, 문자열 결합시 문자열 사이에 공백을 자동으로 삽입해 주지 않으므로 필요시에는 명시적으로 삽입이 필요하다.  

<br>

### 문자열의 복제
`*` 연산자를 이용하여 문자열의 복제가 가능하다.  
`'Na ' * 3		// 'Na Na Na'`  

<br>

### 문자 추출
`letter = 'abcdefghijklmnopqrstuvwxyz'`  
1. 문자열에서 한 문자를 얻기 위해서는 문자열 이름 뒤에 대괄호([])와 오프셋(offset)을 지정한다.  
	가장 왼쪽 0 부터 시작해서 문자열의 크기 -1 만큼의 offset을 지정할 수 있다.  
	음수로도 지정 가능하다. -1을 입력하면 문자열의 가장 마지막 문자를 가리킨다. -2는 뒤에서 두 번째, -3은 뒤에서 세 번째를 가리킨다.  
	`letter[0]	// 'a'`  
	offset 값을 문자열의 길이보다 큰 값으로 지정하면 에러가 발생한다.  
		`IndexError: string index out of range`  
2. Python의 문자열은 불변한 속성이기 때문에 한 인덱스를 임의로 지정하여 변경하는 것이 불가능하다.  
	`letter[1] = 'b'	// 에러 발생`  
	`TypeError: 'str' object does not support item assignment`  

<br>

### 문자열 slice를 이용한 substring 추출
`문자열변수이름[start:end]` : start부터 end offset까지 추출  
`문자열변수이름[start:end:step]` : step 간격으로 start부터 end offset까지 추출  
* start와 end, step 모두 생략 가능하다. 모두 생략하면 (`varname[:]`) string의 전체가 추출된다.  
* 하나만 생략도 가능하다. start를 생략하면 처음부터 end까지, end를 생략하면 start부터 끝까지 substring을 추출한다.  
* 음수 offset도 사용 가능하다.  
	`letter[-3:]	// 'xyz'` 마지막 세 문자 출력  
	`letter[-6:-2]	// 'uvwx'` 끝에서 여섯 번째 문자부터 끝에서 세 번째 문자까지 출력  
	`letter[::-1]	// 'zyxwvutsrqponmlkjihgfedcba` 문자열을 역순으로 출력  

<br>

### 문자열의 길이
`len()`  
`len(letter)	// 26`  

<br>

### 문자열 나누기
`split()`  
어떤 구분자를 기준으로 하나의 문자열을 작은 문자열들의 리스트로 나누기 위해 사용.  
`문자열변수이름.split('구분자')` 구분자를 생략하면 공백 기준으로 분리  
`todos = 'get gloves,get mask,give cat vitamins,call ambulance'`  
comma를 기준으로 분리  
	`todos.split(',')	// ['get gloves', 'get mask', 'give cat vitamins', 'call ambulance']`  
공백을 기준으로 분리  
	`todos.split()	// ['get', 'gloves,get', 'mask,give', 'cat', 'vitamins,call', 'ambulance']`  

<br>

### 문자열 결합
`join()`  
split()과 반대로, 결합할 문자열을 지정한 다음에 문자열 리스트를 결합.  
`'결합할문자열'.join(문자열리스트)`  

```
crypto_list = ['Yeti', 'Bigfoot', 'Loch Ness Monster']
crypto_string = ', '.join(crypto_list)
print(crypto_string)	// Yeti, Bigfoot, Loch Ness Monster
```

<br>

### 문자열 대체
`replace()`  
문자열의 일부를 대체하기 위해 사용.  
`문자열객체.replace('바꿀 문자열', '대체할 문자열', 횟수)`  
횟수를 생략하면 처음 나타나는 문자열 하나만 대체.  


<br>

### 대소문자 관련 문자열 함수

<br>

#### capitalize()
`문자열객체.capitalize()`    
첫번째 단어의 첫 글자를 대문자로 바꾸어 줌.  

<br>

#### title()
`문자열객체.title()`    
모든 단어의 첫번째 글자를 대문자로 바꾸어 줌.  

<br>

#### upper()
`문자열객체.upper()`  
모든 글자를 대문자로 바꾸어 줌.

<br>

#### lower()
`문자열객체.lower()`  
모든 글자를 소문자로 바꾸어 줌.  

<br>

#### swapcase()
`문자열객체.swapcase()`  
대문자는 소문자로, 소문자는 대문자로 바꾸어 줌.  

<br>

### 문자열 정렬

<br>

#### center()
`문자열객체.center(범위)`  
문자열을 지정한 범위 내에서 가운데에 오도록 한다.  
`setup.center(30)	// '  a duck goes into a bar...   '`  

<br>

#### ljust()
`문자열객체.ljust(범위)`  
문자열을 지정한 범위 내에서 왼쪽에 오도록 한다.  
`setup.ljust(30)	// 'a duck goes into a bar...     '`  

<br>

#### rjust()
`문자열객체.rjust(범위)`  
문자열을 지정한 범위 내에서 오른쪽에 오도록 한다.  
`setup.rjust(30)	// '     a duck goes into a bar...'`  

<br>

### 그 외의 문자열 함수들

<br>

#### strip()
`문자열객체.strip('제거할문자')`  
문자열 객체의 양 끝에서 제거할 문자를 찾아서 삭제.  
```
setup = 'a duck goes into a bar...'
setup.strip('.')	// 'a duck goes into a bar'
```

<br>

#### startswith()
`문자열객체.startswith('문자열')`  
문자열객체가 매개변수인 문자열로 시작하는지를 확인.  
boolean값 으로 리턴.  

<br>

#### endswith()
`문자열객체.endswith('문자열')`  
문자열객체가 매개변수인 문자열로 끝나는지를 확인.  
boolean 값으로 리턴.  

<br>

#### find()
`문자열객체.find('문자열')`  
문자열객체에서 매개변수인 문자열이 첫번째로 나오는 곳의 offset 리턴.  

<br>

#### rfind()
`문자열객체.rfind('문자열')`  
문자열객체에서 매개변수인 문자열이 마지막으로 나오는 곳의 offset 리턴.  

<br>

#### count()
`문자열객체.count('문자열')`  
문자열객체에서 매개변수인 문자열이 나오는 빈도 수 리턴.  

<br>

#### isalnum()
`문자열객체.isalnum()`  
문자열객체가 글자와 숫자로만 이루어졌는지를 확인.  
boolean 값으로 리턴.  

