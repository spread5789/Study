문제
==
![캡처](https://user-images.githubusercontent.com/73854324/121453719-8e296e00-c9dc-11eb-95dd-aeee2ed457d1.PNG)
<br><br>
==
####처음 문제를 봤을때 든 생각<br>
그냥 피보나치 수열에서 1234567 로 나눈 값을 리턴하면 되겠네?<br><br>
하지만 실패했다.<br>
피보나치 수열은 조금만 수가 많아져도 int 범위를 벗어나기 때문이다.<br>
그렇다면 long 타입을 사용해서 풀어볼까 했지만 역시나 실패했다.<br>
아무래도 테스트값으로 엄청난 수가 입력되는 것 같다.<br><br>
![2](https://user-images.githubusercontent.com/73854324/121453722-8ec20480-c9dc-11eb-82ad-b855a559bf20.PNG)
이 사진은 문제를 풀면서 내 생각의 흐름(?) 을 적어놓은 것이다.<br><br>

첫번째 줄은 피보나치 수열을 2자리 수까지 적어놓은 것이고,<br>
10 나머지는 각 값들을 10의 나머지로 계산해본 것이다.<br>
여기서 규칙들이 살짝 눈에 들어오기? 시작했다.<br><br>

10의 나머지들도 피보나치 수열과 유사한 형태였다.<br>
10을 넘어간다면 그 값들을 %10 으로 계산해본 결과 점점 찾았다 라는 느낌을 받았다.<br>
<br>
확실히 해보기 위해서 12 로 나눈 나머지들도 계산해보았다.<br>
그 밑 줄의 수식은 짐작인데.. 멍청한 부분이 있지만 마지막 줄에 수정된걸로..<br>
<br>
결론은, 피보나치 수열에 나머지 계산을 적용하면 int 범위 내에서 해결할 수 있다는 것이다.<br>
1234567 로 나눈 나머지 값을 문제에서 괜히 준게 아니였다.<br>
<br>
```
피보나치 수열에서도 나머지 연산이 적용된다.
(a+b)%c = ((a%c)+(b%c))%c
```