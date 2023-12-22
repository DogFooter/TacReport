## unique_ptr 의 샘플코드

 - 다음 샘플 코드를 통해 unique_ptr의 **get()** 과 **역참조 연산자**의 세부로직을 비교해 볼 것이다.
   
 ```cpp

int main() {
    unique_ptr<int> myPtr = make_unique<int>(23);

    auto getPtr = myPtr.get(); // int* 
    auto getPtrDeref = *myPtr.get(); //int(23) 
    auto deref = *myPtr; //int(23)

    return 0;
}
 ```
&nbsp;
## get() 호출 부분의 어셈블리 코드
- get 함수를 호출하고,얻은 값을 getPtr 로 옮기는 부분으로 구성
 ```asm
    10:     auto getPtr = myPtr.get();
00007FF7D2C929E5  lea         rcx,[myPtr]  
00007FF7D2C929E9  call        std::make_shared<int,int> (07FF7D2C9158Ch) //get 함수 호출  
00007FF7D2C929EE  mov         qword ptr [getPtr],rax
 ```
&nbsp;
## *get() 호출 부분의 어셈블리 코드
- myPtr.get() 에 비해 eax에 값을 한번 더 저장하는 과정이 추가
  - rax가 **가리키는 메모리 위치에 있는 값을 eax에 저장**
 ```asm
    11:     auto getPtrDeref = *myPtr.get();
00007FF7D2C929F2  lea         rcx,[myPtr]  
00007FF7D2C929F6  call        std::make_shared<int,int> (07FF7D2C9158Ch)  //get 함수 호출
00007FF7D2C929FB  mov         eax,dword ptr [rax]  
00007FF7D2C929FD  mov         dword ptr [getPtrDeref],eax
```
&nbsp;
## 역참조 부분 어셈블리 코드
- myPtr.get() 에서 get() 대신 역참조 연산자 호출하는 부분이 다름
 - 그러나 MSVC 어셈블리 레벨에서, **get() 과 역참조 연산자(*)의 코드는 포인터 리턴이냐,포인터가 가리키는 값 리턴이냐의 차이**만 존재한다.
 ```asm
    12:     auto deref = *myPtr;
00007FF7D2C92A00  lea         rcx,[myPtr]  
00007FF7D2C92A04  call        std::unique_ptr<int,std::default_delete<int> >::~unique_ptr<int,std::default_delete<int> > (07FF7D2C915A5h)  // 역참조 연산자 호출
00007FF7D2C92A09  mov         eax,dword ptr [rax]  
00007FF7D2C92A0B  mov         dword ptr [deref],eax  

 ```
&nbsp;
### get() 함수의 어셈블리 코드
- 포인터 리턴
```asm
  3250:     _NODISCARD pointer get() const noexcept {
00007FF78EDA2300  mov         qword ptr [rsp+8],rcx  
00007FF78EDA2305  push        rbp  
00007FF78EDA2306  push        rdi  
00007FF78EDA2307  sub         rsp,0E8h  
00007FF78EDA230E  lea         rbp,[rsp+20h]  
00007FF78EDA2313  lea         rcx,[__548E9306_memory (07FF78EDB5062h)]  
00007FF78EDA231A  call        __CheckForDebuggerJustMyCode (07FF78EDA14DDh)
  3251:         return _Mypair._Myval2;
00007FF78EDA231F  mov         rax,qword ptr [this]  
00007FF78EDA2326  mov         rax,qword ptr [rax]  
  3252:     }
```
&nbsp;
### 역참조 연산자의 어셈블리 코드
-포인터가 가리키는 값 리턴
```asm
    _NODISCARD _CONSTEXPR23 add_lvalue_reference_t<_Ty> operator*() const noexcept(noexcept(*_STD declval<pointer>())) {
00007FF7ADB04B40  mov         qword ptr [rsp+8],rcx  
00007FF7ADB04B45  push        rbp  
00007FF7ADB04B46  push        rdi  
00007FF7ADB04B47  sub         rsp,0E8h  
00007FF7ADB04B4E  lea         rbp,[rsp+20h]  
00007FF7ADB04B53  lea         rcx,[__53EF8FA2_memory (07FF7ADB19059h)]  
00007FF7ADB04B5A  call        __CheckForDebuggerJustMyCode (07FF7ADB015FAh)  
        return *_Mypair._Myval2;
00007FF7ADB04B5F  mov         rax,qword ptr [this]  
00007FF7ADB04B66  mov         rax,qword ptr [rax]  
    }
```
