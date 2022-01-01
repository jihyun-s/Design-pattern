# Design Pattern
* [Singleton](https://github.com/jihyun-s/Design-pattern/tree/main#singleton-%ED%8C%A8%ED%84%B4)
* [Abstract Factory](https://github.com/jihyun-s/Design-pattern/blob/main/README.md#abstract-factory-%EC%B6%94%EC%83%81-%ED%8C%A9%ED%86%A0%EB%A6%AC)
* [Factory Method](https://github.com/jihyun-s/Design-pattern/blob/main/README.md#factory-method-%ED%8C%A9%ED%86%A0%EB%A6%AC-%EB%A9%94%EC%84%9C%EB%93%9C)
* [Prototype](https://github.com/jihyun-s/Design-pattern#prototype-%ED%94%84%EB%A1%9C%ED%86%A0%ED%83%80%EC%9E%85)
* [Builder](https://github.com/jihyun-s/Design-pattern#builder-%EB%B9%8C%EB%8D%94)
* [Adapter]()
* [Bridge]()
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
## Reference 
- Java 언어로 배우는 디자인 패턴 입문 
- Chapter 3. 팩토리(Factory) 패턴(https://blog.naver.com/anciid/221793735687)
