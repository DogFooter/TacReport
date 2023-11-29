## 스마트 포인터 unique_ptr 와 shared_ptr 간의 호환성 확인

- unique_ptr 은 자원을 **독점** 하므로,자신이 참조하는 객체는 오로지 자신만 참조하고있다.
- shared_ptr 은 자원을 **공유**하므로, 자신이 참조하는 객체를 참조하는 다른 포인터들이 존재할 수 있다. 

unique_ptr 와 shared_ptr 을 서로 변환한다면, unique_ptr 의 **자원 독점** 특성이 지켜져야 한다.   

따라서, 여러 개의 포인터가 공유하고 있을지도 모르는 대상객체를 참조하는 shared_ptr는   
unique_ptr 로 변환할 수 없다.(**자원 독점 특성 위배**)

그러나, 유일하게 대상 객체를 참조하고 있는 unique_ptr은 shared_ptr 로 변환이 가능하다  
(**자원 공유 특성에 위배되지 않음**)


```cpp
    shared_ptr<int> sp(new int);
    unique_ptr<int> up(new int);

    shared_ptr<int> sp1 = up;       // error -> up 와 sp1 가 같은 대상객체를 가리키게 되므로 unique_ptr 인 up 가 자원 독점 특성에 위배  
    shared_ptr<int> sp2 = move(up); // ok -> usecount 1 인 shared_ptr

    unique_ptr<int> up1 = sp;       // error
    unique_ptr<int> up2 = move(sp); // error
}
```

오로지 **unique_ptr** 를 **shared_ptr** 로 **move** 를 통해 이동시키는 것만이 유효하다.

## unique_ptr 와 shared_ptr 의 호환성에 따른 함수 리턴값 결정

 unique_ptr는 shared_ptr로 변환이 가능하고,  hared_ptr는 절대 unique_ptr로 변환이 불가하므로  
 스마트 포인터를 함수의 리턴값으로 쓸 땐 **확장성을 위해서 unique_ptr 로 반환**하는 것이 좋다. 
