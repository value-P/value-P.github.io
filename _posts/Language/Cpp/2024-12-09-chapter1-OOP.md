---
title: "항목01. 객체지향"
categories: 
    - Cpp
tags: [Cpp]

toc: true
toc_sticky: true

date: 2024-12-09
last_modified_at: 2024-12-11
---

## 객체지향(OOP)?
### 개념
- 절차지향
    - 프로그래밍을 순차적인 명령의 집합으로 작성하는 방식
- 객체지향
    - 프로그램을 데이터와 함수로 이루어진 객체의 상호작용으로 표현하는 방식

### 장점
1. 코드의 재사용성이 높다.
2. 유지보수가 용이하며 확장성이 높아 규모가 큰 프로젝트에 유리하다

### 단점
1. 초기 설계단계에 시간이 많이 소요된다.
2. 절차지향 언어에 비해 속도가 느리다.

## 객체지향의 네가지 특징

### 1. 다형성
🔖 개념  
- 어떤 객체의 속성이나 기능이 상황에 따라서 여러 가지 형태를 가질 수 있는 성질

📝 예시
- 메서드 오버라이딩 (`동적 바인딩`)
- 메서드 오버로딩 (`정적 바인딩`)

### 2. 상속
🔖 개념  
- 기존의 클래스를 재활용해 새로운 클래스를 작성하는 것

### 3. 추상화
🔖 개념  
- 객체의 공통적인 속성과 기능을 추출해 정의하는 것

📝 예시
```cpp
class Vehicle{
public:
    virtual void Move() abstract;
}

class Car : public Vehicle{
public:
    virtual void Move() override { std::cout << "자동차 전진\n"; }
}

class Bicycle : public Vehicle{
public:
    virtual void Move() override { std::cout << "자전거 전진\n"; }
}
```

### 4. 캡슐화
🔖 개념  
- 서로 연관있는 속성과 기능을 하나의 캡슐로 만들어 데이터를 외부로부터 보호하는 것

💎 이유  
- 데이터의 보호 : 외부로부터 클래스에 정의된 속성과 기능 보호
- 데이터 은닉 : 내부 동작을 감추고 외부에는 필요한 부분만 노출
- 외부에서 클래스 내부 데이터를 직접 사용하는 경우 **클래스 수정 시 파급효과**가 커짐 이를 방지하게 하기 위해 캡슐화를 잘 지키는 것이 중요하다!

## SOLID 원칙
### 1. 단일책임의 원칙(Single Responsibility)
🔖 개념  
- 작성된 클래스는 하나의 기능만을 가지고 클래스가 제공하는 모든 서비스는 그 하나의 책임을 수행하는 데 집중됭 있어야 한다는 것
- 책임 영역이 확실하므로 한 책임의 변경이 다른 책임에 영향을 미치는 연쇄작용에서 자유로울 수 있다.

📝 예시
- SRP적용 전
```cpp
class Guitar{
private:
    std::string mSerialNum;
    std::string mModelName;
    int mPrice;    
}
```

- SRP적용 후
```cpp
class GuitarSpec{
private:
    std::string mModelName;
    int mPrice;    
}
```
```cpp
class Guitar{
private:
    std::string mSerialNum;
    class GuitarSpec* mSpec;
}
```

### 2. 개방폐쇄의 원칙(Open Close)
🔖 개념  
- 소프트웨어의 구성요소는 확장에는 열려있고, 변경에는 닫혀있어야 한다는 원리
- 요구사항의 변경이나 추가사항이 발생하더라도, 기존 구성요소는 수정이 일어나지 말아야하며, 기존 구성요소를 확장해 재사용할 수 있어야한다는 것

### 3. 리스코프 치환의 원칙(Liskov Substitution)
🔖 개념  
- 서브타입은 언제나 기반타입으로 교체할 수 있어야한다는 원칙

### 4. 인터페이스 분리의 원칙(Interface Segregation)
🔖 개념  
- 자신이 사용하지 않는 인터페이스는 구현하지 말아야한다는 원리
- 하나의 일반적인 인터페이스보다 여러 개의 구체적인 인터페이스가 낫다
- **인터페이스의 단일책임** 강조

### 5. 의존성 역전의 원칙(Dependency Inversion)
🔖 개념  
- 객체에서 어떤 class를 참조해 사용해야하는 상황이 생긴다면, 해당 class를 직접 참조하는 것이 아닌 대상의 **상위 요소(추상 클래스 or 인터페이스)**로 참조하라는 원칙
- 하위 모듈을 직접 가져다 사용할 경우 **하위 모듈의 구체적인 내용에 의존**하게 되기 때문이다
- 즉, 상위의 인터페이스 객체로 통신하라는 뜻