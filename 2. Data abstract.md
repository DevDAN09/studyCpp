# Chapter 4 정보은닉

# 4-1 정보은닉

- 프로그래머의 실수에 대한 대책이 하나도 준비되어 있지 않으면 안된다.
- 중요한 변수의 값을 바꾸지 않도록 private 해서 접근을 제어해야한다.

→ 멤버변수를 private 으로 선언, 해당 변수에 접근하는 함수를 별도로 정의. 안전한 형태로 멤버변수의 접근을 유도하는 것 

### Access Function : private 멤버변수를 클래스 외부에서 접근을 목적으로 하는 함수

```cpp
int GetX() const;
bool SetX(int xpos);

int GetY() const;
bool SetY(int ypos);
```

→ 호출되지 않을 수도 있지만 구현해야하는 함수.

### const 함수

: 함수 내에서 멤버변수에 저장된 값을 변경하지 않겠다.

- const 함수 내에서는 const가 아닌 함수의 호출이 제한된다 → const 선언되지 않은 함수는 아무리 멤버변수에 저장된 값을 변경하지 않더라도 변경할 수 있는 능력을 지닌 함수
- const 참조자를 호출하여 사용하는 함수내에서도 실행되는 함수가 const가 아니면 컴파일 에러가 난다 → 이는 const의 용도를 헤치지 않기 위함이다.

<aside>
💡 const는 변수의 값이 변경되지 않도록 선언하는 상수화 키워드이다. 이 목적을 해치는 접근을 원천 차단하기 위해 접근되는, 실행되는 함수 또한 const선언되지 않으면 이는 컴파일 오류를 일으킨다.

</aside>

# 4-2 캡슐화

### 캡슐화와 정보은닉의 차이..?

감기약을 먹는 방법

1. 콧물, 재치기, 코막힘용 캡슐을 복용
2. 3개를 하나로 캡슐화한 캡슐을 복용

→ 캡슐화가 어려운 이유 : 캡슐화의 범위를 결정하는 일이 쉽지 않기 때문

<aside>
💡 캡슐화는 정보은닉이 기본적으로 포함 (캡슐화를 진행하지만 진행하는 함수내에서 정보은닉이 동행되어야 한다.

</aside>

# 4-3 생성자 소멸자

### 생성자의 이해

- 클래스의 이름과 함수의 이름이 동일
- 반환형이 선언 X , 실제로 반환 되지 않음
- 객체 생성시 딱 한번 만 호출된다

- 생성자도 함수의 일종이므로 → 함수 오버로딩이 가능하다.
- 생성자도 함수의 일종이므로 → 매개변수에 ‘디폴트 값’을 설정 할 수 있다.

```cpp
#include <iostream>
using namespace std;

class SimpleClass
{
private:
	int num1;
	int nume2;
public:
	SimpleClass()
	{
		num1 = 0;
		num2 = 0;
	}
	SimpleClass(int n)
	{
		num1 = n;
		num2 = 0;
	}
	SimpleClass(int n1, int n2)
	{
		num1 = n1;
		num2 = n2;
	}
	SimpleClass(int n1 = 0, int n2 = 0)
	{
		num1 = n1;
		num2 = n2;
	}
	
	void ShowData() const
	{
		cout << num1 << ' ' << num2 << endl;
	}
}; 
```

SimpelClass sc1() (X)

SimpleClass sc1 ; (O)

SimpleClass * ptr1 = new SimpleClass; (O)

SimpeClass * ptr1 = new SimpleClass(); (O)

### SimpleClass sc1() 으로 객체생성을 할 수 없는 이유

SimpleClass sc1() 를 함수의 원형인지 객체생성문인지 구분할 수 없기 떄문에 이를 함수의 원형선언에만 사용하기로 약속했기 떄문입니다.

### 멤버 이니셜라이저를 이용한 멤버 초기화

```cpp
class Rectangle
{
private:
	Point upLeft;
	Point lowRight;
public:
	Rectangle(const int &x1, const int &y1, const int&x2, const int&y2);
	void ShowRecInfo() const;
};

//
Rectangle::Rectanlge(const int &x1, const int &y1, const int&x2, const int&y2)
										:upLeft(x1,y1), lowRight(x2,y2)
{
	//객체 upLeft의 생성과정에서 인자를 전달받는 생성자 호출
	//객체 downRight의 생성과정에서 인자를 전달닫는 생성자 호출
}
```

### 객체의 생성과정

1. 메모리 공간의 할당
2. 이니셜라이저를 이용한 멤버변수(객체)의 초기화
3. 생성자의 몸체부분 실행

### 멤버 이니셜라이저를 이용한 변수 및 const 상수 초기화

이니셜라이저로 멤버변수를 초기화 할 수 있다

이니셜라이저로 멤버변수를 초기화하면 좋은 점

- 초기화의 대상을 명확히 인식할 수 있다
- 성능의 약간의 이점이 있다.
    - 생성자 몸체의 초기화
        - int num2;
        - num2 = n2;
    - 이니셜라이저의 초기화
        - int num1 = n1;
    
    → 이니셜라이저를 이용하면 선언과 동시에 초기화되는 형태로 바이너리 코드가 생성
    

```cpp
class Sosimple
{
private:
	int num1;
	int num2;
public:
	Sosimple(int n1, int n2) : num1(n1)
	{
		num2 = n2;
	}
	....
};
```

<aside>
💡 const 멤버변수도 이니셜라이저를 이용하면 초기화가 가능(const 변수는 선언과 동시에 초기화 해야한다)

</aside>

### 이니셜라이저의 이러한 특징은 멤버변수로 참조자를 선언할 수 있게 합니다

- 상수화 된 멤버변수를 초기화
- 다른 객체를 맴버변수로 받는 클래스에서 이니셜라이저로 멤버변수인 다른객체를 초기화 할 수 있습니다.

### 디폴트 생성자(Default Constructor)

1. 메모리 공간 할당
2. 생성자의 호출 

→ 객체라고 할 수 있다

**객체가 되기 위해서는 반드시 하나의 생성자가 호출되어야 한다.**

→ 해당 룰을 깨지 않기 위해 생성자가 없는 클래스에 default 생성자가 있다.

```cpp
class AAA
{
private :
	int num;
public:
	AAA(){ } //default 생성자
	int GetNum{ return num; }
};
```

C언어의 malloc 함수를 사용하면 생성자는 호출되지 않는다

→ 클래스 만큼의 크기만 바이트 단위로 전달 → 호출 될 수 없다.

<aside>
💡 객체를 동적할당하기 위해서는 new 연산자를 이용

</aside>

### 생성자 불일치

일치하는 생성자가 없을 시 객체생성을 할 수 없게 된다.

### private 생성자

클래스 내부에서 객체를 생성하고 싶을 때 → 생성방법을 제한하고자 하는 경우에는 유용하게 사용한다

### 소멸자의 이해와 활용

~클래스명()

- 반환형이 선언되엉 있지 않고, 실제로 반환하지 않는다
- 매개변수는 void형으로 선언, 오버로딩, 디폴트 값 설정도 불가능하다.

```cpp
~AAA() { . . . . }
```

- 디폴트 생성자와 마찬가지로 객체소멸 과정에서 자동으로 호출된다.
- 클래스 내부에서 진행된 힙영역 접근을 소멸자를 통해 정의부에서 delete 키워드를 이용하여 동적할당을 해제해준다.

# 4-4 클래스와 배열 그리고 this 포인터

### 객체배열

```cpp
Sosimple arr[10];
Sosimple * ptrArr = new SoSimple[10];
```

- 배열 선언에서는 객체의 생성자에 인자전달이 불가능하다. → Sosimple() { . . . . }의 형태의 생성자가 클래스 내부에 선언되어 있어야한다.
- 배열 선언에 있어서 Sosimple()의 형태에 생성자는 객체의 모든 멤버변수의 값을 초기화 하는 것이 좋다.

### 객체 포인터 배열

```cpp
Person * parr[3];

parr[0]->ShowPersonInfo();
parr[1]->ShowPersonInfo();
```

- 객체 배열을 사용할 때는 2가지 방법중 하나를 선택해야한다
    1. 저장의 대상을 객체
    2. 저장의 대상을 객체의 주소 값

### this 포인터의 이해

: 객체 자신을 가리키는 용도로 사용되는 **포인터**

### this 포인터의 활용

```cpp
public:
	void ThisFunc(int num)
	{
		this->num = 207;
		num = 105; // 매개변수의 값을 105로 변경
	}
	. . . . .
};
```

### Self-Reference의 반환

: 객체 자신을 참조할 수 있는 참조자 

- this 포인터를 이용해서 객체가 자신의 참조에 사용할 수 있는 참조의 반환문을 구성할 수 있다.

```cpp
#include <iostream>
using namepsace std;

class SelfRef
{
private:
	int num;
public:
	SelfRef(int n) : num(n)
	{
		cout << "객체생성" << endl;
	}
	SelfRef& Adder(int n)
	{
		num+=n;
		return *this;
	}
	SelfRef& ShowTwoNumber()
	{
			cout << num << endl;
			return *this;
	}
};

int main(void)
{
	SelfRef obj(3); //객체생성
	SelfRef &ref = obj.Adder(2); //obj(num == 3) 인 객체에 2를 더한 후 ref 참조자 달아준다.
	
	obj.ShowTwoNumber(); // 5
	ref.ShowTwoNumber(); // 5

	ref.Adder(1).ShowTwoNumber().Adder(2).ShowTwoNumber();
	//6
  //8
	return 0;
}
```

### 참조의 정보(참조 값)에 대한 이해

```cpp
int main(void)
{
	int num = 7;
	int &ref = num; // 무엇이 전달 된다고 생각해야하나..?
```

<aside>
💡 변수 num을 참조할 수 있는 참조의 정보가 전달됩니다.

</aside>

- 참조자 선언
- 반환형으로 참조형이 선언

⇒ 그 때 전달되는 정보를 표현하기 위해서 ‘참조의 정보’ 또는 ‘참조 값’이라는 표현 사용