* [Singleton](https://github.com/jihyun-s/Design-pattern/wiki/Design-pattern#singleton-%ED%8C%A8%ED%84%B4)


***

## Singleton 패턴
* 지정한 클래스의 인스턴스가 '절대로' 1개밖에 존재하지 않는 것을 '보증'하고 싶을 때
* 인스턴스가 1개밖에 존재하지 않는 것을 프로그램 상에서 표현하고 싶을 때 


### Singleton 클래스 

Singleton 클래스에서는 인스턴스를 1개 밖에 만들 수 없으며, singleton은 static 필드(클래스 변수)로서 Singleton 클래스의 인스턴스에서 초기화된다. 이 초기화는 Singleton 클래스를 로드할 때 1회만 실행된다. Singleton 클래스의 생성자는 private으로 되어 있다. 이것은 Singleton 클래스 외부에서 생성자의 호출을 금지하기 위해서이다. Singleton 패턴은 프로그래머가 실수를 해도 인스턴스가 1개만 생성되도록 보증하는 패턴으로써 이것을 보증하기 위해 생성자를 private으로 해둘 필요가 있다. 

* _Singleton 클래스 (singleton.java)_
``` 
public class Singleton {
   private static Singleton singleton = new Singleton(); 
   private Singleton() { 
      System.out.println("인스턴스를 생성했습니다.");
   }
   public static Singleton getInstance() {
      return singleton; 
   }
```

### Main 클래스 

Main 클래스는 Singleton 클래스를 이용하는 클래스이다. getInstance 메소드를 사용해서 Singleton 인스턴스를 얻고 있다. 같은 인스턴스가 얻어지고 있는지를 obj1 == obj2 라는 식에서 검사하고 있다. 

* _Singleton 클래스를 이용한 클래스 (Main.java)_
```
public class Main {
   public static void main(String[] args) {
      System.out.println("Start.");
      Singleton obj1 = Singleton.getInstance();
      Singleton obj2 = Singleton.getInstance(); 
      if (obj1 == obj2) {
         System.out.println("obj1과 obj2는 같은 인스턴스입니다.");
      }
      else {
         System.out.println("obj1과 obj2는 다른 인스턴스입니다.");
      }
      System.out.println("End.");
   }
}
```

### 관련 패턴 

* Abstract Factory 패턴
* Builder 패턴 
* Facade 패턴 
* Prototype 패턴 


### Reference 
Java 언어로 배우는 디자인 패턴 입문 
