
기본적인 BaseClassLibrary에 대하여 정리하며, 많이 사용되어지고, 기본적인 부분은 요약하여 넘어가겠습니다.

# 시간처리
## 6.1.1 System.DateTime
```
public DateTime(int year, int month, int day);
public DateTime(int year, int month, int day, int hour, int minute, int second);
public DateTime(int year, int month, int day, int hour, int minute, int second, int millisecond);

// now를 통해 현재 시간을 알아낼 수 있다.
Datetime now = Datetime.now;
```
DateTimeKind
- Unspecified - 어떤 형식인지 지정되지 않은 시간
- Utc - 동시간의 그리니치 천문대 시간
- Local - 시간대를 반영한 지역 시간

## 6.1.2 TimeSpan
시간 계산을 할 때 사용하는 클래스
```
DateTime endOfYear = new DateTime(DateTime.Now.Year, 12, 31);
DateTime now = DateTime.Now;

TimeSpan gap = endOfYear - now;
```

## 6.1.3 StopWatch
시간차 계산을 위해 사용
```
Stopwatch st = new Stopwatch();

st.Start();
SomethingFunction();
st.Stop();
```

# 문자열 처리
## 6.2.1 String
|멤버|유형|설명|
|:---|--|--|
|Contains|인스턴스 메서드|인자로 전달된 문자열을 포함하고 있는지 여부 true/false 반환|
|EndsWith|인스턴스 메서드|인자로 전달된 문자열로 끝나는지 여부를 true/false 반환|
|Format|인스턴스 메서드|형식에 맞는 문자열을 생성해 반환|
|GetHashCode|인스턴스 메서드|문자열의 해시값을 반환|
|IndexOf|인스턴스 메서드|문자 또는 문자열을 포함하는 경우 그 위치를 반환, 없으면 -1|
|Replace|인스턴스 메서드|치환된 문자 반환|
|Split|인스턴스 메서드|특정 문자를 구분자로 나뉜 문자열의 배열|
|StartsWith|인스턴스 메서드|인자로 전달된 문자열로 시작하는지 여부 true/false|
|Substring|인스턴스 메서드|시작과 길이에 해당하는 만큼의 문자열 반환|
|ToLower|인스턴스 메서드|소문자 변환|
|ToUpper|인스턴스 메서드|대문자 변환|
|Trim|인스턴스 메서드|문자열 앞 뒤에 공백문자 제거 반환|
|Length|인스턴스 속성|문자열 길이 반환|
|인덱서[]|인스턴스속성|주어진 정수 위치에 해당하는 문자를 반환

## 6.2.2 StringBuilder
String타입의 경우 불변객체 이기 때문에, String에 대한 변환은 새로운 메모리 할당을 진행.
따라서 StringBuilder를 사용하여 문자열을 만드는게 더 효율적.

## 6.2.3 Encoding
|정적속성|설명|
|:--|--|
|ASCII|7비트 ASCII 문자셋을 위한 인코딩|
|Default|시스템 기본 문자셋을 위한 인코딩|
|Unicode|유니코드 문자셋의 UTF-16 인코딩|
|UTF32|유니코드 문자셋의 UTF-32 인코딩|
|UTF8|유니코드 문자셋의 UTF-8 인코딩|
효율 상 UTF-8 인코딩 방식을 자주 쓰고 있는 추세!
- 한글자가 1~4바이트 중 하나로 인코딩 될 수 있다.
- 가변 바이트 길이를 선언하기 위해 꽤 많은 비트를 잡아먹고도 2,097,151까지 인코딩 할 수 있기 때문에 4바이트만으로도 충분하고 남음

## 6.2.4 System.Text.RegularExpressions.Regex
문자열 처리에 대한 일반적인 규칙을 표현하는 형식 언어.

# 6.3 직렬화/역직렬화

## 6.3.1 System.BitConverter
리틀 엔디안과 빅 엔디안의 바이트 순서 차이 때문에 바이트 배열은 순서가 거꾸로 되어있다.

## 6.3.2 System.IO.MemoryStream
메모리에 바이트 데이터를 순서대로 읽고 쓰는 작업을 수행하는 클래스

## 6.3.3 StreamWriter / StreamReader
Stream에 문자열 데이터를 쓰려면, Encoding 타입을 이용해 바이트 배열로 변환해야하는데,
이 때 사용할 클래스.
```
Memorystream ms = new MemoryStream();

StreamWriter sw = new StreamWriter(ms, Encoding.UTF8)
sw.WriteLine("Hello World");
sw.WriteLine("Anderson");
sw.Write(32000);
sw.Flush();
```

## 6.3.4 BinaryWriter / BinaryReader
2진 데이터를 쓰고 읽는데 특화된 기능을 제공
```
MemoryStream ms = new MemoryStream();
BinaryWriter bw = new BinaryWriter(ms);
bw.Write("Hello world" + Environment.NewLine);
bw.Write("Hello" + Environment.NewLine);
bw.Write(32000);
bw.flush();

BinaryReader br = new BinaryReader(ms);
string first = br.ReadString();
string second = br.ReadString();
int result = br.ReadInt32();
```

사람이 쉽게 읽을 수 있는 데이터를 원할경우, StreamWriter/StreamReader를 사용
가독성이 떨어지더라도 규격이 정해진 데이터를 입출력 원하면 BinaryWriter/BinaryReader 사용

## 6.3.5 BinaryFormatter
사용자 정의 클래스를 직렬화 할 때 사용하는 클래스.
But, 이것을 사용하기 위해선 변환하려는 클래스 위에 [Serializable]을 선언해 주어야 한다.
```
[Serializable]
class Person
{
    /// 생략생략
}

Person person = new Person(36, "이름이당");
BinaryFormatter bf = new BinaryFormatter();

MemoryStream ms = new MemoryStream();
bf.Serialize(ms, person);

Person clone = bf.Deserialize(ms) as Person;
```
2진 데이터로 직렬화 하는 방법.

## 6.3.6 XmlSerializer
클래스의 내용을 문자열로 직렬화 하는 방법.
<b>[Serializeable]도 필요가 없다.</b>

허나, 아래의 제약 사항이 존재
- public 접근 제한자의 클래스여야 한다.
- 기본 생성자를 포함하고 있어야 한다.
- public 접근 제한자가 적용된 필드만 직렬화/역직렬화 한다.

## 6.3.7 DataContractJsonSerializer
BinaryFormatter + XmlSerializer 의 장점을 서로 합친 것
자바스크립트의 객체 직렬화 방식을 닷넷에서 동일하게 구현.
"System.Runtime.Serialization" 을 참조 해야 하며
사용법은 다른 직렬화 방식과 거의 유사.

문자열로 되어있기 때문에 가독성이 높아, 닷넷 이외의 플랫폼에서도 쉽게 데이터를 주고받을 수 있다. 많이 선호하고있는 추세.

# 6.4 컬렉션
정해지지 않은 크기의 배열을 편리하게 구현한 것
- ArrayList
- Hashtable
- SortedList
- Stack
- Queue

# 6.5 파일

# 6.6 스레드
## 6.6.1 System.Threading.Thread
```
static void Main(String[] args)
{
    Thread t = new Thread(threadFunc);
    t.Start();
}

// 이와 같은 방법으로 백그라운드 스레드로 세팅
Thread t = new Thread(threadfunc);
t.IsBackground = true; 
```
- Background와 Foreground 차이점
Foreground 쓰레드는 메인스레드가 종료되더라도 프로세스가 종료되지 않고 계속 실행
Bacground의 경우엔 메인스레드가 종료시 프로세스가 종료됨.

## 6.6.2 System.Threading.Monitor
스레드 동기화 기법 중 하나
```
static void main(String[] args){
    Program pg = inst as Program;
    for(int i = 0; i < 10000; i++)
    {
        Monitor.Enter(pg);
        .
        .
        .
        .
        Monitor.Exit(pg);
    }
}
```
## 6.6.3 System.Threading.Interlocked
lock을 사용하지 않고 ThreadSafe하게 일부 연산을 구현 가능
작업을 원자(atomic)단위로 수행하기 때문에 가능.

## 6.6.4 System.Threading.ThreadPool
사용한 스레드를 없애는 것이 아닌, 풀을 만들어 재사용 가능하게 함
```
MyData data = new MyData();

ThreadPool.QueueUserWorkItem(threadFunc, data);
ThreadPool.QueueUserWorkItem(ThreadFunc, data);
```

## 6.6.5 System.Threading.EventWaitHandle
Monitor 타입 처럼 스레드 동기화 수단 중 하나
스레드로 하여금 이벤트를 기다리게 만들 수 있고 다른 스레드에서 원하는 이벤트를 발생시키는 시나리오에 적합
이벤트는 <b>signal, Non-signal</b> 두가지 상태를 가짐
```
Set : non-signal -> signal
Reset : signal -> non-signal
```

WaitOne 메서드를 통해 이벤트 객체가 signal 상태이면곧바로 제어가 반환되지만,
non-signal 상태일 경우 signal 상태가 될 떄 까지 대기상태로 빠짐.

```
class Program
{
    static void Main(String[] args){

        // non-signal 상태의 이벤트 객체 생성
        // 생성자의 첫번째 인자가 false이면 Non-signal 상태
                                true이면 signal 상태로 시작
        EventWaitHandle weh = new EventWaitHandle(false, EventResetMode.ManualReset);
        Thread t = new Thread(threadFunc);
        t.IsBackground = true;
        t.Start(ewh);

        // Non-Signal 상태에서 waitOne을 호출했으므로 signal 상태로 바뀔 떄 까지 대기
        ewh.WaitOne();
    }

    static void threadFunc(object state){
        EventWaitHandle weh = state as EventWaitHandle;
        Console.WriteLine("60초 후에 프로그램 종료");
        Thread.Sleep(60 * 1000);
        Console.WriteLine("스레드 종료");

        ewh.Set();
    }
}
```

##### as 연산자
형 변환을 위해 탄생한 예약어로, As, Is가 존재한다.
부모 클래스를 자식 클래스에 대입할 때 사용
- As : 형변환이 가능하면 수행, 그렇지 않으면 null
- Is : 형변환이 가능한 여부를 불린형으로 결과값 반환

# 6.6.7 Delegate의 비동기 호출
입출력 장치 뿐 아닌 일반 메서드에 비동기 호출을 할 수 있는 수단.

```
public class Clac
{
    public static long Cumsum(int start, int end)
    {
        long sum = 0;
        for(int i = start; i <= end; i++)
        {
            sum += i;
        }
        return sum;
    }
}

class Program
{
    public delegate long CalcMethod(int start, int end);
    static void Main(string[] args)
    {
        CalcMethod calc = new CalcMethod(Calc.Cumsum);
        long result = calc(1, 100);
        Console.WriteLine(result);
    }
}
```

델리게이트의 BeginInvoke, EndInvoke를 사용하면  calc 인스턴스에 할당된 Calc.Cumsum 메서드의 수행을 ThreadPool의 스레드에서 실행할 수 있다.
```
static void Main(String[] args)
{
    CalcMethod calc = new CalcMethod(Calc.Cumsum);

    // 3번째 인자값이 콜백 메서드.
    IAsyncResult ar = calc.BeginInvoke(1, 100, null, null);

    ar.AsyncWaitHandle.WaitOne();

    // 반환값을 얻기 위해 호출. 반환값이 없어도 EndInvoke를 호출 할 것을 권장.
    long result = calc.EndInvoke(ar);
}
```