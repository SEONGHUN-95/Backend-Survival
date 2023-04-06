# Spring과 그의 특징

스프링은 자바 어플리케이션 개발에 필요한 인프라를 제공하는 웹 프레임워크로서 여러 특징을 가지고 있다. 
이 특징들을 통해 객체지향적인 코드, 유지보수가 용이한 코드를 비교적 쉽게 구현해낼 수 있다.
Spring의 특징에 대해 하나씩 알아보자.

## IoC(Inversion of Control)

IoC는 직역하면 제어의 역전 즉, 프로그래머의 코드가 외부 모듈들을 불러와 사용하는 것이 아니라 외부 코드(프레임워크)가 프로그래머의 코드를 호출하여 이용하는 방식을 의미한다.

IoC는 프레임워크의 특징이라고 할 수 있으며 프레임워크는 이 특징을 통해 효율적인 개발 틀을 제공함으로써 이미 구현된 기능을 쉽게 응용할 수 있게 해준다.
특히 스프링에서는 스프링 컨테이너에 의한 객체 생성, 초기화, 의존성 주입, 소멸 등 객체 생명 주기를 담당하고 AOP, 트랜잭션 등이 이루어지는 부분에서 IoC가 나타난다. 

## Dependency Injection

DI, 의존성 주입이란 클래스가 필요한 모듈들을 외부에서 끌어와 결합도를 낮추는 방법이다. A클래스가 B 모듈을 사용한다고 할 때, A가 B에 의존한다고 할 수 있으며 B라는 의존성을 주입하는 방법은 아래와 같다.

생성자를 통한 DI, setter를 통한 DI, 필드(autowired)를 통한 DI 등이 있다.

```

// 생성자를 통한 DI
public class UserServiceImpl implements UserService {

    private UserRepository userRepository;

    public UserServiceImpl(UserRepository userRepository) {
        this.userRepository = userRepository;
    }

    // ...
}

```

```

// setter를 이용한 DI
public class UserServiceImpl implements UserService {

    private UserRepository userRepository;

    public void setUserRepository(UserRepository userRepository) {
        this.userRepository = userRepository;
    }

    // ...
}

```

```

// @Autowired 어노테이션을 이용한 DI
public class UserServiceImpl implements UserService {

    @Autowired
    private UserRepository userRepository;

    // ...
}

```

여러 방법을 통해 DI를 할 수 있으나 생성자 주입 방식이 가장 추천되곤 한다. 그 이유로는 테스트 코드 작성에 유리하고, 객체의 불변성을 확보할 수 있으며 
순환참조 문제나 의존성 주입을 누락했을 때 컴파일 에러 등을 제공한다는 점에 있다.
  
## Spring AOP(Aspect Oriented Programming)

AOP는 객체 지향 프로그래밍의 한 형태로서 애플리케이션 전반에 필요한 기능을 한 곳에 구현하여 이를 여러 부분에 적용하는 프로그래밍 방식이다. 예를 들어, 애플리케이션 곳곳에서 사용되는 로깅, 트랜잭션 처리 등을 공통된 모듈로 구현함으로써 코드의 중복을 제거하고 유지보수성을 향상시킬 수 있다. 애플리케이션 곳곳에서 중복되어 사용되는 기능을 횡단관심사(cross-cutting concern)라고 하며 핵심 개념들에 대해 알아보자.

1. Advice
"언제" 횡단 관심사를 핵심 로직에 적용할 것인지를 정의한다. @Before(메서드 시작 전), @After(시작 후), @Afterthrowing(예외 발생 후) 등의 어노테이션으로 Joint Point에서 실행될 시점을 적용할 수 있다.

2. JointPoint
AOP에서 어드바이스가 적용될 수 있는 특정 시점을 의미한다. 메서드 실행 시점, 예외 발생 시점과 같이 특정 작업이 시작되는 시점 등이 이에 해당한다. 

3. Pointcut
복수의 JointPoint를 모아둔 것으로서 실제로 Advice가 적용되는 Jointpoint들을 의미한다. 즉, Target의 Aspect 적용 시점을 구체적으로 명시해둔 것이다.

4. Aspect
Aspect는 횡단 관심사를 의미한다. 여러 객체에 공통으로 적용되는 공통 관심 사항이다. Advice와 Pointcut을 합친 것을 의미한다.

5. Target
핵심 로직을 구현한 클래스이다. 즉, 충고를 받는 클래스가 타겟이다.

## Spring Bean

스프링에서 관리되는 객체이다. @Bean, @Component, @Autowired 어노테이션을 통해 빈 객체 등록이 가능하다.

## BeanFactory

객체를 생성하고, 조립하는 IoC 컨테이너의 일종으로 ApplicationContext 인터페이스가 이 인터페이스를 상속받는다.

## ApplicationContext

BeanFactory 인터페이스를 구현한 인터페이스로서 bean 인스턴스를 생성, 구성, 관리하는 기능을 가지고 있다. 또한, Bean 간의 의존성 관리, 스코프 지정, AOP 지원, 메시지 처리, 트랜잭션 관리 등의 기능도 구현되어 있다.

```

@Configuration
public class DemoApplication {
	public static void main(String[] args) {
		ApplicationContext context =
			new AnnotationConfigApplicationContext(DemoApplication.class);
		
		System.out.println("-".repeat(80));
		
		PostController controller = context.getBean("postController", PostController.class);
		System.out.println(controller);
		
		ObjectMapper objectMapper = context.getBean("objectMapper", ObjectMapper.class);
		System.out.println(objectMapper);
	}
	
	@Bean
	public PostController postController() {
		System.out.println("Create bean: postController");
		return new PostController(objectMapper());
	}
	
	@Bean
	public ObjectMapper objectMapper() {
		System.out.println("Create bean: objectMapper");
		return new ObjectMapper();
	}
}

```

## 싱글턴 패턴

클래스에 단 하나의 인스턴스만을 생성, 사용하는 객체 생성 패턴을 의미한다. 스프링 빈 객체는 싱글턴으로 사용되어 객체 생성 비용감소, 전역 객체 제공을 한다는 점에서 이점이 있다.