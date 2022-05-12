---
title:  "Ch3. 함수"
excerpt: "cleancode"

categories:
  - cleancode
---
  
함수 : 어떤 프로그램이든 가장 기본적인 단위
  
---

이 장에서 나오는 내용

- 함수가 읽기 쉽고 이해하기 쉬우려면 어떻게 구현해야 하는가?
- 의도를 분명히 표현하는 함수는 어떻게 구현할 수 있을까?
- 함수에 어떤 속성을 부여해야 처음 읽는 사람이 프로그램 내부를 직관적으로 파악할 수 있을까?

---

**작게 만들어라!**

함수를 만드는 첫째 규칙은 ‘작게!’다.

얼마나 짧아야 좋을까? 각 함수가 이야기 하나를 표현하면 좋다.  
아래 예제는 책에 나오는 예제로, 함수가 짧고 표현이 명백하다.

```java
public static String renderPageWithSetupsAndTearDowns (
	PageData pageData, boolean isSuite) throws Exception {
	if (isTestPage(pageData))
		includeSetupAndTeardownPages(pageData, isSuite);

	return pageData.getHtml();
}
```

중첩 구조가 생길만큼 함수가 커져서는 안된다.  
함수에 들여쓰기 수준은 1단이나 2단을 넘어서면 안 된다. 

---

**한 가지만 해라!**

함수는 한 가지를 해야 한다. 그 한 가지를 잘 해야 한다. 그 한 가지만을 해야 한다!

여기서 문제는 그 ‘한 가지’가 무엇인지 알기가 어렵다는 점이다.
- 지정된 함수 이름 아래에서 추상화 수준이 하나다.
- 단순히 다른 표현이 아니라 의미 있는 이름으로 다른 함수를 추출할 수 있다면 그 함수는 여러 작업을 하는 셈이다.

---

**함수 당 추상화 수준은 하나로!**

- 한 함수 내에 추상화 수준을 섞으면 코드를 읽는 사람이 헷갈린다.
- 위에서 아래로 코드 읽기: 내려가기 법칙
코드는 위에서 아래로 이야기처럼 읽혀야 좋다.  
위에서 아래로 프로그램을 읽으면 함수 추상화 수준이 한 번에 한 단계씩 낮아진다.

---

Switch 문

본질적으로 Switch 문은 N가지를 처리한다.  
다형성을 이용해서 각 switch 문을 저차원 클래스에 숨기고 절대로 반복하지 않도록 하자.

```java
public abstract class Employee {
	public abstract boolean isPayday();
	public abstract Money calculatePay();
	public abstract void deliverPay(Monoey pay);
}

------------------

public interface EmployeeFactory {
	public Employee makeEmployee(EmployeeRecord r) throws InvalidEmployeeType;
}

------------------

public class EmployeeFactoryImpl implements EmployeeFactory {
	public Employee makeEmployee(EmployeeRecord r) throws InvalidEmployeeType {
		switch (r.type) {
			case Commissioned:
				return new CommissionedEmployee(r);
			case Hourly:
				return new HourlyEmployee(r);
			case Salaried:
				return new SalariedEmployee(r);
			default:
				throw new InvalidEmployeeType(r.type);
		}
	}
}
```
