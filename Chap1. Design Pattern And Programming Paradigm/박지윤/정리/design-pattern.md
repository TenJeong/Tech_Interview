## 디자인 패턴

### #️⃣ 디자인 패턴
* **디자인 패턴**이란?      
  * 프로그램을 설계할 때 발생했던 문제점들을 객체 간의 상호 관계 등을 이용하여 해결할 수 있도록 하나의 '규약' 형태로 만들어 놓은 것을 의미한다.

***
***

### 1️⃣ 싱글톤 패턴 (Singleton Pattern)
* 싱글톤 패턴이란?   
  * 하나의 클래스에 오직 하나의 인스턴스만 가지는 패턴이다.
  * 하나의 인스턴스를 만들어 놓고 해당 인스턴스를 다른 모듈들이 공유하며 사용한다.
  * 보통 데이터베이스 연결 모듈에 많이 사용한다.
* 장점
  * 인스턴스를 생성할 때 드는 비용이 줄어든다.
  * 사용하기 쉽고 실용적이다.
* 단점
  * 의존성이 높아진다.


* 자바에서의 싱글톤 패턴
```
class Singleton {
    private static class singleInstanceHolder {
        private static final Singleton INSTANCE = new Singleton();
    }
    public static Singleton getInstance() {
        return singleInstanceHolder.INSTANCE;
    }
}

public class HelloWorld {
    public static void main(String[] args) {
        Singleton a = Singleton.getInstance();
        Singleton b = Singleton.getInstance();
        System.out.println(a.hashCode());
        System.out.println(b.hashCode());
        if(a == b) {
            System.out.println(true);
        }
    }
}
```

***

* 싱글톤 패턴의 단점
  * TDD를 하기 쉽지 않다.
    * TDD(Test Driven Development)를 할 때 단위 테스트를 주로 하는데, 단위테스트는 테스트가 서로 독립적이어야 하며 테스트를 어떤 순서로든 실행할 수 있어야 한다.
    * 하지만 싱글톤 패턴은 미리 생성된 하나의 인스턴스를 기반으로 구현하는 패턴이므로 각 테스트마다 독립적인 인스턴스를 만들기가 어렵다.
  * 모듈 간의 결합을 강하게 만든다.

***

* 의존성 주입 (DI, Dependency Injection)
  * 싱글톤 패턴의 단점인 '모듈 간의 강한 결합'을 해소할 수 있는 방법이다.
  * 의존성이란?
    * 종속성이라고도 한다.
    * A가 B에 의존성이 있다 ➡ B의 변경 사항에 대해 A 또한 변해야 한다
    

  * ![img.png](img.png)
  * 위의 그림처럼 메인 모듈이 '직접' 다른 하위 모듈에 대한 의존성을 주기보다는 중간에 **의존성 주입자**가 이 부분을 가로채 메인 모듈이 '간접'적으로 의존성을 주입하는 방식이다.
  * 이를 통해 메인 모듈(상위 모듈)은 하위 모듈에 대한 의존성이 떨어지게 되고, 디커플링 된다고도 말한다.


  * 의존성 주입의 장점
    * 모듈들을 쉽게 교체할 수 있는 구조가 되어 테스팅 및 마이그레이션 하기 쉽다.
    * 구현 시 추상화 레이어를 넣고 이를 기반으로 구현체를 넣어 주기에,
      * 애플리케이션 의존성 방향이 일관된다.
      * 애플리케이션을 쉽게 추론할 수 있다.
      * 모듈 간의 관계들이 조금 더 명확해진다.
  

  * 의존성 주입의 단점
    * 모듈들이 더욱 분리되므로 클래스 수가 늘어난다.
      * 복잡성이 증가될 수 있다.
      * 런타임 페널티가 생길 수 있다.
      

  * 의존성 주입 원칙
    * 상위 모듈은 하위 모듈에서 어떠한 것도 가져오지 않아야 한다.
    * 상위 모듈과 하위 모듈은 모두 추상화에 의존해야 하며, 이때 추상화는 세부 사항에 의존하지 말아야 한다.
  

***
***

### 2️⃣ 팩토리 패턴 (Factory Pattern)
* 팩토리 패턴이란?
  * 객체를 사용하는 코드에서 객체 생성 부분을 떼어내 추상화된 패턴이자 상속 관계에 있는 두 클래스에서 상위 클래스가 중요한 뼈대를 결정하고, 하위 클래스에서 객체 생성에 관한 구체적인 내용을 결정하는 패턴이다.
  * 상위 클래스와 하위 클래스가 분리된다.
    * 느슨한 결합을 갖는다.
    * 상위 클래스에서는 인스턴스 생성 방식에 대해 전혀 알 필요가 없기에 더 많은 유연성을 갖는다.
  * 객체 생성 로직이 따로 떼어져 있기 때문에 유지 보수성이 증가된다.


* 자바의 팩토리 패턴
```
abstract class Coffee {
  public abstract int getPrice();
  
  @Override
  public String toString() {
    return "Hi this coffee is " + this.getPrice();
  }
}

class CoffeeFactory {
  public static Coffee getCoffee(String type, int price) {
    if ("Latte".equalsIgnoreCase(type))
      return new Latte(price);
    else if ("Americano".equalsIgnoreCase(type))
      return new Americano(price);
    else {
      return new DefaultCoffee();
    }
  }
}

class DefaultCoffee extends Coffee {
  private int price;
  
  public DefaultCoffee() {
    this.price = -1;
  }
  
  @Override
  public int getPrice() {
    return this.price;
  }
}

class Latte extends Coffee {
  private int price;
  
  public Latte(int price) {
    this.price = price;
  }
  
  @Override
  public int getPrice() {
    return this.price;
  }
}

class Americano extends Coffee {
  private int price;
  
  public Americano(int price) {
    this.price = price;
  }
  
  @Override
  public int getPrice() {
    return this.price;
  }
}

public class HelloWorld {
  public static void main(String[] args) {
    Coffee latte = CoffeeFactory.getCoffee("Latte", 4000);
    Coffee ame = CoffeeFactory.getCoffee("Americano", 3000);
    System.out.println("Factory latte ::" + latte);
    System.out.println("Factory ame ::" + ame);
  }
} 
```

***
***

### 3️⃣ 전략 패턴 (Strategy Pattern)
* 전략 패턴이란?
  * 객체의 행위를 바꾸고 싶은 경우 '직접' 수정하지 않고 전략이라고 부르는 '캡슐화된 알고리즘'을 컨텍스트 안에서 바꿔주면서 상호 교체가 가능하게 만드는 패턴이다.
  * 컨텍스트 - 프로그래밍에서의 컨텍스트는 상황, 맥락, 문맥을 의미하며 개발자가 어떠한 작업을 완료하는 데 필요한 모든 관련 정보를 말한다.


* 자바의 전략 패턴
```
import java.text.DecimalFormat;
import java.util.ArrayList;
import java.util.List;

interface PaymentStrategy { 
    public void pay(int amount);
}

class KAKAOCardStrategy implements PaymentStrategy {
    private String name;
    private String cardNumber;
    private String cvv;
    private String dateOfExpiry;
    
    public KAKAOCardStrategy(String nm, String ccNum, String cvv, String expiryDate) {
        this.name = nm;
        this.cardNumber = ccNum;
        this.cvv = cvv;
        this.dateOfExpiry = expiryDate;
    }

    @Override
    public void pay(int amount) {
        System.out.println(amount + " paid using KAKAOCard.");
    }
}

class LUNACardStrategy implements PaymentStrategy {
    private String emailId;
    private String password;
    
    public LUNACardStrategy(String email, String pwd) {
        this.emailId = email;
        this.password = pwd;
    }
    
    @Override
    public void pay(int amount) {
        System.out.println(amount + " paid using LUNACard.");
    }
}

class Item { 
    private String name;
    private int price; 
    public Item(String name, int cost) {
        this.name = name;
        this.price = cost;
    }

    public String getName() {
        return name;
    }

    public int getPrice() {
        return price;
    }
} 

class ShoppingCart { 
    List<Item> items;
    
    public ShoppingCart() {
        this.items = new ArrayList<Item>();
    }
    
    public void addItem(Item item) {
        this.items.add(item);
    }
    
    public void removeItem(Item item) {
        this.items.remove(item);
    }
    
    public int calculateTotal() {
        int sum = 0;
        for (Item item : items) {
            sum += item.getPrice();
        }
        return sum;
    }
    
    public void pay(PaymentStrategy paymentMethod) {
        int amount = calculateTotal();
        paymentMethod.pay(amount);
    }
}  
 
public class HelloWorld {
    public static void main(String[] args) {
        ShoppingCart cart = new ShoppingCart();
        
        Item A = new Item("kundolA", 100);
        Item B = new Item("kundolB", 300);
        
        cart.addItem(A);
        cart.addItem(B);
        
        // pay by LUNACard
        cart.pay(new LUNACardStrategy("kundol@example.com","pukubababo"));
        
        // pay by KAKAOCard
        cart.pay(new KAKAOCardStrategy("Ju hongchul", "123456789","123","12/01"));
    }
}
```
➡ 위의 코드는 쇼핑 카트에 아이템을 담아 *결제 방식*의 전략만 바꿔서 LUNACard 또는 KAKAOCard라는 두 개의 전략으로 결제하는 코드이다.

***
***

### 4️⃣ 옵저버 패턴 (Observer Pattern)
* 옵저버 패턴이란?
  * 주체가 어떤 객체(subject)의 상태 변화를 관찰하다가 상태 변화가 있을 때마다 메서드 등을 통해 옵저버 목록에 있는 옵저버들에게 변화를 알려주는 패턴이다.
    * 주체 - 객체의 상태 변화를 보고 있는 관찰자
    * 옵저버 - 이 객체의 상태 변화에 따라 전달되는 메서드 등을 기반으로 추가 변화 사항이 생기는 객체들
  * 주체와 객체를 따로 두지 않고 상태가 변경되는 객체를 기반으로 구축하기도 한다.


  * 옵저버 패턴을 활용한 서비스
    * 트위터 (어떤 사람인 주체를 팔로우했다면 주체가 포스팅을 올릴 경우 알람이 팔로워들인 옵저버들에게 간다)
    * 주로 이벤트 기반 시스템에 사용된다.
    * MVC(Model-View-Controller) 패턴에도 사용된다.
      * ![img_1.png](img_1.png)
      * ex | 주체라고 볼 수 있는 모델에서 변경 사항이 생겨 update 메서드로 옵저버인 뷰에게 알려주고 이를 기반으로 컨트롤러 등이 작동한다.
      
  
* 자바에서의 옵저버 패턴
```
import java.util.ArrayList;
import java.util.List;

interface Subject {
    public void register(Observer obj);
    public void unregister(Observer obj);
    public void notifyObservers();
    public Object getUpdate(Observer obj);
}

interface Observer {
    public void update(); 
}

class Topic implements Subject {
    private List<Observer> observers;
    private String message;

    public Topic() {
        this.observers = new ArrayList<>();
        this.message = "";
    }

    @Override
    public void register(Observer obj) {
        if (!observers.contains(obj)) observers.add(obj); 
    }

    @Override
    public void unregister(Observer obj) {
        observers.remove(obj); 
    }

    @Override
    public void notifyObservers() {
        this.observers.forEach(Observer::update); 
    }
    
    @Override
    public Object getUpdate(Observer obj) {
        return this.message;
    } 
    
    public void postMessage(String msg) {
        System.out.println("Message sended to Topic: " + msg);
        this.message = msg; 
        notifyObservers();
    }
}

class TopicSubscriber implements Observer {
    private String name;
    private Subject topic;

    public TopicSubscriber(String name, Subject topic) {
        this.name = name;
        this.topic = topic;
    }

    @Override
    public void update() {
        String msg = (String) topic.getUpdate(this); 
        System.out.println(name + ":: got message >> " + msg); 
    } 
}

public class HelloWorld { 
    public static void main(String[] args) {
        Topic topic = new Topic(); 
        Observer a = new TopicSubscriber("a", topic);
        Observer b = new TopicSubscriber("b", topic);
        Observer c = new TopicSubscriber("c", topic);
        topic.register(a);
        topic.register(b);
        topic.register(c);
        
        topic.postMessage("amumu is op champion!!"); 
    }
}
```
➡ topic을 기반으로 옵저버 패턴을 구현하였으며, 여기서 topic은 주체이자 객체가 된다.  
➡ class Topic implements Subject를 통해 Subject interface를 구현했다.  
➡ Observer a = new TopicSubscriber("a", topic);으로 옵저버를 선언할 때 해당 이름과 어떠한 토픽의 옵저버가 될 것인지를 정했다.


* 참고) 자바의 상속과 구현
  * 상속 (extends)
    * 자식 클래스가 부모 클래스의 메서드 등을 상속받아 사용한다.
    * 자식 클래스에서 부모 클래스의 메서드를 추가 및 확장할 수 있다.
    * 재사용성, 중복성의 최소화가 이루어진다.
  * 구현 (implements)
    * 부모 인터페이스를 자식 클래스에서 재정의하여 구현하는 것을 말한다.
    * 상속과 달리 반드시 부모 클래스의 메서드를 재정의하여 구현해야 한다.
    

* 프록시 객체를 이용한 옵저버 구현
  * 프록시 객체를 통해 옵저버 패턴을 구현할 수도 있다.
  * 프록시 객체를 통해 객체의 속성이나 메서드 변화 등을 감지하고 이를 미리 설정해 놓은 옵저버들에게 전달하는 방법으로 구현한다.

***
***

### 5️⃣ 프록시 패턴과 프록시 서버 (Proxy Pattern And Proxy Server)
* 프록시 패턴이란?
  * 대상 객체(subject)에 접근하기 전 그 접근에 대한 흐름을 가로채 대상 객체 앞단의 인터페이스 역할을 한다.
  * 이를 통해 객체의 속성, 변환 등을 보완하며 보안, 데이터 검증, 캐싱, 로깅에 사용한다.
  * 프록시 서버에서의 캐싱
    * 캐시 안에 정보를 담아두고 캐시 안에 있는 정보를 요구하는 요청에 대해 캐시 안에 있는 데이터를 활용하는 것을 말한다.
    * 불필요하게 외부와 연결하지 않기 때문에 트래픽을 줄일 수 있다.


* 프록시 서버란?
  * 서버와 클라이언트 사이에서 클라이언트가 자신을 통해 다른 네트워크 서비스에 간접적으로 접속할 수 있게 해주는 컴퓨터 시스템이나 응용 프로그램을 가리킨다.
  * nginx
    * ![img_3.png](img_3.png)
    * 비동기 이벤트 기반의 구조와 다수의 연결을 효과적으로 처리 가능한 웹서버이다.
    * 주로 Node.js 서버 앞단의 프록시 서버로 활용된다.
    * Node.js 서버를 구축할 때 앞단에 nginx를 둠으로써 익명 사용자의 직접적인 서버로의 접근을 차단하여 보안성을 강화할 수 있다.
  * CloudFlare
    * ![img_4.png](img_4.png)
    * 전세계적으로 분산된 서버가 있고 이를 통해 어떠한 시스템의 콘텐츠 전달을 빠르게 할 수 있는 CDN 서비스이다.
    * CloudFlare를 통해 DDOS 공격 방어, HTTPS 구축 등 공격자로부터 보호 가능하다.
      * DDOS 공격 방어
        * 짧은 기간 동안 네트워크에 많은 요청을 보내 네트워크를 마비시켜 웹 사이트의 가용성을 방해하는 사이버 공격 유형이다.
        * CloudFlare는 사용자가 접속하는 것이 아닌 시스템을 통해 오는 트래픽 등의 의심스러운 트래픽을 자동으로 차단해서 DDOS 공격으로부터 보호한다.
      * HTTPS 구축
        * 서버에서 HTTPS를 구축할 때 인증서를 기반으로 구축할 수도 있지만 CloudFlare를 사용하면 별도의 인증서 설치 없이 좀 더 손쉽게 HTTPS를 구축할 수 있다.
  * CORS와 프론트엔드의 프록시 서버
    * CORS(Cross-Origin Resource Sharing)
      * 서버가 웹 브라우저에서 리소스를 로드할 때 다른 오리진을 통해 로드하지 못하게 하는 HTTP 헤더 기반 매커니즘이다.
      * CORS 에러를 해결하기 위해 프론트엔드에서 프록시 서버를 만들기도 한다.


***
***

### 6️⃣ 이터레이터 패턴 (Iterator Pattern)
* 이터레이터 패턴이란?
  * 이터레이터를 사용하여 컬렉션의 요소들에 접근하는 패턴이다.
  * 이터레이터라는 하나의 인터페이스로 순회가 가능하다.
* 이터레이터 프로토콜 - 이터러블한 객체들을 순회할 때 쓰이는 규칙
* 이터레이터 객체 - 반복 가능한 객체로 배열을 일반화한 객체


***
***

### 7️⃣ 노출모듈 패턴 (Revealing Module Pattern)
* 노출모듈 패턴이란?
  * 즉시 실행 함수를 통해 private, public 같은 접근 제어자를 만드는 패턴을 말한다.
* public - 클래스에서 정의된 함수에서 접근 가능하며 자식 클래스와 외부 클래스에서 접근 가능한 범위
* protected - 클래스에서 정의된 함수에서 접근 가능하며 자식 클래스와 접근 가능하지만 외부 클래스에서 접근 불가능한 범위
* private - 클래스에서 정의된 함수에서 접근 가능하지만 자식 클래스와 외부 클래스에서 접근 불가능한 범위
* 즉시 실행 함수 - 함수를 정의하자마자 바로 호출하는 함수. 초기화 코드, 라이브러리 내 전역 변수의 충돌 방지 등에 사용한다.


***
***

### 8️⃣ MVC 패턴
* MVC 패턴이란?
  * ![img_5.png](img_5.png)
  * 모델(Model), 뷰(View), 컨트롤러(Controller)로 이루어진 디자인 패턴이다.
  * 애플리케이션의 구성요소를 세 가지 역할로 구분하여 개발 프로세스에서 각각의 구성요소에만 집중해서 개발할 수 있다.
  * 재사용성과 확장성이 용이하다.
  * 애플리케이션이 복잡해질수록 모델과 뷰의 관계가 복잡해진다.


* 모델 (Model)
  * 애플리케이션의 데이터인 데이터베이스, 상수, 변수 등을 뜻한다.
  * 뷰에서 데이터를 생성하거나 수정하면 컨트롤러를 통해 모델을 생성하거나 갱신한다.
* 뷰 (View)
  * inputbox, checkbox, textarea 등 사용자 인터페이스 요소를 나타낸다.
  * 모델을 기반으로 사용자가 볼 수 있는 화면을 뜻한다.
  * 모델이 가지고 있는 정보를 따로 저장하지 않아야 하며 단순히 화면에 표시하는 정보만 가지고 있어야 한다.
  * 변경이 일어나면 컨트롤러에 이를 전달해야 한다.
* 컨트롤러 (Controller)
  * 하나 이상의 모델과 하나 이상의 뷰를 잇는 역할을 하며, 이벤트 등 메인 로직을 담당한다.
  * 모델과 뷰의 생명 주기를 관리한다.
  * 모델이나 뷰의 변경 통지를 받으면 이를 해석하여 각각의 구성 요소에 해당 내용에 대해 알려준다.

  
***
***

### 9️⃣ MVP 패턴
* MVP 패턴이란?
  * ![img_6.png](img_6.png)
  * MVC 패턴으로부터 파생되었으며 MVC에서 C에 해당하는 컨트롤러가 프레젠터(presentor)로 교체된 패턴이다.
  * 뷰와 프레젠터는 일대일 관계이기 때문에 MVC 패턴보다 더 강한 결합을 가진다.


***
***

### 🔟 MVVM 패턴
* MVVM 패턴이란?
  * ![img_7.png](img_7.png)
  * MVC의 C에 해당하는 컨트롤러가 뷰를 더 추상화한 계층인 뷰모델(view model)로 바뀐 패턴이다.
  * MVVM 패턴은 MVC 패턴과 다르게 커맨드와 양방향 데이터 바인딩을 가진다.
    * 커맨드 - 여러 가지 요소에 대한 처리를 하나의 액션으로 처리할 수 있게 하는 기법
    * 데이터 바인딩 - 화면에 보이는 데이터와 웹 브라우저의 메모리 데이터를 일치시키는 기법으로, 뷰모델을 변경하면 뷰가 변경된다.
  * UI를 별도의 코드 수정 없이 재사용할 수 있고, 단위 테스팅 하기 쉽다.

  
