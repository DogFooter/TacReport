## shared_ptr 의 샘플코드
 - 다음 샘플 코드를 통해 shared_ptr의 **get()** 과 **역참조 연산자**의 세부로직을 비교해 볼 것이다.
&nbsp;
 ```cpp
int main() {
    shared_ptr<int> myPtr = make_shared<int>(23);

    auto getPtr = myPtr.get();
    auto getPtrDeref = *myPtr.get();
    auto deref = *myPtr;

    return 0;
}
 ```


## get() 호출 부분의 어셈블리 코드
- get 함수를 호출하고,얻은 값을 getPtr 로 옮기는 부분으로 구성(unique_ptr 과 동일)
&nbsp;
 ```asm
    10:     auto getPtr = myPtr.get();
00007FF7E41E2A45  lea         rcx,[myPtr]  
00007FF7E41E2A49  call        std::_Ptr_base<int>::get (07FF7E41E15EBh)  
00007FF7E41E2A4E  mov         qword ptr [getPtr],rax
```
&nbsp;
## *get() 호출 부분의 어셈블리 코드
- myPtr.get() 에 비해 eax에 값을 한번 더 저장하는 과정이 추가(unique_ptr 과 동일)
  - rax가 **가리키는 메모리 위치에 있는 값을 eax에 저장**
&nbsp;
```asm
    11:     auto getPtrDeref = *myPtr.get();
00007FF7E41E2A52  lea         rcx,[myPtr]  
00007FF7E41E2A56  call        std::_Ptr_base<int>::get (07FF7E41E15EBh)  
00007FF7E41E2A5B  mov         eax,dword ptr [rax]  
00007FF7E41E2A5D  mov         dword ptr [getPtrDeref],eax  
```

## 역참조 부분 어셈블리 코드
- myPtr.get() 에서 get() 대신 역참조 연산자 호출하는 부분이 다름
  - 그리고 MSVC 어셈블리 레벨에서, **연산자(*)의 코드는 get()을 호출하여 역참조하는 방식**으로 구현했다.
&nbsp;
```asm
    12:     auto deref = *myPtr;
00007FF69B8A6680  lea         rcx,[myPtr]  
00007FF69B8A6684  call        std::shared_ptr<int>::operator*<int,0> (07FF69B8A10CDh)  
00007FF69B8A6689  mov         eax,dword ptr [rax]  
00007FF69B8A668B  mov         dword ptr [deref],eax  
 ```
### get() 함수의 어셈블리 코드
- 원시포인터를 저장하는 멤버변수 리턴
&nbsp;
 ```asm

  1270:     _NODISCARD element_type* get() const noexcept {
00007FF7E41E2890  mov         qword ptr [rsp+8],rcx  
00007FF7E41E2895  push        rbp  
00007FF7E41E2896  push        rdi  
00007FF7E41E2897  sub         rsp,0E8h  
00007FF7E41E289E  lea         rbp,[rsp+20h]  
00007FF7E41E28A3  lea         rcx,[__548E9306_memory (07FF7E41F5062h)]  
00007FF7E41E28AA  call        __CheckForDebuggerJustMyCode (07FF7E41E14DDh)  
  1271:         return _Ptr;
00007FF7E41E28AF  mov         rax,qword ptr [this]  
00007FF7E41E28B6  mov         rax,qword ptr [rax]  
  1272:     }

 ```
&nbsp;
### 역참조 연산자의 어셈블리 코드
- **get()을 호출한 후**,리턴된 주솟값 역참조
  - get()을 호출하지 않는 unique_ptr과는 미세한 차이가 존재함
    - unique_ptr 과 shared_ptr/weak_ptr 은 서로 다른 클래스로, 원시포인터를 저장하는 멤버변수 구조에 차이가 있음
&nbsp;
```asm
    template <class _Ty2 = _Ty, enable_if_t<!disjunction_v<is_array<_Ty2>, is_void<_Ty2>>, int> = 0>
    _NODISCARD _Ty2& operator*() const noexcept {
00007FF69B8A28D0  mov         qword ptr [rsp+8],rcx  
00007FF69B8A28D5  push        rbp  
00007FF69B8A28D6  push        rdi  
00007FF69B8A28D7  sub         rsp,0E8h  
00007FF69B8A28DE  lea         rbp,[rsp+20h]  
00007FF69B8A28E3  lea         rcx,[__53EF8FA2_memory (07FF69B8BA059h)]  
00007FF69B8A28EA  call        __CheckForDebuggerJustMyCode (07FF69B8A1668h)  
        return *get();
00007FF69B8A28EF  mov         rcx,qword ptr [this]  
00007FF69B8A28F6  call        std::_Ptr_base<int>::get (07FF69B8A11A9h)  
    }
```
