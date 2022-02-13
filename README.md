# Design Pattern
* [Singleton](https://github.com/jihyun-s/Design-pattern/tree/main#singleton-%ED%8C%A8%ED%84%B4)
* [Abstract Factory](https://github.com/jihyun-s/Design-pattern/blob/main/README.md#abstract-factory-%EC%B6%94%EC%83%81-%ED%8C%A9%ED%86%A0%EB%A6%AC)
* [Factory Method](https://github.com/jihyun-s/Design-pattern/blob/main/README.md#factory-method-%ED%8C%A9%ED%86%A0%EB%A6%AC-%EB%A9%94%EC%84%9C%EB%93%9C)
* [Prototype](https://github.com/jihyun-s/Design-pattern#prototype-%ED%94%84%EB%A1%9C%ED%86%A0%ED%83%80%EC%9E%85)
* [Builder](https://github.com/jihyun-s/Design-pattern#builder-%EB%B9%8C%EB%8D%94)
* [Adapter](https://github.com/jihyun-s/Design-pattern#adapter-%EC%96%B4%EB%8C%91%ED%84%B0)
* [Bridge](https://github.com/jihyun-s/Design-pattern#bridge-%EB%B8%8C%EB%A6%BF%EC%A7%80)
* [Composite](https://github.com/jihyun-s/Design-pattern#composite)
* [Decorator](https://github.com/jihyun-s/Design-pattern#decorator)
* [Facade](https://github.com/jihyun-s/Design-pattern/blob/main/README.md#facade) 
* [Flyweight](https://github.com/jihyun-s/Design-pattern/blob/main/README.md#flyweight)
* [Proxy](https://github.com/jihyun-s/Design-pattern#proxy)
* [Chain of Responsibility]() 
* [Command]()
* [Interpreter]()
* [Iterator]()
* [Reference](https://github.com/jihyun-s/Design-pattern/blob/main/README.md#reference)


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



***
## Prototype 프로토타입
복사해서 인스턴스 만들기


### 사용 목적과 용도
클래스 이름을 지정하지 않고 인스턴스를 생성할 때도 있다. 다음와 같은 경우에는 클래스로부터 인스턴스를 만드느 것이 아니라 인스턴스를 복사해서 새로운 인스턴스를 만든다. 
* 종류가 너무 많아 클래스로 정리되지 않는 경우 
* 클래스로부터 인스턴스 생성이 어려운 경우 
* framework와 생성할 인스턴스를 분리하고 싶은 경우


### 클래스 다이어그램
<img src="https://github.com/jihyun-s/Design-pattern/blob/main/Prototype.jpg" width="50%" height="50%" title="Prototype"></img>


### 구현 코드
패키지 | 이름 | 용도
--|--|--
framework | Product | 추상 메소드 use와 createClone이 선언되어 있는 인터페이스
framework | Manager | createClone을 사용해서 인스턴스를 복제하는 클래스 
Anonymous | MessageBox | 문자열을 테두리로 표시하는 클래스. use와 createClone을 구현
Anonymous | UnderlinePen | 문자열에 밑줄을 표시하는 클래스. use와 createClone을 구현
Anonymous | Main | 동작 테스트용 클래스 


* Product 인터페이스 (Product.java) 
```
package framework; 

public interface Product extends Cloneable {
    public abstract void use(String s);
    public abstract Product createClone(); 
}
```

* Manager 클래스 (Manager.java) 
```
package framework;
import java.util.*;

public class Manager {
    private HashMap showcase = new HashMap();
    public void register(String name, Product proto) {
        showcase.put(name, proto);
    }
    public Product create(String protoname) {
        Product p = (Product)showcase.get(protoname);
        return p.createClone();
    }
}
```

* MessageBox 클래스 (MessageBox.java) 
```
import framework.*;

public class MessageBox implements Product {
    private char decochar; 
    public MesesageBox(char decochar) {
        this.decochar = decochar;
    }
    public void use(String s) {
        int length = s.getBytes().length;
        for (int i=0; i<length+4; i++)
            System.out.print(decochar);
        System.out.println(" "); 
        System.out.println(decochar + " " + s + " " + decochar);
        for (int i=0; i<length+4; i++)
            System.out.print(decochar);
        System.out.println(" "); 
    }
    public Product createClone() {
        Product p = null; 
        try {
            p = (Product)clone();
        } catch (CloneNotSupportedException e) {
            e.printStackTree();
        }
        return p;
    }
}
```


* UnderlinePen 클래스 (UnderlinePen.java)
```
import framework.*;

public class UnderlinePen implements Product {
    private char ulchar; 
    public UnderlinePen(char ulchar) {
        this.ulchar = ulchar;
    }
    public void use(String s) {
        int length = s.getBytes().length;
        System.out.println("\"" + s + "\"");
        System.out.println(" "); 
        for(int i=0; i<length; i++) 
            System.out.print(ulchar);
        System.out.println(" "); 
    }
    public Product createClone() {
        Product p = null; 
        try {
            p = (Product)clone();
        } catch (CloneNotSupportedException e) {
            e.printStackTree();
        }
        return p;
    }
}
```


* Main 클래스 (Main.java) 
```
import framework.*; 

public class Main {
    public static void main(String[] args) {
        // 준비
        Manager manager = new Manager();
        UnderlinePen upen = new UnderlinePen('~');
        MessageBox mbox = new MessageBox('-');
        MessageBox sbox = new MessageBox('\'); 
        manager.register("strong message", upen); 
        manager.register("warning box", mbox);
        manager.register("slash box", sbox);
        
        // 생성 
        Product p1 = manager.create("strong message");
        p1.use("Hello, world.");
        Product p2 = manager.create("warning box");
        p2.use("Hello, world.");
        Product p3 = manager.create("slash box");
        p3.use("Hello, world.");
    }
}
```


+ clone 메소드는 피상적인 복사(shallow copy)를 실행한다. 참조만 복사될 뿐이고 배열의 요소 하나하나가 복사되는 것은 아니다. 또한 복사를 할 뿐이며 생성자를 호출하지 않는다.

***
## Builder 빌더 
복잡한 인스턴스 조립하기 

### 사용 목적과 용도
* 복잡한 구조를 가지고 있는 경우 한 번에 완성시키기 어렵기 때문에 전체를 구성하고 있는 각 부분을 만들고 단계를 밟아 만들어 나간다. 
* 구조를 가진 인스턴스를 쌓아 올리는데 그 과정의 상세한 사항이 Director 역할에 의해 감춰진다.


### 클래스 다이어그램
<img src="https://github.com/jihyun-s/Design-pattern/blob/main/Builder.jpg" width="70%" height="70%" title="Builder"></img>


### 구현 코드
이름 | 용도 
--|--
Builder | 문서를 구성하기 위한 메소드를 결정하는 추상 클래스
Director | 한 개의 문서를 만드는 클래스
TextBuilder | 일반 텍스트(보통의 문자열)를 이용해서 문서를 만드는 클래스
HTMLBuilder | HTML 파일을 이용해서 문서를 만드는 클래스
Main | 동작 테스트용 클래스 


* Builder 클래스 (Builder.java) - 문서를 만들 메소드들을 선언하고 있는 추상 클래스
```
public abstract class Builder {
    public abstract void makeTitle(String title); 
    public abstract void makeString(String str);
    public abstract void makeItems(String[] items);
    public abstract void close();
}
```


* Director 클래스 (Director.java) 
```
public class Director {
    private Builder builder;
    public Director(Builder builder) {    // Builder의 하위 클래스의 인스턴스가 주어지므로 
        this.builder = builder;           // builder 필드에 저장해 둔다.
    }
    public void construct() {                // 문서구축
        builder.makeTitle("Greeting");       // 타이틀
        builder.makeString("아침과 낮에");   // 문자열
        builder.makeItems(new String[]{      // 개별 항목 
            "좋은 아침입니다.",
            "안녕하세요.",
        });
        builder.makeString("밤에");          // 별도의 문자열
        builder.makeItems(new String[]{      // 별도의 개별 항목 
            "안녕하세요.",
            "안녕히주무세요.",
            "안녕히계세요.",
        )};
        builder.close();                     // 문서를 완성시킨다
    }
}
```


* TextBuilder 클래스 (TextBuilder.java) 
```
public class TextBuilder extends Builder {
    private StringBuffer buffer = new StringBuffer();       // 필드의 문서를 구축한다. 
    public void makeTitle(String title) {                   // 일반 텍스트의 제목 
        buffer.append("================================\n");   // 장식선
        buffer.append("=" + title + "=\n");                 // == 사용한 제목 
        buffer.append("\n");                                // 빈 행 
    }
    public void makeString(String str) {                    // 일반 텍스트에서의 문자열
        buffer.append("-" + str + "\n");                    // - 사용한 문자열 
        buffer.append("\n");                                // 빈 행 
    }
    public void makeItems(String[] items) {                 // 일반 텍스트에서의 개별항목 
        for (int i=0; i<items.length; i++) 
            buffer.append(" . " + itmes[i] + "\n");         // . 사용한 문자열 
        buffer.append("\n");                                // 빈 행 
    }
    public void close() {                                   // 문서의 완성
        buffer.append("================================\n");   // 장식선
    }
    public String getResult() {                             // 완성한 문서
        return buffer.toString();                           // StringBuffer를 String으로 변환
    }
}
```


* HTMLBuilder 클래스 (HTMLBuilder.java) 
```
import java.io.*;

public class HTMLBuilder extends Builder {
    private String filename;                                // 작성할 파일명 
    private PrintWriter writer;                             // 파일에 기술할 PrintWriter 
    public void makeTitle(String title) {                   // HTML 파일에서의 제목 
        filename = tilte + ".html";                         // 타이틀을 파일명으로 결정 
        try {                                               // PrintWriter를 만든다.
            writer = new PrintWriter(new FileWriter(filename));
        } catch (IOException e) {
            e.printStackTrace();
        }
        writer.println("<html><head><title>" + title + "</title></head></body>"); // 제목 출력
        writer.println("<h1>" + title + "</h1>"):
    }
    public void makeString(String str) {                    // HTML 파일에서의 문자열
        writer.println("<p>" + str + "</p>");               // <p> 태그로 출력
    }
    public void makeItems(String[] items) {                 // HTML 파일에서의 개별항목 
        writer.println("<ul>");                             // <ul>과 <li>로 출력
        for (int i=0; i<items.length; i++)
            writer.println("<li>" + items[i] + "</li>"); 
        writer.println("</ul>");
    }
    public void close() {                                   // 문서의 완성
        writer.println("</body></html>");                   // 태그를 닫는다
        writer.close();                                     // 파일을 닫는다
    }
    public String getResult() {                             // 완성한 문서
        return filename;                                    // 파일명을 반환한다
    }
}
```


* Main 클래스 (Main.java) 
```
public class Main {
    public static void main(String[] args) {
        if(args.length != 1) {
            usage();
            System.exit(0); 
        }
        if(args[0].equals("plain")) {
            TextBuilder textbuilder = new TextBuilder(); 
            Director director = new Director(textbuilder); 
            director.construct();
            String result = textbuilder.getResult(); 
            System.out.println(result);
        }
        else if(args[0].equals("html")) {
            HTMLBuilder htmlbuilder = new HTMLBuilder(); 
            Director director = new Director(htmlbuilder); 
            director.construct();
            String filename = htmlbuilder.getResult(); 
            System.out.println(filename + "가 작성되었습니다.");
        }
        else {
            usage();
            System.exit(0); 
        }
    }
    public static void usage() {
        System.out.println("Usage: Java Main plain 일반 텍스트로 문서작성");
        System.out.println("Usage: Java Main html  HTML 파일로 문서작성");
    }
}
```

***
## Adapter 어댑터
바꿔서 재이용하기

### 사용 목적과 용도
* 이미 제공되어 있는 것을 그대로 사용할 수 없을 때, '이미 제공되어 있는 것'과 '필요한 것' 사이의 차이를 없애주는 패턴 
* Wrapper 패턴으로 불리기도 함

### 클래스 다이어그램 
* 상속을 사용한 Adapter 패턴

<img src="https://github.com/jihyun-s/Design-pattern/blob/main/adapter_class.jpg" width="70%" height="70%" title="Adapter-class"></img>
* 위임을 사용한 Adapter 패턴

<img src="https://github.com/jihyun-s/Design-pattern/blob/main/adapter_instance.jpg" width="70%" height="70%" title="Adapter-instance"></img>


### 구현 코드 
#### 상속을 사용한 Adapter 패턴

. | 예제 프로그램 
 --|--
 제공되고 있는 것 | Banner 클래스(showWithParen, showWithAster)
 교환장치 | PrintBanner 클래스
 필요한 것 | Print 인터페이스 (printWeak, PrintStrong) 


* Banner 클래스(Banner.java) - 미리 제공되어 있는 클래스
```
public class Banner {
    private String string;
    public Banner(String string) {
        this.string = string;
    }
    public void showWithParen() {
        System.out.println("(" + string + ")"); 
    }
    public void showWithAster() {
        System.out.println("*" + string + "*"); 
    }
}
```

* Print 인터페이스 (Print.java) - 필요로 하는 인터페이스
```
public interface Print {
    public abstract void printWeak(); 
    public abstract void printStrong(); 
}
```

* PrintBanner 클래스 (PrintBanner.java) - 어댑터 역할 
```
public class PrintBanner extends Banner implements Print {
    public PrintBanner(String string) {
        supter(string);
    }
    public void printWeak() {
        showWithParen();
    }
    public void printStrong() {
        showWithAster();
    }
}
```

* Main 클래스 (Main.java) 
```
public class Main {
    public static void main(String[] args) {
        Print p = new PrintBanner("Hello"); 
        p.printWeak(); 
        p.printStrong(); 
    }
}
```

#### 위임을 사용한 Adapter 패턴
Main 클래스, Banner 클래스는 위와 동일 

* Print 클래스 (Print.java)
```
public abstract class Print {
    public abstract void printWeak();
    public abstract void printStrong();
}
```

* PrintBanner 클래스 (PrintBanner.java) 
```
public class PrintBanner extends Print {
    private Banner banner; 
    public PrintBanner(String string) {
        this.banner = new Banner(string); 
    }
    public void printWeak() {
        banner.showWithParen();
    }
    public void printStrong() {
        banner.showWithAster();
    }
}
```

***
## Bridge 브릿지
기능 계층과 구현 계층 분리하기

### 사용 목적과 용도
* '기능의 클래스 계층'과 '구현의 클래스 계층'을 두 개의 독립된 클래스 계층으로 분리하고, 두 계층 사이에 다리를 놓는 일
* 새로운 기능을 추가하고 싶은 경우 - 클래스 상속
* 새로운 구현을 추가하고 싶은 경우 - 추상 클래스 구현  

### 클래스 다이어그램   
<img src="https://github.com/jihyun-s/Design-pattern/blob/main/bridge.jpg" width="40%" height="40%" title="Abstract factory"></img>

### 구현 코드 
* 기능의 클래스 계층


이름 | 용도 
--|-- 
Display | '표시한다'는 클래스 
CountDisplay | '지정 횟수만큼 표시한다'는 기능을 추가하는 클래스 

* 구현의 클래스 계층   


이름 | 용도 
--|--
DisplayImpl | '표시한다'는 클래스 
StringDisplayImpl | '문자열을 사용해서 표시한다'는 클래스 


* Display 클래스 (Display.java) - 기능의 클래스 계층
impl 필드가 두 클래스 계층의 다리가 된다.
```
public class Display {
    private DisplayImpl impl; 
    public Display(DisplayImpl impl) {
        this.impl = imp1;
    }
    public void open() {
        impl.rawOpen();
    }
    public void print() {
        impl.rawPrint();
    }
    public void close() {
        impl.rawClose();
    }
    public final void display() {
        open();
        print();
        close();
    }
}
```

* CountDisplay 클래스 (CountDisplay.java) - 기능의 클래스 계층 
```
public class CountDisplay extends Display {
    public CountDisplay(DisplayImpl impl) {
        super(impl);
    }
    pulbic void multiDisplay(int times) {       // times회 반복해서 표시한다.
        open(); 
        for(int i=0; i<times; i++) {
            print();
        }
        close();
    }
}
```

* DisplayImpl 클래스 (DisplayImpl.java) - 구현의 클래스 계층 
```
public abstract class DisplayImpl {
    public abstract void rawOpen(); 
    public abstract void rawPrint(); 
    public abstract void rawClose();
}
```

* StringDisplayImpl 클래스 (StringDisplayImpl.java) - 구현의 클래스 계층 
```
public class StringDisplayImpl extends DisplayImpl {
    private String string;                            // 표시해야 할 문자열
    private int width;                                // 바이트 단위로 계산할 문자열의 길이 
    public StrinigDisplayImpl(String string) {        // 생성자에서 전달된 문자열 string을 
        this.string = string;                         // 필드에 기억해둔다. 
        this.width = string.getBytes().length;        // 그리고 바이트 단위의 길이도 필드에 기억해두고 나중에 사용한다.
    }
    public void rawOpen() {
        printLine();
    }
    public void rawPrint() {
        System.out.println("|" + string + "|");       // 앞뒤에 "|"를 붙여서 표시한다.
    }
    public void rawClose() {
        printLine();
    }
    private void printLine() {
        System.out.println("+");                      // 테두리의 모서리를 표현
        for(int i=0; i<width; i++) {                  // width개의 "-"를 표시해서 
            System.out.println("-");                  // 테두리 선으로 이용한다.
        }
        System.out.println("+");                      // 테두리의 모서리를 표현
    }
}
```

* Main 클래스 (Main.java)
```
public class Main {
    public static void main(String[] args) {
        Display d1 = new Display(new StringDisplayImpl("Hello, Korea.")); 
        Display d2 = new CountDisplay(new StringDisplayImpl("Hello, World."));
        CountDisplay d3 = new CountDisplay(new StringDisplayImpl("Hello, Universe.")); 
        
        d1.display();
        d2.display(); 
        d3.display();
        d3.multiDisplay(5);
    }
}
```


***
## Composite
그릇과 내용물을 동일시하기


### 사용 목적과 용도
그릇과 내용물을 동일시해서 재귀적인 구조를 만들기 위한 디자인 패턴 (ex. directory)


### 클래스 다이어그램
<img src="https://github.com/jihyun-s/Design-pattern/blob/main/composite.jpg" width="50%" height="50%" title="Composite"></img>


### 구현 코드
이름 | 용도 
--|-- 
Entry | File과 Directory를 동일시하는 추상 클래스
File | file을 나타내는 클래스
Directory | directory를 나타내는 클래스 
FileTreatmentException | 파일에 Entry를 추가하려고 할 때 발생하는 예외 클래스 
Main | 동작 테스트용 클래스 


* Entry 클래스 (Entry.java) 
```
public abstract class Entry {
    public abstract String getName();                          // 이름을 얻는다. 
    public abstract int getSize();                             // 크기를 얻는다. 
    public Entry add(Entry entry) throws FileTreatmentException {
        throw new FileTreatmentException();                    // 엔트리를 추가한다.
    }
    public void printList() {
        printList(" "); 
    }
    protected abstract void printList(String prefix);          // prefix를 앞에 붙여서 종류를 표시한다. 
    public String toString() {
        return getName() + " (" + getSize() + ")";
    }
}
```


* File 클래스 (File.java) 
```
public class File extends Entry {
    private String name; 
    private int size; 
    public File(String name, int size) {
        this.name = name;
        this.size = size; 
    }
    public String getName() {
        return name; 
    }
    public int getSize() {
        return size; 
    }
    protected void printList(String prefix) {
        System.out.println(prefix + "/" + this); 
    }
}
```


* Directory 클래스 (Directory.java) 
```
import java.util.Iterator;
import java.util.ArrayList; 

public class Directory extends Entry {
    private String name; 
    private ArrayList directory = new ArrayList(); 
    public Directory(String name) {
        this.name = name; 
    }
    public String getName() {
        return name;
    }
    public int getSize() {
        int size = 0; 
        Iterator it = directory.iterator(); 
        while(it.hasNext()) {
            Entry entry = (Entry)it.next(); 
            size += entry.getSize();
        }
        return size; 
    }
    public Entry add(Entry entry) {
        directory.add(entry); 
        return this;
    }
    protected void printList(String prefix) {               // 엔트리의 종류
        System.out.println(prefix + "/" + this);
        Iterator it = directory.iterator(); 
        while(it.hasNext()) {
            Entry entry = (Entry)it.next();
            entry.printList(prefix + "/" + name);
        }
    }
}
```

* FileTreatmentException 클래스 (FileTreatmentException.java)
```
public class FileTreatmentException extends RuntimeException {
    public FileTreatmentException() {
    }
    public FileTreatmentException(String msg) {
        super(msg);
    }
}
```


* Main 클래스 (Main.java) 
```
public class Main {
    public static void main(String[] args) {
        try {
            System.out.println("Making root entries..."); 
            Directory rootdir = new Directory("root"); 
            Directory bindir = new Directory("bin"); 
            Directory tmpdir = new Directory("tmp"); 
            Directory usrdir = new Directory("usr"); 
            rootdir.add(bindir); 
            rootdir.add(tmpdir);
            rootdir.add(usrdir);
            bindir.add(new File("vi", 10000));
            bindir.add(new File("latex", 20000)); 
            rootdir.printList();
        } catch (FileTreatmentException e) {
            e.printStackTrace(); 
        }
    }
}
```



***
## Decorator
장식과 내용물을 동일시하기 


### 사용 목적과 용도
오브젝트에 장식을 해 나가는 디자인 패턴 


### 클래스 다이어그램
<img src="https://github.com/jihyun-s/Design-pattern/blob/main/decorator.jpg" width="35%" height="35%" title="Decorator"></img>


### 구현 코드
이름 | 용도 
--|-- 
Display | 문자열 표시용 추상 클래스
StringDisplay | 1행으로 구성된 문자열 표시용 클래스
Border | 장식을 나타내는 클래스
SideBorder | 좌우에 장식을 붙이는 클래스 
FullBorder | 상하좌우에 장식을 붙이는 클래스 
Main | 동작 테스트용 클래스


* Display 클래스 (Display.java)
```
public abstract class Display {
    public abstract int getColumns();        // 가로 문자수를 얻는다. 
    public abstract int getRows();           // 세로 행수를 얻는다. 
    public abstract String getRowText(int row); // row번째의 문자열을 얻는다.
    public final void show() {               // 전부 표시한다. 
        for (int i=0; i<getRows(); i++) {
            System.out.println(getRowText(i)); 
        }
    }
}
```


* StringDisplay 클래스 (StringDisplay.java)
```
public class StringDisplay extends Display {
    private String string;                      // 표시 문자열
    public StringDisplay(String string) {       // 인수로 표시 문자열을 지정
        this.string = string; 
    }
    public int getColumns() {                   // 문자수
        return string.getBytes().length;
    }
    public int getRows() {                      // 행수는 1 
        return 1; 
    }
    public String getRowText(int row) {         // row가 0일 때만 반환
        if (row == 0) {
            return string;
        } else {
            return null;
        }
    }
}
```


* Border 클래스 (Border.java) 
```
public abstract class Border extends Display {
    protected Display display;               // 이 장식이 둘러싸고 있는 내용물 
    protected Border(Display display) {      // 인스턴스 생성시에 내용물을 인수로 지정
        this.display = display; 
    }
}
```


* SideBorder 클래스 (SideBorder.java)
```
public class SideBorder extends Border {
    private char borderChar;                    // 장식이 되는 문자 
    public SideBorder(Display display, char ch) { // 생성자에서 Display라는 장식 문자를 지정
        super(display); 
        this.borderChar = ch; 
    }
    public int getColumns() {                   // 문자수는 내용물의 양쪽에 장식 문자수를 더한 것 
        return 1 + display.getColumns() + 1; 
    }
    public int getRows() {                      // 행수는 내용물의 행수와 동일
        return display.getRows(); 
    }
    public String getRowText(int row) {         // 지정된 행의 내용은 내용물의 지정된 행의 양쪽에 장식 문자를 붙인 것 
        return borderChar + display.getRowText(row) + borderChar; 
    }
}
```

* FullBorder 클래스 (FullBorder.java)
```
public class FullBorder extends Border {
    public FullBorder(Display display) {
        super(display);
    }
    public int getColumns() {                   // 문자수는 내용물의 양쪽에 좌우의 장식 문자수를 더한 것
        return 1 + display.getColumns() + 1; 
    }
    public int getRows() {                      // 행수는 내용물의 행수에 상하의 장식문자수를 더한 것
        return 1 + display.getRows() + 1; 
    }
    public String getRowText(int row) {         // 지정한 행의 내용 
        if(row == 0) {                          // 장식의 상단
            return "+" + makeLine('-', display.getColumns()) + "+"; 
        } else if (row == display.getRows() + 1) { // 장식의 하단 
            return "+" + makeLine('-', display.getColumns()) + "+"; 
        } else {                                // 그 외
            return "|" + display.getRowText(row - 1) + "|"; 
        }
    }
    private String makeLine(char ch, int count) { // 문자 ch를 count개 연속시킨 문자열을 만든다.
        StringBuffer buf = new StringBuffer(); 
        for (int i=0; i<count; i++) {
            buf.append(ch); 
        }
        return buf.toString();
    }
}
```


* Main 클래스 (Main.java) 
```
public class Main {
    public static void main(String[] args) {
        Display b1 = new StringDisplay("Hello, world"); 
        Display b2 = new SideBorder(b1, '#'); 
        Display b3 = new FullBorder(b2); 
        b1.show();
        b2.show();
        b3.show();
        Display b4 = new SideBorder( 
                        new FullBorder(
                           new FullBorder(
                              new SideBorder(
                                 new FullBorder(
                                    new StringDisplay("안녕하세요.")
                                 ),
                                 '*'
                              )
                           )
                        ),
                        '/'
                     ); 
        b4.show(); 
    }
}
```
***
## Facade
단순한 창구  


### 사용 목적과 용도
내부적으로 복잡하게 얽혀있는 것을 정리해서 시스템 외부에 단순한 인터페이스(API)를 제공한다.


### 클래스 다이어그램
<img src="https://github.com/jihyun-s/Design-pattern/blob/main/facade.png" width="55%" height="55%" title="Facade"></img>


### 구현 코드
패키지 | 이름 | 용도 
--|--|--
pagemaker | Database | 메일 주소에서 사용자 이름을 얻는 클래스 
pagemaker | HtmlWriter | HTML 파일을 작성하는 클래스 
pagemaker | PageMaker | 메일 주소에서 사용자의 웹 페이지를 작성하는 클래스 - facade 역할 


* Database 클래스 (Database.java) 
```
package pagemaker; 

import java.io.FileInputStream; 
import java.io.IOException; 
import java.util.Porperties; 

public class Database {
    private Database() {      // new로 인스턴스를 생성시키지 않기 위한 private 선언 
    }
    public static Properties getProperties(String dbname) {       // 데이터베이스 이름에서 Properties를 얻는다
        String filename = dbname + ".txt"; 
        Properties prop = new Properties(); 
        try {
            prop.load(new FileInputStream(filename)); 
        } catch (IOException e) {
            Systme.out.println("Warning : " + filename + " is not found."); 
        }
        return prop; 
    }
}
```


* 데이터 파일 (maildata.txt) 
```
youngjin@youngjin.com=Youngjin 
kim@youngjin=Kim 
lee@youngjin=Lee
park@youngjin=Park
```


* HtmlWriter 클래스 (HtmlWriter.java) - title()를 제일 먼저 호출해야 하는 제약이 있음 
```
package pagemaker; 

import java.io.Writer; 
import java.io.IOException; 

public class HtmlWriter {
    private Writer writer; 
    public HtmlWriter(Writer writer) {
        this.writer = writer; 
    }
    public void title(String title) throws IOException {       // 타이틀 출력
        writer.write("<html>"); 
        writer.write("<head>"); 
        writer.write("<title>" + title + "</title>");
        writer.write("</head>");
        writer.write("<body>\n"); 
        writer.write("<h1>" + title + "</h1>\n"); 
    }
    public void paragraph(String msg) throws IOException {     // 단락 출력
        writer.write("<p>" + msg + "</p>\n"); 
    }
    public void link(String href, String caption) throws IOException {     // 링크 출력
        writer.write("<a href=\"" + href + "\">" + caption + "</a>");
    }
    public void mailto(String mailaddr, String username) throws IOException {    // 메일 주소 출력 
        link("mailto:" + mailaddr, username); 
    }
    public void close() throws IOException {       // 닫는다.
        writer.write("</body>"); 
        writer.write("</html>\n"); 
        writer.close(); 
    }
}
```


* PageMaker 클래스 (PageMaker.java) 
```
package pagemaker; 

import java.io.FileWriter; 
import java.io.IOException; 
import java.util.Properties; 

public class PageMaker {
    private PageMaker() {        // 인스턴스는 만들지 않기 때문에 private 선언한다
    }
    public static void makeWelcomePage(String mailaddr, String filename) {
        try {
            Properties mailprop = Database.getProperties("maildata"); 
            String username = mailprop.getProperty(mailaddr); 
            HtmlWriter writer = new HtmlWriter(new FileWriter(filename)); 
            writer.title("Welcome to " + username + " 's page!");
            writer.paragraph(username + "의 페이지에 오신 걸 환영합니다."); 
            writer.paragraph("메일을 기다리고 있습니다."); 
            writer.mailto(mailaddr, username); 
            writer.close(); 
            System.out.println(filename + " is created for " + mailaddr + " (" + username + ")");
        } catch (IOException e) {
            e.printStackTree(); 
        }
    }
}

```


* Main 클래스 (Main.java) 
```
import pagemaker.PageMaker; 

public class Main {
    public static void main(String[] args) {
        PageMaker.makeWelcomePage("youngjin@youngjin.com ", "welcome.html"); 
    }
}

```

***
## Flyweight
동일한 것을 공유해서 낭비 없애기  


### 사용 목적과 용도
오브젝트(객체)가 적은 메모리를 사용하도록 하기 위함. 인스턴스를 가능한 공유시켜서 쓸데없이 new하지 않도록 한다.


### 클래스 다이어그램
<img src="https://github.com/jihyun-s/Design-pattern/blob/main/flyweight.png" width="25%" height="25%" title="Flyweight"></img>


### 구현 코드
이름 | 용도 
--|--
BigChar | 큰 문자를 나타내는 클래스 
BigCharFactory | BigChar의 인스턴스를 공유하면서 생성하는 클래스
BigString | BigChar를 모아서 만든 큰 문자열을 나타내는 클래스 


* BigChar 클래스 (BigChar.java) 
```
import java.io.BufferedReader; 
import java.io.FileReader; 
import java.io.IOException; 

public class BigChar {
    // 문자의 이름 
    private char charname; 
    // 큰 문자를 표현하는 문자열 ('#' '.' '\n'의 열) 
    private String fontdata; 
    // 생성자 
    public BigChar(char charname) {
        this.charname = charname; 
        try {
            BufferedReader reader = new BufferedReader(new FileReader("big" + charname + ".txt"); 
            String line; 
            StringBuffer buf = new StringBuffer(); 
            while ((line = reader.readLine()) != null) {
                buf.append(line); 
                buf.append("\n"); 
            }
            reader.close(); 
            this.fontdata = buf.toString();
        } catch(IOException e) {
            this.fontdata = charname + "?"; 
        }
    }
    // 큰 문자를 표현한다. 
    public void print() {
        System.out.print(fontdata);
    }
}
```


* BigCharFactory 클래스 (BigCharFactory.java) 
```
import java.util.HashMap; 

public class BigCharFactory {
    // 이미 만들어진 BigChar의 인스턴스를 관리 
    private HashMap pool = new HashMap(); 
    // Singleton 패턴
    private static BigCharFactory singleton = new BigCharFactory(); 
    // 생성자
    private BigCharFactory() {
    }
    // 유일한 인스턴스를 얻는다
    public static BigCharFactory getInstance() {
        return singleton;
    }
    // BigChar의 인스턴스 생성(공유) 
    public synchronized BigChar getBigChar(char charname) {
        BigChar bc = (BigChar)pool.get("" + charname); 
        if(bc == null) {
            bc = new BigChar(charname);      // 인스턴스 생성 
            pool.put("" + charname, bc); 
        }
        return bc; 
    }
}

```


* BigString 클래스 (BigString.java)
```
public class BigString {
    // 큰 문자의 배열
    private BigChar[] bigchars; 
    // 생성자 
    public BigString(String string) {
        bigchars = new BigChar[string.length()]; 
        BigCharFactory factory = BigCharFactory.getInstance();
        for(int i=0; i<bigchars.length; i++) {
            bigchars[i] = factory.getBigChar(string.charAt(i)); 
        }
    }
    // 표시 
    public void print() {
        for(int i=0; i<bigchars.length; i++) {
            bigchars[i].print(); 
        }
    }
}
```


* Main 클래스 (Main.java) 
```
public class Main {
    public static void main(String[] args) {
        if(args.length == 0) {
            System.out.println("Usage: java Main digits"); 
            System.out.println("Example: java Main 1212123"); 
            System.exit(0);
        }
        BigString bs = new BigString(args[0]); 
        bs.print(); 
    }
}
```
***

## Proxy
필요해지면 만들기 


### 사용 목적과 용도
실제로 실행하는 단계가 되었을 때(필요할 때) Proxy클래스가 RealSubject 인스턴스를 생성함. 


### 클래스 다이어그램
<img src="https://github.com/jihyun-s/Design-pattern/blob/main/proxy.png" width="70%" height="70%" title="Proxy"></img>


### 구현 코드
이름 | 용도 
--|-- 
Printer | 이름있는 프린터를 나타내는 클래스(본인) 
Printable | Printer와 PrinterProxy 공통의 인스턴스 
PrinterProxy | 이름있는 프린터를 나타내는 클래스(대리인) 



* Printer 클래스 (Printer.java) 
```
public class Printer implements Printable {
    private String name; 
    public Printer() {
        heavyJob("Printer의 인스턴스를 생성 중"); 
    }
    public Printer(String name) {
        this.name = name; 
        heavyJob("Printer의 인스턴스 (" + name + ")을 생성 중"); 
    }
    public void setPrinterName(String name) {
        this.name = name; 
    }
    public String getPrinterName() {
        return name; 
    }
    public void print(String string) {
        System.out.println("=== " + name + " ==="); 
        System.out.println(string); 
    }
    private void heavyJob(String msg) {
        System.out.print(msg); 
        for (int i=0; i<5; i++) {
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
            }
            System.out.print(".");
        }
        System.out.println("완료."); 
    }
}
```


* Printable 인터페이스 (Printable.java)
```
public interface Printable {
    public abstract void setPrinterName(String name); 
    public abstract String getPrinterName(); 
    public abstract void print(String string); 
}
```


* PrinterProxy 클래스 (PrinterProxy.java)
```
public class PrinterProxy implements Printable {
    private String name; 
    private Printer real; 
    public PrinterProxy() {
    }
    public PrinterProxy(String name) {
         this.name = name; 
    }
    public synchronized void setPrinterName(String name) {
        if (real != null) {
            real.setPrinterName(name);
        }
        this.name = name; 
    }
    public String getPrinterName() {
        return name;
    }
    public void print(String string) {
        realize(); 
        real.print(string);
    }
    private synchronized void realize() {
        if (real == null) {
            real = new Printer(name); 
        }
    }
}

```


* Main 클래스 (Main.java) 
```
public class Main() {
    public static void main(String[] args) {
        Printable p = new PrinterProxy("Alice"); 
        System.out.println("이름은 현재 " + p.getPrinterName() + "입니다."); 
        p.setPrinterName("Bob"); 
        System.out.println("이름은 현재 " + p.getPrinterName() + "입니다."); 
        p.print("Hello, world."); 
    }
}
```
***

## Chain of Responsibility
책임 떠넘기기


### 사용 목적과 용도
어떤 요청이 발생했을 때 그 요청을 처리할 객체를 직접 결정할 수 없는 경우 복수의 객체를 사슬(chain)처럼 연결해 두면 그 객체의 사슬을 차례로 돌아다니면서 목적한 객체를 결정하는 방법. 요청과 처리의 연결을 유연하게 해서 각 객체를 부품으로 독립시킬 수 있다. 


### 클래스 다이어그램
<img src="https://github.com/jihyun-s/Design-pattern/blob/main/chain.png" width="70%" height="70%" title="Chain of Responsibility"></img>


### 구현 코드
이름 | 용도 
--|-- 
Trouble | 발생한 트러블을 나타내는 클래스. 트러블 번호(number)를 가진다. 
Support | 트러블을 해결하는 추상 클래스 
NoSupport | 트러블을 해결하는 구상 클래스 (항상 처리하지 않는다) 
LimitSupport | 트러블을 해결하는 구상 클래스 (지정한 번호 미만의 트러블을 해결) 
OddSupport | 트러블을 해결하는 구상 클래스 (홀수 번호의 트러블을 해결) 
SpecialSupport | 트러블을 해결하는 구상 클래스 (특정 번호의 트러블을 해결) 
Main | Support들의 사슬을 만들고 트러블을 발생시키는 동작 테스트용 클래스 


* Trouble 클래스 (Trouble.java) 
```
public class Trouble {
    private int number;             // 트러블 번호 
    public Trouble(int number) {    // 트러블의 생성 
        this.number = number; 
    }
    public int getNumber() {        // 트러블 번호를 얻는다. 
        return number; 
    }
    public String toString() {      // 트러블의 문자열 표현 
        return "[Trouble " + number + "]"; 
    }
}
```


* Support 클래스 (Support.java) 
```
public abstract class Support {
    private String name;                  // 이 트러블 해결자의 이름 
    private Support next;                 // 떠넘기는 곳 
    public Support(String name) {         // 트러블 해결자의 생성
        this.name = name; 
    }
    public Support setNext(Support next) {      // 떠넘기는 곳을 설정 
        this.next = next; 
        return next; 
    }
    public final void support(Trouble trouble) {      // 트러블 해결의 수순 
        if(resolve(trouble)) {
            done(trouble); 
        } else if (next != null) {
            next.support(trouble); 
        } else {
            fail(trouble); 
        }
}
```


* NoSupport 클래스 (NoSupport.java) 
```
public class NoSupport extends Support {
    public NoSupport(String name) {
        super(name); 
    }
    protected boolean resolve(Trouble trouble) {      // 해결용 메소드 
        return false;                                 // 아무것도 처리하지 않음
    }
}
```


* LimitSupport 클래스 (LimitSupport.java) 
```
public class LimitSupport extends Support {
    private int limit;                             // 이 번호 미만이면 해결
    public LimitSupport(String name, int limit) {  // 생성자
        super(name); 
        this.limit = limit;
    }
    protected boolean resolve(Trouble trouble) {      // 해결용 메서드 
        if (trouble.getNumber() < limit) {
            return true;
        } else {
            return false; 
        }
    }
}
```


* OddSupport 클래스 (OddSupport.java) 
```
public class OddSupport extends Support {
    public OddSupport (String name) {
        super(name);
    }
    protected boolean resolve(Trouble trouble) {         // 해결용 메서드 
        if (trouble.getNumber() % 2 == 1) {
            return true;
        } else {
            return false;
        }
}
```


* SpecialSupport 클래스 (SpecialSupport.java) 
```
public class SpecialSupport extends Support {
    private int number;                                  // 이 번호만 해결할 수 있다 
    public SpecialSupport(String name, int number) {
        super(name); 
        this.number = number; 
    }
    protected boolean resolve(Trouble trouble) {         // 해결용 메서드
        if (trouble.getName() == number) {
            return true;
        } else {
            return false; 
        }
    }
}
```


* Main 클래스 (Main.java) 
```
public class Main {
    public static void main(String[] args) {
        Support alice = new NoSupport("Alice"); 
        Support bob   = new LimitSupport("Bob", 100); 
        Support charlie = new SpecialSupport("Charlie", 429); 
        Support diana = new LimitSupport("Diana", 200); 
        Support elmo = new OddSupport("Elmo"); 
        Support fred = new LimitSupport("Fred", 300); 
        // 사슬의 형성 
        alice.setNext(bob).setNext(charlie).setNext(diana).setNext(elmo).setNext(fred); 
        // 다양한 트러블 발생 
        for (int i = 0; i < 500; i += 33) {
            alice.support(new Trouble(i)); 
        }
    }
}
```



***

## Command
명령을 클래스로 표현하기 


### 사용 목적과 용도
작업 요청(커맨드)을 객체 형태로 캡슐화. 명령의 집합을 저장해 두면 같은 명령을 재실행할 수도 있고, 복수의 명령을 모아서 새로운 명령으로 재이용할 수 있다. (취소 기능도 가능, 위임)


### 클래스 다이어그램
<img src="https://github.com/jihyun-s/Design-pattern/blob/main/command.png" width="70%" height="70%" title="Proxy"></img>


### 구현 코드
패키지 | 이름 | 용도 
--|--|-- 
Command | Command | 명령을 표현하는 인터페이스 
Command | MacroCommand | 복수의 명령을 모은 명령을 표현하는 클래스 
drawer | DrawCommand | 점 그리기 명령을 표현하는 클래스 
drawer | Drawable | 그리기 대상을 표현하는 인터페이스 
drawer | DrawCanvas | 그리기 대상을 구현하는 클래스 


* Command 인터페이스 (Command.java) 
```
package command; 

public interface Command { 
    public abstract void execute(); 
}
```


* MacroCommand 클래스 (MacroCommand.java) 
```
package command; 

import java.util.Stack; 
import java.util.Iterator; 

public class MacroCommand implements Command {
    // 명령의 집합
    private Stack commands = new Stack(); 
    // 실행 
    public void execute() {
        Iterator it = commands.iterator(); 
        while (it.hasNext()) 
            ((Command)it.next()).execute(); 
    }
    // 추가 
    public void append(Command cmd) {
        if (cmd != this) 
            commands.push(cmd);
    }
    // 마지막 명령을 삭제 
    public void undo() {
        if (!commands.empty()) 
            commands.pop();
    }
    // 전부 삭제 
    public void clear() {
        commands.clear();
    }
}
```



* DrawCommand 클래스 (DrawCommand.java) 
```
package drawer; 

import command.Command; 
import java.awt.Point; 

public class DrawCommand implements Command {
    // 그림 그리는 대상 
    protected Drawable drawable; 
    // 그림 그리는 위치 
    private Point position; 
    // 생성자 
    public DrawCommand (Drawable drawable, Point position) {
        this.drawable = drawable; 
        this.position = position;
    }
    // 실행 
    public void execute() {
        drawable.draw(position.x, position.y); 
    }
}
```


* Drawable 인터페이스 (Drawable.java)
```
package drawer; 

public interface Drawable {
    public abstract void draw (int x, int y); 
}
```



* DrawCanvas 클래스 (DrawCanvas.java)
```
package drawer; 

import command.*; 

import java.util.*; 
import java.awt.*; 
import java.awt.event.*; 
import javax.swing.*; 

public class DrawCanvas extends Canvas implements Drawable {
    // 그림 그리는 색 
    private Color color = Color.red; 
    // 그림 그리는 점의 반경 
    private int radius = 6; 
    // 이력 
    private MacroCommand history; 
    // 생성자
    public DrawCanvas(int width, int height, MacroCommand history) {
        setSize(width, height); 
        setBackground(Color.white); 
        this.history = history; 
    }
    // 이력 전체를 다시 그리기 
    public void paint(Graphics g) {
        history.execute(); 
    }
    // 그림 그리기 
    public void draw(int x, int y) {
        Graphics g = getGraphics(); 
        g.setColor(color); 
        g.fillOval(x - radius, y - radius, radius * 2, radius * 2); 
    }
}

```


* Main 클래스 (Main.java) 
```
import command.*; 
import drawer.*; 

import java.awt.*; 
import java.awt.event.*; 
import javax.swing.*; 

public class Main extends JFrame implements ActionListener, MouseMotionListener, WindowListener {
    // 그림 그린 이력 
    private MacroCommand history = new MacroCommand(); 
    // 그림 그리는 영역 
    private DrawCanvas canvas = new DrawCanvas(400, 400, history); 
    // 제거 버튼
    private JButton clearButton = new JButton("clear"); 
    
    // 생성자 
    public Main(String title) {
        super(title); 
        
        this.addWindowListener(this); 
        canvas.addMouseMotionListener(this); 
        clearButton.addActionListener(this);
        
        Box buttonBox = new Box(BoxLayout.X_AXIS); 
        buttonBox.add(clearButton); 
        Box mainBox = new Box(BoxLayout.Y_AXIS); 
        mainBox.add(buttonBox); 
        mainBox.add(canvas);
        getContentPane().add(mainBox); 
        
        pack();
        show();
    }
    
    // ActionListener용 
    public void actionPerformed(ActionEvent e) {
        if(e.getSource() == clearButton) {
            history.clear(); 
            canvas.repaint(); 
        }
    }
    
    // MouseMotionListener용 
    public void mouseMoved(MouseEvent e) {
    }
    public void mouseDragged(MouseEvent e) {
        Command cmd = new DrawCommand(canvas, e.getPoint());
        history.append(cmd);
        cmd.execute();
    }
    
    // WindowListener용
    public void windowClosing(WindowEvent e) {
        System.exit(0);
    }
    public void windowActivated(WindowEvent e) {}
    public void windowClosed(WindowEvent e) {}
    public void windowDeactivated(WindowEvent e) {}
    public void windowDeiconified(WindowEvent e) {} 
    public void windowIconified(WindowEvent e) {} 
    public void windowOpened(WindowEvent e) {}
    
    public static void main(String[] args) {
        new Main("Command Pattern Sample"); 
    }
}
```


***

## Interpreter
문법규칙을 클래스로 표현하기 


### 사용 목적과 용도
프로그램이 해결하려는 문제를 간단한 명령으로 표현. 명령을 해석, 실행하는 interpreter 프로그램 작성하여 사용. (재사용성 높임) 


### 클래스 다이어그램
<img src="https://github.com/jihyun-s/Design-pattern/blob/main/interpreter.png" width="65%" height="65%" title="Proxy"></img>


### 구현 코드
이름 | 용도 
--|-- 
Node | 구문 트리의 노드가 되는 클래스 
ProgramNode | program 에 대응하는 클래스
CommandListNode | command list 에 대응하는 클래스
CommandNode | command 에 대응하는 클래스 
RepeatCommandNode | repeat command 에 대응하는 클래스 
PrimitiveCommandNode | primitive command 에 대응하는 클래스 
Context | 구문해석을 위한 전후 관계를 나타내는 클래스 
ParseException | 구문해석 중의 예외 클래스 



* Node 클래스 (Node.java)
```
public abstract class Node {
    public abstract void parse(Context context) throws ParseException;
}
```


* ProgramNode 클래스 (ProgramNode.java) 
```
// <program> ::= program <command list> 
public class ProgramNode extends Node {
    private Node commandListNode; 
    public void parse(Context context) throws ParseException {
        context.skipToken("program");
        commandListNode = new CommandListNode();
        commandListNode.parse(context);
    }
    public String toString() {
        return "[program " + commandListNode + "]"; 
    }
}
```


* CommandListNode 클래스 (CommandListNode.java) 
```
import java.util.ArrayList;

// <command list> ::= <command>* end
public class CommandListNode extends Node {
    private ArrayList list = new ArrayList(); 
    public void parse(Context context) throws ParseException {
        while (true) {
            if (context.currentToken() == null) {
                throw new ParseException("Missing 'end'"); 
            } else if (context.currentToken().equals("end")) {
                context.skipToken("end"); 
                break;
            } else {
                Node commandNode = new CommandNode(); 
                commandNode.parse(context); 
                list.add(commandNode); 
            }
        }
    }
    public String toString() {
        return list.toString();
    }
}
```


* CommandNode 클래스 (CommandNode.java)
```
// <command> ::= <repeat command> | <primitive command> 
public class CommandNode extends Node {
    private Node node; 
    public void parse(Context context) throws ParseException {
        if(context.currentToken().equals("repeat")) {
            node = new RepeatCommandNode(); 
            node.parse(context); 
        } else {
            node = new PrimitiveCommandNode(); 
            node.parse(context); 
        }
    }
    public String toString() {
        return node.toString(); 
    }
}
```


* RepeatCommandNode 클래스 (RepeatCommandNode.java) 
```
// <repeat command> ::= repeat <number> <command list> 
public class RepeatCommandNode extends Node {
    private int number; 
    private Node commandListNode; 
    public void parse(Context context) throws ParseException {
        context.skipToken("repeat"); 
        number = context.currentNumber(); 
        context.nextToken(); 
        commandListNode = new CommandListNode();
        commandListNode.parse(context); 
    }
    public String toString() {
        return "[repeat " + number + " " + commandListNode + "]";
    }
}
```


* PrimitiveCommandNode 클래스 (PrimitiveCommandNode.java) 
```
// <primitive command> ::= go | right | left 
public class PrimitiveCommandNode extends Node {
    private String name; 
    public void parse(Context context) throws ParseException {
        name = context.currentToken(); 
        context.skipToken(name); 
        if(!name.equals("go") && !name.equals("right") && !name.equals("left")) {
            throw new ParseException(name + " is undefined");
        }
    }
    public String toString() {
        return name;
    }
}
```



* Context 클래스 (Context.java) 


이름 | 용도
--|--
nextToken | 다음 토큰을 얻는다 (다음 토큰으로 진행) 
currentToken | 현재 토큰을 얻는다 (다음 토큰으로 진행하지 않는다) 
skipToken | 현재 토큰을 검사한 후, 다음 토큰을 얻는다 (다음 토큰으로 진행) 
currentNumber | 현재 토큰을 수치로 얻는다 (다음 토큰으로 진행하지 않는다) 


```
import java.util.*; 

public class Context {
    private StringTokenizer tokenizer; 
    private String currentToken; 
    public Context(String text) {
        tokenizer = new StringTokenizer(text); 
        nextToken();
    }
    public String nextToken() {
        if (tokenizer.hasMoreTokens()) {
            currentToken = tokenizer.nextToken(); 
        } else {
            currentToken = null;
        }
        return currentToken;
    }
    public String currentToken() {
        return currentToken;
    }
    public void skipToken(String token) throws ParseException {
        if(!token.equals(currentToken)) {
            throw new ParseException("Warning: " + token + " is expected, but " + currentToken + " is found."); 
        }
        nextToken();
    }
    public int currentNumber() throws ParseException {
        int number = 0; 
        try {
            number = Integer.parseInt(currentToken); 
        } catch (NumberFormatException e) {
            throw new ParseException("Warning: " + e);
        }
        return number;
    }
}
```



* ParseException 클래스 (ParseException.java)
```
public class ParseException extends Exception {
    public ParseException(String msg) {
        super(msg);
    }
}
```



* Main 클래스 (Main.java) 
```
import java.util.*; 
import java.io.*; 

public class Main {
    public static void main(String[] args) {
        try {
            BufferedReader reader = new BufferedReader(new FileReader("program.txt"));
            String text; 
            while((text = reader.getLine()) != null) {
                Systme.out.println("text=\"" + text + "\"");
                Node node = new ProgramNode();
                node.parse(new Context(text)); 
                System.out.println("node = " + node); 
            }
        } catch (Exception e) {
            e.printStackTree();
        }
    }
}
```


* program.txt 
```
program end
program go end
program go right go right go right go right end
program repeat 4 go right end end 
program repeat 4 repeat 3 go right go left end right end end 
```



***

## Iterator
순서대로 지정해서 처리하기


### 사용 목적과 용도
순서대로 지정하면서 전체를 검색하는 처리를 위한 것


### 클래스 다이어그램
<img src="https://github.com/jihyun-s/Design-pattern/blob/main/iterator.png" width="55%" height="55%" title="Proxy"></img>


### 구현 코드
이름 | 용도 
--|-- 
Aggregate | 집합체를 나타내는 인터페이스 
Iterator | 하나씩 나열하면서 검색을 실행하는 인터페이스 
Book | 책을 나타내는 클래스 
BookShelf | 서가를 나타내는 클래스 
BookShelfIterator | 서가를 검색하는 클래스 


* Aggregate 인터페이스 (Aggregate.java) 
```
public interface Aggregate {
    public abstract Iterator iterator(); 
}
```


* Iterator 인터페이스 (Iterator.java)
```
public interface Iterator {
    public abstract boolean hasNext(); 
    public abstract Object next();
}
```


* Book 클래스 (Book.java)
```
public class Book {
    private String name; 
    public Book(String name) {
        this.name = name; 
    }
    public String getName() {
        return name; 
    }
}

```


* Bookshelf 클래스 (BookShelf.java)
```
public class BookShelf implements Aggregate {
    private Book[] books; 
    private Int last = 0; 
    public BookShelf(int maxsize) {
        this.books = new Book[maxsize];
    }
    public Book getBookAt(int index) {
        return books[index];
    }
    public void appendBook(Book book) {
        this.books[last] = book; 
        last++;
    }
    public int getLength() {
        return last;
    }
    public Iterator iterator() {
        return new BookShelfIterator(this);
    }
}
```



* BookShelfIterator 클래스 (BookShelfIterator.java) 
```
public class BookShelfIterator implements Iterator {
    private BookShelf bookShelf; 
    private int index; 
    public BookShelfIterator(BookShelf bookShelf) {
        this.bookShelf = bookShelf; 
        this.index = 0;
    }
    public boolean hasNext() {
        if(index < bookShelf.getLength()) {
            return true; 
        } else {
            return false;
        }
    }
    public Object next() {
        Book book = bookShelf.getBookAt(index); 
        index++; 
        return book;
    }
}
```


* Main 클래스 (Main.java)
```
public class Main {
    public static void main(String[] args) {
        BookShelf bookShelf = new BookShelf(4); 
        bookShelf.appendBook(new Book("Around the Word in 80 Days")); 
        bookShelf.appendBook(new Book("Bible")); 
        bookShelf.appendBook(new Book("Cinderella"));
        bookShelf.appendBook(new Book("Daddy-Long-Legs"));
        Iterator it = bookShelf.iterator(); 
        while(it.hasNext()) {
            Book book = (Book)it.next(); 
            System.out.println(book.getName()); 
        }
    }
}
```




***
## Reference 
- Java 언어로 배우는 디자인 패턴 입문 
- Chapter 3. 팩토리(Factory) 패턴(https://blog.naver.com/anciid/221793735687)
