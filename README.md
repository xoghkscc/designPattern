# 디자인패턴

## 24.02.13

### Factory Pattern
#### 정의: 팩토리 메소드 패턴에서는 객체를 생성하기 위한 인터페이스를 정의하는데, 어떤 클래스의 인스턴스를 만들지는 서브클래스에서 결정하게 만들게 하는 패턴

```java
//각 나라에 맞는 IronManLab을 구현할 수 있도록 추상 클래스로 만든다.
public abstract class IronManLab {
    
    // SimpleIronManFactory factory;

    /**
     * 이 부분은 대한민국, 가나, 필리핀 각 입맛에 맞게 Factory 클래스를 만들수 있도록
     * 추상메소드로 선언한다.
     *
     * public IronManLab (SimpleIronManFactory factory) {
     *   this.factory = factory
     * }
    **/
    abstract IronMan assembleIronMan(Enum ironManType);


    public IronMan createIronMan(Enum ironManType) {

        IronMan ironMan;
        ironMan = factory.assembleIronMan(ironManType);

        /**
         * 이 부분은 변경 되지 않아야 한다. 이 부분은 변하지 않는 공통적인 부
        **/
        ironMan.준비();
        ironMan.조립();
        ironMan.자비스설치();
        ironMan.전원ON();

        return ironMan;
    }
}

public class KoreaIronManLab extends IronManLab {

  /**
   * 여기서는 구현만 담당하고 수퍼클래스에서는 아이언맨 일관된 제조를 담당한다.
  **/  
  public IronMan createIronMan(IronManType ironManType) {
        IronMan ironMan = null;

        if(ironManType.equals(IronManType.Battle)) {
            ironMan = new KoreaBattleIronMan();
        } else if(ironManType.equals(IronManType.Nano)) {
            ironMan = new KoreaNanoIronMan();
        }
        return ironMan;
    }
}

public class KoreaBattleIronMan extends IronMan {
    
    public BattleIronMan() {
        ironManName = "한국형 전투용 아이언맨 입니다.";
        type = "미사일을 많이 가지고 있습니다.";
    }
}

public class GanaIronManLab extends IronManLab {

  /**
   * 여기서는 구현만 담당하고 수퍼클래스에서는 아이언맨 일관된 제조를 담당한다.
  **/  
  public IronMan createIronMan(IronManType ironManType) {
        IronMan ironMan = null;

        if(ironManType.equals(IronManType.Battle)) {
            ironMan = new GanaHulkBattleIronMan();
        } else if(ironManType.equals(IronManType.Nano)) {
            ironMan = new GanaNanoIronMan();
        }
        return ironMan;
    }
}

public class GanaBattleIronMan extends IronMan {
    
    public BattleIronMan() {
        ironManName = "가나형 전투용 아이언맨 입니다.";
        type = "총을 많이 가지고 있습니다.";
    }
}

public static void main(String[] args) {
    IronManLab koreaIronManLab = new KoreaIronManLab();
    IronManLab ganaIronManLab = new GanaIronManLab();
    
    IronMan koreaIronMan = koreaIronManLab.createIronMan(IronManType.BATTLE);
    IronMan ganaIronMan = ganaIronManLab.createIronMan(IronManType.BATTLE);    
}

```
![image](https://github.com/xoghkscc/designPattern/assets/82793713/df7dfc68-d401-4d7d-8667-c98998140f20)

-이렇게 사용하는 이유는
객체를 생성하는 코드를 많이 사용하는데 공통적인 부분은 있지만 또 성격에 따라 다른 유형에 객체를 생성하기 원할 때 사용하는 패턴이다.
위와 같은 구조로 코드를 작성하게 되면 공통된 부분으로 코드 중복을 줄일수 있고 인터페이스(추상클래스, 인터페이스 등)를 구현하는 방식을 사용해 다형성에 이점을 가져 갈 수 있어 코드 수정없이 유연한 코드를 작성 할 수 있는 장점이 있어서 사용한다.
