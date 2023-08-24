## Chapter 1: 디자인 패턴과 프로그래밍 패러다임
### Section 1.1: 디자인 패턴
- **디자인 패턴**: 프로그램을 설계할 때 발생했던 문제점들을 객체 간의 상호 관계등을 이용하여 해결할 수 있도록 하나의 '규약'형태로 만들어 놓을 것을 의미한다.
#### 1.1.1 싱글톤 패턴
- **싱글톤 패턴(Singleton pattern)**: 하나의 클래스에 오직 하나의 인스턴스만 가지는 패턴이다. 보통 데이터베이스 연결 모듈에 많이 사용한다.
- 하나의 인스턴스를 만들어 놓고 해당 인스턴스를 다른 모듈에 공유하며 사용한다.
- 인스턴스를 생성할 때 드는 비용이 줄어들지만, 의존성이 높아진다.
- 예시
  - 자바스크립트에서는 리터럴 {}또는 new Object로 객체를 생성하게 되면 다른 어떤 객체와도 같지 않기 때문에 이 자체만으로 싱글톤 패턴을 구현할 수 있다.
  - 아래의 obj와 obj2는 다른 인스턴스를 가진다.
```
const obj = {
    a: 27
}
const obj2 = {
    a: 27
}
console.log(obj == obj2)
// false
```
  - 아래의 코드는 Singleton.instance라는 하나의 인스턴스를 가지는 Singleton 클래스를 구현하여 a와 b가 같은 하나의 인스턴스를 가지도록 하였다.
```
class Singleton{
    constructor(){
        if(!Singleton.instance){
            Singleton.instance = this
        }
        return Singleton.instance
    }
    getInstance(){
        return this.instance
    }
}
const a = new Singleton()
const b = new Singleton()
console.log(a === b) // true
```
  - 아래의 데이터베이스 연결과 같이 비용이 높은 작업을 위한 모듈에 많이 사용한다.
```
const URL = 'mongodb://localhost:27017/kundolapp' 
const createConnection = url => ({"url" : url})    
class DB {
    constructor(url) {
        if (!DB.instance) { 
            DB.instance = createConnection(url)
        }
        return DB.instance
    }
    connect() {
        return this.instance
    }
}
const a = new DB(URL)
const b = new DB(URL) 
console.log(a === b) // true
```
  - 데이터베이스 연결에 싱글톤 패턴이 적용되면 인스턴스 생성 비용을 아낄 수 있다.
  - 자바에서의 싱글톤 패턴 예시
```
class Singleton {
    private static class singleInstanceHolder {
        private static final Singleton INSTANCE = new Singleton();
    }
    public static Singleton getInstance() {
        return singleInstanceHolder.INSTANCE;
    }
}

public class HelloWorld{ 
     public static void main(String []args){ 
        Singleton a = Singleton.getInstance(); 
        Singleton b = Singleton.getInstance(); 
        System.out.println(a.hashCode());
        System.out.println(b.hashCode());  
        if (a == b){
         System.out.println(true); 
        } 
     }
}
/*
705927765
705927765
true
*/

/*
1. 클래스안에 클래스(Holder), static이며 중첩된 클래스인 singleInstanceHolder를 
기반으로 객체를 선언했기 때문에 한 번만 로드되므로 싱글톤 클래스의 인스턴스는 애플리케이션 당 하나만 존재하며 
클래스가 두 번 로드되지 않기 때문에 두 스레드가 동일한 JVM에서 2개의 인스턴스를 생성할 수 없습니다. 
그렇기 때문에 동기화, 즉 synchronized를 신경쓰지 않아도 됩니다. 
2. final 키워드를 통해서 read only 즉, 다시 값이 할당되지 않도록 했습니다.
3. 중첩클래스 Holder로 만들었기 때문에 싱글톤 클래스가 로드될 때 클래스가 메모리에 로드되지 않고 
어떠한 모듈에서 getInstance()메서드가 호출할 때 싱글톤 객체를 최초로 생성 및 리턴하게 됩니다.
*/
```
- mongoose의 싱글톤 패턴
  - Node.js에서 MongoDB를 연결할 때 쓰는 mongoose 모듈에서 볼 수 있다.
  - mongoose의 connect()함수는 싱글톤 인스턴스를 반환한다.
```
Mongoose.prototype.connect = function (uri, options, callback) {
    const _mongoose = this instanceof Mongoose ? this : mongoose;
    const conn = _mongoose.connection;
    return _mongoose._promiseOrCallback(callback, cb => {
        conn.openUri(uri, options, err => {
            if (err != null) {
                return cb(err);
            }
            return cb(null, _mongoose);
        });
    });
};
```
- MySQL의 싱글톤 패턴
  - Node.js에서 MySQL을 연결할 때도 싱글톤 패턴이 사용된다.
```
// 메인 모듈
const mysql = require('mysql');
const pool = mysql.createPool({
    connectionLimit: 10,
    host: 'example.org',
    user: 'kundol',
    password: 'secret',
    database: '승철이디비'
});
pool.connect();
// 모듈 A
pool.query(query, function (error, results, fields) {
    if (error) throw error;
    console.log('The solution is: ', results[0].solution);
});
// 모듈 B
pool.query(query, function (error, results, fields) {
    if (error) throw error;
    console.log('The solution is: ', results[0].solution);
});
```
- 싱글톤 패턴의 단점
  - TDD(Test Driven Development)를 할 때는 단위 테스트를 주로 하는데, 단위 테스트는 테스트가 서로 독립적이어야 하며 테스트를 어떤 순서로든 실행할 수 있어야 합니다.
  - 싱글톤 패턴은 미리 생성된 하나의 인스턴스를 기반으로 구현하는 패턴이므로 각 테스트마다 '독립적인' 인스턴스를 만들기가 어렵다.

- 의존성 주입
  - 싱글톤 패턴은 사용하기가 쉽고 굉장히 실용적이지만 모듈 간의 결합을 강하게 만들 수 있다는 단점이 있다.
  - 이때 의존성 주입 (DI: Dependency Injection)을 통해 모듈간의 결합을 조금 더 느슨하게 만들어 해결한다.
  - 의존성(종속성): A가 B에 의존성이 있다는 것은 B의 변경 사항에 A 또한 변해야 된다는 것을 의미한다.
  - 메인 모듈이 직접 다른 하위 모듈에 대한 의존성을 주기보다는 중간에 의존성 주입자가 이 부분을 가로채 메인 모듈이 간접적으로 의존성을 주입하는 방식이다. 이를 통해 메인 모듈(상위 모듈)은 하위 모듈에 대한 의존성이 떨어지게 된다. 이를 '디커플링이 된다'라고도 한다.

- 의존성 주입의 장점
  - 모듈들을 쉽게 교체할 수 있는 구조가 되어 테스팅하기가 쉽고 마이그레이션하기도 수월하다.
  - 구현할 때 추상화 레이어를 넣고 이를 기반으로 구현체를 넣어 주기 때문에 애플리케이션 의존성 방향이 일관되고, 애플리케이션을 쉽게 추론할 수 있으며, 모듈 간의 관계들이 조금 더 명확해진다.

- 의존성 주입의 단점
  - 모듈들이 더욱더 분리되므로 클래스 수가 늘어나 복잡성이 증가될 수 있으며 약간의 런타임 페널티가 생기기도 한다.

- 의존성 주입 원칙
  - "상위 모듈은 하위 모듈에서 어떠한 것도 가져오지 않아야 한다. 또한 둘 다 추상화에 의존해야 하며, 이때 추상화는 세부 사항에 의존하지 말아야 한다."
#### 1.1.2 팩토리 패턴
- **팩토리 패턴(Factory pattern)**: 객체를 사용하는 코드에서 객체 생성 부분을 떼어내 추상화한 패턴이자 상속 관계에 있는 두 클래스에서 상위 클래스가 중요한 뼈대를 결정하고, 하위 클래스에서 객체 생성에 관한 구체적인 내용을 결정하는 패턴이다.
- 상위 클래스와 하위 클래스가 분리되기 때문에 느슨한 결합을 가지며 상위 클래스에서는 인스턴스 생성 방식에 대해 전혀 알 필요가 없기 때문에 더 많은 유연성을 갖게 된다.
- 객체 생성 로직이 따로 떼어져 있기 때문에 코드를 리팩터링하더라도 한 곳만 고칠 수 있게 되니 유지 보수성이 증가된다.
- 자바스크립트의 팩토리 패턴
  - 자바스크립트에서 팩토리 패턴을 구현한다면 간단하게 new Object()로 구현할 수 있다.
  - 정적 메서드는 클래스의 인스턴스 없이 호출이 가능하여 메모리를 절약할 수 있고 개별 인스턴스에 묶이지 않으며 클래스 내의 함수를 정의할 수 있다는 장점이 있다.
#### 1.1.3 전략 패턴
- **전략 패턴(Strategy pattern)** 은 정책 패턴(policy pattern)이라고도 하며, 객체의 행위를 바꾸고 싶은 경우 '직접' 수정하지 않고 전략이라고 부르는 '캡슐화한 알고리즘'을 컨텍스트 안에서 바꿔주면서 상호 교체가 가능하게 만드는 패턴이다.
- 컨텍스트: 상황, 맥락, 문랙을 의미하며 개발자가 어떠한 작업을 완료하는 데 필요한 모든 관련 정보를 말한다.
- 예시로 인증 모듈을 구현할 때 쓰는 미들웨어 라이브러리인 'passport'가 있다
#### 1.1.4 옵저버 패턴
- **옵저버 패턴(observer pattern)**: 주체가 어떤 객체의 상태 변화를 관찰하다가 상태 변화가 있을 때마다 메서드 등을 통해 옵저버 목록에 있는 옵저버들에게 변화를 알려주는 패턴이다.
- 주체와 객체가 분리되어 있을 수도, 합쳐져 있을 수도 있다.
- 옵저버 패턴은 주로 이벤트 기반 시스템에 사용하며 MVC(Model-View-Controller) 패턴에도 사용된다.
- cf)
  - 상속: 자식 클래스가 부모 클래스의 메서드 등을 상속받아 사용하며 자식 클래스에서 추가 및 확장을 할 수 있는 것을 말한다. 이로 인해 재사용성, 중복성의 최소화가 이루어진다.
  - 구현: 부모 인터페이스를 자식 클래스에서 재정의하여 구현하는 것을 말하며, 상속과는 달리 반드시 부모 클래스의 메서드를 재정의하여 구현해야한다.
  - 상속과 구현의 차이: 상속은 일반 클래스, abstract 클래스를 기반으로 구현하며, 구현은 인터페이스를 기반으로 구현한다.
- 자바스크립트에서의 옵저버 패턴
  - 자바스크립트에서는 프록시 객체를 통해 구현할 수 있다.
  - 프록시 객체: 어떠한 대상의 기본적인 동작 (속성 접근, 할당, 순회, 열거, 함수 호출 등)의 작업을 가로챌 수 있는 객체를 뜻하며, 자바스크립트에서 프록시 객체는 두 개의 매개변수를 가진다.
    - target: 프록시 대상, handler: target 동작을 가로채서 정의할 동작들이 정해져 있는 함수
  - Vue.js 3.0에서 ref나 reactive로 정의하면 해당 값이 변경되었을 때 DOM에서도 값이 바뀌는데, 이는 프록시 객체를 이용한 옵저버 패턴으로 구현한 것이다.
#### 1.1.5 프록시 패턴과 프록시 서버
##### 프록시 패턴
- **프록시 패턴(proxy pattern)**: 대상 객체에 접근하기 전 그 접근에 대한 흐름을 가로체 대상 객체 앞단의 인터페이스 역할을 하는 디자인 패턴이다.
- 객체의 속성, 변환 등을 보완하며 보안, 데이터 검증, 캐싱, 로깅에 사용한다.
- 프록시 객체로 쓰이기도 하지만 프록시 서버로도 활용된다.
- cf) 프록시 서버에서의 캐싱: 캐시 안에 정보를 담아두고, 캐시 안에 있는 정보를 요구하는 요청에 대해 멀리 있는 원격 서버에 요청하지 않고 캐시 안에 있는 데이터를 활용하는 것을 말한다. 이를 통해 불필요하게 외부와 연결하지 않기 때문에 트래픽을 줄일 수 있다.
##### 프록시 서버
- **프록시 서버(proxy server)**: 서버와 클라이언트 사이에서 클라이언트가 자신을 통해 다른 네트워크 서비스에 간접적으로 접속할 수 있게 해주는 컴퓨터 시스템이나 응용 프로그램을 가리킨다.
- nginx는 비동기 이벤트 기반의 구조와 다수의 연결을 효과적으로 처리 가능한 웹서버이며, 주로 Node.js 서버 앞단의 프록시 서버로 활용된다. -> 익명 사용자의 직접적인 서버로의 접근을 차단하고 간접적으로 한 단계를 더 거침으로써 보안성을 더욱 강화할 수 있다. 실제 포트를 숨기거나, 정적 자원을 gzip 압축하거나, 메인 서버 앞단에서의 로깅을 할 수도 있다.
- CloudFlare는 전 세계적으로 분산된 서버가 있고 이를 통해 어떠한 시스템의 콘텐츠 전달을 빠르게 할 수 있는 CDN 서비스이다. CloudFlare를 프록시 서버로 사용하면 DDOS 공격 방어, HTTPS 구축 등을 수행할 수 있다.
##### CORS와 프론트엔드의 프록시 서버
- **CORS(Cross-Origin Resource Sharing)**: 서버가 웹 브라우저에서 리소스를 로드할 때 다른 오리진을 통해 로드하지 못하게 하는 HTTP 헤더 기반 메커니즘이다.
- 프런트엔드 서버를 분리해서 개발하고 백엔드 서버와 통신할 때 주로 CORS 에러를 마주하는데, 이를 해결하기 위해 프런트엔드에 프록시 서버를 만들기도 한다.
- cf) 오리진: 프로토콜과 호스트 이름, 포트의 조합
#### 1.1.6 이터레이터 패턴
- **이터레이터 패턴(itertaor pattern)**: 이터레이터를 사용하여 컬렉션의 요소들에 접근하는 디자인 패턴이다.
- (순회할 수 있는) 여러 가지의 자료형의 구조와는 상관없이 하나의 인터페이스(이터레이터)로 순회할 수 있다.
- 이터레이터 프로토콜: 이터러블한 객체들을 순회할 때 쓰이는 규칙
- 이터러블한 객체: 반복 가능한 객체로 배영을 일반화한 객체
#### 1.1.7 노출모듈 패턴
- **노출모듈 패턴(revealing module pattenrn)**: 즉시 실행 함수를 통해 private, public 같은 접근 제어자를 만드는 패턴을 말한다.
- 자바스크립트에 경우 접근 제어자가 존재하지 않고 전역 범위에서 스크립트가 실행된다. 따라서 노출모듈 패턴을 통해 private와 public 접근 제어자를 구현하기도 한다.
#### 1.1.8 MVC 패턴
- MVC패턴은 모델(Model), 뷰(View), 컨트롤러(Controller)로 이루어진 디자인 패턴이다.
- 애플리케이션의 구성 요소를 세 가지 역할로 구분하여 개발 프로세스에서 각각의 구성 요소에만 집중해서 개발할 수 있다.
- 재사용성과 확장성이 용이하다는 장점이 있으며, 애플리케이션이 복잡해질수록 모델과 뷰의 관계가 복잡해지는 단점이 있다.
##### 모델
- 모델은 애플리케이션의 데이터인 데이터베이스, 상수, 변수 등을 뜻한다.
- 뷰에서 데이터를 생성하거나 수정하면 컨트롤러를 통해 모델을 생성하거나 갱신한다.
##### 뷰
- 뷰는 사용자 인터페이스 요소를 나타낸다. 즉, 모델을 기반으로 사용자가 볼 수 있는 화면을 뜻한다.
- 모델이 가지고 있는 정보를 따로 저장하지 않아야 하며 단순히 화면에 표시하는 정보만 가지고 있어야 한다.
- 변경이 일어나면 컨트롤러에 이를 전달해야 한다.
##### 컨트롤러
- 하나 이상의 모델과 하나 이상의 뷰를 잇는 다리 역할을 하며 이벤트 등 메인 로직을 담당한다.
- 모델과 뷰의 생명주기도 관리하며, 모델이나 뷰의 변경 통지를 받으면 이를 해석하여 각각의 구성 요소에 해당 내용에 대해 알려준다.
- e.g. React.js -> 가상 DOM으로 실제 DOM을 추상화하였다. 대표적인 특성 immutable.
#### 1.1.9 MVP 패턴
- MVP 패턴은 MVC 패턴으로부터 파생되어 컨트롤러 대신 프레젠터(presenter)로 교체된 패턴이다.
- 뷰와 프레젠터는 일대일 관계이기 때문에 MVC 패턴보다 더 강한 결합을 지녔다.
#### 1.1.10 MVVM 패턴
- MVVM 패턴은 MVC 패턴에서 컨트롤러 대신 뷰모델(View Model)로 바뀐 패턴이다.
- 뷰 모델은 뷰를 더 추상화한 계층이다
- MVC와 다르게 MVVM 패턴은 커맨드와 데이터 바인딩을 가지는 것이 특징이다.
- 뷰와 뷰모델 사이의 양방향 데이터 바인딩을 지원하며 UI를 별도의 코드 수정 없이 재사용할 수 있고 단위 테스팅하기 쉽다는 장점이 있다.
- e.g. Vue.js -> 반응형(reactivity)이 특정인 프레임워크, 값 대입만으로도 변수가 변경되며 양방향 바인딩, html을 토대로 컴포넌트를 구축할 수 있다.