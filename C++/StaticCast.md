C++ 형변환에는 아래의 4가지 종류가 존재한다.
1. Static cast
2. Dynamic cast
3. reinterpret cast
4. const cast
---

<h1> Static cast</h1>

C++에서 제공하는 기능 중 하나로, 프로그래머가 형변환을 할 떄 오류를 체크해 준다. 해당 오류는 코드상에서 체크를 진행한다.

<h2>- 일반 변수의 형변환</h2>

```
int main() {
    double dou = 10.1234;
    int int_1 = dou;                    // 묵시적 형변환
    int int_2 = (int)dou;               // 명시적 형변환
    int int_3 = static_cast<int>(dou);  // static_cast 형변환
}
```
일반적인 변수에서는 모두 부담없이 형변환 가능.
단지, double -> int로 넘어갔기 때문에 소수점들은 버려진다.

<h2>- 포인터에서의 형변환</h2>

```
int main() {
    double* dou = new double(10.1234);
    int* int_1 = dou;                    // 묵시적 형변환
    int* int_2 = (int*)dou;               // 명시적 형변환
    int* int_3 = static_cast<int*>(dou);  // static_cast 형변환
}
```
명시적 형변환은 가능하지만, 묵시적 형변환과 static_cast 형변환은 안된다.
데이터 타입이 맞지 않기 때문에, 코드상에서 오류를 출력.

위와 같이, <span style="color:red"> 명시적 형변환으로 코드를 만들게 된다면, 말도 안되는 결과 값이 호출되어 정상작동 되지 않는다. </span>

<h2>- 상속관계에서의 형변환들</h2>

```
class Animal {
public:
    virtual void sound() = 0;
    void info(){
        std::cout << "동물은 숨을 쉽니다.\n";
    }
};

class Dog : public Animal{
private:
    std::string name;
public:
    Dog(std::string s) : name(s) {};
    void sound() { std::cout << "멍멍\n"; }
    void name_print() { std::cout << name << std::endl; }
    void only_dog() { std::cout << "이건 개 클래스\n"; }
};

class Cat : public Animal {
private:
    std::string name;
public:
    Cat(std::string s) : name(s) {};
    void sound() { std::cout << "냐옹\n"; }
    void name_print() { std::cout << name << std::endl; }
    void data() { std::cout << this << std::endl; }
    void only_cat() { std::cout << "이건 고양이 클래스\n"; }
};

int main() {

    Cat* cat_1 = new Cat("나비");
    Dog* dog_1 = new Dog("멍멍이");
    
    cat_1 = dog_1;  //묵시적 형변환
    cat_1 = (Cat*)dog_1;    //명시적 형변환
    cat_1 = static_cast<Cat*>(dog_1);   //static_cast형변환

    return 0;
}
```
위에서도 마찬가지로, 명시적 형변환만 되는걸 확인할 수 있으며, 결과는 아래와 같다.
```
int main() {

    Cat* cat_1 = new Cat("나비");
    Dog* dog_1 = new Dog("멍멍이");
    
    cat_1 = (Cat*)dog_1;    //명시적 형변환

    cat_1->sound();
    cat_1->only_cat();
    return 0;
}
```
생각한 결과와 동일하지 않으며, static_cast를 통하여 오류를 확인할 수 있다.

<h2>- Static_cast 오류 </h2>

```
#include <iostream>

class Animal {
public:
    virtual void sound() = 0;
    void info(){
        std::cout << "동물은 숨을 쉽니다.\n";
    }
};

class Dog : public Animal{
private:
    std::string name;
public:
    Dog(std::string s) : name(s) {};
    void sound() { std::cout << "멍멍\n"; }
    void name_print() { std::cout << name << std::endl; }
    void only_dog() { std::cout << "이건 개 클래스\n"; }
};

class Cat : public Animal {
private:
    std::string name;
public:
    Cat(std::string s) : name(s) {};
    void sound() { std::cout << "냐옹\n"; }
    void name_print() { std::cout << name << std::endl; }
    void data() { std::cout << this << std::endl; }
    void only_cat() { std::cout << "이건 고양이 클래스\n"; }
};

int main() {

    Animal* ani = new Cat("나비");
    Dog* dog = static_cast<Dog*>(ani);

    dog->sound();
    dog->only_dog();
    return 0;
}
```

상속 관계에서, 오류가 생기는 이유는 아래와 같습니다.
1. 상속관계이므로 포인터 관계상 서로 같은 타입으로 인식하게 된다.
2. 컴파일 타임에 RTTI(Run Time Type Information)을 검사하지 않는다.

static_cast를 사용하였을 경우 위와 같은 오류를 검사하지 않기 때문에 퍼포먼스가 좀 더 나오지만, 일반적인 자료형 형변환에서는 static_cast가 좋지만, 상속 관계에 있을때는 dynmaic_cast를 추천한다.