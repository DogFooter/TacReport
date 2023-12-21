 ```cpp

int main() {
    unique_ptr<int> myPtr = make_unique<int>(23);

    auto getPtr = myPtr.get();
    auto getPtrDeref = *myPtr.get();
    auto deref = *myPtr;

    return 0;
}
 ```


 ```asm
     8:     unique_ptr<int> myPtr = make_unique<int>(23);
00007FF7D2C929BD  mov         edx,8  
00007FF7D2C929C2  lea         rcx,[myPtr]  
00007FF7D2C929C6  call        std::make_shared<int,int> (07FF7D2C91591h)  
00007FF7D2C929CB  mov         dword ptr [rbp+144h],17h  
00007FF7D2C929D5  lea         rdx,[rbp+144h]  
00007FF7D2C929DC  lea         rcx,[myPtr]  
00007FF7D2C929E0  call        std::make_unique<int,int,0> (07FF7D2C915AAh)  
     9: 
    10:     auto getPtr = myPtr.get();
00007FF7D2C929E5  lea         rcx,[myPtr]  
00007FF7D2C929E9  call        std::make_shared<int,int> (07FF7D2C9158Ch)  
00007FF7D2C929EE  mov         qword ptr [getPtr],rax  
    11:     auto getPtrDeref = *myPtr.get();
00007FF7D2C929F2  lea         rcx,[myPtr]  
00007FF7D2C929F6  call        std::make_shared<int,int> (07FF7D2C9158Ch)  
00007FF7D2C929FB  mov         eax,dword ptr [rax]  
00007FF7D2C929FD  mov         dword ptr [getPtrDeref],eax  
    12:     auto deref = *myPtr;
00007FF7D2C92A00  lea         rcx,[myPtr]  
00007FF7D2C92A04  call        std::unique_ptr<int,std::default_delete<int> >::~unique_ptr<int,std::default_delete<int> > (07FF7D2C915A5h)  
00007FF7D2C92A09  mov         eax,dword ptr [rax]  
00007FF7D2C92A0B  mov         dword ptr [deref],eax  

 ```

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
