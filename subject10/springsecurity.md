# Spring Security

스프링 시큐리티란 스프링 프레임워크에서 제공하는 웹 어플리케이션의 보안 처리를 쉽게 구현할 수 있게 지원하는 보안 프레임워크이다. 인증, 인가 기능을 제공하고 폼 기반 인증, OAuth2.0 인증 등 다양한 인증 기능을 제공한다. 기본적으로 Filter chain을 사용하여 보안 처리를 사용한다.

> Spring Security PasswordEncoder란?
암호화된 비밀번호를 생성하거나 저장된 암호화된 비밀번호를 검증하기 위한 인터페이스이다. 이 인코더를 통해 사용자 비밀번호를 안전하게 저장하고 인증할 수 있다. encode()와 match() 메서드가 있으며 encode()는 주어진 문자열을 암호화하여 인코딩된 문자열을 반환하고, matches()는 주어진 평문 문자열과 인코딩된 문자열이 일치하는지를 검사한다. Spring Security에는 이 인터페이스를 구현한 다양한 구현체들이 있으며 필요에 따라 선택하여 사용할 수 있다.

```
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;
import org.springframework.security.crypto.password.PasswordEncoder;

public class PasswordEncoderExample {

    public static void main(String[] args) {
        // BCryptPasswordEncoder 생성
        PasswordEncoder passwordEncoder = new BCryptPasswordEncoder();

        // 패스워드 암호화
        String plainPassword = "myPassword";
        String encodedPassword = passwordEncoder.encode(plainPassword);

        System.out.println("Plain Password: " + plainPassword);
        System.out.println("Encoded Password: " + encodedPassword);

        // 패스워드 검증
        boolean isMatch = passwordEncoder.matches(plainPassword, encodedPassword);
        System.out.println("Password Match: " + isMatch);
    }
}

```

## SecurityFilterChain

SecurityFilterChain란 서버에게 전달된 HTTP 요청에 대해 보안에 관련된(인증, 권한, 세션 관리 등) 처리들을 하는 필터들의 목록이다. 

>`@Configuration` 과 `@EnableWebSecurity` 그리고 `@Secured`
@Configuration은 해당 클래스가 스프링에서 설정 클래스임을 의미하며 애플리케이션을 실행할 때 해당 클래스가 실행되면서 애플리케이션 설정정보가 애플리케이션에 적용된다. @EnableWebSecurity는 WebSecurityConfigurerAdapter 클래스를 상속받은 클래스에서 사용되며 스프링 시큐리티를 활성화하는 어노테이션이다.
@Secured는 인자로 받은 권한을 가진 사용자만이 접근할 수 있는 권한을 부여해주는 애너테이션이다. 예컨대, 클래스나 메서드 위에 @Secured("ROLE_ADMIN")과 같은 애너테이션이 붙었다면 이는 관리자 권한을 가진 사용자만이 접근할 수 있는, 인가 단계 작동이 이루어진다.

@Configuration이 붙은 설정 정보를 통해 HttpSecurity 클래스가 실제 필터들을 생성한다. 이 필터들을 WebSecurity 클래스에 전달하면 WebSecurity는 또 FilterChainProxy에 생성자의 인자로 전달하고 그 결과로 FilterChainProxy는 설정 클래스별 필터 목록을 갖게 된다.
한편, 사용자의 인증 요청을 받은 서블릿 필터인 DelegatingFilterProxy는 이 요청들을 FilterChainProxy 클래스에게 위임하고 FilterChainProxy는 각 Filter들에게 순서대로 요청들을 넘긴다. 보안 필터들은 Bean으로 등록되고, 이 연쇄적인 필터들을 FilterChain이라고 하며 대표적인 필터들의 기능은 다음과 같다.

### SecurityContextPersistenceFilter

SecurityContextRepository에서 SecurityContext를 불러오거나 생성하는 역할을 하는 필터이다. 만약 세션에 기존의 SecurityContext가 없으면 생성하여 SecurityContextHolder 안에 저장하고 다음 필터를 실행하며 있다면 SecurityContext를 꺼내와 SecurityContextHolder에 저장한다. 이 때 SecurityContext는 현재 인증된 사용자의 보안 상태가 저장되어 있는 공간으로, 사용자의 인증 정보를 저장하고 유지할 수 있다. 

### UsernamePasswordAuthenticationFilter

인증 객체를 만들어 아이디와 패스워드를 저장하고 AuthenticationManager에게 인증처리를 맡긴다. 그럼 이를 또 AuthenticationProvider라는 검증 클래스에 인증 처리를 위임하고 인증 처리가 완료되면 이 결과를 인증 객체에 저장하여 Security Context로 넘긴다.

### ConcurrentSessionFilter

사용자 계정으로 인증을 받은 사용자가 두 명 이상일 때 실행되는 동시 세션 필터로서 이미 로그인을 했는데 다른 사람이 동일 계정으로 로그인을 시도할 때 동작한다. 이 필터는 매 요청마다 현재 사용자의 세션 만료 여부를 확인하고 세션이 만료되면 해당 사용자를 로그아웃 시킨다.

## OncePerRequestFilter

OncePerRequestFilter는 이름대로 요청당 필터 체인 내에서 한 번만의 필터링이 이루어지게 하는 추상 클래스이다. 중복 인증을 방지해야하거나 토큰을 생성할 때 등 사용할 수 있으며 내부의 doFilterInternal 메서드를 구현하여 사용할 수 있다. 