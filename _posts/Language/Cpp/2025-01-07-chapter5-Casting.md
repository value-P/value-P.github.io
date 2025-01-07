---
title: "항목05. 캐스팅"
categories: 
    - Cpp
tags: [Cpp]

toc: true
toc_sticky: true

date: 2025-01-07
last_modified_at: 2025-01-07
---

# 캐스팅
🔲 데이터 타입을 다른 타입으로 변환하는 과정으로 **명시적 캐스팅**과 **암시적 캐스팅** 이 있다.
<br>
🔲 또한 객체지향 프로그래밍에서는 클래스 간의 상속관계에 따라 **업 캐스팅**과 **다운 캐스팅**으로 나눌 수도 있다.

## 명시적 캐스팅 (Explicit Casting)
🔲 개발자가 명시적으로 데이터 타입 변환을 지시하는 경우
```cpp
double a = 10.5;
int b = (int)a;
```

## 암시적 캐스팅 (Implicit Casting)
🔲 명시적 지시 없이 데이터를 변환하는 경우
```cpp
double a = 10.5;
int b = a;
```

## 업캐스팅
🔲 자식 클래스에서 부모 클래스 타입으로 변환하는 경우
<br>
🔲 암시적으로 가능하며 보통 안전하다.
```cpp
class Parent {};
class Child : public Parent {};
Child child;
Parent* parent = &child;  // 업캐스팅
```

<br>
⚠️ 업 캐스팅 시 **슬라이싱** 발생 주의
<br>
```cpp
#include <iostream>
using namespace std;

class Parent {
public:
    int parentData;
    virtual void show() {
        cout << "Parent data: " << parentData << endl;
    }
};

class Child : public Parent {
public:
    int childData;
    void show() override {
        cout << "Parent data: " << parentData << ", Child data: " << childData << endl;
    }
};

int main() {
    Child child;
    child.parentData = 10;
    child.childData = 20;

    // 업캐스팅 발생 (값 복사)
    Parent parent = child;  // 슬라이싱 발생!
    parent.show();          // "Parent data: 10"만 출력됨 (Child의 정보 손실)

    return 0;
}
```
<br>
✅ 해결 : 슬라이싱은 값 복사로 인해 발생하므로, **포인터**나 **참조**를 사용해 복사를 방지할 수 있다.

## 다운캐스팅
🔲 부모 클래스에서 자식 클래스 타입으로 변환하는 경우
<br>
🔲 명시적으로 캐스팅해야 하며 **미정의 동작을 유발할 가능성**이 있다.
```cpp
Parent* parent = new Child();
Child* child = (Child*)parent;  // 다운캐스팅
```

<br>
⚠️ 문제 : 다운 캐스팅 시 **미정의 동작**이 발생할 수 있음
```cpp
#include <iostream>
using namespace std;

class Parent {
public:
    virtual void show() {
        cout << "This is Parent" << endl;
    }
};

class Child : public Parent {
public:
    void show() override {
        cout << "This is Child" << endl;
    }
    void childSpecificFunction() {
        cout << "Child-specific function" << endl;
    }
};

int main() {
    Parent parent;
    Parent* parentPtr = &parent;

    // 강제 다운 캐스팅
    Child* childPtr = (Child*)parentPtr;

    // 문제 발생: 부모 객체를 자식 클래스처럼 처리하려 함
    childPtr->childSpecificFunction();  // 런타임 오류 또는 정의되지 않은 동작

    return 0;
}
```
<br>

✅ 해결 : dynamic_cast를 통해 캐스팅을 한다
<br>

# C스타일 캐스팅
🔲 **타입 검사를 수행하지 않아** 안정성이 떨어지며 목적성이 명확하지 않아 가독성이 떨어질 수 있다.<br>
🔲 목적성이 명확한 Cpp스타일의 캐스팅이 권장된다<br>
```cpp
double d = 3.14;
int i = (int)d;  // C 스타일 캐스팅
```
<br>

# C++스타일 캐스팅
## static_cast
🔲 **컴파일타임 타입 검사**를 통해 관련성 있는 타입으로의 형변환을 지원한다.<br>
🔲사용<br>
✅ 기본 데이터 타입 간 변환
```cpp
double d = 3.14;
int i = static_cast<int>(d);  // 소수점 제거
```
✅ 업캐스팅(안전) / 다운캐스팅(안전하지 않음)
```cpp
class Parent {};
class Child : public Parent {};
Parent* p = new Child();
Child* c = static_cast<Child*>(p);  // 다운캐스팅 (안전하지 않을 수 있음)
```
<br>
⚠️ 안전하지 않은 형변환 예시 (child->Parent->child2)
```cpp
class Parent {
    virtual void foo() {}  // 다형성을 위해 가상 함수 선언
};

class Child1 : public Parent {
    int data1;
};

class Child2 : public Parent {
    int data2;
};

int main() {
    Child1 child1;
    Parent* parentPtr = &child1;  // 업캐스팅 (Child1 → Parent)
    Child2* child2Ptr = static_cast<Child2*>(parentPtr);  // 다운캐스팅 (Parent → Child2)
    // 위험: Child1 메모리 레이아웃을 Child2로 간주함
    return 0;
}
```
<br>

## dynamic_cast
🔲 **런타임 타입 검사**를 수행하는 캐스팅.<br>
🔲 주로 다형성을 사용하는 경우 안전한 **다운캐스팅을 위해 사용**한다.<br>
🔲 가상함수 테이블을 참조하기 때문에 **가상함수 테이블이 존재해야 동작한다(RTTI 활성화)**.<br>
🔲 변환 실패 시 nullptr반환(포인터) 또는 예외 발생(참조).<br>
```cpp
class Parent {
    virtual void foo() {}  // 다형성을 위해 가상 함수 선언
};
class Child : public Parent {};
class AnotherChild : public Parent {};

Parent* p = new Child();
Child* c = dynamic_cast<Child*>(p);  // 성공
AnotherChild* ac = dynamic_cast<AnotherChild*>(p);  // 실패, nullptr 반환
```
<br>

## const_cast
🔲 ``const`` 또는 ``volatile``속성을 제거하거나 추가할 떄 사용.<br>
```cpp  
void printMessage(const string& msg) {
    string& modifiableMsg = const_cast<string&>(msg);
    modifiableMsg += " (modified)";
    cout << modifiableMsg << endl;
}

const string message = "Hello";
printMessage(message);  // "Hello (modified)"
```
<br>

## reinterpret_cast
🔲 **타입 검사없는 강제 형변환**을 수행하는 캐스팅.<br>
🔲포인터를 다른 포인터 타입으로 변환.<br>
🔲숫자를 포인터로 변환하거나 반대로 변환.<br>
```cpp  
int a = 65;
char* charPtr = reinterpret_cast<char*>(&a);
cout << *charPtr << endl;  // 'A' 출력 (ASCII 값)

void* voidPtr = &a;
int* intPtr = reinterpret_cast<int*>(voidPtr);  // void* → int*
```