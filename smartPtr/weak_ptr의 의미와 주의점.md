## weak ptr의 필요성
- shared_ptr 문제점인 **상호참조**로 인하여, 객체가 제거될 수 없는 현상이 발생 :
  
  - 해결방안 1 : 상호참조를 막기 위해, 객체를 가리켜도 참조 계수를 증가시키지 않는,  
    스마트포인터가 아닌 단순 pointer 사용
    
    - 문제점 : 단순 poiner 는 자신이 가리키는 객체가 유효한지 모르기때문에,  
      이미 파괴된 객체의 주소를 저장하고 있을 수도 있음. 
  - 해결방안 2 : 객체를 가리켜도 참조 계수를 증가시키지 않고, 객체의 유효성도 확인할 수 있는 방법, **weak_ ptr** 을 사용

## 참조계수(use_count)와 weak ptr 의 의미 
#### 참조계수(use_count)의 의미
참조하는 객체의 **수명**에 관여하여, 객체를 가리킬동안(사용할동안) 해당 객체가 파괴되지 않게 할 것이다.
### weak_ptr의 의미 
참조하는 객체의 **수명에 관여하지 않을 것이고** 객체를 가리키기만 할 것이다. 내가 가리키는 객체는 언제든지 파괴될 수 있다.

## weak_ptr 특징 및 주의점
- 가리키는 객체의 **유효성 조사** 필요 : **expired()** 함수 사용
- 가리키는 객체의 수명에 관여하지 않으므로 , **대상 객체에 접근 불가** (***->*** 연산자를 제공하지 않음)
  - 대상 객체에 접근하기 위해서는 **shared_ptr** 로 다시 변환 필요 : 변환하는 찰나에 객체가 파괴되는 것을 방지하기 위해 lock을 걸어야 함
  - 변환 예시
    
    ```cpp
    
      shared_ptr<Car> sp1 = make_shared<Car>();
      weak_ptr<Car> wp1 = sp1;
    
      shared_ptr<Car> sp2 = wp1.lock(); // wp1 호출하는 사이에 객체 파괴 방지
    
      if (sp) // wp1 호출 직전에 객체가 파괴되었을 가능성이 있으므로 유효성 검사 필수
        sp->Go();
    
    ```
- control block 내부에 weak_ptr을 위해 **weak_count** 계수가 따로 존재
  
  - control block 은 **대상 객체가 파괴되었다고 없어지는 것이 아니라** use_count 와 weak_count 가 둘 다 0 이 되었을 때,  
    즉 대상객체를 가리키는 **shared_ptr 과 weak_ptr 이 모두 없어야** 비로소 삭제된다.    

