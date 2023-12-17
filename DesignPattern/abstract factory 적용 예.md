### abstract factory 특징
- **공장들의 추상화**
- 여러 **객체군** 을 생성하기 위한 인터페이스를 제공
-  공장 선택 후 생성할 객체 선택

### abstract factory 적용 전
- 특정한 스타일(win , GTK) 의 버튼과 에디터 클래스
  
![beforeAbstractFactory](https://github.com/DogFooter/TacReport/assets/106423370/2eab3e6c-0f84-4065-b380-d2a1955ff587)

-> 특정한 스타일이 추가된다면 해당 스타일의 전체 컨텐츠(버튼,에디터 등)를 각기 해당하는 인터페이스에서 각각 상속받아 만들어야 함.
### abstract factory 적용 후 
- 특정한 스타일(win , GTK) 의 객체군을 만드는 공장(win , GTK) 
![abstractFactory](https://github.com/DogFooter/TacReport/assets/106423370/a6be3466-97bb-46e5-bc0d-a6ba9cd5ea80)
