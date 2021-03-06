**Spring Boot에서 Cors 설정 전역 변수로 하기**

Cors 허용 설정하는 방법은 크게 2가지가 있습니다. 

1) 어노테이션을 이용하는 방법

2) WebMvcConfigurer 인터페이스를 이용하여 Global로 적용하는 방법

그중에서도 저는 2) 방법을 사용하였습니다.

main 클래스가 있는 패키지 하위에 WebConfig 클래스 생성을 합니다. 

**1) 어노테이션을 이용하는 방법**

```java

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpSession;

import org.springframework.web.bind.annotation.CrossOrigin;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
@CrossOrigin(origins = "*", allowedHeaders = "*")
public class TestController {
	
	@CrossOrigin(origins = "*", allowedHeaders = "*")
	@GetMapping("/public-key")
	public void getPublicKey(HttpServletRequest req) {
		HttpSession session = req.getSession();
		System.out.println(session.getId());
	}
	
	@CrossOrigin(origins = "*", allowedHeaders = "*")
	@PostMapping("/auth")
	public void auth(HttpServletRequest req) {
		HttpSession session = req.getSession();
		System.out.println(session.getId());
	}

}

```

**2) WebMvcConfigurer 인터페이스를 이용하여 Global로 적용하는 방법**

```java
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.CorsRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

@Configuration
public class WebConfig implements WebMvcConfigurer {

	@Override
	public void addCorsMappings(CorsRegistry registry) {
		registry.addMapping("/**").allowedOrigins("*");
	}

}

```

