---
title: Code convention과 개발자가 지켜야할 수칙에 관하여
---

## Summary
---
 요즘 서버 및 웹클라이언트 환경을 javascript로 대부분 진행하고 있다.
 그러던 도중 최근 한 스타트업의 서버 최적화를 진행할 때 굉장히 문제가 많은 코드를 접하였다.
 거의 난독증이 생길정도의 코드였고, 기본 수칙만 지켜도 나타나지 않을 오류들이 많았다.

 이번에는 node js와 web framework를 개발하는 개발자들이 기본적으로 지켜주었으면 하는 내용을 담아보겠다.

---
### Javascript를 떠나 개발자가 코드를 짤 때 지켜야할 수칙
---
 code를 짜다보면 대충 stackoverflow에서 복붙하고 넘어가고 싶고 그것을 으레 당연하게 여기는 개발자가 많음을 느끼고 있다.
 하지만 이는 개발자 자신의 발전에 커다란 걸림돌이 된다. 또한 그 프로젝트가 점점 진행될수록 복붙한 코드가 발목을 잡을 것이다.
 궁금증이 생겼을 때 구글링으로 stackoverflow나 blog를 중심으로 검색한다면 그 또한 성장하지 못하는 이유가 될 수 있다.

 그렇다면 만일 모르는 문제가 생겼을 때는 어떻게 해야 할까?

 먼저 해당 라이브러리 또는 프레임워크의 reference guide부터 찾아보아야 한다. 
 많은 사람들이 spring framework나 노드 개발자는 expressjs framework를 사용하는데,
 블로그에서 해결했다는 내용을 보고 복붙만 하여 결국에는 엉망으로 변한 코드 구조를 발견할 것이다.

 사실 처음에는 나도 블로그를 통해 기술을 익혔었다. 하지만 실제로 프레임워크나 라이브러리를 적용할 때는 
 블로그의 있는 tutorial수준의 정보만으로는 프로젝트를 진행할 수 없었다. 언제나 reference document를 찾았고,
 반복되서 나타나는 궁금증들은 reference 가이드에 항상 명시되어 있고 best solution을 알려주는 경우도 많았다.

 예를 들자면 블로그나 stackoverflow는 사용법, 해법에 대한 내용이 맞는 경우가 있지만 software는 매달마다
 새로운 버전이 나오는 경우도 많고, 지속적인 update로 인해 문법이나 내용이 바뀌는 경우가 종종 있다.
 이럴 경우에는 난관에 봉착하게 되고, 천천히 마음을 다잡고 살펴봤을 때 버전호환에서 문제가 있었다는 것을
 알아차린다.

 무엇보다도 guide 문서를 기반으로 코드를 진행했을 때 팀원과 협업이라는 것을 원활하게 진행할 수 있다.
 개성있는 코드도 멋있지만, design pattern에 기반하여 가이드 라인을 따라가면 시간이 지날수록
 프로젝트 진행에 속도가 붙게된다. 복붙 코드는 결국에는 복붙한 코드의 수정사항이 발생하면 엄청난 괴로움이 밀려오기 때문이다.

 그렇다면 코드를 짤 때 어떤 생각을 가져야 할까?

 항상 염두해야 할 것은 Cross cutting concern이다.
 일명 '횡단적 관심사항'이다. 정말 중요하게 생각하는 부분이다.
 Spring framework를 사용하는 사람은 한번쯤 들어봤을 것이다. AOP의 개념과도 일맥상통하는데
 이걸 염두하지 않으면 굉장히 수고로운 코드를 작성할 것이다.

 API에 모두 인증이 필요하다면 어떻게 짤 것인가? 세션에 원하는 정보가 없다면? 아니면 토큰이 가진 권한에 대해서 체크한다면?
 모든 request 객체 또는 response 객체에 담아야할 필수적인 내용이 있다면?

 이 모든 것을 해결해줄 방법은 AOP의 개념을 적용하면 쉽게 해결할 수 있을 것이다.
 항상 API 앞단에 필수적으로 실행할 method 또는 function 을 두면 해결될 것이다.
 그리고 이 코드는 하나의 모듈로 따로 관리하고 API의 시작점에서 체크하고 뒷단으로 해결하라고 보내주기만 하면된다.

 node js에서는 이를 middleware라 부르기도하고 app.use()에서 해결할 수 있다. spring은 aspect라고 이름 그대로 존재한다.

 그리고 AOP 뿐만 아니라 중요한 것은 DI 의존성 주입이다. 최근에 이것때문에 많이 힘들었다.
 의뢰를 받은 서버 최적화를 진행하려는데 한곳을 바꾸면 우르르 의존성에 의해 엄청난 오류가 물밀듯이 밀려왔기 때문이다.
 모든 모듈은 강하게 결합되어 있었고 분리라는게 사실상 불가능한 상태였다. 결국은 성능이 필요한 부분만 살려냈다.

 각 모듈은 느슨하게 결합되도록 하고 하나의 모듈이 다른 모듈을 필요로 하고 트리 모형으로 의존성이 있으면 좋지만
 망형식으로 의존성이 걸려 있으면 이제 고생길만 남은 것이다. 이것만은 피해야 한다.

---
### if문 for문만 있으면 다 만들 수 있다. 하지만 그 다음은?
---
 정말 분노하게 만드는 코드가 있다면 if문 안에서 switch문으로 구분하고 case에서 for문을 돌리는데 거기서 if문을 사용하는 것이다.
 실제로 이렇게 코드를 짜는 사람이 있다. 모듈화라는 개념이 필요한 사람이다.

 또한 library는 사용을 안하고 자존심으로 구현해서 사용한다는 사람도 있다. 그래서 자신만이 이해하는 코드를 짜고
 그것을 자랑스럽게 남겨놓는 경우가 있다. 협업과 효율성이라는 개념이 필요한 사람이다.

 최근에는 Javascript로 프로젝트를 진행한다. 모두가 그렇듯 String에 대한 처리가 은근 사람 힘들게 한다. 그냥 if문으로 처리하던지 직접 파싱하고 싶을 때가 많다.
 하지만 이러한 방식은 자중해야 한다. 주석을 아무리 열심히 달아놓아도 이해하기 쉽지 않으며 코드가 지저분하다.

 이것을 해결하는 방법은 아주 간단하다. Useful library라고만 쳐도 빈번하게 사용되는 라이브러리들을 찾아볼 수 있으며, 기본적으로 개발자를 괴롭히는
 여러 문제들은 이미 많은 사람들이 고민해서 library화 해놓았다. 가끔 구현내용이 궁금하면 소스코드를 한 번 열어보면되고
 연습삼아 한번하자. 그런데 직접 작업한 내용을 적용하지 말고, 사용자가 많은 적절한 라이브러리를 공부하여 적용하자.
 그게 팀에 대한 예의이며 추후에 후임자를 괴롭히지 않는 최선이 될 것이다.

---
### 그럼 convention을 왜 중요할까?
---
 Convention을 따라야 할 이유는 뭘까?

 무엇보다도 같이 일할 팀원을 위해서이고 다른 개발자들을 배려하기 위해서이다. Convention은 일종의 관습으로 많은 개발자들이 사용하는 룰이다.
 Class는 대문자로 시작하고, 상수는 대문자로만 작성하고, isArray와 같이 함수는 동사형으로 시작하고, 데이터베이스를 구축할 때 외래키는 '테이블명_id'와 같이 
 일종의 룰들이다. for each문을 사용하기 보다는 for(let i=0; i<length; i++)과 같이 사용하는 것도 하나의 convention이다.

 이런 convention들은 개발자들이 성능을 점검하고 협업을 했을 때 보이는 코드 구조의 일관성을 지키기 위해 존재한다.

 이러한 룰을 지키지 않는 경우에는 추후의 유지보수의 기간에 엄청난 시간을 낭비할 수도 있다.

 각 convention들은 회사마다 조금씩 다르지만 전반적으로 거의 같다고 볼 수 있다.

 또한 naming convention도 잘 따라주면 직관적으로 코드를 이해하기 수월해진다.

 각 언어에 대한 code convention은 아래와 같다.

 - Java
    - [http://www.oracle.com/technetwork/java/javase/documentation/codeconvtoc-136057.html](http://www.oracle.com/technetwork/java/javase/documentation/codeconvtoc-136057.html)
    - [http://www.oracle.com/technetwork/java/codeconventions-150003.pdf](http://www.oracle.com/technetwork/java/codeconventions-150003.pdf)
    - 디자인 패턴 [https://www.tutorialspoint.com/design_pattern/](https://www.tutorialspoint.com/design_pattern/)
 - C 
    - [https://users.ece.cmu.edu/~eno/coding/CCodingStandard.html](https://users.ece.cmu.edu/~eno/coding/CCodingStandard.html)
 - C++
    - [https://users.ece.cmu.edu/~eno/coding/CppCodingStandard.html](https://users.ece.cmu.edu/~eno/coding/CppCodingStandard.html)
    - [Google C++ Style Guide](https://google.github.io/styleguide/cppguide.html)
 - Javascript
    - [https://www.w3schools.com/js/js_conventions.asp](https://www.w3schools.com/js/js_conventions.asp)
    - [https://github.com/airbnb/javascript](https://github.com/airbnb/javascript)
    - [https://google.github.io/styleguide/jsguide.html](https://google.github.io/styleguide/jsguide.html)
 - MySQL
    - [https://www.toadworld.com/platforms/mysql/w/wiki/6103.naming-conventions](https://www.toadworld.com/platforms/mysql/w/wiki/6103.naming-conventions)
    - [https://www.toadworld.com/platforms/mysql/w/wiki/6108.naming-indexes](https://www.toadworld.com/platforms/mysql/w/wiki/6108.naming-indexes)
    - [http://www.sqlstyle.guide/](http://www.sqlstyle.guide/)
 - Oracle
    - [https://oracle-base.com/articles/misc/naming-conventions](https://oracle-base.com/articles/misc/naming-conventions)
 - Linux kernel
    - [https://www.kernel.org/doc/html/v4.10/process/coding-style.html](https://www.kernel.org/doc/html/v4.10/process/coding-style.html)
 - Shell
    - [https://google.github.io/styleguide/shell.xml](https://google.github.io/styleguide/shell.xml)
 - MongoDB
    - [https://docs.mongodb.com/manual/meta/style-guide/](https://docs.mongodb.com/manual/meta/style-guide/)
 - HTML
    - [https://www.w3schools.com/html/html5_syntax.asp](https://www.w3schools.com/html/html5_syntax.asp)



 개인적으로 airbnb의 javascript 스타일 가이드를 통해 전반적인 코드의 가독성과 테크닉을 향상시킬 수 있었다.