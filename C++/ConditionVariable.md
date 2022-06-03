std::condition_variable

다른 스레드가 공유 변수를 수정하고 condition_variable로 통지할 때까지 스레드나 여러 스레드를 대기하도록 하는데 사용할 수 있는 동기화 기법.
언제나 뮤텍스와 연동돼서 동작해 스레드에 안전하게 동작한다.

- CV는  User-Level Object. (커널 오브젝트가 아니다.)

동작순서
- Lock을 잡는다 ( 뮤텍스 획득 )
- 공유 변수 값 수정
- Lock을 푼다 ( 뮤텍스 릴리즈 )
- 조건 변수를 통하여 다른 쓰레드에게 통지


notify_one() : wait 중인 쓰레드가 있을 경우, 하나만 깨움
notify_all() : wait 중인 쓰레드 전체 깨움.