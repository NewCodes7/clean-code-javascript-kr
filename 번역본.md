# 클린코드 : 자바스크립트 (한국어 번역)
> [원문 링크](https://github.com/ryanmcdermott/clean-code-javascript)<br>
> 한국어 번역 [@sbyeol3](https://github.com/sbyeol3)
## 목차

1. [도입부](#도입부)
2. [변수](#변수)
3. [함수](#함수)
4. [객체와 자료구조](#객체와-자료구조)
5. [클래스](#클래스)
6. [SOLID](#solid)
7. [테스트](#테스트)
8. [동시성](#동시성)
9. [에러 처리](#에러-처리)
10. [포맷팅](#포맷팅)
11. [주석](#주석)
12. [번역](#번역)

## 도입부

![Humorous image of software quality estimation as a count of how many expletives
you shout when reading code](https://www.osnews.com/images/comics/wtfm.jpg)

소프트웨어 엔지니어링 원리에 대해 로버트 C. 마틴이 저술한 [클린코드](https://www.amazon.com/Clean-Code-Handbook-Software-Craftsmanship/dp/0132350882)를 JavaScript에 적용시킨 것입니다.
이 글은 스타일 가이드가 아니라 JavaScript로 가독성 있고, 재사용 가능하며, 리팩토링 가능한 소프트웨어를 개발하는 것에 대한 가이드입니다.

이 글에 나와 있는 원칙을 모두 엄격하게 준수해야 하는 것은 아니며, 더 적은 수의 원칙들이 보편적으로 합의될 것입니다.
이 글은 지침서 그 이상은 아니지만 _클린 코드_ 저자들의 다년간의 집단적인 경험에 걸쳐 성문화된 것입니다.

우리의 소프트웨어 공학 기술은 이제 막 50년이 조금 넘었을 뿐이고, 우리는 여전히 많은 것들을 배우고 있습니다.
소프트웨어 아키텍처가 아키텍처 자제만큼 오래된 경우 아마도 더 까다로운 규칙을 따라야 할 수도 있습니다.
지금은 이런 지침들이 여러분과 여러분의 팀이 만드는 JavaScript 코드의 퀄리티를 평가하는 시금석이 될 것입니다.

**한 가지 더!** : 이 글을 읽는다고 바로 더 나은 소프트웨어 개발자가 되는 것은 아니며, 오랜 기간 팀으로 일을 했다고 해서 여러분이 실수하지 않는다는 것을 의미하지 않습니다.
젖은 점토가 점차 최종적인 형태로 변해가는 것과 같이 모든 코드 조각들은 초안으로 시작합니다.
마지막으로, 우리는 동료들과 함께 코드를 검토할 때 결함들을 제거할 수 있죠.
개선이 필요한 초안 코드에 대해 자책하지 마세요.
대신 코드를 두들겨 완성합시다!

## **변수**

### 의미 있고 발음 가능한 변수 이름을 사용하라

**나쁜 예:**

```javascript
const yyyymmdstr = moment().format("YYYY/MM/DD");
```

**좋은 예:**

```javascript
const currentDate = moment().format("YYYY/MM/DD");
```

**[⬆ 목차로 이동](#목차)**

### 같은 유형의 변수에 동일한 어휘를 사용하라

**나쁜 예:**

```javascript
getUserInfo();
getClientData();
getCustomerRecord();
```

**좋은 예:**

```javascript
getUser();
```

**[⬆ 목차로 이동](#목차)**

### 검색 가능한 이름을 사용하라

우리는 코드를 쓸 때보다 코드를 읽는 경우가 더 많습니다. 우리가 작성하는 코드를 읽고 검색하는 것이 중요한 이유입니다. 
여러분의 프로그램을 이해하는 데 의미가 있는 변수 이름으로 짓지 않는다면, 여러분의 코드를 읽을 사람들을 힘들게 하는 것이죠. 변수 이름을 지을 때는 검색 가능하게 지으세요.
[buddy.js](https://github.com/danielstjules/buddy.js)와 [ESLint](https://github.com/eslint/eslint/blob/660e0918933e6e7fede26bc675a0763a6b357c94/docs/rules/no-magic-numbers.md) 같은 도구들이 이름 없는 상수를 식별하는 데 도움이 될 수 있습니다.

**나쁜 예:**

```javascript
// 86400000이 도대체 어떤 의미일까요? 😡
setTimeout(blastOff, 86400000);
```

**좋은 예:**

```javascript
// 상수는 대문자로 명명하세요.
const MILLISECONDS_PER_DAY = 60 * 60 * 24 * 1000; //86400000;

setTimeout(blastOff, MILLISECONDS_PER_DAY);
```

**[⬆ 목차로 이동](#목차)**

### 이유를 알려주는 변수를 사용하라

**나쁜 예:**

```javascript
const address = "One Infinite Loop, Cupertino 95014";
const cityZipCodeRegex = /^[^,\\]+[,\\\s]+(.+?)\s*(\d{5})?$/;
saveCityZipCode(
  address.match(cityZipCodeRegex)[1],
  address.match(cityZipCodeRegex)[2]
);
```

**좋은 예:**

```javascript
const address = "One Infinite Loop, Cupertino 95014";
const cityZipCodeRegex = /^[^,\\]+[,\\\s]+(.+?)\s*(\d{5})?$/;
const [_, city, zipCode] = address.match(cityZipCodeRegex) || [];
saveCityZipCode(city, zipCode);
```

**[⬆ 목차로 이동](#목차)**

### 머리로 매핑하지 마라

명시적인 것이 암묵적인 것보다 낫습니다.

**나쁜 예:**

```javascript
const locations = ["Austin", "New York", "San Francisco"];
locations.forEach(l => {
  doStuff();
  doSomeOtherStuff();
  // ...
  // ...
  // ...
  // 잠깐, 'l'이 뭐였더라?
  dispatch(l);
});
```

**좋은 예:**

```javascript
const locations = ["Austin", "New York", "San Francisco"];
locations.forEach(location => {
  doStuff();
  doSomeOtherStuff();
  // ...
  // ...
  // ...
  dispatch(location);
});
```

**[⬆ 목차로 이동](#목차)**

### 불필요한 맥락을 추가하지 마라

여러분의 클래스나 객체가 무언가를 말해주고 있다면, 변수 이름에서 불필요하게 또 말해주지 않아도 됩니다.

**나쁜 예:**

```javascript
const Car = {
  carMake: "Honda",
  carModel: "Accord",
  carColor: "Blue"
};

function paintCar(car, color) {
  car.carColor = color;
}
```

**좋은 예:**

```javascript
const Car = {
  make: "Honda",
  model: "Accord",
  color: "Blue"
};

function paintCar(car, color) {
  car.color = color;
}
```

**[⬆ 목차로 이동](#목차)**

### 짧은 코드나 조건문 대신에 인자의 기본값을 사용하라

인자의 기본값을 주는 것이 보통은 코드 한 줄을 추가하는 것보다 더 명료합니다.
기본값을 설정하게 되면 함수는 `undefined` 값을 가지는 인자의 경우에만 기본값을 제공합니다.
다른 `''`, `""`, `false`, `null`, `0`, `NaN`와 같이 "falsy"한 값들에는 기본값이 주어지지 않습니다.

**니쁜 예:**

```javascript
function createMicrobrewery(name) {
  const breweryName = name || "Hipster Brew Co.";
  // ...
}
```

**좋은 예:**

```javascript
function createMicrobrewery(name = "Hipster Brew Co.") {
  // ...
}
```

**[⬆ 목차로 이동](#목차)**

## **함수**

### 함수의 인자는 2개 이하인 것이 이상적이다

함수 파라미터의 개수를 제한하는 것은 매우 중요합니다. 여러분의 함수를 테스트하는 것이 쉬워지기 때문이죠.
3개 이상의 파라미터를 가지는 것은 파라미터 조합 간의 폭발로 이어집니다. 조합이 많아질 수록 각각의 경우에 대해 테스트해야 할 것이 많아집니다.

1개 또는 2개의 인자를 갖는 것이 이상적이며, 3개는 가능한 피하는 것이 좋습니다.
3개보다 많은 경우는 인자를 합치는 것이 좋습니다.
일반적으로 인수를 3개 이상 갖는 함수라면 너무 많은 일을 하는 함수일 것입니다.
그렇지 않은 경우에는 대부분 상위 수준의 객체로 충분합니다.

자바스크립트를 사용하면 많은 클래스 없이도 바로 객체를 만들 수 있으므로, 많은 인수가 필요할 때는 객체를 사용할 수 있습니다.

함수가 기대하는 프로퍼티가 무엇인지 명시하기 위해 여러분은 ES2015/ES6의 비구조화 할당 문법을 사용할 수 있습니다. 이것은 몇 가지 장점이 있습니다:

1. 누군가가 함수를 봤을 때 그 함수에 어떤 속성이 사용되는지 바로 알 수 있습니다.
2. 명명된 파라미터를 시뮬레이션 하는 데 사용할 수 있습니다.
3. 비구조화는 또한 함수에 전달되는 인수 객체의 원시값들도 복제합니다. 이것은 사이드 이펙트를 방지하는 데 도움이 됩니다. (참고 📝 인수 객체에서 비구조화된 객체나 배열들은 **복제되지 않습니다**)
4. Linter는 여러분이 사용하지 않는 프로퍼티에 대해 경고해줄 수 있습니다. 비구조화한 것이 아니라면 불가능하죠.

**나쁜 예:**

```javascript
function createMenu(title, body, buttonText, cancellable) {
  // ...
}

createMenu("Foo", "Bar", "Baz", true);

```

**좋은 예:**

```javascript
function createMenu({ title, body, buttonText, cancellable }) {
  // ...
}

createMenu({
  title: "Foo",
  body: "Bar",
  buttonText: "Baz",
  cancellable: true
});
```

**[⬆ 목차로 이동](#목차)**

### 함수는 한 가지 일을 해야 한다

이 룰은 소프트웨어 공학에서 가장 중요합니다.
함수가 한 가지 일보다 더 많은 일을 할 때, 힘수를 구성하고 테스트하고 추론하는 것이 더 어려워집니다.
여러분이 함수가 한 가지 일을 하도록 분리시킬 때 그 함수는 리팩토링 하기가 쉬워지고 여러분의 코드도 더 명료해질 것입니다.
다른 것들보다 이 지침만을 잘 따른다면 여러분은 많은 개발자를 앞설 수 있습니다.

**나쁜 예:**

```javascript
function emailClients(clients) {
  clients.forEach(client => {
    const clientRecord = database.lookup(client);
    if (clientRecord.isActive()) {
      email(client);
    }
  });
}
```

**좋은 예:**

```javascript
function emailActiveClients(clients) {
  clients.filter(isActiveClient).forEach(email);
}

function isActiveClient(client) {
  const clientRecord = database.lookup(client);
  return clientRecord.isActive();
}
```

**[⬆ 목차로 이동](#목차)**

### 함수의 이름이 하는 일을 말하도록 하라

**나쁜 예:**

```javascript
function addToDate(date, month) {
  // ...
}

const date = new Date();

// 함수 이름만 보면 무엇이 더해지는 지 알기 어렵군요 🥲
addToDate(date, 1);
```

**좋은 예:**

```javascript
function addMonthToDate(month, date) {
  // ...
}

const date = new Date();
addMonthToDate(1, date);
```

**[⬆ 목차로 이동](#목차)**

### 함수는 추상화의 하나의 레벨이어야 한다

여러분의 함수가 둘 이상의 추상화를 가질 때, 그 함수는 너무 많은 일을 하고 있는 것입니다.
함수를 쪼개는 것은 함수의 재사용성을 높여주고 테스트를 용이하게 합니다.

**나쁜 예:**

```javascript
function parseBetterJSAlternative(code) {
  const REGEXES = [
    // ...
  ];

  const statements = code.split(" ");
  const tokens = [];
  REGEXES.forEach(REGEX => {
    statements.forEach(statement => {
      // ...
    });
  });

  const ast = [];
  tokens.forEach(token => {
    // lex...
  });

  ast.forEach(node => {
    // parse...
  });
}
```

**좋은 예:**

```javascript
function parseBetterJSAlternative(code) {
  const tokens = tokenize(code);
  const syntaxTree = parse(tokens);
  syntaxTree.forEach(node => {
    // parse...
  });
}

function tokenize(code) {
  const REGEXES = [
    // ...
  ];

  const statements = code.split(" ");
  const tokens = [];
  REGEXES.forEach(REGEX => {
    statements.forEach(statement => {
      tokens.push(/* ... */);
    });
  });

  return tokens;
}

function parse(tokens) {
  const syntaxTree = [];
  tokens.forEach(token => {
    syntaxTree.push(/* ... */);
  });

  return syntaxTree;
}
```

**[⬆ 목차로 이동](#목차)**

### 중복되는 코드를 제거하라

중복되는 코드를 작성하지 않도록 최선을 다해야 합니다. 로직을 변경해야 할 때 변경해야 하는 위치가 두 개 이상이 되므로 중복되는 코드는 나쁩니다.

여러분이 식당을 운영하고 있는데 토마토, 양파, 마늘, 향신료 등이 있는 재고를 추적해야 한다고 상상해보세요.
관리해야 하는 목록이 여러 개 있는 경우라면, 토마토가 들어간 요리를 손님에게 주었다면 여러 목록이 모두 업데이트 되어야 할 것입니다. 재고 목록이 한 개뿐이라면 그 목록 하나만 업데이트하면 되고요!

종종 여러분은 공통되는 부분이 많지만 두 개 이상의 약간은 다른 부분이 있기에 중복 코드를 갖고 있을 것입니다. 그 차이로 인해 거의 동일한 작업을 수행하는 두 개 이상의 개별적인 함수들이 필요하게 됩니다.
중복되는 코드를 제거하는 것은 하나의 함수/모듈/클래스 만으로도 이러한 서로 다른 것들을 처리할 수 있는 추상화를 만듦을 의미합니다.

추상화를 올바르게 하는 것은 매우 중요하기 때문에 여러분은 _클래스_ 장에서 제시되는 SOLID 원칙을 따라야 합니다.
나쁜 추상화는 중복 코드보다 더 좋지 않으니 주의하세요! 이렇게 말하기는 했지만, 여러분이 추상화를 잘 하신다면 그렇게 하세요! 반복할 수록 여러분은 변경하고 싶은 것이 생길 때마다 여러 곳을 업데이트해야 할 것입니다.

**나쁜 예:**

```javascript
function showDeveloperList(developers) {
  developers.forEach(developer => {
    const expectedSalary = developer.calculateExpectedSalary();
    const experience = developer.getExperience();
    const githubLink = developer.getGithubLink();
    const data = {
      expectedSalary,
      experience,
      githubLink
    };

    render(data);
  });
}

function showManagerList(managers) {
  managers.forEach(manager => {
    const expectedSalary = manager.calculateExpectedSalary();
    const experience = manager.getExperience();
    const portfolio = manager.getMBAProjects();
    const data = {
      expectedSalary,
      experience,
      portfolio
    };

    render(data);
  });
}
```

**좋은 예:**

```javascript
function showEmployeeList(employees) {
  employees.forEach(employee => {
    const expectedSalary = employee.calculateExpectedSalary();
    const experience = employee.getExperience();

    const data = {
      expectedSalary,
      experience
    };

    switch (employee.type) {
      case "manager":
        data.portfolio = employee.getMBAProjects();
        break;
      case "developer":
        data.githubLink = employee.getGithubLink();
        break;
    }

    render(data);
  });
}
```

**[⬆ 목차로 이동](#목차)**

### `Object.assign`으로 객체의 기본값을 설정하라

**나쁜 예:**

```javascript
const menuConfig = {
  title: null,
  body: "Bar",
  buttonText: null,
  cancellable: true
};

function createMenu(config) {
  config.title = config.title || "Foo";
  config.body = config.body || "Bar";
  config.buttonText = config.buttonText || "Baz";
  config.cancellable =
    config.cancellable !== undefined ? config.cancellable : true;
}

createMenu(menuConfig);
```

**좋은 예:**

```javascript
const menuConfig = {
  title: "Order",
  // User did not include 'body' key
  buttonText: "Send",
  cancellable: true
};

function createMenu(config) {
  let finalConfig = Object.assign(
    {
      title: "Foo",
      body: "Bar",
      buttonText: "Baz",
      cancellable: true
    },
    config
  );
  return finalConfig
  // config는 이제 다음과 같습니다 : {title: "Order", body: "Bar", buttonText: "Send", cancellable: true}
  // ...
}

createMenu(menuConfig);
```

**[⬆ 목차로 이동](#목차)**

### 함수의 파라미터로 플래그 변수를 사용하지 마라

플래그 변수는 이 함수가 둘 이상의 일을 한다는 것을 여러분의 사용자에게 알려줍니다.
함수들은 한 가지 일을 해야 합니다.
boolean 값을 기반으로 다른 코드를 따라야 하는 경우 함수를 분할하세요.

**나쁜 예:**

```javascript
function createFile(name, temp) {
  if (temp) {
    fs.create(`./temp/${name}`);
  } else {
    fs.create(name);
  }
}
```

**좋은 예:**

```javascript
function createFile(name) {
  fs.create(name);
}

function createTempFile(name) {
  createFile(`./temp/${name}`);
}
```

**[⬆ 목차로 이동](#목차)**

### 사이트 이펙트를 피하라 1

함수가 값을 받아 다른 값(들)을 리턴하는 경우 함수는 사이드 이펙트를 만들어 냅니다. 
사이드 이펙트는 파일에 쓰거나, 어떤 전역 변수를 수정하거나, 어쩌면 실수로 당신의 모든 돈을 낯선 이에게 송금해버리는 것이 될 수도 있죠.

이제 여러분은 가끔 프로그램에서 사이드 이펙트를 가질 필요가 있습니다.  이전 예시와 같이 여러분은 파일에 write 작업이 필요할 수도 있죠.
여러분이 하고 있는 것들을 중앙화 하는 것이 좋습니다.
특정 파일에 쓰는 함수나 클래스를 여러 개 가지지 마세요.
그것을 하는 단 하나를 가지세요.

핵심은 어떠한 구조 없이 객체들 간에 상태를 공유하거나, 무엇으로든 써질 수 있는 mutable한 데이터 타입을 사용하거나, 사이드 이펙트가 발생하는 곳을 중앙화 하지 않는 일반적인 함정을 피하는 것입니다.
이런 함정들을 피할 수 있다면 여러분은 대부분의 다른 프로그래머들보다 더 행복해질 겁니다.

**나쁜 예:**

```javascript
// 다음 함수에 의해 참조되는 전역 변수
// name을 사용하는 다른 함수가 있다면, 배열이 되어 깨져버릴지도 몰라요.
let name = "Ryan McDermott";

function splitIntoFirstAndLastName() {
  name = name.split(" ");
}

splitIntoFirstAndLastName();

console.log(name); // ['Ryan', 'McDermott'];
```

**좋은 예:**

```javascript
function splitIntoFirstAndLastName(name) {
  return name.split(" ");
}

const name = "Ryan McDermott";
const newName = splitIntoFirstAndLastName(name);

console.log(name); // 'Ryan McDermott';
console.log(newName); // ['Ryan', 'McDermott'];
```

**[⬆ 목차로 이동](#목차)**

### 사이드 이펙트를 피하라 2

자바스크립트에서 어떤 값들은 변경이 불가능(불변, immutable)하고 어떤 값들은 변경할 수 있습니다(가변, mutable).
객체나 배열은 변경 가능한 값들의 두 종류이기 때문에 이 값들은 함수에 파라미터로 전달할 때 조심히 다룰 필요가 있습니다.
자바스크립트 함수는 객체의 프로퍼티를 변경하거나 배열의 요소들을 바꿀 수 있기 때문에 다른 곳에서 쉽게 버그를 일으킬 수 있습니다

장바구니를 나타내는 배열 파라미터를 받아들이는 함수가 있다고 가정해보세요. 만약 함수가 구매할 아이템을 추가하는 것과 같이 장바구니 배열에 변경사항을 만든다면 동일한 `장바구니` 배열을 사용하는 다른 함수들도 아이템 추가에 의해 영향을 받게 될 겁니다.
좋아요, 하지만 안 좋을 수도 있어요. 좋지 않은 상황을 상상해봅시다.

사용자가 "구매" 버튼을 클릭하면 네트워크 요청을 생성하여 서버에 `cart` 배열을 보내는 `purchase` 함수를 호출됩니다.
네트워크 연결 불량으로 `purchase` 함수는 네트워크 요청을 계속 재시도 합니다.
그 사이 네트워크 요청이 시작되기 전에 사용자가 실제로 원하지 않는 항목의 "장바구니에 추가" 버튼을 실수로 클릭한 경우에는 어떻게 해야 할까요? 네트워크 요청이 시작되었다면, `purchase` 함수는 `cart` 배열이 변경되었기 때문에 실수로 추가된 아이템을 전송할 겁니다.

가장 좋은 해결책은 `addItemToCart` 함수가 언제나 `cart`를 복제하여 수정한 후 복제본을 반환하는 것입니다.
이 방법은 여전히 이전 장바구니 값을 사용하기 때문에 변경 사항의 영향을 받지 않음을 보장합니다.

이 접근 방식에 대한 두 가지 주의사항이 있습니다.

1. 인풋 객체를 실제로 수정하고 싶을 때도 있지만 실무에서 프로그래밍을 하다보면 실제로는 그런 경우가 드물다는 것을 아실 겁니다. 대부분의 함수들은 사이드 이펙트가 없어 리팩토링이 가능합니다!

2. 거대한 객체를 복제하는 것은 퍼포먼스 측면에서 비용이 많이 들 수 있습니다. 다행히도 직접 복제할 때보다 빠르고 메모리를 덜 쓰는 [좋은 라이브러리들](https://facebook.github.io/immutable-js/)이 있기 때문에 실제로는 큰 이슈가 아닙니다.

**나쁜 예:**

```javascript
const addItemToCart = (cart, item) => {
  cart.push({ item, date: Date.now() });
};
```

**좋은 예:**

```javascript
const addItemToCart = (cart, item) => {
  return [...cart, { item, date: Date.now() }];
};
```

**[⬆ 목차로 이동](#목차)**

### 전역 함수에 Write 하지 마라

전역 값들을 오염시키는 것은 자바스크립트에서 매우 나쁜 방법입니다. 다른 라이브러리와 충돌할 수 있으며 당신의 API를 사용하는 사용자는 프로덕션에서 예외가 발생할 때까지는 아무것도 알 수 없습니다. 
한 가지 예시를 생각해볼까요. 
만약 여러분이 JavaScript의 네이티브 배열 메소드를 확장하여 두 배열의 차이를 보여주는 `diff` 메소드를 사용하고 싶다면 어떻게 해야 할까요? 여러분은 `Array.prototype`에 새로운 함수를 쓸 수 있을 겁니다. 
이렇게 하면 같은 작업을 하려는 다른 라이브러리와 충돌할 수도 있죠. 다른 라이브러리가 배열의 첫 요소와 마지막 요소의 차이를 찾고자 `diff`를 사용했다면 어떻게 될까요?
이 예시는 ES2015/ES6 클래스를 사용하여 `Array`를 상속받는 것이 훨씬 더 나은 이유를 보여줍니다.

**나쁜 예:**

```javascript
Array.prototype.diff = function diff(comparisonArray) {
  const hash = new Set(comparisonArray);
  return this.filter(elem => !hash.has(elem));
};
```

**좋은 예:**

```javascript
class SuperArray extends Array {
  diff(comparisonArray) {
    const hash = new Set(comparisonArray);
    return this.filter(elem => !hash.has(elem));
  }
}
```

**[⬆ 목차로 이동](#목차)**

### 명령형 프로그래밍보다 함수형 프로그래밍을 선호하라

자바스크립트는 Haskell처럼 함수형 언어는 아니지만, 함수형 언어와 같은 면이 있습니다.
함수형 언어들은 더 명료하고 테스트하기 쉽습니다.
가능할 때 여러분은 함수형 프로그래밍 스타일을 선호하세요.

**나쁜 예:**

```javascript
const programmerOutput = [
  {
    name: "Uncle Bobby",
    linesOfCode: 500
  },
  {
    name: "Suzie Q",
    linesOfCode: 1500
  },
  {
    name: "Jimmy Gosling",
    linesOfCode: 150
  },
  {
    name: "Gracie Hopper",
    linesOfCode: 1000
  }
];

let totalOutput = 0;

for (let i = 0; i < programmerOutput.length; i++) {
  totalOutput += programmerOutput[i].linesOfCode;
}
```

**좋은 예:**

```javascript
const programmerOutput = [
  {
    name: "Uncle Bobby",
    linesOfCode: 500
  },
  {
    name: "Suzie Q",
    linesOfCode: 1500
  },
  {
    name: "Jimmy Gosling",
    linesOfCode: 150
  },
  {
    name: "Gracie Hopper",
    linesOfCode: 1000
  }
];

const totalOutput = programmerOutput.reduce(
  (totalLines, output) => totalLines + output.linesOfCode,
  0
);
```

**[⬆ 목차로 이동](#목차)**

### 조건문을 캡슐화하라

**나쁜 예:**

```javascript
if (fsm.state === "fetching" && isEmpty(listNode)) {
  // ...
}
```

**좋은 예:**

```javascript
function shouldShowSpinner(fsm, listNode) {
  return fsm.state === "fetching" && isEmpty(listNode);
}

if (shouldShowSpinner(fsmInstance, listNodeInstance)) {
  // ...
}
```

**[⬆ 목차로 이동](#목차)**

### 부정 조건문을 지양하라

**나쁜 예:**

```javascript
function isDOMNodeNotPresent(node) {
  // ...
}

if (!isDOMNodeNotPresent(node)) {
  // ...
}
```

**좋은 예:**

```javascript
function isDOMNodePresent(node) {
  // ...
}

if (isDOMNodePresent(node)) {
  // ...
}
```

**[⬆ 목차로 이동](#목차)**

### 조건문을 피하라

이 원칙은 불가능한 일처럼 보일 것입니다. 이 말을 처음 듣자마자 대부분의 사람들은 "`if`문 없이 어떻게 뭘 하겠어요?"라고 말합니다.
그 답은 많은 경우에서 같은 일을 하기 위해 다형성을 사용할 수 있다는 것입니다.
두 번째 질문은 보통 "음 좋네요, 근데 왜 제가 그렇게 하고 싶겠어요?" 입니다.
이전에 우리가 배웠던 클린 코드 개념에 답이 있습니다. 함수는 오직 한 가지 일만 해야 한다는 것이죠.
여러분이 `if`문을 사용하는 클래스와 함수를 가질 때, 여러분은 그 함수가 두 가지 이상의 일을 한다고 말하는 것입니다.
오직 한 가지 일을 해야 한다는 것을 기억하세요.

**나쁜 예:**

```javascript
class Airplane {
  // ...
  getCruisingAltitude() {
    switch (this.type) {
      case "777":
        return this.getMaxAltitude() - this.getPassengerCount();
      case "Air Force One":
        return this.getMaxAltitude();
      case "Cessna":
        return this.getMaxAltitude() - this.getFuelExpenditure();
    }
  }
}
```

**좋은 예:**

```javascript
class Airplane {
  // ...
}

class Boeing777 extends Airplane {
  // ...
  getCruisingAltitude() {
    return this.getMaxAltitude() - this.getPassengerCount();
  }
}

class AirForceOne extends Airplane {
  // ...
  getCruisingAltitude() {
    return this.getMaxAltitude();
  }
}

class Cessna extends Airplane {
  // ...
  getCruisingAltitude() {
    return this.getMaxAltitude() - this.getFuelExpenditure();
  }
}
```

**[⬆ 목차로 이동](#목차)**

### 타입 체크를 피하라 1

자바스크립트의 함수는 어떤 타입의 인자든 받을 수 있습니다. 때때로 여러분은 이러한 타입으로부터의 자유에 사로잡혀 함수에서 타입 체크를 하고자 할 것입니다.
타입 체크를 피할 수 있는 많은 방법이 있습니다. 첫 번째는 일관된 API입니다.

**나쁜 예:**

```javascript
function travelToTexas(vehicle) {
  if (vehicle instanceof Bicycle) {
    vehicle.pedal(this.currentLocation, new Location("texas"));
  } else if (vehicle instanceof Car) {
    vehicle.drive(this.currentLocation, new Location("texas"));
  }
}
```

**좋은 예:**

```javascript
function travelToTexas(vehicle) {
  vehicle.move(this.currentLocation, new Location("texas"));
}
```

**[⬆ 목차로 이동](#목차)**

### 타입 체크를 피하라 2

문자열이나 정수와 같은 원시값으로 작업을 하고 있다면 여러분은 다형성을 사용할 수는 없겠지만 여전히 타입 체크가 필요할 수 있습니다.
그렇다면 타입스크립트를 고려해보세요. 타입스크립트는 표준 자바스크립트 구문 위에 정적 타이핑 기능을 제공하기 때문에 자바스크립트의 좋은 대안이 됩니다.
일반 자바스크립트에서 "타입 안정성"을 보충하기 위해서는 장황한 말이 필요해지므로 가독성이 저하됩니다.
여러분의 자바스크립트 코드를 깨끗하게 유지하고, 좋은 테스트를 작성하고, 좋은 코드 리뷰를 하세요.
그렇지 않으면 타입스크립트를 사용하세요 (말했듯이 타입스크립트는 좋은 대안입니다!).

**나쁜 예:**

```javascript
function combine(val1, val2) {
  if (
    (typeof val1 === "number" && typeof val2 === "number") ||
    (typeof val1 === "string" && typeof val2 === "string")
  ) {
    return val1 + val2;
  }

  throw new Error("Must be of type String or Number");
}
```

**좋은 예:**

```javascript
function combine(val1, val2) {
  return val1 + val2;
}
```

**[⬆ 목차로 이동](#목차)**

### 지나치게 최적화하지 마라

최신 브라우저는 런타임에 많은 최적화를 수행합니다. 여러분이 작업 시간의 대부분을 최적화 하는 데 사용한다면 시간을 낭비하고 있는 겁니다.
최적화가 부족한 곳을 볼 수 있는 [좋은 리소스들](https://github.com/petkaantonov/bluebird/wiki/Optimization-killers)이 있습니다.
문제점들을 수정할 때까지 타겟으로 삼으세요.

**나쁜 예:**

```javascript
// 오래된 브라우저에서는 `list.length`이 캐싱되지 않아 다시 계산을 해야 하므로 비용이 많이 들 수 있습니다.
// 최신 브라우저에서는 최적화 됩니다.
for (let i = 0, len = list.length; i < len; i++) {
  // ...
}
```

**좋은 예:**

```javascript
for (let i = 0; i < list.length; i++) {
  // ...
}
```

**[⬆ 목차로 이동](#목차)**

### 죽은 코드는 제거하라

죽은 코드는 중복된 코드처럼 좋지 않습니다. 여러분의 코드에 죽은 코드를 냅둘 이유가 없습니다.
더 이상 호출되지 않는다면 지워 버리세요! 만약 그 코드가 다시 필요하더라도 버전 기록에서는 안전하니까요.

**나쁜 예:**

```javascript
function oldRequestModule(url) {
  // ...
}

function newRequestModule(url) {
  // ...
}

const req = newRequestModule;
inventoryTracker("apples", req, "www.inventory-awesome.io");
```

**좋은 예:**

```javascript
function newRequestModule(url) {
  // ...
}

const req = newRequestModule;
inventoryTracker("apples", req, "www.inventory-awesome.io");
```

**[⬆ 목차로 이동](#목차)**

## 객체와 자료구조

### getter와 setter를 사용하라

getter와 setter를 사용하여 객체의 데이터에 접근하는 것은 단순하게 객체에서 프로퍼티를 찾는 것보다 낫습니다.
"왜?"냐고 물으실 수도 있습니다. 글쎄요, 아래 이유들이 있습니다.

- 여러분이 객체 프로퍼티를 가져오는 것을 넘어 무언가를 하고 싶을 때 여러분은 코드베이스의 모든 액세스 권한을 찾아 변경할 필요는 없습니다.
- `set`을 수행할 때 검증 과정을 간단하게 추가할 수 있습니다.
- 내부 표현을 캡슐화합니다.
- 로깅과 에러 핸들링이 쉽습니다.
- 객체의 프로퍼티를 로드하는 것을 느리게 할 수 있습니다.

**나쁜 예:**

```javascript
function makeBankAccount() {
  // ...

  return {
    balance: 0
    // ...
  };
}

const account = makeBankAccount();
account.balance = 100;
```

**좋은 예:**

```javascript
function makeBankAccount() {
  // this one is private
  let balance = 0;

  // a "getter", made public via the returned object below
  function getBalance() {
    return balance;
  }

  // a "setter", made public via the returned object below
  function setBalance(amount) {
    // ... validate before updating the balance
    balance = amount;
  }

  return {
    // ...
    getBalance,
    setBalance
  };
}

const account = makeBankAccount();
account.setBalance(100);
```

**[⬆ 목차로 이동](#목차)**

### 객체가 프라이빗 멤버를 갖게 해라

이 원칙은 클로저를 통해 수행될 수 있습니다. (ES5 이하)

**나쁜 예:**

```javascript
const Employee = function(name) {
  this.name = name;
};

Employee.prototype.getName = function getName() {
  return this.name;
};

const employee = new Employee("John Doe");
console.log(`Employee name: ${employee.getName()}`); // Employee name: John Doe
delete employee.name;
console.log(`Employee name: ${employee.getName()}`); // Employee name: undefined
```

**좋은 예:**

```javascript
function makeEmployee(name) {
  return {
    getName() {
      return name;
    }
  };
}

const employee = makeEmployee("John Doe");
console.log(`Employee name: ${employee.getName()}`); // Employee name: John Doe
delete employee.name;
console.log(`Employee name: ${employee.getName()}`); // Employee name: John Doe
```

**[⬆ 목차로 이동](#목차)**

## 클래스

### ES2015/ES6 클래스보다 ES5 일반 함수를 선호하라

고전적인 ES5 클래스의 읽기 가능한 클래스 상속, 구성, 메서드 정의를 얻는 것은 어렵습니다.
상속이 필요하다면 ES2015/ES6의 클래스를 사용하세요.
그러나 더 크고 복잡한 객체가 필요할 때까지 클래스 보다는 작은 함수를 선호하는 것이 좋습니다.

**나쁜 예:**

```javascript
const Animal = function(age) {
  if (!(this instanceof Animal)) {
    throw new Error("Instantiate Animal with `new`");
  }

  this.age = age;
};

Animal.prototype.move = function move() {};

const Mammal = function(age, furColor) {
  if (!(this instanceof Mammal)) {
    throw new Error("Instantiate Mammal with `new`");
  }

  Animal.call(this, age);
  this.furColor = furColor;
};

Mammal.prototype = Object.create(Animal.prototype);
Mammal.prototype.constructor = Mammal;
Mammal.prototype.liveBirth = function liveBirth() {};

const Human = function(age, furColor, languageSpoken) {
  if (!(this instanceof Human)) {
    throw new Error("Instantiate Human with `new`");
  }

  Mammal.call(this, age, furColor);
  this.languageSpoken = languageSpoken;
};

Human.prototype = Object.create(Mammal.prototype);
Human.prototype.constructor = Human;
Human.prototype.speak = function speak() {};
```

**좋은 예:**

```javascript
class Animal {
  constructor(age) {
    this.age = age;
  }

  move() {
    /* ... */
  }
}

class Mammal extends Animal {
  constructor(age, furColor) {
    super(age);
    this.furColor = furColor;
  }

  liveBirth() {
    /* ... */
  }
}

class Human extends Mammal {
  constructor(age, furColor, languageSpoken) {
    super(age, furColor);
    this.languageSpoken = languageSpoken;
  }

  speak() {
    /* ... */
  }
}
```

**[⬆ 목차로 이동](#목차)**

### 메서드 체이닝을 사용하라

이 패턴은 자바스크립트에서 아주 유용하며 jQuery나 Loadsh와 같은 다른 많은 라이브러리에서도 보입니다.
메서드 체이닝은 코드의 표현을 더 풍부하게 해주고 장황함을 줄입니다.
이러한 이유로 메서드 체이닝을 사용하고 얼마나 코드가 깔끔해지는지 살펴보라고 합니다.
여러분의 클래스 함수에서, 모든 함수의 끝에서 단순히 `this`를 반환하고 더 많은 클래스 메서드를 연결할 수 있습니다.

**나쁜 예:**

```javascript
class Car {
  constructor(make, model, color) {
    this.make = make;
    this.model = model;
    this.color = color;
  }

  setMake(make) {
    this.make = make;
  }

  setModel(model) {
    this.model = model;
  }

  setColor(color) {
    this.color = color;
  }

  save() {
    console.log(this.make, this.model, this.color);
  }
}

const car = new Car("Ford", "F-150", "red");
car.setColor("pink");
car.save();
```

**좋은 예:**

```javascript
class Car {
  constructor(make, model, color) {
    this.make = make;
    this.model = model;
    this.color = color;
  }

  setMake(make) {
    this.make = make;
    // NOTE: Returning this for chaining
    return this;
  }

  setModel(model) {
    this.model = model;
    // NOTE: Returning this for chaining
    return this;
  }

  setColor(color) {
    this.color = color;
    // NOTE: Returning this for chaining
    return this;
  }

  save() {
    console.log(this.make, this.model, this.color);
    // NOTE: Returning this for chaining
    return this;
  }
}

const car = new Car("Ford", "F-150", "red").setColor("pink").save();
```

**[⬆ 목차로 이동](#목차)**

### 상속보다 구성을 선호하라


The Gang of Four가 저술한 [_디자인 패턴_](https://en.wikipedia.org/wiki/Design_Patterns)에서 잘 알려진 바와 같이 여러분은 가능한 상속보다는 구성을 더 선호해야 합니다.
상속을 사용하는 좋은 이유들이 있고 구성을 사용해야 하는 더 좋은 이유들이 있습니다.
이 격언의 핵심은 여러분이 본능적으로 상속을 원한다면 구성이 여러분의 문제를 더 잘 모델링할 수도 있다고 생각하라는 것입니다. 어떤 경우에는 그렇습니다.

여러분은 "상속을 언제 사용해야 하나요?"라는 궁금증이 생길 수 있습니다.
이것은 어떤 문제이냐에 따라 다르지만 아래 목록은 구성보다 상속이 더 타당한 때를 보여주는 경우입니다.

1. 상속이 "has-a"가 아닌 "is-a" 관계를 보여주는 경우 (사람->동물 vs. 사용자->사용자 상세).
2. 베이스 클래스로부터 코드를 재사용하는 경우 (사람은 다른 동물들처럼 움직일 수 있음).
3. 베이스 클래스를 변경하여 파생된 클래스를 전역적으로 변경하려는 경우 (모든 동물은 움직일 때 칼로리 소비량이 바뀜).

**나쁜 예:**

```javascript
class Employee {
  constructor(name, email) {
    this.name = name;
    this.email = email;
  }

  // ...
}

// Employee는 세금 데이터를 "가지기 때문에" 좋지 않습니다.
// EmployeeTaxData는 Employee의 타입이 아닙니다.
class EmployeeTaxData extends Employee {
  constructor(ssn, salary) {
    super();
    this.ssn = ssn;
    this.salary = salary;
  }

  // ...
}
```

**좋은 예:**

```javascript
class EmployeeTaxData {
  constructor(ssn, salary) {
    this.ssn = ssn;
    this.salary = salary;
  }

  // ...
}

class Employee {
  constructor(name, email) {
    this.name = name;
    this.email = email;
  }

  setTaxData(ssn, salary) {
    this.taxData = new EmployeeTaxData(ssn, salary);
  }
  // ...
}
```

**[⬆ 목차로 이동](#목차)**

## **SOLID**

### 단일 책임 원칙 (SRP)

클린 코드에서 명시된 것과 같이 "클래스가 변경되어야 하는 이유가 한 가지보다 더 많으면 안됩니다".
하나의 클래스를 다양한 기능으로 포장하는 건 매력적입니다. 여러분이 비행기를 탈 때 하나의 캐리어만 가지고 탈 수 있을 때처럼 말이죠.
그러나 문제는 여러분의 클래스가 개념적으로 응집되지 않고 변경될 많은 이유를 준다는 겁니다.
클래스를 변경시켜야 하는 시간을 줄이는 것은 중요합니다.
너무 많은 기능이 하나의 클래스에 있고 그 중의 하나를 수정해야 한다면, 여러분의 코드베이스를 기반으로 하고 있는 다른 모듈에 어떤 영향을 줄지 알기가 어려워지기 때문입니다.

**나쁜 예:**

```javascript
class UserSettings {
  constructor(user) {
    this.user = user;
  }

  changeSettings(settings) {
    if (this.verifyCredentials()) {
      // ...
    }
  }

  verifyCredentials() {
    // ...
  }
}
```

**좋은 예:**

```javascript
class UserAuth {
  constructor(user) {
    this.user = user;
  }

  verifyCredentials() {
    // ...
  }
}

class UserSettings {
  constructor(user) {
    this.user = user;
    this.auth = new UserAuth(user);
  }

  changeSettings(settings) {
    if (this.auth.verifyCredentials()) {
      // ...
    }
  }
}
```

**[⬆ 목차로 이동](#목차)**

### 개방/폐쇄 원칙 (OCP)

Bertrand Meyer가 말했던 것처럼 "소프트웨어 엔티티(클래스, 모듈, 함수 등)는 확장에 열려 있어야 하고, 변경에 닫혀 있어야" 합니다. 그런데 이게 무슨 뜻일까요?
이 원칙은 기본적으로 여러분이 사용자에게 이미 존재하는 코드를 변경하지 않고 새로운 기능을 추가하는 건 허용한다는 것을 의미합니다.

**나쁜 예:**

```javascript
class AjaxAdapter extends Adapter {
  constructor() {
    super();
    this.name = "ajaxAdapter";
  }
}

class NodeAdapter extends Adapter {
  constructor() {
    super();
    this.name = "nodeAdapter";
  }
}

class HttpRequester {
  constructor(adapter) {
    this.adapter = adapter;
  }

  fetch(url) {
    if (this.adapter.name === "ajaxAdapter") {
      return makeAjaxCall(url).then(response => {
        // transform response and return
      });
    } else if (this.adapter.name === "nodeAdapter") {
      return makeHttpCall(url).then(response => {
        // transform response and return
      });
    }
  }
}

function makeAjaxCall(url) {
  // request and return promise
}

function makeHttpCall(url) {
  // request and return promise
}
```

**좋은 예:**

```javascript
class AjaxAdapter extends Adapter {
  constructor() {
    super();
    this.name = "ajaxAdapter";
  }

  request(url) {
    // request and return promise
  }
}

class NodeAdapter extends Adapter {
  constructor() {
    super();
    this.name = "nodeAdapter";
  }

  request(url) {
    // request and return promise
  }
}

class HttpRequester {
  constructor(adapter) {
    this.adapter = adapter;
  }

  fetch(url) {
    return this.adapter.request(url).then(response => {
      // transform response and return
    });
  }
}
```

**[⬆ 목차로 이동](#목차)**

### 리스코프 치환 원칙 (LSP)

이 원칙은 매우 간단한 개념에 대한 무서운 용어입니다.
"S가 T의 하위유형이라면, T 타입의 객체가 프로그램의 바람직한 특성들(정확성, 수행된 작업 등)을 변경하지 않고 S 타입의 객체로 대체될 수 있다"는 것으로 정의됩니다.
훨씬 더 무서운 정의네요.

이 원칙에 대한 가장 좋은 설명은 여러분이 부모 클래스와 자식 클래스가 있을 때, 이상한 결과를 발생시키지 않으며 두 클래스끼리 상호교환될 수 있다는 것입니다.
조금 혼란스러울 수도 있지만 고전적인 정사각형-직사각형 예시를 살펴볼까요.
수학적으로 정사각형은 직사각형이지만, 상속을 통해 여러분이 "is-a" 관계를 모델링했다면 금방 문제에 봉착하게 될 것입니다.

**나쁜 예:**

```javascript
class Rectangle {
  constructor() {
    this.width = 0;
    this.height = 0;
  }

  setColor(color) {
    // ...
  }

  render(area) {
    // ...
  }

  setWidth(width) {
    this.width = width;
  }

  setHeight(height) {
    this.height = height;
  }

  getArea() {
    return this.width * this.height;
  }
}

class Square extends Rectangle {
  setWidth(width) {
    this.width = width;
    this.height = width;
  }

  setHeight(height) {
    this.width = height;
    this.height = height;
  }
}

function renderLargeRectangles(rectangles) {
  rectangles.forEach(rectangle => {
    rectangle.setWidth(4);
    rectangle.setHeight(5);
    const area = rectangle.getArea(); // 문제: 정사각형은 20이 아닌 25를 리턴함
    rectangle.render(area);
  });
}

const rectangles = [new Rectangle(), new Rectangle(), new Square()];
renderLargeRectangles(rectangles);
```

**좋은 예:**

```javascript
class Shape {
  setColor(color) {
    // ...
  }

  render(area) {
    // ...
  }
}

class Rectangle extends Shape {
  constructor(width, height) {
    super();
    this.width = width;
    this.height = height;
  }

  getArea() {
    return this.width * this.height;
  }
}

class Square extends Shape {
  constructor(length) {
    super();
    this.length = length;
  }

  getArea() {
    return this.length * this.length;
  }
}

function renderLargeShapes(shapes) {
  shapes.forEach(shape => {
    const area = shape.getArea();
    shape.render(area);
  });
}

const shapes = [new Rectangle(4, 5), new Rectangle(4, 5), new Square(5)];
renderLargeShapes(shapes);
```

**[⬆ 목차로 이동](#목차)**

### 인터페이스 분리 원칙 (ISP)

자바스크립트는 인터페이스를 갖지 않으므로 다른 언어들처럼 엄격하게 적용되는 원칙은 아닙니다.
그러나 자바스크립트의 타입 시스템의 부족 때문에 중요하고 관련있는 원칙이기도 합니다.

ISP 원칙은 "클라이언트는 그들이 사용하지 않는 인터페이스에 의존하도록 강제받지 않는다"는 것을 의미합니다.
인터페이스는 duck 타이핑때문에 자바스크립트에서 암시적인 계약입니다.

> 역 : [duck typing](https://en.wikipedia.org/wiki/Duck_typing)이란 동적 타이핑의 한 종류로, 인터페이스 구현으로 타입을 구분하지 않고 변수와 메소드에 의해 타입이 결정되는 것을 의미합니다.

자바스크립트에서 이 원칙을 설명하는 좋은 예시는 클래스가 무수히 많은 세팅 객체가 필요한 경우입니다.
클라이언트가 매우 많은 옵션을 설정하지 않는 것이 좋은데, 대부분의 경우 모든 설정이 필요한 것은 아니기 때문입니다.
설정을 선택적으로 하는 것이 "뚱뚱한 인터페이스"를 방지하는 데 도움이 됩니다.

**나쁜 예:**

```javascript
class DOMTraverser {
  constructor(settings) {
    this.settings = settings;
    this.setup();
  }

  setup() {
    this.rootNode = this.settings.rootNode;
    this.settings.animationModule.setup();
  }

  traverse() {
    // ...
  }
}

const $ = new DOMTraverser({
  rootNode: document.getElementsByTagName("body"),
  animationModule() {} // Most of the time, we won't need to animate when traversing.
  // ...
});
```

**좋은 예:**

```javascript
class DOMTraverser {
  constructor(settings) {
    this.settings = settings;
    this.options = settings.options;
    this.setup();
  }

  setup() {
    this.rootNode = this.settings.rootNode;
    this.setupOptions();
  }

  setupOptions() {
    if (this.options.animationModule) {
      // ...
    }
  }

  traverse() {
    // ...
  }
}

const $ = new DOMTraverser({
  rootNode: document.getElementsByTagName("body"),
  options: {
    animationModule() {}
  }
});
```

**[⬆ 목차로 이동](#목차)**

### 의존 역전 원칙 (DIP)

이 원칙은 두 가지 중요한 사실을 말해줍니다:

1. 고수준의 모듈은 저수준의 모듈에 의존적이지 않아야 합니다. 두 모듈은 추상화에 의존적이어야 합니다.
2. 추상화는 세부사항에 의존적이지 않아야 합니다. 세부사항은 추상화에 의존적이어야 합니다.

처음에는 이 원칙을 이해하는 것은 어려울 수 있지만, 여러분이 AngularJS로 작업을 해보셨다면, 의존성 주입(DI)의 형태로 이 원칙을 구현해본 적이 있습니다.
두 가지가 동일한 개념은 아니지만, DIP 원칙은 고수준 모듈이 저수준 모듈의 세부사항을 알고 이를 설정하는 것을 방지합니다.
이 원칙의 가장 큰 장점은 두 모듈 간의 의존성을 낮춰준다는 것이죠.
모듈 간의 의존성이 높을수록 리팩토링을 어렵게 만들기 때문에 이는 아주 나쁜 개발 패턴입니다.

이전에 말했듯이 자바스크립트는 인터페이스가 없으므로 추상화에 의존하는 것은 암묵적인 약속입니다.
즉, 다른 객체나 클래스에 노출되는 메소드와 프로퍼티가 암묵적인 약속이 되는 것이죠.
아래 예제에서 암묵적인 약속은 `InventoryTracker`에 대한 모든 요청 모듈이 `requestItems` 메소드를 가진다는 것입니다.

**나쁜 예:**

```javascript
class InventoryRequester {
  constructor() {
    this.REQ_METHODS = ["HTTP"];
  }

  requestItem(item) {
    // ...
  }
}

class InventoryTracker {
  constructor(items) {
    this.items = items;

    // 🙅 : 특정 요청 구현에 대한 의존성을 만들었으므로 좋지 않아요.
    // requestItems이 request 메소드에 의해 달라지도록 해야 합니다.
    this.requester = new InventoryRequester();
  }

  requestItems() {
    this.items.forEach(item => {
      this.requester.requestItem(item);
    });
  }
}

const inventoryTracker = new InventoryTracker(["apples", "bananas"]);
inventoryTracker.requestItems();
```

**좋은 예:**

```javascript
class InventoryTracker {
  constructor(items, requester) {
    this.items = items;
    this.requester = requester;
  }

  requestItems() {
    this.items.forEach(item => {
      this.requester.requestItem(item);
    });
  }
}

class InventoryRequesterV1 {
  constructor() {
    this.REQ_METHODS = ["HTTP"];
  }

  requestItem(item) {
    // ...
  }
}

class InventoryRequesterV2 {
  constructor() {
    this.REQ_METHODS = ["WS"];
  }

  requestItem(item) {
    // ...
  }
}

// 의존성을 외부에서 만들어 주입함으로써, 요청 모듈을 더 좋은 웹소켓 모듈로 쉽게 바꿀 수 있어요.
const inventoryTracker = new InventoryTracker(
  ["apples", "bananas"],
  new InventoryRequesterV2()
);
inventoryTracker.requestItems();
```

**[⬆ 목차로 이동](#목차)**

## 테스트

테스트는 배포하는 것보다 중요합니다.
여러분이 테스트를 하지 않거나 충분히 테스트를 하지 않았다면, 버그가 발생하지 않을 거라고 확신할 수 없는 코드가 됩니다.
테스트 커버리지를 얼마나 할 것인지는 팀마다 다르지만, 100% 커버리지를 갖게 된다면 매우 높은 코드의 신뢰성과 개발자의 내적 평화도 함께 가지게 됩니다.
이 말은 좋은 테스트 프레임워크와 더불어 [좋은 커버리지 툴]((https://gotwarlost.github.io/istanbul/))을 가지는 것이 필요하다는 것이죠.

테스트를 작성하지 않는 것은 변명의 여지가 없습니다. [다양하고 좋은 자바스크립트 테스트 프레임워크들]((https://jstherightway.org/#testing-tools))이 많으니 여러분의 팀에 맞는 프레임워크를 찾아보세요.
여러분의 팀과 잘 맞는 프레임워크를 골랐다면, 모든 새로운 기능/모듈을 작성할 때 항상 테스트를 작성하는 것을 목표로 하세요.
여러분의 팀이 테스트 주도 개발 방법론(TDD)을 선호한다면 아주 좋은 선택일 수 있습니다.
그러나 어떤 기능에 착수하거나 이미 있는 코드를 리팩토링하기 전에 여러분이 정한 커버리지 목표를 달성하는 것이 더 중요합니다.

### 하나의 테스트는 하나의 목표를 가지게 해라

**나쁜 예:**

```javascript
import assert from "assert";

describe("MomentJS", () => {
  it("handles date boundaries", () => {
    let date;

    date = new MomentJS("1/1/2015");
    date.addDays(30);
    assert.equal("1/31/2015", date);

    date = new MomentJS("2/1/2016");
    date.addDays(28);
    assert.equal("02/29/2016", date);

    date = new MomentJS("2/1/2015");
    date.addDays(28);
    assert.equal("03/01/2015", date);
  });
});
```

**좋은 예:**

```javascript
import assert from "assert";

describe("MomentJS", () => {
  it("handles 30-day months", () => {
    const date = new MomentJS("1/1/2015");
    date.addDays(30);
    assert.equal("1/31/2015", date);
  });

  it("handles leap year", () => {
    const date = new MomentJS("2/1/2016");
    date.addDays(28);
    assert.equal("02/29/2016", date);
  });

  it("handles non-leap year", () => {
    const date = new MomentJS("2/1/2015");
    date.addDays(28);
    assert.equal("03/01/2015", date);
  });
});
```

**[⬆ 목차로 이동](#목차)**

## 동시성

### callback 대신 Promise를 사용하라

Callback은 깔끔하지 않고, 너무 많은 중첩을 만듭니다.
ES2015/ES6부터는 Promise가 내장되어 있으니 이걸 사용하세요!

**나쁜 예:**

```javascript
import { get } from "request";
import { writeFile } from "fs";

get(
  "https://en.wikipedia.org/wiki/Robert_Cecil_Martin",
  (requestErr, response, body) => {
    if (requestErr) {
      console.error(requestErr);
    } else {
      writeFile("article.html", body, writeErr => {
        if (writeErr) {
          console.error(writeErr);
        } else {
          console.log("File written");
        }
      });
    }
  }
);
```

**좋은 예:**

```javascript
import { get } from "request-promise";
import { writeFile } from "fs-extra";

get("https://en.wikipedia.org/wiki/Robert_Cecil_Martin")
  .then(body => {
    return writeFile("article.html", body);
  })
  .then(() => {
    console.log("File written");
  })
  .catch(err => {
    console.error(err);
  });
```

**[⬆ 목차로 이동](#목차)**

### Async/Await은 Promise보다 깔끔하다

Promise는 callback의 아주 좋은 대안이 되지만 ES2017/ES8에 도입된 async/await은 이보다 더 좋은 해결책이 됩니다.
함수 앞에 `async` 키워드를 붙이고 함수의 `then` 체인 대신에 여러분의 로직만을 작성하면 됩니다.
여러분이 ES2017/ES8을 사용할 수 있다면 async/await을 사용하세요!

**나쁜 예:**

```javascript
import { get } from "request-promise";
import { writeFile } from "fs-extra";

get("https://en.wikipedia.org/wiki/Robert_Cecil_Martin")
  .then(body => {
    return writeFile("article.html", body);
  })
  .then(() => {
    console.log("File written");
  })
  .catch(err => {
    console.error(err);
  });
```

**좋은 예:**

```javascript
import { get } from "request-promise";
import { writeFile } from "fs-extra";

async function getCleanCodeArticle() {
  try {
    const body = await get(
      "https://en.wikipedia.org/wiki/Robert_Cecil_Martin"
    );
    await writeFile("article.html", body);
    console.log("File written");
  } catch (err) {
    console.error(err);
  }
}

getCleanCodeArticle()
```

**[⬆ 목차로 이동](#목차)**

## 에러 처리

에러를 던진다는 것은 좋은 겁니다!
프로그램이 무언가 잘못되었을 때 현재 스택에서 함수를 실행하는 것을 중단하고, 해당 프로세스를 종료하고 스택을 추적하여 콘솔에서 알려주는 것은 런타임이 에러를 성공적으로 알아차렸다는 것을 의미합니다.
### 찾아낸 에러를 무시하지 마라

찾은 에러를 가만히 두는 것은 에러를 고치거나 대응하는 능력을 주지 않습니다.
콘솔에 에러를 찍는 것(`console.log`)은 콘솔에 출력된 많은 것들을 잃어버리는 것만큼 더 좋은 방법은 아닙니다.
특정 코드를 `try/catch`로 둘러싼다는 것은 그 곳에서 에러가 발생할 수도 있다는 것이므로 에러가 발생할 경우에 대한 계획이나 코드 경로를 만들어야 합니다.

**나쁜 예:**

```javascript
try {
  functionThatMightThrow();
} catch (error) {
  console.log(error);
}
```

**좋은 예:**

```javascript
try {
  functionThatMightThrow();
} catch (error) {
  // 1. console.error을 사용하기 (console.log보다 알기 좋음)
  console.error(error);
  // 2. 사용자에게 알려주기
  notifyUserOfError(error);
  // 3. 서비스 자체에 에러를 알리기
  reportErrorToService(error);
  // 그 외 여러 방법이 있습니다
}
```

### reject된 Promise를 무시하지 마라

위 `try/catch`에서 설명한 이유와 동일합니다.

**나쁜 예:**

```javascript
getdata()
  .then(data => {
    functionThatMightThrow(data);
  })
  .catch(error => {
    console.log(error);
  });
```

**좋은 예:**

```javascript
getdata()
  .then(data => {
    functionThatMightThrow(data);
  })
  .catch(error => {
    // One option (more noisy than console.log):
    console.error(error);
    // Another option:
    notifyUserOfError(error);
    // Another option:
    reportErrorToService(error);
    // OR do all three!
  });
```

**[⬆ 목차로 이동](#목차)**

## 포맷팅

포맷팅은 주관적입니다. 어렵지 않고 빠르게 적용할 수 있는 규칙들이 많습니다.
포맷팅의 중요한 점은 포맷팅에 과하게 신경 쓸 필요가 없다는 겁니다.
자동으로 해주는 [아주 많은 도구들]((https://standardjs.com/rules.html))이 있으니까 도구를 사용하세요!
포맷팅에 관해 엔지니어들끼리 논쟁하는 것은 시간과 돈을 아주 낭비하고 있는 거예요.

자동으로 포맷팅해주는 범위(들여쓰기, 탭 vs. 스페이스, 큰 따옴표 vs. 작은 따옴표 등)에 포함되지 않는 것들은 몇 가지 지침을 따르는 것이 좋습니다. 
### 일관되게 대문자를 사용하라

자바스크립트는 타입이 정해져 있지 않기 때문에 대소문자는 어떤 값이 변수인지, 함수인지와 같은 여러 가지 정보를 제공해줍니다.
이런 규칙들은 주관적이기 때문에 여러분의 팀이 선택한 규칙을 따라야 합니다.
어떤 규칙을 따르든 일관되게 작성하는 것이 중요합니다.

**나쁜 예:**

```javascript
const DAYS_IN_WEEK = 7;
const daysInMonth = 30;

const songs = ["Back In Black", "Stairway to Heaven", "Hey Jude"];
const Artists = ["ACDC", "Led Zeppelin", "The Beatles"];

function eraseDatabase() {}
function restore_database() {}

class animal {}
class Alpaca {}
```

**좋은 예:**

```javascript
const DAYS_IN_WEEK = 7;
const DAYS_IN_MONTH = 30;

const SONGS = ["Back In Black", "Stairway to Heaven", "Hey Jude"];
const ARTISTS = ["ACDC", "Led Zeppelin", "The Beatles"];

function eraseDatabase() {}
function restoreDatabase() {}

class Animal {}
class Alpaca {}
```

**[⬆ 목차로 이동](#목차)**

### 함수를 호출하는 것과 호출되는 것은 가까이 위치시켜라

함수가 다른 함수를 호출한다면 코드 파일에서 수직으로 가까이 두어야 합니다.
이상적으로 호출되는 것이 호출하는 것 바로 위에 있는 것이 가장 좋습니다.
우리는 코드를 신문 읽듯이 위에서부터 아래로 있기 때문에 여러분은 이를 고려하는 것이 좋습니다.

**나쁜 예:**

```javascript
class PerformanceReview {
  constructor(employee) {
    this.employee = employee;
  }

  lookupPeers() {
    return db.lookup(this.employee, "peers");
  }

  lookupManager() {
    return db.lookup(this.employee, "manager");
  }

  getPeerReviews() {
    const peers = this.lookupPeers();
    // ...
  }

  perfReview() {
    this.getPeerReviews();
    this.getManagerReview();
    this.getSelfReview();
  }

  getManagerReview() {
    const manager = this.lookupManager();
  }

  getSelfReview() {
    // ...
  }
}

const review = new PerformanceReview(employee);
review.perfReview();
```

**좋은 예:**

```javascript
class PerformanceReview {
  constructor(employee) {
    this.employee = employee;
  }

  perfReview() {
    this.getPeerReviews();
    this.getManagerReview();
    this.getSelfReview();
  }

  getPeerReviews() {
    const peers = this.lookupPeers();
    // ...
  }

  lookupPeers() {
    return db.lookup(this.employee, "peers");
  }

  getManagerReview() {
    const manager = this.lookupManager();
  }

  lookupManager() {
    return db.lookup(this.employee, "manager");
  }

  getSelfReview() {
    // ...
  }
}

const review = new PerformanceReview(employee);
review.perfReview();
```

**[⬆ 목차로 이동](#목차)**

## 주석

### 비즈니스 로직이 복잡한 경우에만 주석을 남겨라

주석은 배려이지, 반드시 해야 하는 것은 아닙니다.
좋은 코드는 _대부분_ 주석이 없어도 그 자체로 설명이 됩니다.

**나쁜 예:**

```javascript
function hashIt(data) {
  // 해쉬값의 초기화
  let hash = 0;

  // 문자열 길이
  const length = data.length;

  // 각 문자마다 반복
  for (let i = 0; i < length; i++) {
    // 문자 코드 연산
    const char = data.charCodeAt(i);
    // 해쉬값 생성
    hash = (hash << 5) - hash + char;
    // 32비트 정수로 변환
    hash &= hash;
  }
}
```

**좋은 예:**

```javascript
function hashIt(data) {
  let hash = 0;
  const length = data.length;

  for (let i = 0; i < length; i++) {
    const char = data.charCodeAt(i);
    hash = (hash << 5) - hash + char;

    // 32비트 정수로 변환
    hash &= hash;
  }
}
```

**[⬆ 목차로 이동](#목차)**

### 코드 베이스에서 주석이 된 코드는 제거하라

버전 관리가 존재하는 이유가 있습니다.
오래된 코드는 지우세요.

**나쁜 예:**

```javascript
doStuff();
// doOtherStuff();
// doSomeMoreStuff();
// doSoMuchStuff();
```

**좋은 예:**

```javascript
doStuff();
```

**[⬆ 목차로 이동](#목차)**

### 일지를 주석으로 기록하지 마라

버전 관리 시스템을 사용할 수 있다는 것을 항상 잊지 마세요!
죽은 코드나 주석이 된 코드, 특히 일지를 기록하는 주석은 있을 필요가 없습니다.
기록을 확인하려면 `git log` 명령어를 활용하세요!

**나쁜 예:**

```javascript
/**
 * 2016-12-20: Removed monads, didn't understand them (RM)
 * 2016-10-01: Improved using special monads (JP)
 * 2016-02-03: Removed type-checking (LI)
 * 2015-03-14: Added combine with type-checking (JR)
 */
function combine(a, b) {
  return a + b;
}
```

**좋은 예:**

```javascript
function combine(a, b) {
  return a + b;
}
```

**[⬆ 목차로 이동](#목차)**

### 위치를 나타내는 마커를 지양하라

그것들은 코드를 지저분하게 만듭니다.
적절한 들여쓰기와 포맷팅이 함께 하는 함수나 변수이름은 시각적으로 좋습니다.

**나쁜 예:**

```javascript
////////////////////////////////////////////////////////////////////////////////
// Scope Model Instantiation
////////////////////////////////////////////////////////////////////////////////
$scope.model = {
  menu: "foo",
  nav: "bar"
};

////////////////////////////////////////////////////////////////////////////////
// Action setup
////////////////////////////////////////////////////////////////////////////////
const actions = function() {
  // ...
};
```

**좋은 예:**

```javascript
$scope.model = {
  menu: "foo",
  nav: "bar"
};

const actions = function() {
  // ...
};
```

**[⬆ 목차로 이동](#목차)**

## 번역

다른 언어로도 읽어볼 수 있어요!

- ![am](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Armenia.png) **아르메니아어**: [hanumanum/clean-code-javascript/](https://github.com/hanumanum/clean-code-javascript)
- ![bd](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Bangladesh.png) **벵골어(বাংলা)**: [InsomniacSabbir/clean-code-javascript/](https://github.com/InsomniacSabbir/clean-code-javascript/)
- ![br](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Brazil.png) **브라질 포르투갈어**: [fesnt/clean-code-javascript](https://github.com/fesnt/clean-code-javascript)
- ![cn](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/China.png) **간체자**:
  - [alivebao/clean-code-js](https://github.com/alivebao/clean-code-js)
  - [beginor/clean-code-javascript](https://github.com/beginor/clean-code-javascript)
- ![tw](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Taiwan.png) **정체자**: [AllJointTW/clean-code-javascript](https://github.com/AllJointTW/clean-code-javascript)
- ![fr](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/France.png) **프랑스어**: [GavBaros/clean-code-javascript-fr](https://github.com/GavBaros/clean-code-javascript-fr)
- ![de](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Germany.png) **독일어**: [marcbruederlin/clean-code-javascript](https://github.com/marcbruederlin/clean-code-javascript)
- ![id](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Indonesia.png) **인도네시아어**: [andirkh/clean-code-javascript/](https://github.com/andirkh/clean-code-javascript/)
- ![it](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Italy.png) **이탈리아어**: [frappacchio/clean-code-javascript/](https://github.com/frappacchio/clean-code-javascript/)
- ![ja](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Japan.png) **일본어**: [mitsuruog/clean-code-javascript/](https://github.com/mitsuruog/clean-code-javascript/)
- ![kr](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/South-Korea.png) **한국어**: 
  - [qkraudghgh/clean-code-javascript-ko](https://github.com/qkraudghgh/clean-code-javascript-ko)
  - [sbyeol3/clean-code-javascript-kr](https://github.com/sbyeol3/clean-code-javascript-kr)
- ![pl](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Poland.png) **폴란드어**: [greg-dev/clean-code-javascript-pl](https://github.com/greg-dev/clean-code-javascript-pl)
- ![ru](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Russia.png) **러시아어**:
  - [BoryaMogila/clean-code-javascript-ru/](https://github.com/BoryaMogila/clean-code-javascript-ru/)
  - [maksugr/clean-code-javascript](https://github.com/maksugr/clean-code-javascript)
- ![es](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Spain.png) **스페인어**: [tureey/clean-code-javascript](https://github.com/tureey/clean-code-javascript)
- ![es](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Uruguay.png) **우루과이 스페인어**: [andersontr15/clean-code-javascript](https://github.com/andersontr15/clean-code-javascript-es)
- ![rs](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Serbia.png) **세르비아어**: [doskovicmilos/clean-code-javascript/](https://github.com/doskovicmilos/clean-code-javascript)
- ![tr](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Turkey.png) **터키어**: [bsonmez/clean-code-javascript](https://github.com/bsonmez/clean-code-javascript/tree/turkish-translation)
- ![ua](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Ukraine.png) **우크라이나어**: [mindfr1k/clean-code-javascript-ua](https://github.com/mindfr1k/clean-code-javascript-ua)
- ![vi](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Vietnam.png) **베트남어**: [hienvd/clean-code-javascript/](https://github.com/hienvd/clean-code-javascript/)

**[⬆ 목차로 이동](#목차)**
