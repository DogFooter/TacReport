## Factory method 패턴 특징
- **"객체 생성 공장"** 을 지어서 객체를 같은 방법으로 찍어낸다.
  
- 찍어낼 객체는 미리 생성 함수 안에 정의해놓을수도 있지만(결합도 높음)  
  **static 생성함수를 전달**하여 등록해놓을 수도 있다(결합도 낮음)

## Prototype 패턴 특징
- **이미 생성된 기존의 객체를 이용하여 같은 상태의 객체를 생성**한다.
- 복사생성을 이용하므로 얕은 복사에 주의한다.

## Prototype 패턴을 사용하는 경우
- 취급할 객체의 종류가 많은 경우 (사각형 -> 노란 사각형,빨간 사각형,파란 사각형,..)
  
- 클래스로부터의 인스턴스 생성이 어려운 경우(ex 사용자 입력이 필요, ..)  

- 객체를 생성하는 부분(함수,프레임워크 등)을 해당 클래스에 의존하지 않게 하고싶은 경우  
  (해당 클래스 이름을 직접 지정해서 객체를 생성하지 않고,견본을 받아 등록하고 그것을 통해 객체 생성 가능)

## Prototype 패턴과 Factory method 패턴을 결합 : Prototype을 사용한 Factory  

- 공장에 견본품을 등록
   &nbsp;
```cpp

class ShapeFactory
{
	std::map<int, Shape*> prototype_map; 
public:
	void register_sample(int key, Shape* sample) 
	{
		prototype_map[key] = sample;
	}

	Shape* create(int type)
	{
		Shape* p = nullptr;

		if (prototype_map[type] != nullptr)
		{
			p = prototype_map[type]->clone(); 
		}
		return p;
	}
};
```
