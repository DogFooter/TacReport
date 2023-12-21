
 ```cpp
int main() {
    shared_ptr<int> myPtr = make_shared<int>(23);

    auto getPtr = myPtr.get();
    auto getPtrDeref = *myPtr.get();
    auto deref = *myPtr;

    return 0;
}
 ```





 ```asm
     8:     shared_ptr<int> myPtr = make_shared<int>(23);
00007FF7E41E2A1D  mov         edx,10h  
00007FF7E41E2A22  lea         rcx,[myPtr]  
00007FF7E41E2A26  call        std::_Ptr_base<int>::_Incref (07FF7E41E15C3h)  
00007FF7E41E2A2B  mov         dword ptr [rbp+154h],17h  
00007FF7E41E2A35  lea         rdx,[rbp+154h]  
00007FF7E41E2A3C  lea         rcx,[myPtr]  
00007FF7E41E2A40  call        std::make_shared<int,int> (07FF7E41E15E6h)  
     9: 
    10:     auto getPtr = myPtr.get();
00007FF7E41E2A45  lea         rcx,[myPtr]  
00007FF7E41E2A49  call        std::_Ptr_base<int>::get (07FF7E41E15EBh)  
00007FF7E41E2A4E  mov         qword ptr [getPtr],rax  
    11:     auto getPtrDeref = *myPtr.get();
00007FF7E41E2A52  lea         rcx,[myPtr]  
00007FF7E41E2A56  call        std::_Ptr_base<int>::get (07FF7E41E15EBh)  
00007FF7E41E2A5B  mov         eax,dword ptr [rax]  
00007FF7E41E2A5D  mov         dword ptr [getPtrDeref],eax  
    12:     auto deref = *myPtr;
00007FF7E41E2A60  lea         rcx,[myPtr]  
00007FF7E41E2A64  call        std::unique_ptr<int,std::default_delete<int> >::unique_ptr<int,std::default_delete<int> ><std::default_delete<int>,0> (07FF7E41E15D2h)  
00007FF7E41E2A69  mov         eax,dword ptr [rax]  
00007FF7E41E2A6B  mov         dword ptr [deref],eax  
 ```

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
