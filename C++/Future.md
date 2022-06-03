- deferred: lazy evaluation. 지연해서 실행.
- async: 별도의 쓰레드를 만들어서 실행
- defferd | async 둘 중 알아서 골라준다.


객체 대상도 아래와 같이 사용할 수 있다.
- Async
  
  원하는 함수를 비동기적으로 실행.
```
class Knight
{
    public:
        int64 GetHp() { return 100; }
};

Knight knight;
std::future<int64> future = std::async(std::launch::async, &Knight::GetHp, knight);
```

future 자체는 언젠가 미래에 결과를 뱉어줄꺼야 라는 의미를 가짐.

- Promise
  
promise를 이용하여 만들 수 있음.
결과물을 promise를 통해 future로 받아준다

```
std::promise<string> promise;
std::future<string> future = promise.get_future();

다른 쓰레드에서 promise에 세팅을 진행할 경우
get_future()함수를 호출하여 해당 데이터를 가져올 수 있다.
```

- std::packaged_task
원하는 함수의 실행 결과를 packaged_task를 통해 future로 받아줌.
```
void TaskWorker(std::packaged_task<int64(void)> &&task)
{
    task();
}

//<아웃풋(인풋)>
std::packaged_task<int64(void)> task(Calculate);
std::future<int64> future = task.get_future();

std::thread t(TaskWorker, std::move(task))
```


위와 같이 mutex, condition_variable 까지 가지않고 단순한 애들을 처리할 수 있다.
단, 일회성으로 사용하는 애들의 경우 사용에 용이.