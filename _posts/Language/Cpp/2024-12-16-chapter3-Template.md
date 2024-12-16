---
title: "항목03. 템플릿"
categories: 
    - Cpp
tags: [Cpp]

toc: true
toc_sticky: true

date: 2024-12-16
last_modified_at: 2024-12-16
---

# 템플릿

## 개념
🔲 일반화 프로그래밍의 도구로서 동일한 코드를 여러 데이터 타입에 따라 재사용할 수 있게 도와준다.  

## 동작
🔲 컴파일 타임에 해당 템플릿을 사용하는 타입들에 대한 인스턴스를 생성

✅ 사용 코드
```cpp
template <typename T>
T Add(T a, T b) { return a + b; }

int main()
{
    int iRes = Add<int>(1,1);
    float fRes = Add<float>(1.f,1.f);

    return 0;
}
```

✅ 템플릿 화
```cpp
int Add(int a, int b) { return a + b;}
float Add(float a, float b) { return a + b;}
```

## 함수 템플릿
🔲 여러 타입에서 동작하는 함수 정의
```cpp
template <typename T>
T Add(T a, T b) { return a + b; }
```

## 클래스 템플릿
🔲 여러 타입에 독립적인 클래스를 정의
```cpp
template <typename T>
class Widget
{
public:
    Widget() = default;

    void Set(const T& _value) { m_data = _value;}
    T Get() { return m_data; }

private:
    T m_data;
}
```

## 템플릿 특수화
🔲 특정 타입에 대해서 템플릿의 기본 정의와 다른 동작을 제공
### 완전 특수화
✅ 특정 데이터 타입에 대해 템플릿의 동작을 완전히 재정의 
```cpp
#include <iostream>
using namespace std;

template<typename T>
void Print(T _value) const { cout << "Generic Template : " << _value << '\n'; }

template<>
void Print(float _value) const { cout << "Float Case : " << _value << '\n';}

int main()
{
    Print(10); // Generic Template : 10
    
    Print(10.f); // Float Case : 10

    return 0;
}

```

### 부분 특수화
✅ 클래스 템플릿에서 일부 템플릿 매개변수를 특정화 하거나 조건을 설정하는 방식으로 **함수 템플릿에서는 사용할 수 없음**  
<br>
✅ 기본 클래스 템플릿
```cpp
template <typename T1, typename T2>
class Pair {
private:
    T1 first;
    T2 second;
public:
    Pair(T1 f, T2 s) : first(f), second(s) {}
    void print() const {
        cout << "Generic Pair: " << first << ", " << second << endl;
    }
};
```
<br>
✅ 부분 특수화 : 두 매개변수가 같은 타입일 경우
```cpp
template <typename T>
class Pair<T, T> {
private:
    T first;
    T second;
public:
    Pair(T f, T s) : first(f), second(s) {}
    void print() const {
        cout << "Specialized Pair for same types: " << first << ", " << second << endl;
    }
};
```
<br>
✅ 사용 예제
```cpp
#include <iostream>
using namespace std;

template <typename T>
class Pair; // 전방 선언

int main() {
    Pair<int, double> p1(1, 2.5);  // 일반 템플릿
    Pair<int, int> p2(1, 2);       // 부분 특수화된 템플릿

    p1.print();  // 출력: Generic Pair: 1, 2.5
    p2.print();  // 출력: Specialized Pair for same types: 1, 2

    return 0;
}
```

## 템플릿 비대화
🔲 템플릿의 과도한 인스턴스화로 인해 목적 코드의 크기가 커지는 문제

### 해결방법
1️⃣ 템플릿 코드의 크기 자체를 줄이기  
<br>
✅ 매개변수에 독립적인 코드를 외부로 빼 템플릿 코드 자체를 줄인다, ex)Pimple패턴  

2️⃣ 비타입 인수 사용 자제  
<br>
✅ int, float같은 비타입 인수는 무한하게 생성할 수 있으므로 꼭 필요한 경우가 아니라면 자제하는 것이 방법일 것이다.

3️⃣ 템플릿 메타 프로그래밍의 남용 금지  
<br>
✅ 남용 예시 : Factorial을 Tmp로 구현
```cpp
template <unsigned int N>
struct Factorial {
    static const unsigned int value = N * Factorial<N-1>::value;
};

template <>
struct Factorial<0> {
    static const unsigned int value = 1;
};
```