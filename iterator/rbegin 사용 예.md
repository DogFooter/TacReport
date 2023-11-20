## rbegin() 의미

Returns an **iterator to the reverse-beginning** of the given range (cppreference.com 참조)  

즉, 인자로 받은 컨테이너의 **std::reverse_iterator** 를 반환한다.


## rbegin() 사용 조건
맨 마지막 원소부터 시작해서 **역순**으로 컨테이너를 순회해야하므로, **양방향 반복자 레벨 이상** 에서 사용 가능하다   
( forward_list 같이 싱글 연결리스트에서 rbegin 사용 불가 )

vector를 비롯한 대부분의 컨테이너들은 멤버함수로 rbegin() 을 제공하며,
멤버함수가 없는 C 스타일의 배열의 경우에도 **std::rbegin()** 을 사용해서 reverse_iterator 를 얻을 수 있다. 

## rbegin()의 사용 예
  
  ```cpp

	int x[10] = { 1,2,3,4,5,6,7,8,9,10 };
	int y[10] = { 0 }; // want : {10,9,8,7,6,5,4,3,2,1}

  //copy(x + 10, x , begin(y)); 망한 예
	copy(x, x + 10, std::rbegin(y));

  ```

위와 같이 STL 함수로 인자를 넘길 때 요긴하게 쓰인다. 

x 를 뒤집은 모양의 y 를 만들기 위해 std::copy를 이용한다고 가정하자.

먼저, 주석처리한 copy 함수처럼 배열 x를 역순으로 순회하며 y에 넣어보려고 시도해본다.  
그러나 주석처리한 첫번째 copy 함수처럼, first 와 last에 해당하는 인자를 뒤집어서 전달한다고 해서,   
함수가 이를 인지하고 last -> first 순으로 컨테이너를 순회해주는 것이 아니다.

last를 첫번째 인자로 받은 copy 함수는 이것을 first 인자로 취급하여, 여기서부터 first++ 를 하며 순회하려고 시도하다가 오류가 날 것이다.

이를 해결할 수 있는 방법으로 destination 컨테이너 y의 reverse_iterator를 넘겨서 거꾸로 순회하며 값을 넣는 방법이다.  

first 와 last 가 고정되어있는 source 컨테이너의 순회 방향을 우리가 수정할 수는 없지만,   
**rbegin 을 통한 reverse_iterator 로 destination 컨테이너의 순회 방향을 뒤집을 수는 있다.**

