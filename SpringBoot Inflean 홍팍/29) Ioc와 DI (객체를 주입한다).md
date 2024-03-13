
지금까지 객체를 만들지 않았는데 클래스를 사용할 수 있었다.

그 이유는 스프링부트가 제공하는 `IoC Container` 덕분이다.
`IoC Container`는 핵심 객체를 관리하는 창고 역할이다.

그리고 `IoC Container`는 필요한 객체를 주입해주기 때문이다.

이렇게 사용자 코드 외에서 객체를 관리해주는 것을 IoC라고 하고
객체를 주입해주는것을 DI 라고한다.

![[Pasted image 20240313131119.png]]


@Autowried를 통해 IoC컨테이너에 있는 객체를 주입시킬 수 있다.

@Component를 통해 객체를 IoC컨테이너에 포함할 수 있다.

![[Pasted image 20240313140836.png]]

## 요약

##### IoC 장점
- 프로그램의 진행 흐름과 구체적인 구현을 분리시킬 수 있다.
- 개발자는 비즈니스 로직에 집중할 수 있다.
- 구현체 사이의 변경이 용이하다.
- 객체 간 의존성이 낮아진다.

##### DI 사용 장점
- 의존성이 줄어든다. (변경에 덜 취약해진다.)
- 모의 객체를 주입할 수 있기 때문에 단위 테스트가 쉬워진다.
- 가독성이 높아진다.
- 재사용성이 높아진다.

## 전체코드

#### ChefTest.java
```
package com.example.springboot_hongpark.ioc;

import static org.junit.jupiter.api.Assertions.*;

import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;

@SpringBootTest
class ChefTest {

    @Autowired
    IngredientFactory ingredientFactory;

    @Autowired
    Chef chef;

    @Test
    void 돈가스_요리하기() {
        // 준비
//        IngredientFactory ingredientFactory = new IngredientFactory();
//        Chef chef = new Chef(ingredientFactory);
        String menu = "돈가스";

        // 수행
        String food = chef.cook(menu);

        // 예상
        String expected = "한돈 등심으로 만든 돈가스";

        // 검증
        assertEquals(expected, food);
        System.out.println(food);
    }

    @Test
    void 스테이크_요리하기() {

        // 준비
//        IngredientFactory ingredientFactory = new IngredientFactory();
//        Chef chef = new Chef(ingredientFactory);
        String menu = "스테이크";

        // 수행
        String food = chef.cook(menu);

        // 예상
        String expected = "한우 꽃등심으로 만든 스테이크";

        // 검증
        assertEquals(expected, food);
        System.out.println(food);
    }

    @Test
    void 크리스티_치킨_요리하기() {
        // 준비
//        IngredientFactory ingredientFactory = new IngredientFactory();
//        Chef chef = new Chef(ingredientFactory);
        String menu = "크리스피 치킨";

        // 수행
        String food = chef.cook(menu);

        // 예상
        String expected = "국내산 10호 닭으로 만든 크리스피 치킨";

        // 검증
        assertEquals(expected, food);
        System.out.println(food);
    }
}
```

#### IngredientFactory.java
```
@Component  // 해당 클래스를 객체로 만들고, 이를 IoC 컨테이너에 등록!
public class IngredientFactory {
    public Ingredient get(String menu) {
        switch (menu) {
            case "돈가스":
                return  new Pork("한돈 등심");
            case "스테이크":
                return  new Beef("한우 꽃등심");
            case "크리스피 치킨":
                return new Chicken("국내산 10호 닭");
            default:
                return null;
        }
    }
}
```

#### Ingredient.java
```
package com.example.springboot_hongpark.ioc;

public abstract class Ingredient {
    private String name;

    public Ingredient(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }
}
```

#### Chef.java
```
package com.example.springboot_hongpark.ioc;

import org.springframework.stereotype.Component;

@Component
public class Chef {
    // 셰프는 식재료 공장을 알고있음
    private IngredientFactory ingredientFactory;

    public Chef(IngredientFactory ingredientFactory) {
        this.ingredientFactory = ingredientFactory;
    }

    public String cook(String menu) {

        // 재료 준비
        Ingredient ingredient = ingredientFactory.get(menu);

        // 요리 반환
        return ingredient.getName() + "으로 만든 " + menu;
    }
}
```

#### Beaf.java Chicken.java Pork.java
```
package com.example.springboot_hongpark.ioc;

public class Beef extends Ingredient{

    public Beef(String name) {
        super(name);
    }
}

package com.example.springboot_hongpark.ioc;

public class Chicken extends Ingredient {
    public Chicken(String name) {
        super(name);
    }
}

package com.example.springboot_hongpark.ioc;

public class Pork extends Ingredient{

    public Pork(String name) {
        super(name);
    }
}
```