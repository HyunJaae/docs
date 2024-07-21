---
description: 전략 패턴에 대해 알아봅시다.
---

# 전략 패턴(Strategy Pattern)

### 변화에 대응하는 방법

#### **상속**

변화하는 요구사항에 대응하기 위한 방법으로 상속은 좋지 않습니다.

상속을 계속 활용한다면 규격(조건)이 바뀔 때마다 서브클래스의 메서드를 전부 살펴보고 상황에 따라 오버라이드해야 하기 때문입니다.

<figure><img src="https://blog.kakaocdn.net/dn/Exh1a/btsD1bXiZdN/JQ4HiWJg09V68Cip2OzZk0/img.jpg" alt=""><figcaption></figcaption></figure>

위 클래스 다이어그램과 같이 Duck 이라는 슈퍼클래스를 상속하는 여러 오리 클래스가 있다고 합시다. 여기서 오리를 날게 하려면 어떻게 해야 할까요?

<figure><img src="https://blog.kakaocdn.net/dn/cUijqn/btsD5zoyNPr/ko7I0uBL4l2dYWQ26rKGaK/img.jpg" alt=""><figcaption></figcaption></figure>

위 다이어그램처럼 모든 오리 클래스들은 Duck 클래스를 상속 받으니까 Duck 클래스에 fly() 메서드만 추가하면 되는걸까요?

<figure><img src="https://blog.kakaocdn.net/dn/ckf2Sc/btsD0D0NvLp/bOZnRdwwaqGc8DeiLk6pB0/img.jpg" alt=""><figcaption></figcaption></figure>

만약 Duck 클래스를 상속하는 고무 오리 클래스가 있다면 어떻게 해야할까요? 날면 안되고 '꽥꽥'이 아니라 '삑삑' 소리가 나야 합니다. quack() 메서드를 오버라이드한 것처럼 fly() 메서드도 오버라이드해서 아무 동작도 하지 않게 해야할까요? 수백가지의 오리 클래스가 있다면, 오리마다 나는 방식이 달라지면 모든 오리 클래스를 확인해가면서 수정해야 합니다.

이처럼 상속은 변화하는 조건들에 대응하기엔 좋은 해결책이 아닙니다.

#### **캡슐화**

이제 해결책을 생각해봅시다. 이 상황에 딱 어울리는 디자인 원칙이 있습니다.

> 디자인 원칙\
> 애플리케이션에서 달라지는 부분을 찾아내고, 달라지지 않는 부분과 분리한다.

&#x20;

다시 말해 코드에 새로운 요구 사항이 있을 때마다 바뀌는 부분이 있다면 분리해야 합니다.

바뀌는 부분을 캡슐화해야 한다는 말과 같습니다. 그러면 나중에 바뀌지 않는 부분에는 영향을 미치지 않고 변경되는 부분만 수정하거나 확장할 수 있습니다.

이제 Duck 클래스에서 변화하는 부분들을 뽑아봅시다.

| 변화하지 않는 부분        | 변화하는 부분        |
| ----------------- | -------------- |
| swim(), display() | quack(), fly() |

&#x20;

quack() 메서드와 fly() 메서드를 Duck 클래스에서 분리하기 위해 각 행동을 나타낼 클래스 집합을 새로 만듭니다.

<figure><img src="https://blog.kakaocdn.net/dn/byjEJS/btsD4vfM8EE/UPntvYKK7yEDjuiUR99Vf1/img.jpg" alt=""><figcaption></figcaption></figure>

&#x20;

인터페이스를 통해 2개의 메서드를 분리했습니다. 이제 다른 형식의 객체에서도 나는 행동과 꽥꽥 거리는 행동을 재사용할 수 있습니다. 그리고 기존의 행동 클래스를 수정하거나 날아다니는 행동을 사용하는 Duck 클래스를 전혀 건드리지 않고도 새로운 행동을 추가할 수 있습니다. 그런데 왜 인터페이스를 사용했을까요?

#### **인터페이스**

인터페이스를 통해 변화하는 부분을 분리하는 방법은 앞서 살펴본 Duck 클래스에서 quck() 이나 fly() 메서드를 구체적으로 구현하거나 서브 클래스 자체에서 별도로 구현하는 방법과는 상반된 방법입니다. 특정 구현에 의존하지 않고 코드 추가 없이 행동을 변경할 수 있습니다.

따라서 Duck 클래스와 서브 클래스들은 변화하는 행동들을 구체적으로 구현할 필요 없이 필요한 행동 인터페이스만 참조하면 됩니다.

> 디자인 원칙\
> 구현보다는 인터페이스에 맞춰서 프로그래밍한다.

&#x20;

그럼 이제 코드로 구현해볼까요?

#### **코드 예시**

우선 Duck 클래스에 각 인터페이스 형식의 인스턴스 변수를 추가합니다. 각 오리 객체에서는 실행 시에 이 변수에 특정 행동 형식의 레퍼런스를 다형적으로 설정합니다. 그리고 Duck 클래스와 모든 서브 클래스에서 fly() 와 quack() 메서드를 제거합니다.

대신 performFly()와 performQuack()이라는 메서드를 추가합니다.

```java
public abstract class Duck {
    FlyBehavior flyBehavior;
    QuackBehavior quackBehavior;

    public void performQuack() {
        quackBehavior.quack();
    }
    
    public void performFly() {
        flyBehavior.fly();
    }

    public void swim() {
        System.out.println("자고로 오리라면 물에 떠야 합니다.");
    }

    public void display() {
    }
}
```

&#x20;

꽥꽥 거리는 행동을 하고 싶을 땐 quackBehavior에 의해 참조되는 객체에서 꽥꽥거리도록 하면 됩니다. 객체의 종류에는 전혀 신경 쓸 필요 없이 quack()을 실행할 줄만 알면 됩니다. 다음으로 각 인터페이스와 구현 클래스를 살펴봅시다.

```java
public interface FlyBehavior {
    void fly();
}

public class FlyWithWings implements FlyBehavior{
    @Override
    public void fly() {
        System.out.println("fly... fly...");
    }
}

public class FlyNoWay implements FlyBehavior{
    @Override
    public void fly() {
        System.out.println("I can't fly!");
    }
}

public class FlyRocketPowered implements FlyBehavior{
    @Override
    public void fly() {
        System.out.println("to infinity and beyond!");
    }
}
```

```java
public interface QuackBehavior {
    void quack();
}

public class Quack implements QuackBehavior{
    @Override
    public void quack() {
        System.out.println("Quack!! Quack!!");
    }
}

public class Squeak implements QuackBehavior{
    @Override
    public void quack() {
        System.out.println("삑. 삑.");
    }
}

public class MuteQuack implements QuackBehavior{
    @Override
    public void quack() {
        System.out.println("silence is best!");
    }
}
```

&#x20;

이제 MallardDuck 클래스를 입력해서 컴파일 해봅시다.

```java
public class MallardDuck extends Duck {

    public MallardDuck() {
        quackBehavior = new Quack();
        flyBehavior = new FlyWithWings();
    }

    @Override
    public void display() {
        System.out.println("청둥오리 탄생!");
    }
}

public class Main {
    public static void main(String[] args) {
        Duck mallard = new MallardDuck();
        mallard.display();
        mallard.swim();
        mallard.performQuack();
        mallard.performFly();
    }
}

/*
청둥오리 탄생!
자고로 오리라면 물에 떠야 합니다.
Quack!! Quack!!
fly... fly...
*/
```

#### **동적으로 행동 지정하기**

오리의 행동 형식을 생성자에서 인스턴스를 만드는 방법이 아닌 Duck의 서브클래스에서 세터 메서드를 호출하는 방법으로 설정할 수 있어야 하지 않을까요?

Duck 클래스에 메서드 2개를 추가합니다.

```java
public void setFlyBehavior(FlyBehavior fb) {
    flyBehavior = fb;
}

public void setQuackBehavior(QuackBehavior qb) {
    quackBehavior = qb;
}
```

이 두 메서드를 호출하면 언제든지 오리의 행동을 즉석에서 바꿀 수 있습니다.

이제 ModelDuck이라는 서브 클래스를 새로 만듭니다.

```java
public class ModelDuck extends Duck {
    public ModelDuck() {
        flyBehavior = new FlyNoWay();
        quackBehavior = new Quack();
    }

    @Override
    public void display() {
        System.out.println("모형 오리 등장!");
    }
}
```

Main 클래스를 수정하고 컴파일 해봅니다.

```java
public class Main {
    public static void main(String[] args) {
        Duck mallard = new MallardDuck();
        mallard.display();
        mallard.swim();
        mallard.performQuack();
        mallard.performFly();

        System.out.println("===================================");

        Duck model = new ModelDuck();
        model.display();
        model.performFly();
        model.setFlyBehavior(new FlyRocketPowered());
        model.performFly();
    }
}

/*
청둥오리 탄생!
자고로 오리라면 물에 떠야 합니다.
Quack!! Quack!!
fly... fly...
===================================
모형 오리 등장!
I can't fly!
to infinity and beyond!
*/
```

{% hint style="info" %}
**전략 패턴**\
알고리즘군(구현체 집합)을 정의하고 캡슐화해서 각각의 알고리즘군을 수정해서 쓸 수 있게 해줍니다.\
전략 패턴을 사용하면 클라이언트로부터 알고리즘을 분리해서 독립적으로 변경할 수 있습니다.
{% endhint %}

***

헤드퍼스트 디자인패턴 책을 참고하여 작성한 글입니다.
