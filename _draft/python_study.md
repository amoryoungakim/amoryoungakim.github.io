추상 클래스란
상속을 목적으로 하는 Parent Class
인스턴스를 만들 수 없음
Child Class가 갖고 있어야 하는 메서드를 @abstractmethod로 지정해 줌

any(), all()
any는 하나라도 조건을 만족하면 True, all은 모두가 조건을 만족해야 True를 반환한다.

staticmethod, classmethod
클래스의 메소드 중에서 staticmethod와 classmethod는 @를 이용해 데코레이터를 달아 표시해준다.
staticmethod는 self라는 파라미터를 가지고 있지 않다.
인스턴스가 가지고 있는 고유 값으로 연산하는게 아닐 때 staticmethod를 쓴다. class를 통제할 수 없다.
classmethod는 self대신 cls라는 파라미터를 갖는다. 클래스 자신을 참조한다.
이 메소드를 쓰면 어느 인스턴스에서도 전체 클래스의 variable을 통제할 수 있다.

attribute란
클래스의 메소드나 변수를 attribute(속성)이라고 함. 클래스나 인스턴스의 'state'를 나타내 줌.
클래스속성, 인스턴스 속성이 있음.
비밀 속성은 언더스코어 두개를 붙여서 variable을 지정하면, 클래스 바깥에서 접근할 수 없음. 함부로 수정 못하게 하는 것임

named tupel은
collections 모듈로부터 namedtuple을 임포트해서 만든다
튜플의 각 항목에 이름을 지정하고 그 이름으로 인덱싱을 할 수 있다.

캡슐화(encapsulation)란 추상화(Abstraction)과 같은 것임
객체 내부의 상세한 것은 몰라도 되게끔 만들어 놓은 것
객체 내부의 데이터는 보호하고
parent class는 encapsualte되고 그 데이터를 child class가 접근하지 못함

parameter = arguments?

