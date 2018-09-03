## MIXIN

### mixin 이란

기본적으로 메서드들이 정의된 모듈을 클래스에 포함하여 그 클래스의 일부로 사용하는 기법이다. 믹스인이라는 것은 객체와 모듈의 관계이고, Mixin은 코드를 재사용하는 패턴중 하나이다. 그리고 다중상속의 다른 표현법이라 볼 수도 있다.

 전통적인 클래스 상속을 이용할 때에는 필요하지 않은 부분도 모조리 상속되는 그런 불편함이 있다. 하지만 Mixin으로 클래스를 조립한다면 더 깔끔한 구현이 가능하게 된다. 



###  java와 Ruby

java에서는 클래스 다중 상속을 지원하지 않는다. 하지만 인터페이스는 예외로 다중상속을 지원한다.

인터페이스 자체로 인스턴스화 될 수 없고, 껍데기만 있다. 그리고 인터페이스를 상속받는 클래스들은 껍데기인 인터페이스의 메서드들을 구현해야만 한다.

![img](https://stonzeteam.github.io/assets/img/160611_interface.png)

날 수 있는 것, 동물은 인터페이스이다. 이 인터페이스를 상속받는 클래스들이 있고 새는 두 인터페이스를 상속받아서 구현할 수 있다. 이런 식의 인터페이스 다중 상속은 가능하지만, 클래스를 다중 상속받아야 할 일이 있다면 어떻게 해야 할까

![img](https://stonzeteam.github.io/assets/img/160611_mixin.png)

사람인 엄마, 아빠의 유전자를 내가 물려받는 것을 표현한 구조이다. 여기서 사람은 인터페이스, 엄마, 아빠는 클래스이고 나 또한 클래스이다.

위에서 보았던 인터페이스를 다중 상속받는 것과 달리 이 그림에선 클래스를 다중 상속받는다.java는 다중 상속을 지원하지 않는데 이런 경우 루비에서는 Mixin을 이용하여 가볍게 해결할 수 있다.



~~~Ruby
module M1
  def m1_m
    p "m1_m"
  end
end
module M2
  def m2_m
    p "m2_m"
  end
end
class C
  include M1, M2                                                                                        
end
c = C.new()
c.m1_m()
c.m2_m()
~~~

먼저 모듈 M1과 M2가 선언되며, 클래스 C가 이 모듈들을 include한다. 모든 과정이 끝난후에 C 클래스의 인스턴스를 만들어 주고, include한 두 모듈들의 메소드를 호출하면 에러없이 정상적으로 동작하게 된다. 모듈은 인스턴스를 가질 수 없다. 클래스가 아니기 때문이다. 하지만 클래스 선언에 모듈을 포함할 수 있다. 모듈을 포함하면 이 모듈의 모든 인스턴스 메서드는 클래스의 메서드처럼 동작한다. 

즉 메서드가 클래스에 녹아서 석여 버린(mixed in)  것이다. 믹스인된 모듈은 실제로는 일종의 부모 클래스처럼 동작한다. 

**C가 M1과 M2의 메소드를 포함하고 있는 것처럼 행동하는 것이 믹스인의 핵심** 이다.



### 메서드 이름 중복 

 같은 메서드가 클래스, 부모 클래스, 클래스의 믹스인에도 있는 경우 루비는 가장 먼저 객체의 클래스 그 자체를 찾아본 후 클래스에 포함된 믹스인을 찾아보고, 그 후에 상위 클래스와 상위 클래스의 믹스인을 살핀다. 여러 개의 모듈이 믹스인 되어 있다면, 마지막에 포함된  것부터 찾는다. 



### class vs module

모듈은 다양한 클래스에서 사용가능한 메소드를 제공하는 라이브러리라고 생각하면 된다. 클래스는 객체, 모듈은 기능이라고 생각하면 된다. 

![img](http://cfile1.uf.tistory.com/image/26533246582C15492D9B69)

모듈은 기본적으로 여러 클래스에서 같은 메서드를 사용하고자 할 때 + 하지만 상속관계로 설정하고 싶지 않을 때 쓰면 된다.

예를 들면 Bird 라는 클래스에 `fly` 가 구현되어 있다고 하면 하위클래스로 Swallow, Eagle 을 만들어서 Bird를 상속받을 수 있지만   `fly`가 꼭 Bird 안에 구현되어있을 필요는 없는거다. Plane이나 Kite 같은 클래스도 `fly` 메서드를 사용할 수 있기때문이다. 이 클래스들은 Bird 클래스를 상속받기엔 부적절하다.

이럴때 `Flyable` 모듈에다 `fly` 메서드를 만들어서 각각의 클래스에 `include` (불러오기) 하면 부모-자식 관계와는 관계없이 사용가능하다. 

Module는 Class와 비슷하게 메소드, 상수, 모듈, 클래스를 포함할 수 있다. 그러나 Class와 달리 모듈을 상속받아서 객체를 생성할 수는 없다. 하지만 이 Module의 인스턴스 메소드를 클래스에서 사용할 수 있다. 이렇게 상속을 하지 않고도 여러개의 Class가 같은 Module을 mixin해서 사용하거나 여러개의 Module을 하나의 클래스에 mixin하여 사용할 수 있다.



### instance method vs module method

 instance method의 경우는 해당 모듈을 include하는 클래스에서 instance method로 사용할 수 있지만, module method는 instance method로 사용할 수 없는 차이가 있다. 

반대로 module method는 receiver 객체없이 사용할 수 있지만, instance method는 반드시 receiver 객체와 함께 호출되어야 한다.

```Ruby
module Foo
    def foo    
        puts 'heyyyyoooo!'  
    end
end

class Bar  
    include Foo
end

Bar.new.foo # heyyyyoooo!
Bar.foo # NoMethodError: undefined method ‘foo’ for Bar:Class

class Baz  
    extend Foo
end

Baz.foo # heyyyyoooo!
Baz.new.foo # NoMethodError: undefined method ‘foo’ for #<Baz:0x1e708>

```

위의 코드에서, Bar 클래스와 Baz 클래스는 Foo 모듈을 각각 `include`와 `extend` 명령으로 상속받게 된다. 이 두가지 명령의 차이점은, `include`로 Foo 모듈을 상속받은 경우, Bar 클래스에서는 Foo 모듈의 foo 메소드를 instance method로 상속받게 되고, `extend`로 Foo 모듈을 상속받는 Baz 클래스는 Foo 모듈의 foo 메소드를 class method로 상속받게 된다는 것이다.

따라서, Foo 모듈을 `extend`로 상속받는 Baz 클래스를 이용하여 새로운 객체를 생성한 후(Baz.new)에 클래스 메소드인 foo 메소드를 적용하면 당연히 NoMethodError가 발생하게 되는 것이다. 물론 Baz 클래스에 클래스 메소드인 foo 메소드를 적용하면(Baz.foo) 에러없이 실행될 것이다.



한편, 루비에서는 흔히 instance method 뿐만 아니라 class method를 추가하기 위해서도 `include`를 사용한다. 그 이유는 include 명령의 경우는 self.included 라는 콜백메소드를 가지고 있기 때문이다. 반대로 `extend`는 이러한 콜백메소드를 가지고 있지 않다.

~~~Ruby
module Foo
  def self.included(base)
    base.extend(ClassMethods)
  end
  module ClassMethods
    def bar
      puts 'class method'
    end
  end
  def foo
    puts 'instance method'
  end
end

class Baz
  include Foo
end

Baz.bar # class method
Baz.new.foo # instance method
Baz.foo # NoMethodError: undefined method ‘foo’ for Baz:Class
Baz.new.bar # NoMethodError: undefined method ‘bar’ for #<Baz:0x1e3d4>
~~~

여기서 Baz 클래스가 Foo 모듈을 `include`할 때 foo 메소드를 instance method로 사용할 수 있게 될 뿐만아니라, 모듈을 `include`할 때 콜백으로 호출되는 self.included(ClassMethods) 명령으로 인하여 ClassMethods 모듈의 메소드를 `extend` 명령으로 확장하여 클래스 메소드인 bar 메소드도 사용할 수 있게 되는 것이다.



### 소스코드

예제 코드는 `Math`와 `Stringfy`라는 두가지 Module를 가지고 있고, `Number`라는 클래스를 상속받아서 만든 `BigInteger`에 Mixin을 해서 객체에 모듈의 메소드를 추가하는 예제이다.

Math 모듈은 `add`라는 메소드를 가지고 있는데 두 수를 더하는 값을 `BigInteger`에 초기 값으로 넘겨주는 메소드이다.

```ruby
# filename : Math.rb

module Math  
  def add(value_one, value_two)
    BigInteger.new(value_one+value_two)
  end
end  

```

`Stringfy` 모듈은 `stringfy`라는 메소드를 가지고 있는데 `@value`라는 객체 변수의 값에 따라서 문자열을 반환해주는 메소드이다.

```ruby
#filename : Stringfy.rb

module Stringfy  
  def stringfy
    if @value == 1
      "One"
    elsif @value == 2
      "Two"
    elsif @value == 3
      "Three"
    end
  end
end  

```

`Number` 클래스는 `inteValue`라는 메소드를 가지고 있는데 이것은 `@value`객체를 반환하는 메소드이다.

```ruby
# filename : Number.rb

class Number  
  def intValue
    @value
  end
end  

```

위의 `Number` 클래스를 상속해서 `BigInteger` 클래스를 만드는데 이 클래스에 `Stringfy`를 mixin하고 `Math` 메소드를 확장했다.

```ruby
#filename : BigInteger.rb

require 'Stringfy'  
require 'Math'  
require 'Number'

class BigInteger < Number  
  include Stringfy
  extend Math

  def initialize(value)
    @value = value
  end
end  

```

아래 코드는 단순하게 `BigInteger`를 사용해서 생성자에 10이라는 값을 넣고 객체를 생성해서 `Number`가 가지고 있었던 `intValue`로`@value`객체 변수의 값을 출력하는 코드이다.

```ruby
#filename test.rb
require 'BigInteger'

bigint1 = BigInteger.new(10)  
puts bigint1.intValue  
```

다음은 Math를 Mixin으로 추가된 `Math`의 `add`를 사용해서 `BigInteger` 객체를 생성해보자.

```ruby
#filename test.rb
require 'BigInteger'

bigint1 = BigInteger.new(10)  
puts bigint1.intValue

bigint2 = BigInteger.add(-2, 4)  
puts bigint2.intValue  

```

Mixin으로 Math의 `add`가 실행되었는데 이 때 입력받은 두 인자값은 Math의 `add` 메소드안에 포함된 클래스 `BigInteger.new(a+b)`로 두 인자 값을 더해서 객체를 생성시키는 것이다. 그래서 결국은 `BigInteger`의 객체변수 `@value`에 두 인자를 더한 2의 값이 저장되어 있다.

```ruby
#filename test.rb
require 'BigInteger'

bigint1 = BigInteger.new(10)  
puts bigint1.intValue

bigint2 = BigInteger.add(-2, 4)  
puts bigint2.intValue  
puts bigint2.stringfy

module CurrencyFormatter  
  def format
    "$#{@value}"
  end
end

bigint2.extend CurrencyFormatter  
puts bigint2.format  
puts bigint1.foramt

```

 그럼 `CurrecyFormatter`를 Mixin하지 않은`bigint1`이라는 객체는  Mixin되지 않는 객체에서 format 메소드를 불렀기 때문에 에러가 발생한다.

#### [참조]

http://new93helloworld.tistory.com/214

https://stonzeteam.github.io/%EC%BD%94%EB%93%9C-%EC%9E%AC%EC%82%AC%EC%9A%A9%EC%9D%84-%EC%9C%84%ED%95%9C-Mixin/

http://weicomes.tistory.com/109

http://weicomes.tistory.com/77

http://blog.saltfactory.net/ruby/understanding-mixin-using-with-ruby.html

https://rorlab.org/rblogs/147