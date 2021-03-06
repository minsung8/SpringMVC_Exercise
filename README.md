# SpringMVC_Exercise

MVC (Model-View-Controller)
- Model : 모델은 뷰가 렌더링하는데 필요한 데이터 
- View : 웹 어플리케이션에서 뷰는 실제로 보이는 부분이며 모델을 이용하여 렌더링 => JSP, JSF, PDF, XML 등으로 결과 표현 
- Controller : 컨트롤러는 사용자의 액션에 응답하는 컴포넌트 => 모델 업데이트, 다른 액션 수행 


DispatcherServlet
- Front Controller
- 클라이언트의 모든 요청을 받아 이를 처리할 여러 핸들러들에게 넘기고 여러 핸들러가 처리한 결과를 받아 사용자에게 응답결과를 보여준다.
- 여러 컴포넌트를 이용해 작업 처리
- 내부 동작 흐름 
  1. 사용자 요청 ->  2. HandlerExecutionChain 결정 -> 3. HandlerExecutionChain 실행 -> 4. 뷰 렌더링 -> 5. 사용자 요청 처리

 - 내부 동작 중 요청 선처리 작업
  1. 사용자 요청 ->  2. Locale 결정 -> 3. RequestContextHolder에 요청 저장 -> 4. Flash Map 복원 -> 5. 핸들러 결정과 실행
  - 사용된 컴포넌트
    org.springframework.web.servlet.LocaleResolver
    - 지역 정보를 결정해주는 전략 오브젝트이다.
    - 디폴트인 AcceptHeaderLocalResolver는 HTTP 헤더의 정보를 보고 지역정보를 설정해준다.
    
    org.springframework.web.servlet.FlashMapManager
    - FlashMap객체를 조회(retrieve) & 저장을 위한 인터페이스
    - RedirectAttributes의 addFlashAttribute메소드를 이용해서 저장한다.
    - 리다이렉트 후 조회를 하면 바로 정보는 삭제된다.
    
    org.springframework.web.context.request.RequestContextHolder
    - 일반 빈에서 HttpServletRequest, HttpServletResponse, HttpSession 등을 사용할 수 있도록 한다.
    - 해당 객체를 일반 빈에서 사용하게 되면, Web에 종속적이 될 수 있다.
    
    org.springframework.web.multipart.MultipartResolver
    - 멀티파트 파일 업로드를 처리하는 전략
  
  - 내부 동작 중 요청 전달
   1. 사용자 요청 ->  2. HandlerMapping으로 HandlerExecutionChain 결정 -> 3. HandlerAdapter 결정 -> 4. 요청 처리
    org.springframework.web.servlet.HandlerMapping
    - HandlerMapping구현체는 어떤 핸들러가 요청을 처리할지에 대한 정보를 알고 있다.
    - 디폴트로 설정되는 있는 핸들러매핑은 BeanNameHandlerMapping과 DefaultAnnotationHandlerMapping 2가지가 설정되어 있다.
    
    org.springframework.web.servlet.HandlerExecutionChain
    - HandlerExecutionChain구현체는 실제로 호출된 핸들러에 대한 참조를 가지고 있다.
    - 즉, 무엇이 실행되어야 될지 알고 있는 객체라고 말할 수 있으며, 핸들러 실행전과 실행후에 수행될 HandlerInterceptor도 참조하고 있다.
    
    org.springframework.web.servlet.HandlerAdapter
    - 실제 핸들러를 실행하는 역할을 담당한다.
    - 핸들러 어댑터는 선택된 핸들러를 실행하는 방법과 응답을 ModelAndView로 변화하는 방법에 대해 알고 있다.
    - 디폴트로 설정되어 있는 핸들러어댑터는 HttpRequestHandlerAdapter, SimpleControllerHandlerAdapter, AnnotationMethodHanlderAdapter 3가지이다.
    - @RequestMapping과 @Controller 애노테이션을 통해 정의되는 컨트롤러의 경우 DefaultAnnotationHandlerMapping에 의해 핸들러가 결정되고, 그에 대응되는 AnnotationMethodHandlerAdapter에       의해 호출이 일어난다.

  - 내부 동작 중 요청 처리
   1. 결정된 HandlerExecutionChain ->  2. 핸들러 실행 -> 3. 뷰 렌더링
    org.springframework.web.servlet.ModelAndView
    - ModelAndView는 Controller의 처리 결과를 보여줄 view와 view에서 사용할 값을 전달하는 클래스이다.
    
    org.springframework.web.servlet.RequestToViewNameTranslator
    - 컨트롤러에서 뷰 이름이나 뷰 오브젝트를 제공해주지 않았을 경우 URL과 같은 요청정보를 참고해서 자동으로 뷰 이름을 생성해주는 전략 오브젝트이다. 
     디폴트는 DefaultRequestToViewNameTranslator이다.
    
  - 내부 동작 중 예외 처리 
   org.springframework.web.servlet.handlerexceptionresolver
   - 기본적으로 DispatcherServlet이 DefaultHandlerExceptionResolver를 등록한다.
   - HandlerExceptionResolver는 예외가 던져졌을 때 어떤 핸들러를 실행할 것인지에 대한 정보를 제공한다.

  - 내부 동작 중 뷰 렌더링
     1. ViewResolver로 View 구현체 찾음 ->  2. 뷰 구현체로 렌더링 -> 3. 요청 처리 재게
    org.springframework.web.servlet.ViewResolver
    - 컨트롤러가 리턴한 뷰 이름을 참고해서 적절한 뷰 오브젝트를 찾아주는 로직을 가진 전략 오프젝트이다.
    - 뷰의 종류에 따라 적절한 뷰 리졸버를 추가로 설정해줄 수 있다.
    
  
  
