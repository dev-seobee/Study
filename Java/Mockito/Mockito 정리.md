
# Mock과 Mockbean의 차이
Junit5를 통하여, 유닛테스트를 진행하면서 embedded mongo를 사용해 test를 진행하려 하였는데, 굳이 필요가 없을 것 같아서, 로직 검증만 진행하려 하였다,
Mockito를 사용해서 DB나 다른 외부 서비스를 mock객체를 사용하려 하였으나, 
사용하는데 있어서 Mockito 관련하여 개념 정리가 필요하여 해당 페이지를 만들었다.

<b>가짜 객체가 필요한 이유는</b>, 실제 객체를 만드는데 드는 시간을 절약하고, 의존성의 연결고리가 많이 연결된 경우, 구현의 복잡함을 피하기 위해서 생성한다.

## Mock?
해당 @Mock를 사용하기위해선 @ExtendWith({MockitoExtension.class})를 클래스 상단에 정의해야 한다.
```
@ExtendWith({MockitoExtension.class})
public class SpaceServiceTest {
    private SpaceService spaceService;
}
```
일반적으로 @Mock만 사용하게 되면 해당 객체는 null이기 때문에, ExtendWith 어노테이션을 추가해서 사용할 수 있게 한다.

Service 레이어를 테스트할 때, Repository, DomainService를 가짜 객체로 만드는 용도로 사용될 수 있다 --> <b>해당 경우가 우리가 필요한 경우.</b>

만일, 위와같이 @ExtendWith 어노테이션을 붙히지 않았다면, 아래와 같은 작업이 필요
```
publc class Test(){
    @Before
    public void setUp(){
            MockitoAnnotations.initMocks(this);
    }
}
```

### InjectMock?
의존성을 필요로하는 field에 붙이는 어노테이션, Mock이나 Spy어노테이션이 붙은 필드를 주입 받는다.
@Mock이 붙은 객체를 @InjectMocks이 붙은 객체에 주입할 때 사용.
InjectMock(Service), Mock(DAO) 서비스 테스트 목객체에 DAO 목객체를 주입시켜 사용.

## Mockbean ?
Mock과는 달리 org.springframework.boot.test.mock.mockito 패키지 하위에 존재한다.
(springboot-test에서 제공하는 어노테이션.)
Mockito의 Mock객체들을 Spring의 ApplicationContext에 넣어준다. 
그리고 동일한 타입의 Bean이 존재할 경우 MockBean으로 교체해주는 역할을 함.

@WebMvcTest를 이용한 테스트에서 사용할 수 있다.
Controller를 테스트할 때 주로 이용되며, 단일 클래스의 테스트를 진행하므로 @MockBean을 통해 가짜 객체를 만들어 준다.
<b>Controller 객체까지만 생성되고, Service 객체는 생성되지 않습니다.</b>




