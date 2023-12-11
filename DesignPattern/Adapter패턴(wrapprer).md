## 용도  

- 서로 다른 클래스 **사이에 끼워서 재사용**
  - 이미 제공된 것과 필요한 것 사이의 차이를 메울 때 씀 (신 버전과 구 버전의 호환성 해결, ..)

## 구조에 따른 구분

- 상속을 사용
  - 클래스에 의한 Adapter 패턴, 감싸기
    - 주어진 A 클래스와 실제 필요한 B 클래스가 있을 떄, A,B를 **상속** 받아서 어댑터 클래스인 C 를 만듦  
  &nbsp;
```java
public class GivenNeeds extends Needs implments Given{
  public GivenNeeds(String string){
    super(string);
}

@override
public void NeedsFunction1(){ // Given 함수와 Need 함수가 서로 너무 동떨어진 기능일 경우엔 변환 불가
  GivenFunction1();
}

@override
public void NeedsFunction2(){
  GivenFunction2();
}


}


```
   &nbsp;
- 위임을 사용
  - 인스턴스에 의한 Adapter 패턴, 끼워넣기
    - 주어진 A 클래스와 실제 필요한 B 클래스가 있을 때, B를 상속받고 A를 멤버변수로 둔 어댑터 클래스 C를 만듦   
  &nbsp;  
```java
public class GivenNeeds extends Needs {

private Given given; // 주어진 클래스

  public GivenNeeds(String string){
    this.given = new Given(string);
}

@override
public void NeedsFunction1(){
  given.GivenFunction1();
}

@override
public void NeedsFunction2(){
  given.GivenFunction2();
}


}

  
```
  &nbsp;
## 연관된 디자인 패턴    
- Bridge 패턴 : 기능계층과 구현계층을 연결시키는 Bridge패턴과 서로 다른 클래스를 연결하는 Adapter 패턴은 서로 닮아있다.  
  
- Decorator 패턴 : 인터페이스(API)를 변경하지 않고 기능을 추가하여 인터페이스의 차이를 메우는 Decorator 패턴과 서로 다른 클래스의 차이를 메우는
   Adapter 패턴은 서로 닮아있다. 
  &nbsp;
## 주의점 
  &nbsp;
- 상위 클래스의 모든 것을 물려받는 상속의 특성 상, 일반적으로 상속보다는 **위임**을 사용하는 것이 문제가 적음
    


    
