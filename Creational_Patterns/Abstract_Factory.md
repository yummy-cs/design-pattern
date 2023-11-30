# 추상 팩토리 패턴(Abstract Factory Pattern) 이란?

- `연관성이 있는 객체 군`을 묶어 `추상화`하고 어떤 `구체적인 상황`이 주어지면 팩토리 객체에서 집합으로 묶은 객체 군을 구현화 하는 생성 패턴.
- 여기서 말하는 객체들의 연관성과 구체적인 상황
    - 추상 팩토리 메서드를 적용하기에 적절한 연관성과 구체적인 상황의 예시 → 자전거 공장
    - 이전에는 페달, 휠, 프레임을 생산하는 자전거 공장에 대해 프로그래밍을 한다고 가정했을 때는, 해당 공장을 페달 생산부, 휠 생산부, 프레임 생산 부서로 구분하여 프로그래밍 했을 것이다.
    - 하지만 추상 팩토리를 적용하기에 적절한 연관성과 구체적인 상황은 이 자전거 공장이 삼천리, 자이언트, 비앙키 회사의 페달/휠/프레임 을 생산하는 공장이라고 가정하는 것이 구체적인 상황에 해당 된다.
    - 또한 생산부를 삼천리/자이언트/비앙키로 나눠 각 생산부에서 휠/페달/프레임에 대한 생산을 담당하도록 하는 것을 의미한다.

- 따라서 추상 팩토리의 핵심은 제품 ‘군’집합을 타입 별로 분류된 생산부의 공장처럼 찍어 낼 수 있다는 것이다.
    - 똑같은 핸드폰의 부품이지만 애플, 삼성, 구글 등의 회사를 중심으로 분류한다.

---

# 추상 팩토리 패턴 구조

![image](https://github.com/kkkwp/CS-study/assets/97209766/34bccdfb-9af3-461a-a301-42ebe931b054)

- **AbstactFactory**
    - 최상위 팩토리 클래스로 각각의 생산 부서의 제품을 생성하는 공통된 형식의 메서드를 추상화해서 가지고 있다.
- **ConcreteFactory**
    - 서브 팩토리 클래스들로 분류한 제품 ‘군’ 에 대응되도록 클래스가 존재하며 공장 내부의 삼천리, 자이언트, 비앙키 와 같이 각 회사에 대한 부품 생산을 담당하는 생산부이며, 최상위 팩토리 클래스의 메서드를 자신의 담당 집합 ‘군’ 에 맞도록 메서드를 재정의 해서 가지고 있다.
- **AbstractProduct**
    - 집합 ‘군’이 생성해야 하는 자전거 부품인 페달/휠/프레임에 해당되며 이는 대분류로 추상화 클래스에 해당된다.
- **ConcreteProduct**
    - 앞서 AbstractProduct에 해당되는 페달/휠/프레임을 각 자전거 회사에 맞도록 삼천리 페달/삼천리 휠/삼천리 프레임 , .., 비앙키 휠/비앙키 프레임에 해당되는 구현체 클래스
- **Client**
    - 추상화된 인터페이스 만을 통해서 제품을 가져올 수 있으며, 구체적인 제품의 구성, 공장에 대한 내용을 알 필요 없는 존재로, 기존의 Service - Repository 에서 Service가 Repository 의 Client 라고 하는 것과 비슷한 맥락의 Client이다.

---

# 예제로 보는 추상화 메소드 패턴과 팩토리 메서드 패턴의 차이점과 오해

## 상황

불쌍한 개발자 취준생 현범이가 FactortyMethod, AbstractFactory 두 회사 중 하나에 취업할 기회가 생겼다, 두 회사 모두 삼성/카카오/네이버 의 FE, BE, CI/CD 솔루션을 하청하는 기업이다.

하지만 FactoryMethod 는 팩토리 메서드와 같은 방식으로 솔루션을 제공하고 AbstractFactory 는 추상화 메서드 패턴과 같은 방식으로 솔루션을 개발 및 유지보수 하고 있다.

이 때, 진짜 불쌍한 현범이는 그나마 덜 힘들기 위해 잔머리를 굴려서 취업하고자 한다, 나는 어느 회사로 취업해야 덜 힘들까?

## 공통 가정 상황

- 각 업체에 제공하고 있는 솔루션
    
    ![image](https://github.com/kkkwp/CS-study/assets/97209766/a073a7de-d9be-40d3-bd23-7bc9be4b934f)
    
- ServiceProduct 라는 추상화 객체 하위에 FrontEnd, BackEnd, CIAndCD 라는 AbstractProduct 에 해당되는 제품 대분류 서비스 추상화 객체가 있으며, 그 하위에 NaverFE, KakaoFE, SamsungFE/NaverBE, KakaoBE, SamsungBE/NaverCICD, KakaoCICD, SamsungCICD 솔루션 구현체가 존재한다.

### 제공하는 서비스 상품 공통 코드

- 상위 AbstractProduct 인터페이스 → ServiceProduct
    
    ```java
    public interface ServiceProduct {
    	void makeToSolution();
    }
    ```
    

- 하위 AbstractProduct-1 → BackEnd
    
    ```java
    public class BackEnd implements ServiceProduct {
    	@Override
    	public void makeToSolution() {
    		System.out.println("BE 솔루션 제공 -> 고객사의 공통 설정");
    	}
    }
    ```
    

- 하위 AbstractProduct-2 → FrontEnd
    
    ```java
    public class BackEnd implements ServiceProduct {
    	@Override
    	public void makeToSolution() {
    		System.out.println("BE 솔루션 제공 -> 고객사의 공통 설정");
    	}
    }
    ```
    

- 하위 AbstractProduct-3 → CIAndCD
    
    ```java
    public class BackEnd implements ServiceProduct {
    	@Override
    	public void makeToSolution() {
    		System.out.println("BE 솔루션 제공 -> 고객사의 공통 설정");
    	}
    }
    ```
    

- AbstractProduct-1 의 ConcreteProduct → KakaoBE/NaverBE/SamsungBE
    
    ```java
    // KakaoBE
    public class KakaoBE extends BackEnd {
    	@Override
    	public void makeToSolution() {
    		super.makeToSolution();
    		System.out.println("카카오 BE 솔루션");
    	}
    }
    ```
    
    ```java
    // NaverBE
    public class NaverBE extends BackEnd{
    	@Override
    	public void makeToSolution() {
    		super.makeToSolution();
    		System.out.println("네이버 BE 솔루션");
    	}
    }
    ```
    
    ```java
    // SamsungBE
    public class SamsungBE extends BackEnd{
    	@Override
    	public void makeToSolution() {
    		super.makeToSolution();
    		System.out.println("삼성 BE 솔루션");
    	}
    }
    ```
    

- AbstractProduct-2 의 ConcreteProduct → KakaoFE/NaverFE/SamsungFE
    
    ```java
    // KakaoFE
    public class KakaoFE extends FrontEnd{
    	@Override
    	public void makeToSolution() {
    		super.makeToSolution();
    		System.out.println("카카오 FE 솔루션");
    	}
    }
    ```
    
    ```java
    // NaverFE
    public class NaverFE extends FrontEnd {
    	@Override
    	public void makeToSolution() {
    		super.makeToSolution();
    		System.out.println("네이버 FE 솔루션");
    	}
    }
    ```
    
    ```java
    // SamsungFE
    public class SamsungFE extends FrontEnd{
    	@Override
    	public void makeToSolution() {
    		super.makeToSolution();
    		System.out.println("삼성 FE 솔루션");
    	}
    }
    ```
    

- AbstractProduct-3 의 ConcreteProduct→KakaoCICD/NaverCICD/SamsungCICD
    
    ```java
    // KakaoCICD  
    public class KakaoCICD extends CIAndCD{
    	@Override
    	public void makeToSolution() {
    		super.makeToSolution();
    		System.out.println("카카오 CI/CD 솔루션");
    	}
    }
    ```
    
    ```java
    // NaverCICD 
    public class NaverCICD extends CIAndCD{
    	@Override
    	public void makeToSolution() {
    		super.makeToSolution();
    		System.out.println("네이버 CI/CD 솔루션");
    	}
    }
    ```
    
    ```java
    // SamsungCICD 
    public class SamsungCICD extends CIAndCD {
    	@Override
    	public void makeToSolution() {
    		super.makeToSolution();
    		System.out.println("삼성 CI/CD 솔루션");
    	}
    }
    ```
    

## FactoryMethod

- 생산부에 해당되는 ConcreteFactory 의 분류와 각 솔루션 생산을 담당하는 관계
    - 제품 생산 파트의 분류를 CICD 파트, BackEnd 파트, FrontEnd 파트로 나누어 담당하고 있다.
    
    ![image](https://github.com/kkkwp/CS-study/assets/97209766/69365504-7b66-4c1e-9be2-c3de7b63be7f)
    

### FactoryMethod 패턴의 이란?

- `조건에 따른 객체 생성을 팩토리 클래스로 위임`하여, 팩토리 클래스에서 구현체를 생성하는 패턴.

### FactoryMethod Pattern 기반의 개발 부서 코드

- 최상위 FactoryMethod 인터페이스 → FactoryMethodCompany
    
    ```java
    public interface FactoryMethodCompany {
    	void serviceForCompany(String company);
    }
    ```
    
- FactoryMethod 구현체 클래스-1 → BackEndService
    
    ```java
    public class BackEndService implements FactoryMethodCompany {
    	@Override
    	public void serviceForCompany(String company) {
    		switch (company.toLowerCase()) {
    			case "samsung" -> {
    				SamsungBE samsungBE = new SamsungBE();
    				samsungBE.makeToSolution();
    				System.out.println();
    			}
    			case "kakao" -> {
    				KakaoBE kakaoBE = new KakaoBE();
    				kakaoBE.makeToSolution();
    				System.out.println();
    			}
    			case "naver" -> {
    				NaverBE naverBE = new NaverBE();
    				naverBE.makeToSolution();
    				System.out.println();
    			}
    		}
    	}
    }
    ```
    
- FactoryMethod 구현체 클래스-2 → FrontEndService
    
    ```java
    public class FrontEndService implements FactoryMethodCompany {
    
    	@Override
    	public void serviceForCompany(String company) {
    		switch (company.toLowerCase()) {
    			case "samsung" -> {
    				SamsungFE samsungFE = new SamsungFE();
    				samsungFE.makeToSolution();
    				System.out.println();
    			}
    			case "kakao" -> {
    				KakaoFE kakaoFE = new KakaoFE();
    				kakaoFE.makeToSolution();
    				System.out.println();
    			}
    			case "naver" -> {
    				NaverFE naverFE = new NaverFE();
    				naverFE.makeToSolution();
    				System.out.println();
    			}
    		}
    	}
    }
    ```
    
- FactoryMethod 구현체 클래스-3 → CICDService
    
    ```java
    public class CICDService implements FactoryMethodCompany {
    
    	@Override
    	public void serviceForCompany(String company) {
    		switch (company.toLowerCase()) {
    			case "samsung" -> {
    				SamsungCICD samsungCICD = new SamsungCICD();
    				samsungCICD.makeToSolution();
    				System.out.println();
    			}
    			case "kakao" -> {
    				KakaoCICD kakaoCICD = new KakaoCICD();
    				kakaoCICD.makeToSolution();
    				System.out.println();
    			}
    			case "naver" -> {
    				NaverCICD naverCICD = new NaverCICD();
    				naverCICD.makeToSolution();
    				System.out.println();
    			}
    		}
    	}
    }
    ```
    

- FactoryMethod 의 Client
    
    ```java
    public class TestApp {
    	public static void main(String[] args) {
    		FactoryMethodCompany serviceForBE = new BackEndService();
    		FactoryMethodCompany serviceForCICD = new CICDService();
    		FactoryMethodCompany serviceForFE = new FrontEndService();
    
    		serviceForBE.serviceForCompany("Samsung");
    		serviceForFE.serviceForCompany("Samsung");
    		serviceForCICD.serviceForCompany("Samsung");
    
    		serviceForBE.serviceForCompany("Kakao");
    		serviceForFE.serviceForCompany("Kakao");
    		serviceForCICD.serviceForCompany("Kakao");
    
    		serviceForBE.serviceForCompany("Naver");
    		serviceForFE.serviceForCompany("Naver");
    		serviceForCICD.serviceForCompany("Naver");
    	}
    }
    ```
    

## AbstractFactory

- 생산부에 해당되는 ConcreteFactory 의 분류와 각 솔루션 생산을 담당하는 관계
    - 제품 생산 파트의 분류를 네이버 파트, 삼성 파트, 카카오 파트로 나누어 담당하고 있다.
    
    ![image](https://github.com/kkkwp/CS-study/assets/97209766/88b214f3-aea3-40df-b433-df6811e24d71)
    

### AbstractMethodFactoryPattern 기반의 개발 부서 코드

- 최상위 AbstractFactory 인터페이스 → AbstractFactoryCompany
    
    ```java
    public interface AbstractFactoryCompany {
    	void serviceToBE();
    	void serviceToCIAndCD();
    	void serviceToFE();
    }
    ```
    
- AbstractFactory 의 구현체 ConcreteFactory-1 → ServiceForKakao
    
    ```java
    public class ServiceForKakao implements AbstractFactoryCompany {
    	@Override
    	public void serviceToBE() {
    		KakaoBE kakaoBE = new KakaoBE();
    		kakaoBE.makeToSolution();
    		System.out.println();
    	}
    
    	@Override
    	public void serviceToCIAndCD() {
    		KakaoCICD kakaoCICD = new KakaoCICD();
    		kakaoCICD.makeToSolution();
    		System.out.println();
    	}
    
    	@Override
    	public void serviceToFE() {
    		KakaoFE kakaoFE = new KakaoFE();
    		kakaoFE.makeToSolution();
    		System.out.println();
    	}
    }
    ```
    

- AbstractFactory 의 구현체 ConcreteFactory-2 → ServiceForNaver
    
    ```java
    public class ServiceForNaver implements AbstractFactoryCompany {
    	@Override
    	public void serviceToBE() {
    		NaverBE naverBE = new NaverBE();
    		naverBE.makeToSolution();
    		System.out.println();
    	}
    
    	@Override
    	public void serviceToCIAndCD() {
    		NaverCICD naverCICD = new NaverCICD();
    		naverCICD.makeToSolution();
    		System.out.println();
    	}
    
    	@Override
    	public void serviceToFE() {
    		NaverFE naverFE = new NaverFE();
    		naverFE.makeToSolution();
    		System.out.println();
    	}
    }
    ```
    
- AbstractFactory 의 구현체 ConcreteFactory-3 → ServiceForSamsung
    
    ```java
    public class ServiceForSamsung implements AbstractFactoryCompany {
    	@Override
    	public void serviceToBE() {
    		SamsungBE samsungBE = new SamsungBE();
    		samsungBE.makeToSolution();
    		System.out.println();
    	}
    
    	@Override
    	public void serviceToCIAndCD() {
    		SamsungCICD samsungCICD = new SamsungCICD();
    		samsungCICD.makeToSolution();
    		System.out.println();
    	}
    
    	@Override
    	public void serviceToFE() {
    		SamsungFE samsungFE = new SamsungFE();
    		samsungFE.makeToSolution();
    		System.out.println();
    	}
    }
    ```
    

- AbstractFactory 의 Client
    
    ```java
    public class TestApp {
    	public static void main(String[] args) {
    		AbstractFactoryCompany forSamsung = new ServiceForSamsung();
    		AbstractFactoryCompany forKakao = new ServiceForKakao();
    		AbstractFactoryCompany forNaver = new ServiceForNaver();
    
    		forSamsung.serviceToBE();
    		forSamsung.serviceToCIAndCD();
    		forSamsung.serviceToFE();
    
    		forKakao.serviceToBE();
    		forKakao.serviceToCIAndCD();
    		forKakao.serviceToFE();
    
    		forNaver.serviceToBE();
    		forNaver.serviceToCIAndCD();
    		forNaver.serviceToFE();
    	}
    }
    ```
    

## 차이점

- 공통상황 인 제품분류를 기준으로 FactoryMethod 는 제품분류에 맞게 개발 및 유지보수 파트를 분류하고, AbstractFactory는 고객사(추상 메서드 팩토리에서의 집합 ‘군’)를 기준으로 파트를 분류하여 회사의 프로젝트를 진행하고 있다.

- FactoryMethod 의 경우 위와 같은 `구체적인 상황` 과 `집합 '군' 분류` 와 같은 형식인 경우에 Client Code 에서 구현체가 필요로 하는 매개변수를 알고 있어야 한다.
- AbstractMethod 는 단순히 인터페이스를 통해서 별 다른 정보 없이 원하는 제품을 인터페이스로부터 받아올 수 있다.
- 의문점
    - FactoryMethod 의 경우 제어의 역전이나 의존성 주입과 같은 형식인데 단지 매개변수 즉, 실제 서비스에 필요한 값을 Client code 가 알고 있어야 한다는 이유로 무조건 단점이라고 말 할 수 있나?
    - 후자의 경우 협업을 하되 Client Code와 AbstractFactory 파트가 상이 할 때 한정적으로 효과적인 것은 아닌가?

- FactoryMethod 패턴의 특징 상 분기에 따라서 특정 구현부를 구성하는 코드를 작성하므로 위와 같은 `구체적인 상황` 과 `집합 '군' 분류` 와 같은 형식인 경우에 LG 회사가 고객사로 추가된 경우, `확장을 위해 기존 코드를 변경하므로 OCP( 확장에 열리고, 변경에 닫혀있어야 한다. ) 를 위반한다.`
- 그러나 AbstractFactory 와 같이 구현된다면 단순히 LG 에 대한 ConcreteProduct 와 이를 유지 보수 및 개발하는 ConcreteFactory를 추가하면 된다. 그렇기 때문에 이 와같은 상황에서는 AbstractFactory 사용함으로써 OCP 를 준수할 수 있게 된다.

- SRP 도 위와 같은 상황에서는 후자가 더 적합하게 지키고 있다고 말하는데, 어떤 시각에서 바라봐야 그런 것인지 고민한 결과 FactoryMethod 의 경우 FactoryMethod 구현체에서 비지니스 로직에 대한 흐름과 타 객체 생성, 로직 수행을 모두 하고 있지만,  AbstractFactory 의 경우 타 객체 생성 후 바로 로직을 수행하며 흐름의 결정은 개발자가 맡게 되기 때문이라고 판단했다.

- 이 상황에서 그렇다면 당연히 현범이는 AbstractFactoryCompany 에 취업하는 것이 현명할 것이다.,.,.,.,

---

# 두 패턴 간 풀어야 할 오해와  추상화 메서드 팩토리 정리

## AbstractFactory 와 FactoryMethod 비교 정리

- (출처: [인파님 블로그](https://inpa.tistory.com/entry/GOF-%F0%9F%92%A0-%EC%B6%94%EC%83%81-%ED%8C%A9%ED%86%A0%EB%A6%ACAbstract-Factory-%ED%8C%A8%ED%84%B4-%EC%A0%9C%EB%8C%80%EB%A1%9C-%EB%B0%B0%EC%9B%8C%EB%B3%B4%EC%9E%90))
    
    ![image](https://github.com/kkkwp/CS-study/assets/97209766/45898f85-f0b3-4812-b8c2-439ca23139cb)
    

## 두 패턴 간 오해하지 말아야 할 점

- (출처: [인파님 블로그](https://inpa.tistory.com/entry/GOF-%F0%9F%92%A0-%EC%B6%94%EC%83%81-%ED%8C%A9%ED%86%A0%EB%A6%ACAbstract-Factory-%ED%8C%A8%ED%84%B4-%EC%A0%9C%EB%8C%80%EB%A1%9C-%EB%B0%B0%EC%9B%8C%EB%B3%B4%EC%9E%90))
    
    ![image](https://github.com/kkkwp/CS-study/assets/97209766/3528a6ff-41b3-4ad2-a193-aefba1cd98c0)
    

---

# 느낀점

- 이 글에서 AbstractMethod Factroy 패턴의 장/단점을 말하지 않은 이유는 대부분의 디자인 패턴이 `구체적인 상황`, `비지니스 로직의 구현체` 와 같이 어떤 상황에 맞춰서 `SOLID` 원칙을 준수한다고 말하고 있어서 따로 설명하고 싶지 않았다.
- 개인적으로 `구체적인 상황`, `비지니스 로직의 구현체` 가 주어지지 않는다면 눈에 붙이면 눈, 코에 붙면 코 같은 느낌이다.
- 그럼에도 이와 같은 디자인 패턴을 사용하는 이유는 객체 지향의 핵심이라 생각되는 `다형성` 을 중심으로 `어떤 구체적인 상황` 에 맞게 이를 준수하며 좀 더 효율적으로 개발을 진행할 수 있는가에 대한 대답이라고 생각한다.
- 하지만 여기서 말하는 `구체적인 상황` 에 맞는 판단을 하기 위해서는 `도메인` 에 대한 공부가 가장 중요하다고 생각한다, 그래야 현재의 상황이 어떤 패턴을 사용하기 적합한지 판단할 수 있다고 본다.
