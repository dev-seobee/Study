std::this_thread 현재 쓰레드에 적용되는 몇가지 도우미 함수들이 존재.

<h2>std::this_thread::sleep_for()</h2>
최소 sleep_duration 만큼의 시간 동안 현재 쓰레드의 실행을 멈춘다.

```
#include <iostream>
#include <chrono>
#include <string>
#include <thread>

void PrintCurrentTime() {
    // 현재 시간 출력
}

void PrintMessage(const std::string& message)
{
    std::cout << "Sleep now ... ";
    PrintCurrentTime();
    
    std::this_thread::sleep_for(std::chrono::seconds(3));

    std::cout << message << " ";
    PrintCurrentTime();
}
int main()
{
    std::thread thread(PrintMessage, "Message from a child thread.");

    PrintMessage("Message from a main thread.");

    thread.join();

    return 0;
}
```

<h2>std::this_thread::yield()</h2>
sleep과 비슷하지만, sleep이 쓰레드를 일시 정지 상태로 바꾸는 반면, yield는 계속 실행 대기 상태를 유지한다.

```
#include <iostream>
#include <chrono>
#include <string>
#include <thread>

void PrintMessage(const std::string& message, std::chrono::seconds delay)
{
    auto end = std::chrono::high_resolution_clock::now() + delay;

    while (std::chrono::high_resolution_clock::now() < end)
    {
        std::this_thread::yield();
    }

    std::cout << message << std::endl;
}
int main()
{
    std::thread thread(PrintMessage, "Message from a child thread.", std::chrono::seconds(3));

    PrintMessage("Message from a main thread.", std::chrono::seconds(2));

    thread.join();

    return 0;
}
```