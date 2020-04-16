# clean-code-javascript-ua

## Зміст

1. [Вступ](#Вступ)
2. [Змінні](#Змінні)
3. [Функції](#Функції)
4. [Об'єкти та структури даних](#Об'єкти-та-структури-даних)
5. [Класи](#Класи)
6. [SOLID](#SOLID)
7. [Тестування](#Тестування)
8. [Асинхронність](#Асинхронність)
9. [Обробка помилок](#Обробка-помилок)
10. [Форматування](#Форматування)
11. [Коментарі](#comments)
12. [Переклад](#translation)

## Вступ

![Жартівливе зображення оцінки якості програмного забезпечення як кількості разів, що ви лаялись,
читаючи код](https://www.osnews.com/images/comics/wtfm.jpg)

Принципи програмної інженерії з книги Роберта С. Мартіна 
[_Clean Code_](https://www.amazon.com/Clean-Code-Handbook-Software-Craftsmanship/dp/0132350882), адаптовані для JavaScript. Це не гайд зі стилю кода. Це посібник для розробки програмного забезпечення
на JavaScript, котре 
[легко читати, повторно використовувати і рефакторити](https://github.com/ryanmcdermott/3rs-of-software-architecture).

Не кожен згаданий тут принцип обов'язковий до дотримання, і лише деякі з них не
викличуть розбіжностей. Це поради і нічого більше, але їх документували
на основі багаторічного колективного досвіду авторів _Clean Code_.

Нашому ремеслу програмної інженерії трохи більше 50 років, і ми все ще багато чого пізнаємо.
Коли архітектура програмного забезпечення буде такою ж старою, як і сама архітектура,
можливо тоді в нас будуть більш жорсткі правила до дотримання. А зараз нехай ці поради слугують
критерієм для оцінки JavaScript коду, який створюєте ви і ваша команда.

Ще одна річ: знання цих принципів не зробить вас кращим розробником миттєво, 
і багаторічна праця з ними не значить, що ви не будете робити помилок.
Кожен фрагмент коду починається з чорнового варіанту, подібно мокрій глині, що набуває своєї кінцевої
форми. По завершенню, ми винищуємо недоліки, коли рецензуємо код з колегами. Не коріть себе за перші чорнові версії коду, що потребують поліпшення. Поліпшуйте код замість цього!

## **Змінні**

### Використовуйте виразні та вимовні імена змінних

**Погано:**

```javascript
const yyyymmdstr = moment().format("YYYY/MM/DD");
```

**Добре:**

```javascript
const currentDate = moment().format("YYYY/MM/DD");
```

**[⬆ повернутися до змісту](#Зміст)**

### Використовуйте однакову лексику для однакового типу змінної

**Погано:**

```javascript
getUserInfo();
getClientData();
getCustomerRecord();
```

**Добре:**

```javascript
getUser();
```

**[⬆ повернутися до змісту](#Зміст)**

### Використовуйте імена, доступні для пошуку

Ми прочитаємо більше коду, ніж коли-небудь напишемо. Важливо, щоб наш код був легким для читання 
і пошуку. _Не_ використовуючи імена для змінних, що є важливими для розуміння нашої
програми, ми шкодимо тим, хто читає наш код.
Робіть імена доступними для пошуку. Такі засоби, як
[buddy.js](https://github.com/danielstjules/buddy.js) та
[ESLint](https://github.com/eslint/eslint/blob/660e0918933e6e7fede26bc675a0763a6b357c94/docs/rules/no-magic-numbers.md)
можуть допомогти у знаходженні безіменних констант.

**Погано:**

```javascript
// Якого біса означає 86400000?
setTimeout(blastOff, 86400000);
```

**Добре:**

```javascript
// Оголошуйте їх у якості іменованих констант з верхнім регістром.
const MILLISECONDS_IN_A_DAY = 86_400_000;

setTimeout(blastOff, MILLISECONDS_IN_A_DAY);
```

**[⬆ повернутися до змісту](#Зміст)**

### Використовуйте наочні змінні

**Погано:**

```javascript
const address = "One Infinite Loop, Cupertino 95014";
const cityZipCodeRegex = /^[^,\\]+[,\\\s]+(.+?)\s*(\d{5})?$/;
saveCityZipCode(
  address.match(cityZipCodeRegex)[1],
  address.match(cityZipCodeRegex)[2]
);
```

**Добре:**

```javascript
const address = "One Infinite Loop, Cupertino 95014";
const cityZipCodeRegex = /^[^,\\]+[,\\\s]+(.+?)\s*(\d{5})?$/;
const [_, city, zipCode] = address.match(cityZipCodeRegex) || [];
saveCityZipCode(city, zipCode);
```

**[⬆ повернутися до змісту](#Зміст)**

### Уникайте розумового зв'язування

Явне краще за неявне.

**Погано:**

```javascript
const locations = ["Austin", "New York", "San Francisco"];
locations.forEach(l => {
  doStuff();
  doSomeOtherStuff();
  // ...
  // ...
  // ...
  // Почекайте, що там означає `l`?
  dispatch(l);
});
```

**Добре:**

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

**[⬆ повернутися до змісту](#Зміст)**

### Не додавайте непотрібний контекст

Якщо ім'я вашого класу/об'єкта щось говорить вам, не повторюйте це у назві змінної.

**Погано:**

```javascript
const Car = {
  carMake: "Honda",
  carModel: "Accord",
  carColor: "Blue"
};

function paintCar(car) {
  car.carColor = "Red";
}
```

**Добре:**

```javascript
const Car = {
  make: "Honda",
  model: "Accord",
  color: "Blue"
};

function paintCar(car) {
  car.color = "Red";
}
```

**[⬆ повернутися до змісту](#Зміст)**

### Використовуйте аргументи за замовчуванням замість короткого обчислення умови OR

Аргументи за замовчуванням часто є більш чистими, ніж коротке обчислення. Майте на увазі, що при
використанні аргументів за замовчуванням ваша функція надасть значення за замовчуванням тільки для `undefined` аргументів. Інші "хибні" (falsy) значення, як-то `''`, `""`, `false`, `null`, `0`, і
`NaN`, не будуть замінені значенням за замовчуванням.

**Погано:**

```javascript
function createMicrobrewery(name) {
  const breweryName = name || "Hipster Brew Co.";
  // ...
}
```

**Добре:**

```javascript
function createMicrobrewery(name = "Hipster Brew Co.") {
  // ...
}
```

**[⬆ повернутися до змісту](#Зміст)**

## **Функції**

### Аргументи функції (ідеально 2 або менше)

Обмеження кількості параметрів функції надзвичайно важливо, тому що це робить тестування вашої функції простішим. Наявність більше трьох параметрів призводить до комбінаторного вибуху, коли вам потрібно
тестувати безліч різноманітних ситуацій з кожним окремим аргументом.

Один або два аргументи є ідеальним випадком, трьох аргументів слід уникати по можливості.
Якщо аргументів більше, їх слід об'єднати. Зазвичай, якщо у вас більше двох аргументів, ваша функція намагається виконувати забагато дій. В тих випадках, коли це не так, майже завжди буде достатньо об'єкта вищого рівня.

Так як JavaScript дозволяє вам створювати об'єкти на льоту, без використання синтаксису класів, ви можете використовувати об'єкт, якщо потребуєте багатьох аргументів.

Щоб було очевидно, які властивості функція очікує, ви можете використовувати ES2015/ES6 синтаксис деструктуризації. Такий підхід має декілька переваг:

1. Коли хтось дивиться на сигнатуру функції, одразу зрозуміло, які властивості використовуються.
2. Деструктуризацію можна використовувати, щоб імітувати іменовані параметри.
3. Деструктуризація, окрім того, клонує вказані примітивні значення об'єкту аргумента, переданого у функцію. Це може допомогти запобіганню побічним ефектам. Примітка: об'єкти та масиви, які деструктурували з об'єкта аргументу, НЕ клонуються.
4. Лінтери можуть попередити вас щодо невикористаних властивостей, що було б неможливим без деструктуризації.

**Погано:**

```javascript
function createMenu(title, body, buttonText, cancellable) {
  // ...
}

createMenu("Foo", "Bar", "Baz", true);

```

**Добре:**

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

**[⬆ повернутися до змісту](#Зміст)**

### Функції повинні виконувати одну дію

Це, безумовно, найважливіше правило в програмній інженерії. Коли функції виконують більше одної дії, їх важко поєднувати, тестувати та обґрунтовувати. Коли ви можете обмежити функцію до тільки одної дії, її можна легко рефакторити і ваш код буде набагато чистішим. Навіть якщо ви засвоїте з цього посібника тільки цю пораду, ви будете попереду багатьох розробників.

**Погано:**

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

**Добре:**

```javascript
function emailActiveClients(clients) {
  clients.filter(isActiveClient).forEach(email);
}

function isActiveClient(client) {
  const clientRecord = database.lookup(client);
  return clientRecord.isActive();
}
```

**[⬆ повернутися до змісту](#Зміст)**

### Імена функцій повинні говорити, що роблять функції

**Погано:**

```javascript
function addToDate(date, month) {
  // ...
}

const date = new Date();

// По імені функції важко сказати, що саме додається
addToDate(date, 1);
```

**Добре:**

```javascript
function addMonthToDate(month, date) {
  // ...
}

const date = new Date();
addMonthToDate(1, date);
```

**[⬆ повернутися до змісту](#Зміст)**

### Функції мають містити тільки один рівень абстракції

Наявність більше одного рівня абстракції зазвичай означає, що ваша функція виконує забагато дій. Розділення функцій призводить до повторного використання та більш легкого тестування.

**Погано:**

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
    // правило...
  });

  ast.forEach(node => {
    // парсинг...
  });
}
```

**Добре:**

```javascript
function parseBetterJSAlternative(code) {
  const tokens = tokenize(code);
  const syntaxTree = parse(tokens);
  syntaxTree.forEach(node => {
    // парсинг...
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

**[⬆ повернутися до змісту](#Зміст)**

### Видаляйте повторюваний код

Робіть все можливе, щоб уникнути повторюваного коду. Повторюваний код поганий тому, що означає наявність більше одного місця для змін у разі, якщо вам потрібно буде змінити деяку логіку.

Уявіть, що ви керуєте рестораном і стежите за своїм інвентарем: помідори, цибуля, часник, спеції тощо. Якщо у вас є кілька списків, у яких ви все зберігаєте, тоді кожен з них потрібно оновлювати, коли ви подаєте страву з помідорами. Якщо у вас є лише один список, є лише одне місце для оновлення!

Часто у вас є повторюваний код, коли ви маєте дві або більше трохи різних сутностей, які мають багато спільного, але їх відмінності змушують вас мати дві або більше окремих функцій, які виконують багато однакових дій. Видалення повторюваного коду означає створення абстракції, яка може обробляти цю множину
різних сутностей лише однією функцією/модулем/класом.

Правильне розуміння абстракції є критично важливим, саме тому ви повинні слідувати принципам SOLID, викладеним в розділі _Класи_. Погані абстракції можуть бути гіршими за повторюваний код, тому будьте обережні! Маючи це на увазі, якщо ви можете зробити гарну абстракцію, зробіть її! Не повторюйте себе, інакше вам доведеться оновлювати декілька місць, коли ви захочете змінити лише одне.

**Погано:**

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

**Добре:**

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

**[⬆ повернутися до змісту](#Зміст)**

### Встановлюйте об'єкти за замовчуванням за допомогою Object.assign

**Погано:**

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

**Добре:**

```javascript
const menuConfig = {
  title: "Order",
  // Користувач не додав ключ 'body'
  buttonText: "Send",
  cancellable: true
};

function createMenu(config) {
  config = Object.assign(
    {
      title: "Foo",
      body: "Bar",
      buttonText: "Baz",
      cancellable: true
    },
    config
  );

  // config тепер дорівнює: {title: "Order", body: "Bar", buttonText: "Send", cancellable: true}
  // ...
}

createMenu(menuConfig);
```

**[⬆ повернутися до змісту](#Зміст)**

### Не використовуйте флаги в якості параметрів функції

Флаги повідомляють користувачу, що ця функція виконує більше однієї дії. Функції повинні виконувати лише одну дію. Розділіть функції, якщо вони дотримуються різних кодових шляхів на основі булевої змінної.

**Погано:**

```javascript
function createFile(name, temp) {
  if (temp) {
    fs.create(`./temp/${name}`);
  } else {
    fs.create(name);
  }
}
```

**Добре:**

```javascript
function createFile(name) {
  fs.create(name);
}

function createTempFile(name) {
  createFile(`./temp/${name}`);
}
```

**[⬆ повернутися до змісту](#Зміст)**

### Уникайте побічних ефектів (частина 1)

Функція створює побічний ефект, якщо робить щось інше, окрім приймання вхідного значення і повернення іншого значення або значень. Побічним ефектом може бути запис у файл, зміна якоїсь глобальної змінної або випадкове пересилання всіх ваших грошей незнайомцю.

Проте, час від часу вам потрібно мати побічні ефекти в програмі. Як у попередньому прикладі, вам може знадобитися запис у файл. Що вам потрібно зробити - це централізувати місце, де ви виконуєте таку логіку. Не створюйте декількох функцій та класів, які роблять запис у певний файл. Майте один сервіс, який це робить. Один і тільки один.

Основна думка тут - це уникання поширених помилок, таких, як розділення стану між об'єктами
без будь-якої структури, використання змінних типів даних, в які можна записати що завгодно, і відсутність централізування у місцях виникнення побічних ефектів. Якщо ви зможете це зробити, ви будете щасливішими за переважну більшість інших програмістів.

**Погано:**

```javascript
// Глобальна змінна, на яку посилається наступна функція.
// Якщо б у нас була ще одна функція, що використовує це ім'я, то зараз ця змінна була б масивом
// і функція могла б зламати змінну.
let name = "Ryan McDermott";

function splitIntoFirstAndLastName() {
  name = name.split(" ");
}

splitIntoFirstAndLastName();

console.log(name); // ['Ryan', 'McDermott'];
```

**Добре:**

```javascript
function splitIntoFirstAndLastName(name) {
  return name.split(" ");
}

const name = "Ryan McDermott";
const newName = splitIntoFirstAndLastName(name);

console.log(name); // 'Ryan McDermott';
console.log(newName); // ['Ryan', 'McDermott'];
```

**[⬆ повернутися до змісту](#Зміст)**

### Уникайте побічних ефектів (частина 2)

У JavaScript примітиви передаються за значенням, а об'єкти/масиви передаються за посиланням. У випадку об'єктів і масивів, якщо ваша функція вносить зміни у масив кошика для покупок, наприклад, додаючи товар для придбання, тоді це додавання вплине на будь-яку іншу функцію, яка використовує масив `cart`. Це може бути чудово, проте це може бути і погано. Давайте уявимо погану ситуацію:

Користувач натискає кнопку "Придбати", кнопка викликає функцію `purchase`, що створює мережевий запит і надсилає масив `cart` на сервер. Через погане мереже з'єднання, функція `purchase` повинна створювати повторний запит. А якщо тим часом користувач випадково натисне "Додати в кошик" на товарі, який йому не потрібен, до початку мережевого запиту? Якщо це трапляється і мережевий запит починається, то  функція придбання відправить випадково доданий товар, оскільки він має посилання на масив кошика покупок, котрий функція `addItemToCart` модифікувала додаванням небажаного товару.

Чудовим рішенням для `addItemToCart` було б завжди клонувати `cart`, редагувати його і повертати клон. Це гарантує, що сторонні зміни не вплинуть на жодну функцію, що посилається на кошик для покупок.

Два застереження, які слід згадати при такому підході:

1. Можуть бути випадки, коли ви дійсно хочете змінити об'єкт на вході, але коли ви застосуєте цю практику  програмування, ви виявите, що ці випадки є досить рідкісними. Більшість речей можна відрефакторити так, щоб уникнути побічних ефектів!

2. Клонування великих об'єктів може бути дуже дорогим з точки зору продуктивності. На щастя, це не є великою проблемою на практиці, оскільки є [чудові бібліотеки](https://facebook.github.io/immutable-js/), що дозволяють такому підходу програмування бути швидким і не таким вибагливим до пам'яті, як клонування об’єктів та масивів вручну.

**Погано:**

```javascript
const addItemToCart = (cart, item) => {
  cart.push({ item, date: Date.now() });
};
```

**Добре:**

```javascript
const addItemToCart = (cart, item) => {
  return [...cart, { item, date: Date.now() }];
};
```

**[⬆ повернутися до змісту](#Зміст)**

### Не розширюйте глобальні функції

Забруднення глобальних змінних є поганою практикою в JavaScript, оскільки у вас може виникнути конфлікт з іншою бібліотекою, і користувач вашого API не буде цього знати, доки не отримає виняток у продакшені.  Давайте подумаємо про приклад: що, якби ви хотіли розширити нативний метод масиву JavaScript так, щоб `Array` мав метод `diff`, який міг би показати різницю між двома масивами? Ви можете записати свою нову функцію в `Array.prototype`, але вона може вступити в конфлікт з іншою бібліотекою, яка намагається
робити те саме. А що як та інша бібліотека просто використовувала `diff` для пошуку різниці між першим та останнім елементами масиву? Ось чому було б набагато краще просто використовувати ES2015 / ES6 класи і просто розширити глобальний об'єкт `Array`.

**Погано:**

```javascript
Array.prototype.diff = function diff(comparisonArray) {
  const hash = new Set(comparisonArray);
  return this.filter(elem => !hash.has(elem));
};
```

**Добре:**

```javascript
class SuperArray extends Array {
  diff(comparisonArray) {
    const hash = new Set(comparisonArray);
    return this.filter(elem => !hash.has(elem));
  }
}
```

**[⬆ повернутися до змісту](#Зміст)**

### Віддавайте перевагу функціональному програмуванню над імперативним програмуванням

JavaScript не є функціональною мовою у сенсі Haskell, але має функціональний присмак. Функціональні мови можуть бути більш чистими та легшими для тестування. Віддавайте перевагу цьому стилю програмування, коли можете.

**Погано:**

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

**Добре:**

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

**[⬆ повернутися до змісту](#Зміст)**

### Інкапсулюйте умовні вирази

**Погано:**

```javascript
if (fsm.state === "fetching" && isEmpty(listNode)) {
  // ...
}
```

**Добре:**

```javascript
function shouldShowSpinner(fsm, listNode) {
  return fsm.state === "fetching" && isEmpty(listNode);
}

if (shouldShowSpinner(fsmInstance, listNodeInstance)) {
  // ...
}
```

**[⬆ повернутися до змісту](#Зміст)**

### Уникайте негативних умовних виразів

**Погано:**

```javascript
function isDOMNodeNotPresent(node) {
  // ...
}

if (!isDOMNodeNotPresent(node)) {
  // ...
}
```

**Добре:**

```javascript
function isDOMNodePresent(node) {
  // ...
}

if (isDOMNodePresent(node)) {
  // ...
}
```

**[⬆ повернутися до змісту](#Зміст)**

### Уникайте умовних виразів

Це здається неможливим завданням. Більшість людей, почувши це, говорять: "як я можу зробити хоч щось без `if` виразу?" Відповідь полягає в тому, що ви можете використовувати поліморфізм для досягнення однієї і тієї ж мети у багатьох випадках. Друге питання, як правило: "ну це чудово, але чому я хотів би це зробити?" Відповідь - це попередня концепція чистого коду, яку ми дізналися: функція повинна виконувати лише одну дію. Коли у вас є класи та функції, у яких наявні `if` вирази, ви говорите вашому користувачеві, що ваша функція виконує більше однієї дії. Пам'ятайте, виконуйте тільки одну дію.

**Погано:**

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

**Добре:**

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

**[⬆ повернутися до змісту](#Зміст)**

### Уникайте перевірки типів (частина 1)

JavaScript є слабо типізованою мовою - це означає, що ваші функції можуть приймати аргументи будь-якого типу. Іноді вам незручна ця свобода, і здається спокусливим зробити перевірку типів у ваших функціях. Є багато способів уникнути такої необхідності. Перше, що слід врахувати, - це послідовні API.

**Погано:**

```javascript
function travelToTexas(vehicle) {
  if (vehicle instanceof Bicycle) {
    vehicle.pedal(this.currentLocation, new Location("texas"));
  } else if (vehicle instanceof Car) {
    vehicle.drive(this.currentLocation, new Location("texas"));
  }
}
```

**Добре:**

```javascript
function travelToTexas(vehicle) {
  vehicle.move(this.currentLocation, new Location("texas"));
}
```

**[⬆ повернутися до змісту](#Зміст)**

### Уникайте перевірки типів (частина 2)

Якщо ви працюєте з базовими примітивними значеннями, такими як рядки та цілі числа,
і ви не можете використовувати поліморфізм, але все ще відчуваєте потребу перевірити тип, вам слід розглянути можливість використання TypeScript. Це відмінна альтернатива звичайному JavaScript, оскільки вона надає вам статичну типізацію над стандартним JavaScript синтаксисом. Проблема з ручною перевіркою типовів у звичайному JavaScript полягає в тому, що для такої перевірки потрібно стільки додаткового коду, що отримана вами штучна "безпека типів" не компенсує втрату читабельності. Слідкуйте за чистотою вашого JavaScript, пишіть хороші тести та проводьте якісне рецензування коду. В іншому випадку перевіряйте типи, але з TypeScript (який, як я вже сказав, є чудовою альтернативою!).

**Погано:**

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

**Добре:**

```javascript
function combine(val1, val2) {
  return val1 + val2;
}
```

**[⬆ повернутися до змісту](#Зміст)**

### Не оптимізуйте надмірно

Сучасні браузери роблять багато оптимізації під капотом. Здебільшого, якщо ви оптимізуєте, то ви просто витрачаєте свій час. [Є хороші ресурси](https://github.com/petkaantonov/bluebird/wiki/Optimization-killers), що показують, де оптимізації не вистачає. Використовуйте їх до тих пір, доки ситуація не покращиться.

**Погано:**

```javascript
// У старих браузерах кожна ітерація `list.length` без кешування буде дорогою
// через перерахунок `list.length`. У сучасних браузерах це оптимізовано.
for (let i = 0, len = list.length; i < len; i++) {
  // ...
}
```

**Добре:**

```javascript
for (let i = 0; i < list.length; i++) {
  // ...
}
```

**[⬆ повернутися до змісту](#Зміст)**

### Видаляйте мертвий код

Мертвий код так само поганий, як і повторюваний. Немає жодних підстав тримати його у вашій кодовій базі. Якщо його не викликають, позбудьтесь його! Він все ще буде у безпеці в історії версій, якщо вам знадобиться.

**Погано:**

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

**Добре:**

```javascript
function newRequestModule(url) {
  // ...
}

const req = newRequestModule;
inventoryTracker("apples", req, "www.inventory-awesome.io");
```

**[⬆ повернутися до змісту](#Зміст)**

## **Об'єкти та структури даних**

### Використовутйе геттери та сеттери

Використання геттерів та сеттерів для доступу до даних об'єктів може бути кращим за просте отримання властивості об'єкта. "Чому?" - ви можете запитати. Ось перелік причин:

- Коли ви хочете зробити більше, ніж отримати властивість об’єкта, вам не потрібно шукати та змінювати кожне місце доступу до властивості об'єкта.
- Робить валідацію простою при роботі з `set`.
- Інкапсулює внутрішнє представлення.
- Легко додавати логування та обробку помилок під час отримання та встановлення властивостей.
- Ви можете ліниво завантажити властивості об'єкта, скажімо, отримуючи їх з сервера.

**Погано:**

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

**Добре:**

```javascript
function makeBankAccount() {
  // ця властивість приватна
  let balance = 0;

  // "геттер", є публічним через повернутий об’єкт нижче
  function getBalance() {
    return balance;
  }

  // "сеттер", є публічним через повернутий об’єкт нижче
  function setBalance(amount) {
    // ... валідація перед оновленням балансу
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

**[⬆ повернутися до змісту](#Зміст)**

### Створюйте приватні поля в об'єктах

Цього можна досягти за допомогою замикань (для ES5 і нижче).

**Погано:**

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

**Добре:**

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

**[⬆ повернутися до змісту](#Зміст)**

## **Класи**

### Віддавайте перевагу ES2015 / ES6 класам над звичайними ES5 функціями

Дуже важко отримати читабельне наслідування класів, їх побудову та визначення методів у класичних ES5 класах. Якщо вам потрібне наслідування (майте на увазі, що це може бути не так), тоді віддайте перевагу ES2015 / ES6 класам. Однак віддавайте перевагу невеликим функціям над класами, доки вам не знадобляться більш великиі і складні об'єкти.

**Погано:**

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

**Добре:**

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

**[⬆ повернутися до змісту](#Зміст)**

### Використовуйте прив'язку методів (method chaining)

Цей паттерн дуже корисний у JavaScript, і ви спостерігаєте його у багатьох бібліотеках, таких як jQuery і Lodash. Прив'язка методів дозволяє вашому коду бути виразним і менш багатослівним. Спробуйте використати прив'язку методів і погляньте, наскільки чистим буде ваш код. У функціях класу просто поверніть `this` в кінці кожної функції, і ви можете прив'язувати до нього подальші методи класу.

**Погано:**

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

**Добре:**

```javascript
class Car {
  constructor(make, model, color) {
    this.make = make;
    this.model = model;
    this.color = color;
  }

  setMake(make) {
    this.make = make;
    // ПРИМІТКА: Повертаємо this для прив'язування
    return this;
  }

  setModel(model) {
    this.model = model;
    // ПРИМІТКА: Повертаємо this для прив'язування
    return this;
  }

  setColor(color) {
    this.color = color;
    // ПРИМІТКА: Повертаємо this для прив'язування
    return this;
  }

  save() {
    console.log(this.make, this.model, this.color);
    // ПРИМІТКА: Повертаємо this для прив'язування
    return this;
  }
}

const car = new Car("Ford", "F-150", "red").setColor("pink").save();
```

**[⬆ повернутися до змісту](#Зміст)**

### Віддавайте перевагу композиції над наслідуванням

Як відомо зазначено в книзі [_Design Patterns_](https://en.wikipedia.org/wiki/Design_Patterns) Бандою Чотирьох, вам слід віддати перевагу композиції над наслідуванням, де це можливо. Є багато вагомих причин використовувати наслідування і є безліч вагомих причин використовувати композицію. Основним моментом цієї максими є те, що якщо ваш розум інстинктивно дотримується наслідування, спробуйте подумати, чи композиція могла б краще моделювати вашу проблему. У деяких випадках вона могла б.

Тоді вам може бути цікаво: "коли я повинен використовувати наслідування?" Це залежить від вашої поточної проблеми, але це пристойний перелік ситуацій, коли наслідування має більше сенсу, ніж композиція:

1. Ваше наслідування представляє відносини типу "є чимось", а не "має щось" (Людина->Тварина проти Користувач->ДеталіКористувача).
2. Ви можете повторно використовувати код з базових класів (Люди можуть рухатися, як і всі тварини).
3. Ви хочете внести глобальні зміни до похідних класів, змінивши базовий клас. (Зміна витрати калорій всіх тварин, коли вони рухаються).

**Погано:**

```javascript
class Employee {
  constructor(name, email) {
    this.name = name;
    this.email = email;
  }

  // ...
}

// Погано, тому що Співробітники "мають" податкові дані. EmployeeTaxData не є типом Employee
class EmployeeTaxData extends Employee {
  constructor(ssn, salary) {
    super();
    this.ssn = ssn;
    this.salary = salary;
  }

  // ...
}
```

**Добре:**

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

**[⬆ повернутися до змісту](#Зміст)**

## **SOLID**

### Принцип єдиного обов'язку (SRP)

Як зазначено в Чистому коді: "Ніколи не повинно бути більше однієї причини для зміни класу". Привабливо наповнити клас великою кількістю функціоналу, як у ситуації, коли ви можете взяти лише одну валізу у свій рейс. Проблема в тому, що ваш клас не буде концептуально єдиним, і це дасть йому багато причин для змін.Мінімізування кількості змін класу - це важливо. Це важливо тому, що якщо в одному класі забагато функціоналу і ви модифікуєте його частину, може бути важко зрозуміти, як це вплине на інші залежні модулі у вашій кодовій базі.

**Погано:**

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

**Добре:**

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

**[⬆ повернутися до змісту](#Зміст)**

### Принцип відкритості/закритості (OCP)

Як зазначає Бертран Меєр: "програмні об'єкти (класи, модулі, функції, тощо) мають бути відкритими для розширення, але закритими для внесення змін". Що мається на увазі? Загалом, цей принцип говорить, що ви повинні дозволити користувачам додавати новий функціонал без зміни існуючого коду.

**Погано:**

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
        // трансформувати відповідь і повернути її
      });
    } else if (this.adapter.name === "nodeAdapter") {
      return makeHttpCall(url).then(response => {
        // трансформувати відповідь і повернути її
      });
    }
  }
}

function makeAjaxCall(url) {
  // виконати запит і повернути проміс
}

function makeHttpCall(url) {
  // виконати запит і повернути проміс
}
```

**Добре:**

```javascript
class AjaxAdapter extends Adapter {
  constructor() {
    super();
    this.name = "ajaxAdapter";
  }

  request(url) {
    // виконати запит і повернути проміс
  }
}

class NodeAdapter extends Adapter {
  constructor() {
    super();
    this.name = "nodeAdapter";
  }

  request(url) {
    // виконати запит і повернути проміс
  }
}

class HttpRequester {
  constructor(adapter) {
    this.adapter = adapter;
  }

  fetch(url) {
    return this.adapter.request(url).then(response => {
      // трансформувати відповідь і повернути її
    });
  }
}
```

**[⬆ повернутися до змісту](#Зміст)**

### Принцип підстановки Лісков (LSP)

Це страшний термін для дуже простої концепції. Формально вона визначена так: "Якщо S
є підтипом T, тоді об'єкти типу T можуть бути замінені об'єктами типу S
(тобто об'єкти типу S можуть підставлятися замість об'єктів типу T), не змінюючи жодної важливої властивості програми (коректність, виконання завдань, тощо)". Це ще страшніше визначення.

Найкраще пояснення цьому - якщо у вас є батьківський клас та дочірній клас, то їх можна використовувати взаємозамінно, не отримуючи неправильних результатів. Це все ще може бентежити, тому давайте подивимось на класичний приклад Квадрат-Прямокутник. Математично квадрат є прямокутником, але якщо ви змоделюєте це за допомогою відносини "є чимось" шляхом наслідування, ви швидко отримаєте проблему.

**Погано:**

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
    const area = rectangle.getArea(); // ПОГАНО: Повертає 25 для Квадрату. Повинно бути 20.
    rectangle.render(area);
  });
}

const rectangles = [new Rectangle(), new Rectangle(), new Square()];
renderLargeRectangles(rectangles);
```

**Добре:**

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

**[⬆ повернутися до змісту](#Зміст)**

### Принцип розділення інтерфейсу (ISP)

У JavaScript немає інтерфейсів, тому цей принцип не застосовується так суворо, як інші. Однак він є важливим і актуальним навіть при відсутності системи типів у JavaScript.

ISP зазначає: "Клієнти не повинні залежати від інтерфейсів, які вони не використовують." Інтерфейси є неявними контрактами в JavaScript через качину типізацію (duck typing).

Хороший приклад, що демонструє цей принцип в JavaScript - це класи, які потребують великих об'єктів налаштувань. Не вимагати від клієнтів встановлення величезної кількості варіантів вигідно, тому що більшу частину часу вони не потребують всіх налаштувань. Якщо зробити налаштування необов’язковими, це допоможе запобігти появі "жирного інтерфейсу".

**Погано:**

```javascript
class DOMTraverser {
  constructor(settings) {
    this.settings = settings;
    this.setup();
  }

  setup() {
    this.rootNode = this.settings.rootNode;
    this.animationModule.setup();
  }

  traverse() {
    // ...
  }
}

const $ = new DOMTraverser({
  rootNode: document.getElementsByTagName("body"),
  animationModule() {} // Більшу частину часу ми не потребуємо анімації під час обходу DOM.
  // ...
});
```

**Добре:**

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

**[⬆ повернутися до змісту](#Зміст)**

### Принцип інверсії залежностей (DIP)

Цей принцип визначає дві важливі речі:

1. Модулі високого рівня не повинні залежати від модулів низького рівня. І ті й інші повинні залежати від абстракцій.
2. Абстракції не повинні залежати від деталей. Деталі повинні залежати від абстракцій.

Спочатку це може бути важко зрозуміти, але якщо ви працювали з AngularJS, ви бачили реалізацію цього принципу у формі впровадження залежностей (DI). Хоча вони не є ідентичними поняттями, DIP утримує модулі високого рівня від знання деталей модулів низького рівня та їх налаштування. Цього можна досягти за допомогою DI. Величезна перевага впровадження залежностей полягає в тому, що воно зменшує зв'язування між модулями. Зв'язування - це дуже поганий паттерн розробки, оскільки це робить ваш код важким для рефакторингу.

Як було сказано раніше, у JavaScript немає інтерфейсів, тому абстракції є неявними контрактами. Тобто методами та властивостями, які об'єкт/клас показує іншому об'єкту/класу. У наведеному нижче прикладі неявний контракт полягає в тому, що будь-який модуль запиту для `InventoryTracker` матиме метод `requestItems`.

**Погано:**

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

    // ПОГАНО: Ми створили залежність від реалізації конкретного запиту.
    // Треба щоб requestItems залежав тільки від методу запиту: `request`
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

**Добре:**

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

// Побудувавши залежності зовні та впровадивши їх, ми можемо легко
// замінити наш модуль запиту на новий та модний, що використовує вебсокети.
const inventoryTracker = new InventoryTracker(
  ["apples", "bananas"],
  new InventoryRequesterV2()
);
inventoryTracker.requestItems();
```

**[⬆ повернутися до змісту](#Зміст)**

## **Тестування**

Тестування важливіше, ніж доставка коду. Якщо у вас немає тестів або їх кількість недостатня, то кожного разу, надсилаючи код, ви не будете впевнені, що нічого не зламали. Питання про те, що становить адекватну кількість тестів, вирішується вашою командою, але 100% покриття (усі вирази та гілки) - це те, як ви досягаєте дуже високої впевненості та спокою як розробник. Це означає, що окрім чудового фреймворку тестування, вам також потрібно використовувати [гарний інструмент покриття](https://gotwarlost.github.io/istanbul/).

Немає приводу не писати тестів. Існує [безліч хороших JS фреймворків](https://jstherightway.org#testing-tools), тож знайдіть такий, якому віддає перевагу ваша команда. Коли ви знайдете такий, що підходить вашій команді, намагайтесь завжди писати тести для кожної нової функції/модуля, який ви вводите.Якщо вашим улюбленим методом є розробка через тестування (TDD), це чудово, але головне - просто переконайтеся, що ви досягаєте своїх планів з покриття тестами, перш ніж запускати будь-яку функцію або рефакторити існуючу.

### Одна концепція на один тест

**Погано:**

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

**Добре:**

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

**[⬆ повернутися до змісту](#Зміст)**

## **Асинхронність**

### Використовуйте проміси замість колбеків

Колбеки не є чистими, і вони викликають надмірну кількість вкладеності. З ES2015 / ES6,
проміси - це вбудований глобальний тип. Використовуйте їх!

**Погано:**

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

**Добре:**

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

**[⬆ повернутися до змісту](#Зміст)**

### Async/Await ще чистіший за проміси

Проміси є дуже чистою альтернативою колбекам, але ES2017 / ES8 приносить async та await, які пропонують ще більш чисте рішення. Все, що вам потрібно, - це функція, яка має в префіксі ключове слово `async`, і тоді ви можете імперативно писати логіку без прив'язки функцій до `then`.

**Погано:**

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

**Добре:**

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

**[⬆ повернутися до змісту](#Зміст)**

## **Обробка помилок**

Викидання помилок - це гарна річ! Воно означає, що під час виконання программа успішно
ідентифікувала, коли щось пішло не так, і дозволяє вам знати про це, зупиняючи виконання функції на поточному стеку, вбиваючи процесс (у Node) та повідомляючи вас трасировкою стеку у консолі.

### Не ігноруйте перехоплені помилки

Якщо нічого не робити із перехопленою помилкою, ви не зможете її виправити або зреагувати на неї. Логування помилки у консолі (`console.log`) не набагато краще, оскільки часто цей лог може загубитися в морі того, що друкується у консолі. Якщо ви вкладаєте фрагмент коду в `try/catch`, це означає, що ви
передбачаєте там помилку, і тому ви повинні мати план або створити шлях коду, коли помилка виникає.

**Погано:**

```javascript
try {
  functionThatMightThrow();
} catch (error) {
  console.log(error);
}
```

**Добре:**

```javascript
try {
  functionThatMightThrow();
} catch (error) {
  // Один варіант (більш помітний, ніж console.log):
  console.error(error);
  // Інший варіант:
  notifyUserOfError(error);
  // Інший варіант:
  reportErrorToService(error);
  // АБО використовуйте всі три!
}
```

### Не ігноруйте відхилені проміси

З тих причин, що і перехоплені помилки з `try/catch`.

**Погано:**

```javascript
getdata()
  .then(data => {
    functionThatMightThrow(data);
  })
  .catch(error => {
    console.log(error);
  });
```

**Добре:**

```javascript
getdata()
  .then(data => {
    functionThatMightThrow(data);
  })
  .catch(error => {
    // Один варіант (більш помітний, ніж console.log):
    console.error(error);
    // Інший варіант:
    notifyUserOfError(error);
    // AІнший варіант:
    reportErrorToService(error);
    // OАБО використовуйте всі три!
  });
```

**[⬆ повернутися до змісту](#Зміст)**

## **Форматування**

Formatting is subjective. Like many rules herein, there is no hard and fast
rule that you must follow. The main point is DO NOT ARGUE over formatting.
There are [tons of tools](https://standardjs.com/rules.html) to automate this.
Use one! It's a waste of time and money for engineers to argue over formatting.

For things that don't fall under the purview of automatic formatting
(indentation, tabs vs. spaces, double vs. single quotes, etc.) look here
for some guidance.

### Use consistent capitalization

JavaScript is untyped, so capitalization tells you a lot about your variables,
functions, etc. These rules are subjective, so your team can choose whatever
they want. The point is, no matter what you all choose, just be consistent.

**Bad:**

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

**Good:**

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

**[⬆ back to top](#table-of-contents)**

### Function callers and callees should be close

If a function calls another, keep those functions vertically close in the source
file. Ideally, keep the caller right above the callee. We tend to read code from
top-to-bottom, like a newspaper. Because of this, make your code read that way.

**Bad:**

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

**Good:**

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

**[⬆ back to top](#table-of-contents)**

## **Comments**

### Only comment things that have business logic complexity.

Comments are an apology, not a requirement. Good code _mostly_ documents itself.

**Bad:**

```javascript
function hashIt(data) {
  // The hash
  let hash = 0;

  // Length of string
  const length = data.length;

  // Loop through every character in data
  for (let i = 0; i < length; i++) {
    // Get character code.
    const char = data.charCodeAt(i);
    // Make the hash
    hash = (hash << 5) - hash + char;
    // Convert to 32-bit integer
    hash &= hash;
  }
}
```

**Good:**

```javascript
function hashIt(data) {
  let hash = 0;
  const length = data.length;

  for (let i = 0; i < length; i++) {
    const char = data.charCodeAt(i);
    hash = (hash << 5) - hash + char;

    // Convert to 32-bit integer
    hash &= hash;
  }
}
```

**[⬆ back to top](#table-of-contents)**

### Don't leave commented out code in your codebase

Version control exists for a reason. Leave old code in your history.

**Bad:**

```javascript
doStuff();
// doOtherStuff();
// doSomeMoreStuff();
// doSoMuchStuff();
```

**Good:**

```javascript
doStuff();
```

**[⬆ back to top](#table-of-contents)**

### Don't have journal comments

Remember, use version control! There's no need for dead code, commented code,
and especially journal comments. Use `git log` to get history!

**Bad:**

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

**Good:**

```javascript
function combine(a, b) {
  return a + b;
}
```

**[⬆ back to top](#table-of-contents)**

### Avoid positional markers

They usually just add noise. Let the functions and variable names along with the
proper indentation and formatting give the visual structure to your code.

**Bad:**

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

**Good:**

```javascript
$scope.model = {
  menu: "foo",
  nav: "bar"
};

const actions = function() {
  // ...
};
```

**[⬆ back to top](#table-of-contents)**

## Translation

This is also available in other languages:

- ![am](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Armenia.png) **Armenian**: [hanumanum/clean-code-javascript/](https://github.com/hanumanum/clean-code-javascript)
- ![bd](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Bangladesh.png) **Bangla(বাংলা)**: [InsomniacSabbir/clean-code-javascript/](https://github.com/InsomniacSabbir/clean-code-javascript/)
- ![br](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Brazil.png) **Brazilian Portuguese**: [fesnt/clean-code-javascript](https://github.com/fesnt/clean-code-javascript)
- ![cn](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/China.png) **Simplified Chinese**:
  - [alivebao/clean-code-js](https://github.com/alivebao/clean-code-js)
  - [beginor/clean-code-javascript](https://github.com/beginor/clean-code-javascript)
- ![tw](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Taiwan.png) **Traditional Chinese**: [AllJointTW/clean-code-javascript](https://github.com/AllJointTW/clean-code-javascript)
- ![fr](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/France.png) **French**: [GavBaros/clean-code-javascript-fr](https://github.com/GavBaros/clean-code-javascript-fr)
- ![de](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Germany.png) **German**: [marcbruederlin/clean-code-javascript](https://github.com/marcbruederlin/clean-code-javascript)
- ![id](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Indonesia.png) **Indonesia**: [andirkh/clean-code-javascript/](https://github.com/andirkh/clean-code-javascript/)
- ![it](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Italy.png) **Italian**: [frappacchio/clean-code-javascript/](https://github.com/frappacchio/clean-code-javascript/)
- ![ja](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Japan.png) **Japanese**: [mitsuruog/clean-code-javascript/](https://github.com/mitsuruog/clean-code-javascript/)
- ![kr](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/South-Korea.png) **Korean**: [qkraudghgh/clean-code-javascript-ko](https://github.com/qkraudghgh/clean-code-javascript-ko)
- ![pl](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Poland.png) **Polish**: [greg-dev/clean-code-javascript-pl](https://github.com/greg-dev/clean-code-javascript-pl)
- ![ru](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Russia.png) **Russian**:
  - [BoryaMogila/clean-code-javascript-ru/](https://github.com/BoryaMogila/clean-code-javascript-ru/)
  - [maksugr/clean-code-javascript](https://github.com/maksugr/clean-code-javascript)
- ![es](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Spain.png) **Spanish**: [tureey/clean-code-javascript](https://github.com/tureey/clean-code-javascript)
- ![es](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Uruguay.png) **Spanish**: [andersontr15/clean-code-javascript](https://github.com/andersontr15/clean-code-javascript-es)
- ![tr](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Turkey.png) **Turkish**: [bsonmez/clean-code-javascript](https://github.com/bsonmez/clean-code-javascript/tree/turkish-translation)
- ![vi](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Vietnam.png) **Vietnamese**: [hienvd/clean-code-javascript/](https://github.com/hienvd/clean-code-javascript/)

**[⬆ back to top](#table-of-contents)**
