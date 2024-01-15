# static factory method
직접적으로 클래스 Constructor를 통해 객체 생성을 하지 않고 static method를 통해 클래스 객체를 간접적으로 생성하는 디자인 패턴  

~~~
public class Member {
    private final String id;
    private final String name;
    
    private Member(string id, String name) {
        this.id = id;
        this.name = name;
    }
    
    public static Member registMember(String id, String name) {
        return new Member(id, name);
    }
}
~~~

## 정적 팩토리 메서드 특징
### 생성 목적에 대한 표현이 가능하다.
생성 목적에 따라 생성자 오버로딩으로 구분하여 객체를 생성하였다. 이렇게 오버로딩으로 된 객체를 생성하기 위해서
사용자는 생성자의 인자와 순서 및 내부 구조를 알고 있어야 목적에 맞는 객체 생성이 가능하다.
생성 목적을 메서드 네이밍으로 표현하게 되어 객체의 역할을 유추할 수 있고 유지보수시 가독성에도 뛰어나다.
클래스를 만든 자만 사용한다면 큰 메리트가 없을 수 있지만 외부 사용자가 클래스를 이용하게 되면 이야기는 달라진다.
단순히 new 키워드로 생성하게 될 때 외부 사용자는 내부 코드를 봐야하는 상황이 올 수 있고 만약 바이트코드로 되어 있으면
Decompile을 해야한다.

### 인스턴스에 대한 통제 및 관리가 가능하다.
객체를 생성하기 위해선  호출과 생성 사이에 메서드가 존재하기에 객체 생성과 관리가 유리한 측면이 있다.
메서드 내부를 변경하여 새로운 객체 생성이나 기능 변경이 가능하다. 
또 단일 객체를 생성하여 재사용할 수 있다. 싱글턴패턴
~~~
class SingleTon {
    private static SingleTon singleton;
    private SingleTon() {}
    
    public synchronizied SingleTon getSingleToneInstance() {
        if (singleton == null) {
            singleton = new Singleton();
        }
        return singleton;
    }
}
~~~

캐싱 절차 구조를 정적팩토리 메서드로 제공할 수 있다.
인스턴스를 재사용하여 메모리를 절약할 수 있다.

~~~
class Day {
  private String day;
  public Day(String day) {
    this.day = day;
  }
}
class Factory {
  private static final Map<String, Day> cache = new HashMap<>();
  
  static {
    cache.put("mon", new Day("mon"));
    cache.put("tue", new Day("tue"));
  }
  
  public static Day from(String day) {
    if (cache.containsKey(day)) {
      return cache.get(day);
    } else {
      Day day1 = new Day(day);
      cache.put(day, day1);
      return day1;
    }
  }
}
~~~
인스턴스를 통제하는 것은 인스턴스가 단 하나뿐임을 보장  
이렇게 인스턴스의 생성에 관여하여, 생성되는 인스턴스의 수를 통제할 수 있는 클래스를 인스턴스 통제 (instance-controlled) 클래스라고 한다.

### 하위 자료형 객체 반환
팩토리 메소드를 통해서 유연하게 하위 클래스의 인스턴스를 생성할 수 있다.
~~~
interface Animal {}

class Dog implements Animal{}
class Cat implements Animal{}
class Rabbit implements Animal{}

public class Animals {
    public static Animals getDog() {
        return new Dog();
    }
    public static Animals getCat() {
        return new Cat();
    }
}
~~~

### 캡슐화
정적 팩토리 메서드로 외부로 부터 구현 코드를 숨길 수 있는 캡슐화가 된다.
외부로 정보를 노출하지 않아 정보은닉의 장점을 갖고 구현체를 숨기기 때문에 외부와 직접적인 의존성을
갖지 않게 된다.

## 단점
private 생성자일 경우 상속 불가능


### 참조
https://inpa.tistory.com/entry/GOF-%F0%9F%92%A0-%EC%A0%95%EC%A0%81-%ED%8C%A9%ED%86%A0%EB%A6%AC-%EB%A9%94%EC%84%9C%EB%93%9C-%EC%83%9D%EC%84%B1%EC%9E%90-%EB%8C%80%EC%8B%A0-%EC%82%AC%EC%9A%A9%ED%95%98%EC%9E%90
