# Design Pattern
* [Singleton](https://github.com/jihyun-s/Design-pattern/tree/main#singleton-%ED%8C%A8%ED%84%B4)
* [Abstract Factory : 추상 팩토리]()
* [Factory Method : 팩토리 메서드]()
* [Reference]()


***

## Singleton 패턴
인스턴스를 한 개만 만들기
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

---
## Abstract Factory 추상 팩토리 
관련 부품을 조합해서 제품 만들기 

### 사용 목적과 용도
* 부품의 구체적인 구현에는 주목하지 않고 인터페이스(API)에 주목하여 인터페이스만을 사용해서 부품을 조립하고 제품으로 완성한다. 
* 하위 클래스 단계에서 구체적인 구현을 실행한다. (구체적인 공장이 등장하고 구체적인 부품을 조립해서 구체적인 제품을 만든다) 

### 클래스 다이어그램 
<img src="https://github.com/jihyun-s/Design-pattern/blob/main/abstract_factory.jpg" width="70%" height="70%" title="Abstract factory"></img>

### 구현 코드 
* Item 클래스 (Item.java) - 추상적인 부품
```
package factory; 

public abstract class Item {
    protected String caption;
    public Item(String caption){
        this.caption = caption;
    }
    public abstract String makeHTML();
}
```


* Link 클래스 (Link.java) - 추상적인 부품
```
package factory;

public abstract class Link extends Item {
    protected String url;
    public Link(String caption, String url) {
        super(caption); 
        this.url = url;
    }
}
```


* Tray 클래스 (Tray.java) - 추상적인 부품 
```
package factory; 
import java.util.ArrayList; 

public abstract class Tray extends Item {
    protected ArrayList tray = new ArrayList();
    public Tray(String caption) {
        super(caption);
    }
    public void add(Item item) {
        tray.add(item);
    }
}
```


* Page 클래스 (Page.java) - 추상적인 부품 
```
package factory; 
import java.io.*;
import java.util.ArrayList; 

public abstract class Page {
    protected String title;
    protected String author; 
    protected ArrayList content = new ArrayList(); 
    public Page(String title, String author) {
        this.title = title;
        this.author = author;
    }
    public void add(Item item) {
        content.add(item);
    }
    public void output() {
        try {
            String filename = title + ".html";
            Writer writer = new FileWriter(filename); 
            Writer.writer(this.makeHTML());
            writer.close(); 
            System.out.println(filename + " 을 작성했습니다.");
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
    public abstract String makeHTML(); 
}
```


* Factory 클래스 (Factory.java) - 추상적인 공장 
    - getFactory 안에서는 Class 클래스의 forName 메소드를 사용해서 그 클래스를 동적으로 읽는다. 그리고 newInstance()를 이용해서 그 클래스의 인스턴스를 한 개 작성한다. (반환값)
```
package factory;

public abstract class Factory {
    public static Factory getFactory(String classname) {
        Factory factory = null;
        try {
            factory = (Factory)Class.forName(classname).newInstance();
        } catch (ClassNotFoundException e) {
            System.err.println("클래스" + classname + "이 발견되지 않습니다.");
        } catch (Exception e) {
            e.printStackTrace();
        }
        return factory;
    }
    public abstract Link createLink(String caption, String url);
    public abstract Tray createTray(String caption); 
    public abstract Page createPage(String title, String author); 
}
```

* Main 클래스 - 공장을 사용해서 부품을 조합하고 제품 만들기 
    - 이 클래스에서는 구체적인 부품, 제품, 공장을 전혀 이용하지 않는다. 
    - 구체적인 공장의 클래스 이름은 커맨드 라인에서 지정하고, 이 인수(arg[0])를 기초로 getFactory에서 공장을 만들고 변수 factory에 대입한다.
```
import factory.*; 

public class Main() {
    public static void main(String[] args) {
        if(args.length != 1) {
            System.out.println("Usage: java Main class.name.of.ConcreteFactory");
            System.out.println("Example 1: java Main listfactory.ListFactory");
            System.exit(0);
        }
        Factory factory = Factory.getFactory(args[0]); 
        
        Link joins = factory.createLink("중앙일보", "http://www.joins.com/");
        Link chosun = factory.createLink("조선일보", "http://www.chosun.com/");
        Link naver = factory.createLink("네이버", "http://www.naver.com/");
        Link google = factory.createLink("구글", "http://www.google.com/");
        
        Tray traynews = factory.createTray("신문"); 
        traynews.add(joins);
        traynews.add(chosun); 
        
        Tray traysearch = factory.createTray("검색엔진");
        traysearch.add(naver);
        traysearch.add(google);
        
        Page page = factory.createPage("LinkPage", "Sophia");
        page.add(traynews);
        page.add(traysearch);
        page.output();
    }
}
```

* ListFactory 클래스 (ListFactory.java) - 구체적인 공장 
```
package listfactory; 
import factory.*; 

public class ListFactory extends Factory {
    public Link createLink(String caption, String url) {
        return new ListLink(caption, url);
    }
    public Tray createTray(String caption) {
        return new ListTray(caption);
    }
    public Page createPage(String title, String author) {
        return new ListPage(title, author);
    }
}
```

* ListLink 클래스 (ListLink.java) - 구체적인 부품
```
package listfacotry; 
import factory.*; 

public class ListLink extends Link {
    public ListLink(String caption, String url) {
        super(caption, url); 
    }
    public String makeHTML() {
        retrun " <li><a href=\"" + url + "\">" + caption + "</a></li>\n";
    }
}
```

* ListTray 클래스 (ListTray.java) - 구체적인 부품 
```
package listfactory;
import factory.*; 
import java.util.Iterator; 

public class ListTray extends Tray {
    public ListTray(String caption) {
        super(caption);
    }
    
    public String makeHTML() {
        StringBuffer buffer = new StringBuffer(); 
        buffer.append("<li>\n");
        buffer.append(caption + "\n");
        buffer.append("<ul>\n");
        Iterator it = tray.iterator(); 
        while(it.hasNext()) {
            Item item = (Item)it.next(); 
            buffer.append(item.makeHTML()); 
        }
        buffer.append("<ul>\n");
        buffer.append("<li>\n");
        return buffer.toString();
    }
}
```

* ListPage 클래스 (ListPage.java) - 구체적인 부품 
```
package listfactory;
import factory.*; 
import java.util.Iterator; 

public class ListPage extends Page() {
    public ListPage extends Page {
        super(title, author); 
    }
    public String makeHTML() {
        StringBuffer buffer = new StringBuffer(); 
        buffer.append("<html><head><title>" + title + "</title></head>\n");
        buffer.append("<body>\n");
        buffer.append("<h1>" + title + "</h1>\n");
        buffer.append("<ul>\n");
        Iterator it = content.iterator();
        while (it.hasNext()) { 
            Item item = (Item)it.next(); 
            buffer.append(item.makeHTML()); 
        }
        buffer.append("</ul>\n");
        buffer.append("<hr><address>" + author + "</address>");
        buffer.append("</body></html>\n");
        return buffer.toString();
    }
}
```
---
## Factory Method 팩토리 메서드 
하위 클래스에서 인스턴스 만들기


### 사용 목적과 용도
* 인스턴스를 만드는 방법을 상위 클래스 측에서 결정하지만 구체적인 클래스 이름까지는 결정하지 않는다. 구체적인 내용은 모두 하위 클래스 측에서 수행한다. (위임)
* 인스턴스 생성을 위한 골격(framework)과 실제의 인스턴스 생성 클래스를 분리해서 생각할 수 있다. 
    

### 클래스 다이어그램 
<img src="https://github.com/jihyun-s/Design-pattern/blob/main/factory_met.jpg" width="50%" height="50%" title="Factory method"></img>

### 구현 코드 
* Product 클래스 (Product.java) 
```
package framework; 

public abstract class Product {
    public abstract void use();
}
```

* Factory 클래스 (Factory.java) 
```
package framework;

public abstract class Factory {
    public final Product create(String owner) {
        Product p = createProduct(owner);
        registerProduct(p);
        return p;
    }
    protected abstract Product createProduct(String owner);
    protected abstract void registerProduct(Product product);
}
```

* IDCard 클래스 (IDCard.java)
```
package idcard;
import framework.*;

public class IDCard extends Product {
    private String owner; 
    IDCard(String owner) {
        System.out.println(owner + "의 카드를 만듭니다."); 
        this.owner = owner;
    }
    public void user() {
        System.out.println(owner + "의 카드를 사용합니다."); 
    }
    public String getOwner() {
        return owner;
    }
}
```

* IDCardFactory 클래스 (IDCardFactory.java) 
```
package idcard; 
import framework.*;
import java.util.*;

public class IDCardFactory extends Factory(){
    private List owners = new ArrayList(); 
    protected Product createProduct(String owner){
        return new IDCard(owner);
    }
    protected void registerProduct(Product product){
        owners.add(((IDCard)product).getOwner());
    }
    public List GetOwners() {
        return owners;
    }
}
```

* Main 클래스 (Main.java)
```
import framework.*;
import idcard.*;

public class Main() {
    public static void main(String[] args){
        Factory factory = new IDCardFactory();
        Product card1 = factory.create("홍길동");
        Product card2 = factory.create("이순신");
        Product card3 = factory.create("강감찬");
        card1.use();
        card2.use();
        card3.use();
    }
}
```

## Abstract Factory & Factory Method 패턴 차이점 
Abstract Factory | Factory Method 
--|--
연관된 객체들의 패밀리 생성을 위한 하나 이상의 팩토리 메소드를 포함 | 객체 생성 메소드를 클라이언트에 노출
다른 클래스의 객체를 만들기 위해 컴포지션(composition)을 사용해 책임을 위임 | 어떤 객체를 생성할지 결정하기 위해 서브클래스와 상속을 사용
연관된 product들의 패밀리를 생성하는 것 | 하나의 product를 생성하기 위해 사용됨

## Reference 
- Java 언어로 배우는 디자인 패턴 입문 
- Chapter 3. 팩토리(Factory) 패턴(https://blog.naver.com/anciid/221793735687)
