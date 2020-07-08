# Руководство по написанию кода

## Оглавление

  1. [Типы](#types)
  1. [Объявление переменных](#references)
  1. [Объекты](#objects)
  1. [Массивы](#arrays)
  1. [Деструктуризация](#destructuring)
  1. [Строки](#strings)
  1. [Функции](#functions)
  1. [Стрелочные функции](#arrow-functions)
  1. [Классы и конструкторы](#classes--constructors)
  1. [Модули](#modules)
  1. [Итераторы и генераторы](#iterators-and-generators)
  1. [Свойства](#properties)
  1. [Переменные](#variables)
  1. [Подъём](#hoisting)
  1. [Операторы сравнения и равенства](#comparison-operators--equality)
  1. [Блоки](#blocks)
  1. [Управляющие операторы](#control-statements)
  1. [Комментарии](#comments)
  1. [Пробелы](#whitespace)
  1. [Запятые](#commas)
  1. [Точка с запятой](#semicolons)
  1. [Приведение типов](#type-casting--coercion)
  1. [Соглашение об именовании](#naming-conventions)
  1. [Аксессоры](#accessors)
  1. [События](#events)
  1. [jQuery](#jquery)
  1. [Поддержка ECMAScript 5](#ecmascript-5-compatibility)
  1. [Возможности ECMAScript 6+ (ES 2015+)](#ecmascript-6-es-2015-styles)
  1. [Стандартная библиотека](#standard-library)
  1. [Тестирование](#testing)
  1. [Производительность](#performance)
  1. [Ресурсы](#resources)
  1. [В реальной жизни](#in-the-wild)
  1. [Переводы](#translation)
  1. [Пообщаться с разработчиками Airbnb](#chat-with-us-about-javascript)
  1. [Участники перевода](#contributors)
  1. [Лицензия](#license)
  1. [Поправки](#amendments)


## <a name="references">Объявление переменных</a>

  <a name="references--prefer-const"></a><a name="2.1"></a>
  - [2.1](#references--prefer-const) Используйте `const` для объявления переменных; избегайте `var`. eslint: [`prefer-const`](https://eslint.org/docs/rules/prefer-const.html), [`no-const-assign`](https://eslint.org/docs/rules/no-const-assign.html)

    > Почему? Это гарантирует, что вы не сможете переопределять значения, т.к. это может привести к ошибкам и к усложнению понимания кода.

    ```javascript
    // плохо
    var a = 1;
    var b = 2;

    // хорошо
    const a = 1;
    const b = 2;
    ```

  <a name="references--disallow-var"></a><a name="2.2"></a>
  - [2.2](#references--disallow-var) Если вам необходимо переопределять значения, то используйте `let` вместо `var`. eslint: [`no-var`](https://eslint.org/docs/rules/no-var.html)

    > Почему? Область видимости `let` — блок, у `var` — функция.

    ```javascript
    // плохо
    var count = 1;
    if (true) {
      count += 1;
    }

    // хорошо, используйте let.
    let count = 1;
    if (true) {
      count += 1;
    }
    ```

  <a name="references--block-scope"></a><a name="2.3"></a>
  - [2.3](#references--block-scope) Помните, что у `let` и `const` блочная область видимости.

    ```javascript
    // const и let существуют только в том блоке, в котором они определены.
    {
      let a = 1;
      const b = 1;
    }
    console.log(a); // ReferenceError
    console.log(b); // ReferenceError
    ```
**[⬆ к оглавлению](#Оглавление)**

## <a name="objects">Объекты</a>

  <a name="objects--no-new"></a><a name="3.1"></a>
  - [3.1](#objects--no-new) Для создания объекта используйте литеральную нотацию. eslint: [`no-new-object`](https://eslint.org/docs/rules/no-new-object.html)

    ```javascript
    // плохо
    const item = new Object();

    // хорошо
    const item = {};
    ```

  <a name="es6-computed-properties"></a><a name="3.4"></a>
  - [3.2](#es6-computed-properties) Используйте вычисляемые имена свойств, когда создаёте объекты с динамическими именами свойств.

    > Почему? Они позволяют вам определить все свойства объекта в одном месте.

    ```javascript

    function getKey(k) {
      return `a key named ${k}`;
    }

    // плохо
    const obj = {
      id: 5,
      name: 'San Francisco',
    };
    obj[getKey('enabled')] = true;

    // хорошо
    const obj = {
      id: 5,
      name: 'San Francisco',
      [getKey('enabled')]: true,
    };
    ```

  <a name="es6-object-shorthand"></a><a name="3.5"></a>
  - [3.3](#es6-object-shorthand) Используйте сокращённую запись метода объекта. eslint: [`object-shorthand`](https://eslint.org/docs/rules/object-shorthand.html)

    ```javascript
    // плохо
    const atom = {
      value: 1,

      addValue: function (value) {
        return atom.value + value;
      },
    };

    // хорошо
    const atom = {
      value: 1,

      addValue(value) {
        return atom.value + value;
      },
    };
    ```

  <a name="es6-object-concise"></a><a name="3.6"></a>
  - [3.4](#es6-object-concise) Используйте сокращённую запись свойств объекта. eslint: [`object-shorthand`](https://eslint.org/docs/rules/object-shorthand.html)

    > Почему? Это короче и понятнее.

    ```javascript
    const lukeSkywalker = 'Luke Skywalker';

    // плохо
    const obj = {
      lukeSkywalker: lukeSkywalker,
    };

    // хорошо
    const obj = {
      lukeSkywalker,
    };
    ```

  <a name="objects--grouped-shorthand"></a><a name="3.7"></a>
  - [3.5](#objects--grouped-shorthand) Группируйте ваши сокращённые записи свойств в начале объявления объекта.

    > Почему? Так легче сказать, какие свойства используют сокращённую запись.

    ```javascript
    const anakinSkywalker = 'Anakin Skywalker';
    const lukeSkywalker = 'Luke Skywalker';

    // плохо
    const obj = {
      episodeOne: 1,
      twoJediWalkIntoACantina: 2,
      lukeSkywalker,
      episodeThree: 3,
      mayTheFourth: 4,
      anakinSkywalker,
    };

    // хорошо
    const obj = {
      lukeSkywalker,
      anakinSkywalker,
      episodeOne: 1,
      twoJediWalkIntoACantina: 2,
      episodeThree: 3,
      mayTheFourth: 4,
    };
    ```

  <a name="objects--quoted-props"></a><a name="3.8"></a>
  - [3.6](#objects--quoted-props) Только недопустимые идентификаторы помещаются в кавычки. eslint: [`quote-props`](https://eslint.org/docs/rules/quote-props.html)

    > Почему? На наш взгляд, такой код легче читать. Это улучшает подсветку синтаксиса, а также облегчает оптимизацию для многих JS-движков.

    ```javascript
    // плохо
    const bad = {
      'foo': 3,
      'bar': 4,
      'data-blah': 5,
    };

    // хорошо
    const good = {
      foo: 3,
      bar: 4,
      'data-blah': 5,
    };
    ```

  <a name="objects--rest-spread"></a>
  - [3.8](#objects--rest-spread) Используйте оператор расширения вместо [`Object.assign`](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Object/assign) для поверхностного копирования объектов. Используйте синтаксис оставшихся свойств, чтобы получить новый объект с некоторыми опущенными свойствами.

    ```javascript
    // очень плохо
    const original = { a: 1, b: 2 };
    const copy = Object.assign(original, { c: 3 }); // эта переменная изменяет `original` ಠ_ಠ
    delete copy.a; // если сделать так

    // плохо
    const original = { a: 1, b: 2 };
    const copy = Object.assign({}, original, { c: 3 }); // copy => { a: 1, b: 2, c: 3 }

    // хорошо
    const original = { a: 1, b: 2 };
    const copy = { ...original, c: 3 }; // copy => { a: 1, b: 2, c: 3 }

    const { a, ...noA } = copy; // noA => { b: 2, c: 3 }
    ```

**[⬆ к оглавлению](#Оглавление)**

## <a name="arrays">Массивы</a>

  <a name="arrays--literals"></a><a name="4.1"></a>
  - [4.1](#arrays--literals) Для создания массива используйте литеральную нотацию. eslint: [`no-array-constructor`](https://eslint.org/docs/rules/no-array-constructor.html)

    ```javascript
    // плохо
    const items = new Array();

    // хорошо
    const items = [];
    ```

  <a name="arrays--push"></a><a name="4.2"></a>
  - [4.2](#arrays--push) Для добавления элемента в массив используйте [Array#push](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Array/push) вместо прямого присваивания.

    ```javascript
    const someStack = [];

    // плохо
    someStack[someStack.length] = 'abracadabra';

    // хорошо
    someStack.push('abracadabra');
    ```

  <a name="es6-array-spreads"></a><a name="4.3"></a>
  - [4.3](#es6-array-spreads) Для копирования массивов используйте оператор расширения `...`.

    ```javascript
    // плохо
    const len = items.length;
    const itemsCopy = [];
    let i;

    for (i = 0; i < len; i += 1) {
      itemsCopy[i] = items[i];
    }

    // хорошо
    const itemsCopy = [...items];
    ```

  <a name="arrays--from"></a>
  <a name="arrays--from-iterable"></a><a name="4.4"></a>
  - [4.4](#arrays--from-iterable) Для преобразования итерируемого объекта в массив используйте оператор расширения `...` вместо [`Array.from`](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Array/from).

    ```javascript
    const foo = document.querySelectorAll('.foo');

    // хорошо
    const nodes = Array.from(foo);

    // отлично
    const nodes = [...foo];
    ```

  <a name="arrays--from-array-like"></a>
  - [4.5](#arrays--from-array-like) Используйте [`Array.from`](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Array/from) для преобразования массивоподобного объекта в массив.

    ```javascript
    const arrLike = { 0: 'foo', 1: 'bar', 2: 'baz', length: 3 };

    // плохо
    const arr = Array.prototype.slice.call(arrLike);

    // хорошо
    const arr = Array.from(arrLike);
    ```

  <a name="arrays--mapping"></a>
  - [4.6](#arrays--mapping) Используйте [`Array.from`](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Array/from) вместо оператора расширения `...` для маппинга итерируемых объектов, это позволяет избежать создания промежуточного массива.

    ```javascript
    // плохо
    const baz = [...foo].map(bar);

    // хорошо
    const baz = Array.from(foo, bar);
    ```

  <a name="arrays--callback-return"></a><a name="4.5"></a>
  - [4.7](#arrays--callback-return) Используйте операторы `return` внутри функций обратного вызова в методах массива. Можно опустить `return`, когда тело функции состоит из одной инструкции, возвращающей выражение без побочных эффектов. [8.2](#arrows--implicit-return). eslint: [`array-callback-return`](https://eslint.org/docs/rules/array-callback-return)

    ```javascript
    // хорошо
    [1, 2, 3].map((x) => {
      const y = x + 1;
      return x * y;
    });

    // хорошо
    [1, 2, 3].map((x) => x + 1);

    // плохо - нет возвращаемого значения, следовательно, `acc` становится `undefined` после первой итерации
    [[0, 1], [2, 3], [4, 5]].reduce((acc, item, index) => {
      const flatten = acc.concat(item);
    });

    // хорошо
    [[0, 1], [2, 3], [4, 5]].reduce((acc, item, index) => {
      const flatten = acc.concat(item);
      return flatten;
    });

    // плохо
    inbox.filter((msg) => {
      const { subject, author } = msg;
      if (subject === 'Mockingbird') {
        return author === 'Harper Lee';
      } else {
        return false;
      }
    });

    // хорошо
    inbox.filter((msg) => {
      const { subject, author } = msg;
      if (subject === 'Mockingbird') {
        return author === 'Harper Lee';
      }

      return false;
    });
    ```

  <a name="arrays--bracket-newline"></a>
  - [4.8](#arrays--bracket-newline) Если массив располагается на нескольких строках, то используйте разрывы строк после открытия и перед закрытием скобок.

    ```javascript
    // плохо
    const arr = [
      [0, 1], [2, 3], [4, 5],
    ];

    const objectInArray = [{
      id: 1,
    }, {
      id: 2,
    }];

    const numberInArray = [
      1, 2,
    ];

    // хорошо
    const arr = [[0, 1], [2, 3], [4, 5]];

    const objectInArray = [
      {
        id: 1,
      },
      {
        id: 2,
      },
    ];

    const numberInArray = [
      1,
      2,
    ];
    ```

**[⬆ к оглавлению](#Оглавление)**

## <a name="destructuring">Деструктуризация</a>

  <a name="destructuring--object"></a><a name="5.1"></a>
  - [5.1](#destructuring--object) При обращении к нескольким свойствам объекта используйте деструктуризацию объекта. eslint: [`prefer-destructuring`](https://eslint.org/docs/rules/prefer-destructuring)

    > Почему? Деструктуризация избавляет вас от создания временных переменных для этих свойств.

    ```javascript
    // плохо
    function getFullName(user) {
      const firstName = user.firstName;
      const lastName = user.lastName;

      return `${firstName} ${lastName}`;
    }

    // хорошо
    function getFullName(user) {
      const { firstName, lastName } = user;
      return `${firstName} ${lastName}`;
    }

    // отлично
    function getFullName({ firstName, lastName }) {
      return `${firstName} ${lastName}`;
    }
    ```

  <a name="destructuring--array"></a><a name="5.2"></a>
  - [5.2](#destructuring--array) Используйте деструктуризацию массивов. eslint: [`prefer-destructuring`](https://eslint.org/docs/rules/prefer-destructuring)

    ```javascript
    const arr = [1, 2, 3, 4];

    // плохо
    const first = arr[0];
    const second = arr[1];

    // хорошо
    const [first, second] = arr;
    ```

  <a name="destructuring--object-over-array"></a><a name="5.3"></a>
  - [5.3](#destructuring--object-over-array) Используйте деструктуризацию объекта для множества возвращаемых значений, но не делайте тоже самое с массивами.

    > Почему? Вы сможете добавить новые свойства через некоторое время или изменить порядок без последствий.

    ```javascript
    // плохо
    function processInput(input) {
      // затем происходит чудо
      return [left, right, top, bottom];
    }

    // при вызове нужно подумать о порядке возвращаемых данных
    const [left, __, top] = processInput(input);

    // хорошо
    function processInput(input) {
      // затем происходит чудо
      return { left, right, top, bottom };
    }

    // при вызове выбираем только необходимые данные
    const { left, top } = processInput(input);
    ```

**[⬆ к оглавлению](#Оглавление)**

## <a name="strings">Строки</a>

  <a name="strings--line-length"></a><a name="6.2"></a>
  - [6.2](#strings--line-length) Строки, у которых в строчке содержится более 120 символов, не пишутся на нескольких строчках с использованием конкатенации.

    > Почему? Работать с разбитыми строками неудобно и это затрудняет поиск по коду.

    ```javascript
    // плохо
    const errorMessage = 'This is a super long error that was thrown because \
    of Batman. When you stop to think about how Batman had anything to do \
    with this, you would get nowhere \
    fast.';

    // плохо
    const errorMessage = 'This is a super long error that was thrown because ' +
      'of Batman. When you stop to think about how Batman had anything to do ' +
      'with this, you would get nowhere fast.';

    // хорошо
    const errorMessage = 'This is a super long error that was thrown because of Batman. When you stop to think about how Batman had anything to do with this, you would get nowhere fast.';
    ```

  <a name="es6-template-literals"></a><a name="6.4"></a>
  - [6.3](#es6-template-literals) При создании строки программным путём используйте шаблонные строки вместо конкатенации. eslint: [`prefer-template`](https://eslint.org/docs/rules/prefer-template.html) [`template-curly-spacing`](https://eslint.org/docs/rules/template-curly-spacing)

    > Почему? Шаблонные строки дают вам читабельность, лаконичный синтаксис с правильными символами перевода строк и функции интерполяции строки.

    ```javascript
    // плохо
    function sayHi(name) {
      return 'How are you, ' + name + '?';
    }

    // плохо
    function sayHi(name) {
      return ['How are you, ', name, '?'].join();
    }

    // плохо
    function sayHi(name) {
      return `How are you, ${ name }?`;
    }

    // хорошо
    function sayHi(name) {
      return `How are you, ${name}?`;
    }
    ```

  <a name="strings--escaping"></a>
  - [6.5](#strings--escaping) Не используйте в строках необязательные экранирующие символы. eslint: [`no-useless-escape`](https://eslint.org/docs/rules/no-useless-escape)

    > Почему? Обратные слеши ухудшают читабельность, поэтому они должны быть только при необходимости.

    ```javascript
    // плохо
    const foo = '\'this\' \i\s \"quoted\"';

    // хорошо
    const foo = '\'this\' is "quoted"';
    const foo = `my name is '${name}'`;
    ```

**[⬆ к оглавлению](#Оглавление)**

## <a name="functions">Функции</a>

  <a name="es6-rest"></a><a name="7.6"></a>
  - [7.6](#es6-rest) Никогда не используйте `arguments`, вместо этого используйте синтаксис оставшихся параметров `...`. eslint: [`prefer-rest-params`](https://eslint.org/docs/rules/prefer-rest-params)

    > Почему? `...` явно говорит о том, какие именно аргументы вы хотите извлечь. Кроме того, такой синтаксис создаёт настоящий массив, а не массивоподобный объект как `arguments`.

    ```javascript
    // плохо
    function concatenateAll() {
      const args = Array.prototype.slice.call(arguments);
      return args.join('');
    }

    // хорошо
    function concatenateAll(...args) {
      return args.join('');
    }
    ```

  <a name="es6-default-parameters"></a><a name="7.7"></a>
  - [7.7](#es6-default-parameters) Используйте синтаксис записи аргументов по умолчанию, а не изменяйте аргументы функции.

    ```javascript
    // очень плохо
    function handleThings(opts) {
      // Нет! Мы не должны изменять аргументы функции.
      // Плохо вдвойне: если переменная opts будет ложной,
      // то ей присвоится пустой объект, а не то что вы хотели.
      // Это приведёт к коварным ошибкам.
      opts = opts || {};
      // ...
    }

    // всё ещё плохо
    function handleThings(opts) {
      if (opts === void 0) {
        opts = {};
      }
      // ...
    }

    // хорошо
    function handleThings(opts = {}) {
      // ...
    }
    ```

  <a name="functions--default-side-effects"></a><a name="7.8"></a>
  - [7.8](#functions--default-side-effects) Избегайте побочных эффектов с параметрами по умолчанию.

    > Почему? И так всё понятно.

    ```javascript
    var b = 1;
    // плохо
    function count(a = b++) {
      console.log(a);
    }
    count();  // 1
    count();  // 2
    count(3); // 3
    count();  // 3
    ```

  <a name="functions--defaults-last"></a><a name="7.9"></a>
  - [7.9](#functions--defaults-last) Всегда вставляйте последними параметры по умолчанию.

    ```javascript
    // плохо
    function handleThings(opts = {}, name) {
      // ...
    }

    // хорошо
    function handleThings(name, opts = {}) {
      // ...
    }
    ```

  <a name="functions--reassign-params"></a><a name="7.13"></a>
  - [7.13](#functions--reassign-params) Никогда не переназначайте параметры. eslint: [`no-param-reassign`](https://eslint.org/docs/rules/no-param-reassign.html)

    > Почему? Переназначенные параметры могут привести к неожиданному поведению, особенно при обращении к `arguments`. Это также может вызывать проблемы оптимизации, особенно в V8.

    ```javascript
    // плохо
    function f1(a) {
      a = 1;
      // ...
    }

    function f2(a) {
      if (!a) { a = 1; }
      // ...
    }

    // хорошо
    function f3(a) {
      const b = a || 1;
      // ...
    }

    function f4(a = 1) {
      // ...
    }
    ```

  <a name="functions--spread-vs-apply"></a><a name="7.14"></a>
  - [7.14](#functions--spread-vs-apply) Отдавайте предпочтение использованию оператора расширения `...` при вызове вариативной функции. eslint: [`prefer-spread`](https://eslint.org/docs/rules/prefer-spread)

    > Почему? Это чище, вам не нужно предоставлять контекст, и не так просто составить `new` с `apply`.

    ```javascript
    // плохо
    const x = [1, 2, 3, 4, 5];
    console.log.apply(console, x);

    // хорошо
    const x = [1, 2, 3, 4, 5];
    console.log(...x);

    // плохо
    new (Function.prototype.bind.apply(Date, [null, 2016, 8, 5]));

    // хорошо
    new Date(...[2016, 8, 5]);
    ```

  <a name="functions--signature-invocation-indentation"></a>
  - [7.15](#functions--signature-invocation-indentation) Функции с многострочным определением или запуском должны содержать такие же отступы, как и другие многострочные списки в этом руководстве: с каждым элементом на отдельной строке, с запятой в конце элемента. eslint: [`function-paren-newline`](https://eslint.org/docs/rules/function-paren-newline)

    ```javascript
    // плохо
    function foo(bar,
                 baz,
                 quux) {
      // ...
    }

    // хорошо
    function foo(
      bar,
      baz,
      quux,
    ) {
      // ...
    }

    // плохо
    console.log(foo,
      bar,
      baz);

    // хорошо
    console.log(
      foo,
      bar,
      baz,
    );
    ```

**[⬆ к оглавлению](#Оглавление)**

## <a name="arrow-functions">Стрелочные функции</a>

  <a name="arrows--use-them"></a><a name="8.1"></a>
  - [8.1](#arrows--use-them) Когда вам необходимо использовать анонимную функцию (например, при передаче встроенной функции обратного вызова), используйте стрелочную функцию. eslint: [`prefer-arrow-callback`](https://eslint.org/docs/rules/prefer-arrow-callback.html), [`arrow-spacing`](https://eslint.org/docs/rules/arrow-spacing.html)

    > Почему? Таким образом создаётся функция, которая выполняется в контексте `this`, который мы обычно хотим, а также это более короткий синтаксис.

    > Почему бы и нет? Если у вас есть довольно сложная функция, вы можете переместить эту логику внутрь её собственного именованного функционального выражения.

    ```javascript
    // плохо
    [1, 2, 3].map(function (x) {
      const y = x + 1;
      return x * y;
    });

    // хорошо
    [1, 2, 3].map((x) => {
      const y = x + 1;
      return x * y;
    });
    ```

  <a name="arrows--implicit-return"></a><a name="8.2"></a>
  - [8.2](#arrows--implicit-return) Если тело функции состоит из одного оператора, возвращающего [выражение](https://developer.mozilla.org/ru/docs/Web/JavaScript/Guide/Expressions_and_Operators#Выражения) без побочных эффектов, то опустите фигурные скобки и используйте неявное возвращение. В противном случае, сохраните фигурные скобки и используйте оператор `return`. eslint: [`arrow-parens`](https://eslint.org/docs/rules/arrow-parens.html), [`arrow-body-style`](https://eslint.org/docs/rules/arrow-body-style.html)

    > Почему? Синтаксический сахар. Когда несколько функций соединены вместе, то это лучше читается.

    ```javascript
    // плохо
    [1, 2, 3].map((number) => {
      const nextNumber = number + 1;
      `A string containing the ${nextNumber}.`;
    });

    // хорошо
    [1, 2, 3].map((number) => `A string containing the ${number + 1}.`);

    // хорошо
    [1, 2, 3].map((number) => {
      const nextNumber = number + 1;
      return `A string containing the ${nextNumber}.`;
    });

    // хорошо
    [1, 2, 3].map((number, index) => ({
      [index]: number,
    }));

    // Неявный возврат с побочными эффектами
    function foo(callback) {
      const val = callback();
      if (val === true) {
        // Сделать что-то, если функция обратного вызова вернёт true
      }
    }

    let bool = false;

    // плохо
    foo(() => bool = true);

    // хорошо
    foo(() => {
      bool = true;
    });
    ```

  <a name="arrows--paren-wrap"></a><a name="8.3"></a>
  - [8.3](#arrows--paren-wrap) В случае, если выражение располагается на нескольких строках, то необходимо обернуть его в скобки для лучшей читаемости.

    > Почему? Это чётко показывает, где функция начинается и где заканчивается.

    ```javascript
    // плохо
    ['get', 'post', 'put'].map((httpMethod) => Object.prototype.hasOwnProperty.call(
        httpMagicObjectWithAVeryLongName,
        httpMethod,
      )
    );

    // хорошо
    ['get', 'post', 'put'].map((httpMethod) => (
      Object.prototype.hasOwnProperty.call(
        httpMagicObjectWithAVeryLongName,
        httpMethod,
      )
    ));
    ```

  <a name="arrows--one-arg-parens"></a><a name="8.4"></a>
  - [8.4](#arrows--one-arg-parens) Всегда оборачивайте аргументы круглыми скобками для ясности и согласованности. eslint: [`arrow-parens`](https://eslint.org/docs/rules/arrow-parens.html)
    > Почему? Минимизирует различия при удалении или добавлении аргументов.

    ```javascript
    // плохо
    [1, 2, 3].map(x => x * x);

    // хорошо
    [1, 2, 3].map((x) => x * x);

    // плохо
    [1, 2, 3].map(number => (
      `A long string with the ${number}. It’s so long that we don’t want it to take up space on the .map line!`
    ));

    // хорошо
    [1, 2, 3].map((number) => (
      `A long string with the ${number}. It’s so long that we don’t want it to take up space on the .map line!`
    ));

    // плохо
    [1, 2, 3].map(x => {
      const y = x + 1;
      return x * y;
    });

    // хорошо
    [1, 2, 3].map((x) => {
      const y = x + 1;
      return x * y;
    });
    ```

  <a name="arrows--confusing"></a><a name="8.5"></a>
  - [8.5](#arrows--confusing) Избегайте схожести стрелочной функции (`=>`) с операторами сравнения (`<=`, `>=`). eslint: [`no-confusing-arrow`](https://eslint.org/docs/rules/no-confusing-arrow)

    ```javascript
    // плохо
    const itemHeight = (item) => item.height <= 256 ? item.largeSize : item.smallSize;

    // плохо
    const itemHeight = (item) => item.height >= 256 ? item.largeSize : item.smallSize;

    // хорошо
    const itemHeight = (item) => (item.height <= 256 ? item.largeSize : item.smallSize);

    // хорошо
    const itemHeight = (item) => {
      const { height, largeSize, smallSize } = item;
      return height <= 256 ? largeSize : smallSize;
    };
    ```

**[⬆ к оглавлению](#Оглавление)**

## <a name="classes--constructors">Классы и конструкторы</a>

  <a name="constructors--chaining"></a><a name="9.3"></a>
  - [9.3](#constructors--chaining) Методы могут возвращать `this`, чтобы делать цепочки вызовов.

    ```javascript
    // плохо
    Jedi.prototype.jump = function () {
      this.jumping = true;
      return true;
    };

    Jedi.prototype.setHeight = function (height) {
      this.height = height;
    };

    const luke = new Jedi();
    luke.jump(); // => true
    luke.setHeight(20); // => undefined

    // хорошо
    class Jedi {
      jump() {
        this.jumping = true;
        return this;
      }

      setHeight(height) {
        this.height = height;
        return this;
      }
    }

    const luke = new Jedi();

    luke.jump().setHeight(20);
    ```

  <a name="constructors--tostring"></a><a name="9.4"></a>
  - [9.4](#constructors--tostring) Вы можете определить свой собственный метод `toString()`, просто убедитесь, что он успешно работает и не создаёт никаких побочных эффектов.

    ```javascript
    class Jedi {
      constructor(options = {}) {
        this.name = options.name || 'no name';
      }

      getName() {
        return this.name;
      }

      toString() {
        return `Jedi - ${this.getName()}`;
      }
    }
    ```

  <a name="constructors--no-useless"></a><a name="9.5"></a>
  - [9.5](#constructors--no-useless) У классов есть конструктор по умолчанию, если он не задан явно. Можно опустить пустой конструктор или конструктор, который только делегирует выполнение родительскому классу. eslint: [`no-useless-constructor`](https://eslint.org/docs/rules/no-useless-constructor)

    ```javascript
    // плохо
    class Jedi {
      constructor() {}

      getName() {
        return this.name;
      }
    }

    // плохо
    class Rey extends Jedi {
      constructor(...args) {
        super(...args);
      }
    }

    // хорошо
    class Rey extends Jedi {
      constructor(...args) {
        super(...args);
        this.name = 'Rey';
      }
    }
    ```

  <a name="classes--no-duplicate-members"></a>
  - [9.6](#classes--no-duplicate-members) Избегайте дублирующих членов класса. eslint: [`no-dupe-class-members`](https://eslint.org/docs/rules/no-dupe-class-members)

    > Почему? Если объявление члена класса повторяется, без предупреждения будет использовано последнее. Наличие дубликатов скорее всего приведёт к ошибке.

    ```javascript
    // плохо
    class Foo {
      bar() { return 1; }
      bar() { return 2; }
    }

    // хорошо
    class Foo {
      bar() { return 1; }
    }

    // хорошо
    class Foo {
      bar() { return 2; }
    }
    ```

  <a name="classes--methods-use-this"></a>
  - [9.7](#classes--methods-use-this) Метод класса должен использовать `this` или быть преобразованным в статический метод, если только внешняя библиотека или фреймворк не требуют использования определённых нестатических методов. Будучи методом экземпляра, следует указать, что он ведёт себя по-разному в зависимости от свойств получателя. eslint: [`class-methods-use-this`](https://eslint.org/docs/rules/class-methods-use-this)

    ```javascript
    // плохо
    class Foo {
      bar() {
        console.log('bar');
      }
    }
    // хорошо - используется this
    class Foo {
      bar() {
        console.log(this.bar);
      }
    }
    // хорошо - конструктор освобождается
    class Foo {
      constructor() {
        // ...
      }
    }
    // хорошо - статические методы не должны использовать this
    class Foo {
      static bar() {
        console.log('bar');
      }
    }
    ```

**[⬆ к оглавлению](#Оглавление)**

## <a name="modules">Модули</a>

  <a name="modules--use-them"></a><a name="10.1"></a>
  - [10.1](#modules--use-them) Всегда используйте модули (`import`/`export`) вместо нестандартных модульных систем. Вы всегда сможете транспилировать код в вашу любимую модульную систему.

    > Почему? Модули — это будущее. Давайте начнём использовать будущее уже сейчас!

    ```javascript
    // плохо
    const AirbnbStyleGuide = require('./AirbnbStyleGuide');
    module.exports = AirbnbStyleGuide.es6;

    // ok
    import AirbnbStyleGuide from './AirbnbStyleGuide';
    export default AirbnbStyleGuide.es6;

    // отлично
    import { es6 } from './AirbnbStyleGuide';
    export default es6;
    ```

  <a name="modules--no-duplicate-imports"></a>
  - [10.4](#modules--no-duplicate-imports) Импортируйте из пути только один раз.
 eslint: [`no-duplicate-imports`](https://eslint.org/docs/rules/no-duplicate-imports)
    > Почему? Наличие нескольких строк, которые импортируют из одного и того же файла, может сделать код неподдерживаемым.

    ```javascript
    // плохо
    import foo from 'foo';
    // … какие-то другие импорты … //
    import { named1, named2 } from 'foo';

    // хорошо
    import foo, { named1, named2 } from 'foo';

    // хорошо
    import foo, {
      named1,
      named2,
    } from 'foo';
    ```

  <a name="modules--no-mutable-exports"></a>
  - [10.5](#modules--no-mutable-exports) Не экспортируйте изменяемые переменные.
 eslint: [`import/no-mutable-exports`](https://github.com/benmosher/eslint-plugin-import/blob/master/docs/rules/no-mutable-exports.md)
    > Почему? Вообще, следует избегать мутации, в особенности при экспорте изменяемых переменных. Несмотря на то, что эта техника может быть необходима в редких случаях, в основном только константа должна быть экспортирована.

    ```javascript
    // плохо
    let foo = 3;
    export { foo };

    // хорошо
    const foo = 3;
    export { foo };
    ```

  <a name="modules--imports-first"></a>
  - [10.7](#modules--imports-first) Поместите все импорты выше остальных инструкций.
 eslint: [`import/first`](https://github.com/benmosher/eslint-plugin-import/blob/master/docs/rules/first.md)
    > Почему? Так как `import` обладает подъёмом, то хранение их всех в начале файла предотвращает от неожиданного поведения.

    ```javascript
    // плохо
    import foo from 'foo';
    foo.init();

    import bar from 'bar';

    // хорошо
    import foo from 'foo';
    import bar from 'bar';

    foo.init();
    ```

  <a name="modules--multiline-imports-over-newlines"></a>
  - [10.8](#modules--multiline-imports-over-newlines) Импорты на нескольких строках должны быть с отступами как у многострочных литералов массива и объекта.

    > Почему? Фигурные скобки следуют тем же правилам отступа как и любая другая фигурная скобка блока в этом руководстве, тоже самое касается висячих запятых.

    ```javascript
    // плохо
    import {longNameA, longNameB, longNameC, longNameD, longNameE} from 'path';

    // хорошо
    import {
      longNameA,
      longNameB,
      longNameC,
      longNameD,
      longNameE,
    } from 'path';
    ```

  <a name="modules--no-webpack-loader-syntax"></a>
  - [10.9](#modules--no-webpack-loader-syntax) Запретите синтаксис загрузчика Webpack в импорте.
 eslint: [`import/no-webpack-loader-syntax`](https://github.com/benmosher/eslint-plugin-import/blob/master/docs/rules/no-webpack-loader-syntax.md)
    > Почему? Использование Webpack синтаксиса связывает код с упаковщиком модулей. Предпочтительно использовать синтаксис загрузчика в `webpack.config.js`.

    ```javascript
    // плохо
    import fooSass from 'css!sass!foo.scss';
    import barCss from 'style!css!bar.css';

    // хорошо
    import fooSass from 'foo.scss';
    import barCss from 'bar.css';
    ```

**[⬆ к оглавлению](#Оглавление)**

## <a name="properties">Свойства</a>

  <a name="properties--dot"></a><a name="12.1"></a>
  - [12.1](#properties--dot) Используйте точечную нотацию для доступа к свойствам. eslint: [`dot-notation`](https://eslint.org/docs/rules/dot-notation.html)

    ```javascript
    const luke = {
      jedi: true,
      age: 28,
    };

    // плохо
    const isJedi = luke['jedi'];

    // хорошо
    const isJedi = luke.jedi;
    ```

  <a name="properties--bracket"></a><a name="12.2"></a>
  - [12.2](#properties--bracket) Используйте скобочную нотацию `[]`, когда название свойства хранится в переменной.

    ```javascript
    const luke = {
      jedi: true,
      age: 28,
    };

    function getProp(prop) {
      return luke[prop];
    }

    const isJedi = getProp('jedi');
    ```

  <a name="es2016-properties--exponentiation-operator"></a>
  - [12.3](#es2016-properties--exponentiation-operator) Используйте оператор `**` для возведения в степень. eslint: [`no-restricted-properties`](https://eslint.org/docs/rules/no-restricted-properties).

    ```javascript
    // плохо
    const binary = Math.pow(2, 10);

    // хорошо
    const binary = 2 ** 10;
    ```

**[⬆ к оглавлению](#Оглавление)**

## <a name="variables">Переменные</a>

  <a name="variables--one-const"></a><a name="13.2"></a>
  - [13.2](#variables--one-const) Используйте объявление `const` или `let` для каждой переменной или присвоения. eslint: [`one-var`](https://eslint.org/docs/rules/one-var.html)

    > Почему? Таким образом проще добавить новые переменные. Также вы никогда не будете беспокоиться о перемещении `;` и `,` и об отображении изменений в пунктуации. Вы также можете пройтись по каждому объявлению с помощью отладчика, вместо того, чтобы прыгать через все сразу.

    ```javascript
    // плохо
    const items = getItems(),
        goSportsTeam = true,
        dragonball = 'z';

    // плохо
    // (сравните с кодом выше и попытайтесь найти ошибку)
    const items = getItems(),
        goSportsTeam = true;
        dragonball = 'z';

    // хорошо
    const items = getItems();
    const goSportsTeam = true;
    const dragonball = 'z';
    ```

  <a name="variables--const-let-group"></a><a name="13.3"></a>
  - [13.3](#variables--const-let-group) В первую очередь группируйте `const`, а затем `let`.

    > Почему? Это полезно, когда в будущем вам понадобится создать переменную, зависимую от предыдущих.

    ```javascript
    // плохо
    let i, len, dragonball,
        items = getItems(),
        goSportsTeam = true;

    // плохо
    let i;
    const items = getItems();
    let dragonball;
    const goSportsTeam = true;
    let len;

    // хорошо
    const goSportsTeam = true;
    const items = getItems();
    let dragonball;
    let i;
    let length;
    ```

  <a name="variables--define-where-used"></a><a name="13.4"></a>
  - [13.4](#variables--define-where-used) Создавайте переменные там, где они вам необходимы, но помещайте их в подходящее место.

    > Почему? `let` и `const` имеют блочную область видимости, а не функциональную.

    ```javascript
    // плохо - вызов ненужной функции
    function checkName(hasName) {
      const name = getName();

      if (hasName === 'test') {
        return false;
      }

      if (name === 'test') {
        this.setName('');
        return false;
      }

      return name;
    }

    // хорошо
    function checkName(hasName) {
      if (hasName === 'test') {
        return false;
      }

      const name = getName();

      if (name === 'test') {
        this.setName('');
        return false;
      }

      return name;
    }

  <a name="variables--no-unused-vars"></a>
  - [13.8](#variables--no-unused-vars) Запретить неиспользуемые переменные. eslint: [`no-unused-vars`](https://eslint.org/docs/rules/no-unused-vars)

    > Почему? Переменные, которые объявлены и не используются в коде, скорее всего, являются ошибкой из-за незавершённого рефакторинга. Такие переменные занимают место в коде и могут привести к путанице при чтении.

**[⬆ к оглавлению](#Оглавление)**

## <a name="comparison-operators--equality">Операторы сравнения и равенства</a>

  <a name="comparison--eqeqeq"></a><a name="15.1"></a>
  - [15.1](#comparison--eqeqeq) Используйте `===` и `!==` вместо `==` и `!=`. eslint: [`eqeqeq`](https://eslint.org/docs/rules/eqeqeq.html)

  <a name="comparison--shortcuts"></a><a name="15.3"></a>
  - [15.3](#comparison--shortcuts) Используйте сокращения для булевских типов, а для строк и чисел применяйте явное сравнение.

    ```javascript
    // плохо
    if (isValid === true) {
      // ...
    }

    // хорошо
    if (isValid) {
      // ...
    }

    // плохо
    if (name) {
      // ...
    }

    // хорошо
    if (name !== '') {
      // ...
    }

    // плохо
    if (collection.length) {
      // ...
    }

    // хорошо
    if (collection.length > 0) {
      // ...
    }
    ```

  <a name="comparison--switch-blocks"></a><a name="15.5"></a>
  - [15.5](#comparison--switch-blocks) Используйте фигурные скобки для `case` и `default`, если они содержат лексические декларации (например, `let`, `const`, `function`, и `class`). eslint: [`no-case-declarations`](https://eslint.org/docs/rules/no-case-declarations.html).

    > Почему? Лексические декларации видны во всем `switch` блоке, но инициализируются только при присваивании, которое происходит при входе в блок `case`. Возникают проблемы, когда множество `case` пытаются определить одно и то же.

    ```javascript
    // плохо
    switch (foo) {
      case 1:
        let x = 1;
        break;
      case 2:
        const y = 2;
        break;
      case 3:
        function f() {
          // ...
        }
        break;
      default:
        class C {}
    }

    // хорошо
    switch (foo) {
      case 1: {
        let x = 1;
        break;
      }
      case 2: {
        const y = 2;
        break;
      }
      case 3: {
        function f() {
          // ...
        }
        break;
      }
      case 4:
        bar();
        break;
      default: {
        class C {}
      }
    }
    ```

  <a name="comparison--nested-ternaries"></a><a name="15.6"></a>
  - [15.6](#comparison--nested-ternaries) Тернарные операторы не должны быть вложены и в большинстве случаев должны быть расположены на одной строке. eslint: [`no-nested-ternary`](https://eslint.org/docs/rules/no-nested-ternary.html).

    ```javascript
    // плохо
    const foo = maybe1 > maybe2
      ? "bar"
      : value1 > value2 ? "baz" : null;

    // отлично
    // разбит на два отдельных тернарных выражения
    const maybeNull = value1 > value2 ? 'baz' : null;

    const foo = maybe1 > maybe2 ? 'bar' : maybeNull;
    ```

  <a name="comparison--unneeded-ternary"></a><a name="15.7"></a>
  - [15.7](#comparison--unneeded-ternary) Избегайте ненужных тернарных операторов. eslint: [`no-unneeded-ternary`](https://eslint.org/docs/rules/no-unneeded-ternary.html).

    ```javascript
    // плохо
    const foo = a ? a : b;
    const bar = c ? true : false;
    const baz = c ? false : true;

    // хорошо
    const foo = a || b;
    const bar = !!c;
    const baz = !c;
    ```

  <a name="comparison--no-mixed-operators"></a>
  - [15.8](#comparison--no-mixed-operators) При смешивании операторов, помещайте их в круглые скобки. Единственным исключением являются стандартные арифметические операторы: `+`, `-` и `**`, так как их приоритет широко известен. Мы рекомендуем заключить `/` и `*` в круглые скобки, поскольку их приоритет может быть неоднозначным, когда они смешиваются. eslint: [`no-mixed-operators`](https://eslint.org/docs/rules/no-mixed-operators.html)

    > Почему? Это улучшает читаемость и уточняет намерения разработчика.

    ```javascript
    // плохо
    const foo = a && b < 0 || c > 0 || d + 1 === 0;

    // плохо
    const bar = a ** b - 5 % d;

    // плохо
    // можно ошибиться, думая что это (a || b) && c
    if (a || b && c) {
      return d;
    }

    // плохо
    const bar = a + b / c * d;

    // хорошо
    const foo = (a && b < 0) || c > 0 || (d + 1 === 0);

    // хорошо
    const bar = a ** b - (5 % d);

    // хорошо
    if (a || (b && c)) {
      return d;
    }

    // хорошо
    const bar = a + (b / c) * d;
    ```

**[⬆ к оглавлению](#Оглавление)**

## <a name="blocks">Блоки</a>

  <a name="blocks--braces"></a><a name="16.1"></a>
  - [16.1](#blocks--braces) Используйте фигурные скобки, когда блок кода занимает несколько строк. eslint: [`nonblock-statement-body-position`](https://eslint.org/docs/rules/nonblock-statement-body-position)

    ```javascript
    // плохо
    if (test)
      return false;

    // хорошо
    if (test) return false;

    // хорошо
    if (test) {
      return false;
    }

    // плохо
    function foo() { return false; }

    // хорошо
    function bar() {
      return false;
    }
    ```

  <a name="blocks--cuddled-elses"></a><a name="16.2"></a>
  - [16.2](#blocks--cuddled-elses) Если блоки кода в условии `if` и `else` занимают несколько строк, расположите оператор `else` на той же строчке, где находится закрывающая фигурная скобка блока `if`. eslint: [`brace-style`](https://eslint.org/docs/rules/brace-style.html)

    ```javascript
    // плохо
    if (test) {
      thing1();
      thing2();
    }
    else {
      thing3();
    }

    // хорошо
    if (test) {
      thing1();
      thing2();
    } else {
      thing3();
    }
    ```

  <a name="blocks--no-else-return"></a><a name="16.3"></a>
  - [16.3](#blocks--no-else-return) Если в блоке `if` всегда выполняется оператор `return`, последующий блок `else` не нужен. `return`  внутри блока `else if`, следующем за блоком `if`, который содержит `return`, может быть разделён на несколько блоков `if`. eslint: [`no-else-return`](https://eslint.org/docs/rules/no-else-return)

    ```javascript
    // плохо
    function foo() {
      if (x) {
        return x;
      } else {
        return y;
      }
    }

    // плохо
    function cats() {
      if (x) {
        return x;
      } else if (y) {
        return y;
      }
    }

    // плохо
    function dogs() {
      if (x) {
        return x;
      } else {
        if (y) {
          return y;
        }
      }
    }

    // хорошо
    function foo() {
      if (x) {
        return x;
      }

      return y;
    }

    // хорошо
    function cats() {
      if (x) {
        return x;
      }

      if (y) {
        return y;
      }
    }

    // хорошо
    function dogs(x) {
      if (x) {
        if (z) {
          return y;
        }
      } else {
        return z;
      }
    }
    ```

**[⬆ к оглавлению](#Оглавление)**

## <a name="control-statements">Управляющие операторы</a>

  <a name="control-statements"></a>
  - [17.1](#control-statements) Если ваш управляющий оператор (`if`, `while` и т.д.) слишком длинный или превышает максимальную длину строки, то каждое (сгруппированное) условие можно поместить на новую строку. Логический оператор должен располагаться в начале строки.

    > Почему? Наличие операторов в начале строки приводит к выравниванию операторов и напоминает цепочку методов. Это также улучшает читаемость, упрощая визуальное отслеживание сложной логики.

    ```javascript
    // плохо
    if ((foo === 123 || bar === 'abc') && doesItLookGoodWhenItBecomesThatLong() && isThisReallyHappening()) {
      thing1();
    }

    // плохо
    if (foo === 123 &&
      bar === 'abc') {
      thing1();
    }

    // плохо
    if (foo === 123
      && bar === 'abc') {
      thing1();
    }

    // плохо
    if (
      foo === 123 &&
      bar === 'abc'
    ) {
      thing1();
    }

    // хорошо
    if (
      foo === 123
      && bar === 'abc'
    ) {
      thing1();
    }

    // хорошо
    if (
      (foo === 123 || bar === 'abc')
      && doesItLookGoodWhenItBecomesThatLong()
      && isThisReallyHappening()
    ) {
      thing1();
    }

    // хорошо
    if (foo === 123 && bar === 'abc') {
      thing1();
    }
    ```

**[⬆ к оглавлению](#Оглавление)**

## <a name="comments">Комментарии</a>

  <a name="comments--multiline"></a><a name="17.1"></a>
  - [18.1](#comments--multiline) Используйте конструкцию `/** ... */` для многострочных комментариев.

    ```javascript
    // плохо
    // make() возвращает новый элемент
    // соответствующий переданному названию тега
    //
    // @param {String} tag
    // @return {Element} element
    function make(tag) {

      // ...

      return element;
    }

    // хорошо
    /**
     * make() возвращает новый элемент
     * соответствующий переданному названию тега
     */
    function make(tag) {

      // ...

      return element;
    }
    ```

  <a name="comments--singleline"></a><a name="17.2"></a>
  - [18.2](#comments--singleline) Используйте двойной слеш `//` для однострочных комментариев. Располагайте такие комментарии отдельной строкой над кодом, который хотите пояснить. Если комментарий не является первой строкой блока, добавьте сверху пустую строку.

    ```javascript
    // плохо
    const active = true;  // это текущая вкладка

    // хорошо
    // это текущая вкладка
    const active = true;

    // плохо
    function getType() {
      console.log('fetching type...');
      // установить по умолчанию тип 'no type'
      const type = this.type || 'no type';

      return type;
    }

    // хорошо
    function getType() {
      console.log('fetching type...');

      // установить по умолчанию тип 'no type'
      const type = this.type || 'no type';

      return type;
    }

    // тоже хорошо
    function getType() {
      // установить по умолчанию тип 'no type'
      const type = this.type || 'no type';

      return type;
    }
    ```

  <a name="comments--spaces"></a>
  - [18.3](#comments--spaces) Начинайте все комментарии с пробела, так их проще читать. eslint: [`spaced-comment`](https://eslint.org/docs/rules/spaced-comment)

    ```javascript
    // плохо
    //это текущая вкладка
    const active = true;

    // хорошо
    // это текущая вкладка
    const active = true;

    // плохо
    /**
     *make() возвращает новый элемент
     *соответствующий переданному названию тега
     */
    function make(tag) {

      // ...

      return element;
    }

    // хорошо
    /**
     * make() возвращает новый элемент
     * соответствующий переданному названию тега
     */
    function make(tag) {

      // ...

      return element;
    }
    ```

  <a name="comments--actionitems"></a><a name="17.3"></a>
  - [18.4](#comments--actionitems) Если комментарий начинается со слов `FIXME` или `TODO`, то это помогает другим разработчикам быстро понять, когда вы хотите указать на проблему, которую надо решить, или когда вы предлагаете решение проблемы, которое надо реализовать. Такие комментарии, в отличие от обычных, побуждают к действию: `FIXME: -- нужно разобраться с этим` или `TODO: -- нужно реализовать`.

  <a name="comments--fixme"></a><a name="17.4"></a>
  - [18.5](#comments--fixme) Используйте `// FIXME:`, чтобы описать проблему.

    ```javascript
    class Calculator extends Abacus {
      constructor() {
        super();

        // FIXME: здесь не должна использоваться глобальная переменная
        total = 0;
      }
    }
    ```

  <a name="comments--todo"></a><a name="17.5"></a>
  - [18.6](#comments--todo) Используйте `// TODO:`, чтобы описать решение проблемы.

    ```javascript
    class Calculator extends Abacus {
      constructor() {
        super();

        // TODO: нужна возможность задать total через параметры
        this.total = 0;
      }
    }
    ```

**[⬆ к оглавлению](#Оглавление)**

## <a name="whitespace">Пробелы</a>

  <a name="whitespace--before-blocks"></a><a name="18.2"></a>
  - [19.2](#whitespace--before-blocks) Ставьте 1 пробел перед открывающей фигурной скобкой у блока. eslint: [`space-before-blocks`](https://eslint.org/docs/rules/space-before-blocks.html)

    ```javascript
    // плохо
    function test(){
      console.log('test');
    }

    // хорошо
    function test() {
      console.log('test');
    }

    // плохо
    dog.set('attr',{
      age: '1 year',
      breed: 'Bernese Mountain Dog',
    });

    // хорошо
    dog.set('attr', {
      age: '1 year',
      breed: 'Bernese Mountain Dog',
    });
    ```

  <a name="whitespace--around-keywords"></a><a name="18.3"></a>
  - [19.3](#whitespace--around-keywords) Ставьте 1 пробел перед открывающей круглой скобкой в операторах управления (`if`, `while` и т.п.). Не оставляйте пробелов между списком аргументов и названием в объявлениях и вызовах функций. eslint: [`keyword-spacing`](https://eslint.org/docs/rules/keyword-spacing.html)

    ```javascript
    // плохо
    if(isJedi) {
      fight ();
    }

    // хорошо
    if (isJedi) {
      fight();
    }

    // плохо
    function fight () {
      console.log ('Swooosh!');
    }

    // хорошо
    function fight() {
      console.log('Swooosh!');
    }
    ```

  <a name="whitespace--infix-ops"></a><a name="18.4"></a>
  - [19.4](#whitespace--infix-ops) Разделяйте операторы пробелами. eslint: [`space-infix-ops`](https://eslint.org/docs/rules/space-infix-ops.html)

    ```javascript
    // плохо
    const x=y+5;

    // хорошо
    const x = y + 5;
    ```

  <a name="whitespace--newline-at-end"></a><a name="18.5"></a>
  - [19.5](#whitespace--newline-at-end) В конце файла оставляйте одну пустую строку. eslint: [`eol-last`](https://github.com/eslint/eslint/blob/master/docs/rules/eol-last.md)

    ```javascript
    // плохо
    import { es6 } from './AirbnbStyleGuide';
      // ...
    export default es6;
    ```

    ```javascript
    // плохо
    import { es6 } from './AirbnbStyleGuide';
      // ...
    export default es6;↵
    ↵
    ```

    ```javascript
    // хорошо
    import { es6 } from './AirbnbStyleGuide';
      // ...
    export default es6;↵
    ```


  <a name="whitespace--after-blocks"></a><a name="18.7"></a>
  - [19.7](#whitespace--after-blocks) Оставляйте пустую строку между блоком кода и следующей инструкцией.

    ```javascript
    // плохо
    if (foo) {
      return bar;
    }
    return baz;

    // хорошо
    if (foo) {
      return bar;
    }

    return baz;

    // плохо
    const obj = {
      foo() {
      },
      bar() {
      },
    };
    return obj;

    // хорошо
    const obj = {
      foo() {
      },

      bar() {
      },
    };

    return obj;

    // плохо
    const arr = [
      function foo() {
      },
      function bar() {
      },
    ];
    return arr;

    // хорошо
    const arr = [
      function foo() {
      },

      function bar() {
      },
    ];

    return arr;
    ```

  <a name="whitespace--padded-blocks"></a><a name="18.8"></a>
  - [19.8](#whitespace--padded-blocks) Не добавляйте отступы до или после кода внутри блока. eslint: [`padded-blocks`](https://eslint.org/docs/rules/padded-blocks.html)

    ```javascript
    // плохо
    function bar() {

      console.log(foo);

    }

    // тоже плохо
    if (baz) {

      console.log(qux);
    } else {
      console.log(foo);

    }

    // хорошо
    function bar() {
      console.log(foo);
    }

    // хорошо
    if (baz) {
      console.log(qux);
    } else {
      console.log(foo);
    }
    ```

  <a name="whitespace--no-multiple-blanks"></a>
  - [19.9](#whitespace--no-multiple-blanks) Не используйте множество пустых строк для заполнения кода. eslint: [`no-multiple-empty-lines`](https://eslint.org/docs/rules/no-multiple-empty-lines)

    <!-- markdownlint-disable MD012 -->
    ```javascript
    // плохо
    class Person {
      constructor(fullName, email, birthday) {
        this.fullName = fullName;


        this.email = email;


        this.setAge(birthday);
      }


      setAge(birthday) {
        const today = new Date();


        const age = this.getAge(today, birthday);


        this.age = age;
      }


      getAge(today, birthday) {
        // ..
      }
    }

    // good
    class Person {
      constructor(fullName, email, birthday) {
        this.fullName = fullName;
        this.email = email;
        this.setAge(birthday);
      }

      setAge(birthday) {
        const today = new Date();
        const age = getAge(today, birthday);
        this.age = age;
      }

      getAge(today, birthday) {
        // ..
      }
    }
    ```

  <a name="whitespace--in-parens"></a><a name="18.9"></a>
  - [19.10](#whitespace--in-parens) Не добавляйте пробелы между круглыми скобками и их содержимым. eslint: [`space-in-parens`](https://eslint.org/docs/rules/space-in-parens.html)

    ```javascript
    // плохо
    function bar( foo ) {
      return foo;
    }

    // хорошо
    function bar(foo) {
      return foo;
    }

    // плохо
    if ( foo ) {
      console.log(foo);
    }

    // хорошо
    if (foo) {
      console.log(foo);
    }
    ```

  <a name="whitespace--in-brackets"></a><a name="18.10"></a>
  - [19.11](#whitespace--in-brackets) Не добавляйте пробелы между квадратными скобками и их содержимым. eslint: [`array-bracket-spacing`](https://eslint.org/docs/rules/array-bracket-spacing.html)

    ```javascript
    // плохо
    const foo = [ 1, 2, 3 ];
    console.log(foo[ 0 ]);

    // хорошо
    const foo = [1, 2, 3];
    console.log(foo[0]);
    ```

  <a name="whitespace--in-braces"></a><a name="18.11"></a>
  - [19.12](#whitespace--in-braces) Добавляйте пробелы между фигурными скобками и их содержимым. eslint: [`object-curly-spacing`](https://eslint.org/docs/rules/object-curly-spacing.html)

    ```javascript
    // плохо
    const foo = {clark: 'kent'};

    // хорошо
    const foo = { clark: 'kent' };
    ```

  <a name="whitespace--max-len"></a><a name="18.12"></a>
  - [19.13](#whitespace--max-len) Старайтесь не допускать, чтобы строки были длиннее 120 символов (включая пробелы). Замечание: согласно [пункту выше](#strings--line-length), длинные строки с текстом освобождаются от этого правила и не должны разбиваться на несколько строк. eslint: [`max-len`](https://eslint.org/docs/rules/max-len.html)

    > Почему? Это обеспечивает удобство чтения и поддержки кода.

    ```javascript
    // плохо
    const foo = jsonData && jsonData.foo && jsonData.foo.bar && jsonData.foo.bar.baz && jsonData.foo.bar.baz.quux && jsonData.foo.bar.baz.quux.xyzzy;

    // плохо
    $.ajax({ method: 'POST', url: 'https://airbnb.com/', data: { name: 'John' } }).done(() => console.log('Congratulations!')).fail(() => console.log('You have failed this city.'));

    // хорошо
    const foo = jsonData
      && jsonData.foo
      && jsonData.foo.bar
      && jsonData.foo.bar.baz
      && jsonData.foo.bar.baz.quux
      && jsonData.foo.bar.baz.quux.xyzzy;

    // хорошо
    $.ajax({
      method: 'POST',
      url: 'https://airbnb.com/',
      data: { name: 'John' },
    })
      .done(() => console.log('Congratulations!'))
      .fail(() => console.log('You have failed this city.'));
    ```

  <a name="whitespace--block-spacing"></a>
  - [19.14](#whitespace--block-spacing) Требуйте согласованного расстояния между открывающим символом блока и следующим символом на одной и той же строке. Тоже самое касается расстояния между закрывающим символом блока и предыдущим символом. eslint: [`block-spacing`](https://eslint.org/docs/rules/block-spacing)

    ```javascript
    // плохо
    function foo() {return true;}
    if (foo) { bar = 0;}

    // хорошо
    function foo() { return true; }
    if (foo) { bar = 0; }
    ```

  <a name="whitespace--comma-spacing"></a>
  - [19.15](#whitespace--comma-spacing) Избегайте пробелов перед запятыми и ставьте его после. eslint: [`comma-spacing`](https://eslint.org/docs/rules/comma-spacing)

    ```javascript
    // плохо
    var foo = 1,bar = 2;
    var arr = [1 , 2];

    // хорошо
    var foo = 1, bar = 2;
    var arr = [1, 2];
    ```

  <a name="whitespace--computed-property-spacing"></a>
  - [19.16](#whitespace--computed-property-spacing) Избегайте пробелов внутри скобок вычисляемого свойства. eslint: [`computed-property-spacing`](https://eslint.org/docs/rules/computed-property-spacing)

    ```javascript
    // плохо
    obj[foo ]
    obj[ 'foo']
    var x = {[ b ]: a}
    obj[foo[ bar ]]

    // хорошо
    obj[foo]
    obj['foo']
    var x = { [b]: a }
    obj[foo[bar]]
    ```

  <a name="whitespace--func-call-spacing"></a>
  - [19.17](#whitespace--func-call-spacing) Избегайте пробелов между функциями и их вызовами. eslint: [`func-call-spacing`](https://eslint.org/docs/rules/func-call-spacing)

    ```javascript
    // плохо
    func ();

    func
    ();

    // хорошо
    func();
    ```

  <a name="whitespace--key-spacing"></a>
  - [19.18](#whitespace--key-spacing) Обеспечьте согласованное расстояние между ключами и значениями в свойствах литералов объекта. eslint: [`key-spacing`](https://eslint.org/docs/rules/key-spacing)

    ```javascript
    // плохо
    var obj = { "foo" : 42 };
    var obj2 = { "foo":42 };

    // хорошо
    var obj = { "foo": 42 };
    ```

  <a name="whitespace--no-trailing-spaces"></a>
  - [19.19](#whitespace--no-trailing-spaces) Избегайте пробелов в конце строки. eslint: [`no-trailing-spaces`](https://eslint.org/docs/rules/no-trailing-spaces)

  <a name="whitespace--no-multiple-empty-lines"></a>
  - [19.20](#whitespace--no-multiple-empty-lines) Избегайте множества пустых строк и новой строки в начале файлов. Разрешайте только одну пустую строку в конце файла. eslint: [`no-multiple-empty-lines`](https://eslint.org/docs/rules/no-multiple-empty-lines)

    <!-- markdownlint-disable MD012 -->
    ```javascript
    // плохо - множество пустых строк
    var x = 1;


    var y = 2;

    // плохо - 2+ новых строк в конце файла
    var x = 1;
    var y = 2;


    // плохо - 1+ новая строка в начале файла

    var x = 1;
    var y = 2;

    // хорошо
    var x = 1;
    var y = 2;

    ```
    <!-- markdownlint-enable MD012 -->

**[⬆ к оглавлению](#Оглавление)**

## <a name="commas">Запятые</a>

  <a name="commas--leading-trailing"></a><a name="19.1"></a>
  - [20.1](#commas--leading-trailing) Не начинайте строку с запятой. eslint: [`comma-style`](https://eslint.org/docs/rules/comma-style.html)

    ```javascript
    // плохо
    const story = [
        once
      , upon
      , aTime
    ];

    // хорошо
    const story = [
      once,
      upon,
      aTime,
    ];

    // плохо
    const hero = {
        firstName: 'Ada'
      , lastName: 'Lovelace'
      , birthYear: 1815
      , superPower: 'computers'
    };

    // хорошо
    const hero = {
      firstName: 'Ada',
      lastName: 'Lovelace',
      birthYear: 1815,
      superPower: 'computers',
    };
    ```

  <a name="commas--dangling"></a><a name="19.2"></a>
  - [20.2](#commas--dangling) Добавляйте висячие запятые. eslint: [`comma-dangle`](https://eslint.org/docs/rules/comma-dangle.html)

    > Почему? Такой подход даёт понятную разницу при просмотре изменений. Кроме того, транспиляторы типа Babel удалят висячие запятые из собранного кода, поэтому вы можете не беспокоиться о [проблемах](https://github.com/airbnb/javascript/blob/es5-deprecated/es5/README.md#commas) в старых браузерах.

    ```diff
    // плохо - git diff без висячей запятой
    const hero = {
         firstName: 'Florence',
    -    lastName: 'Nightingale'
    +    lastName: 'Nightingale',
    +    inventorOf: ['coxcomb chart', 'modern nursing']
    };

    // хорошо - git diff с висячей запятой
    const hero = {
         firstName: 'Florence',
         lastName: 'Nightingale',
    +    inventorOf: ['coxcomb chart', 'modern nursing'],
    };
    ```

    ```javascript
    // плохо
    const hero = {
      firstName: 'Dana',
      lastName: 'Scully'
    };

    const heroes = [
      'Batman',
      'Superman'
    ];

    // хорошо
    const hero = {
      firstName: 'Dana',
      lastName: 'Scully',
    };

    const heroes = [
      'Batman',
      'Superman',
    ];

    // плохо
    function createHero(
      firstName,
      lastName,
      inventorOf
    ) {
      // ничего не делает
    }

    // хорошо
    function createHero(
      firstName,
      lastName,
      inventorOf,
    ) {
      // ничего не делает
    }

    // хорошо (обратите внимание, что висячей запятой не должно быть после rest-параметра)
    function createHero(
      firstName,
      lastName,
      inventorOf,
      ...heroArgs
    ) {
      // ничего не делает
    }

    // плохо
    createHero(
      firstName,
      lastName,
      inventorOf
    );

    // хорошо
    createHero(
      firstName,
      lastName,
      inventorOf,
    );

    // хорошо (обратите внимание, что висячей запятой не должно быть после rest-аргумента)
    createHero(
      firstName,
      lastName,
      inventorOf,
      ...heroArgs
    );
    ```

**[⬆ к оглавлению](#Оглавление)**

## <a name="semicolons">Точка с запятой</a>

  <a name="semicolons--required"></a><a name="20.1"></a>
  - [21.1](#semicolons--required) **Да.** eslint: [`semi`](https://eslint.org/docs/rules/semi.html)

    > Почему? Когда JavaScript встречает перенос строки без точки с запятой, он использует правило под названием [Автоматическая Вставка Точки с запятой (Automatic Semicolon Insertion)](https://tc39.github.io/ecma262/#sec-automatic-semicolon-insertion), чтобы определить, стоит ли считать этот перенос строки как конец выражения и (как следует из названия) поместить точку с запятой в вашем коде до переноса строки. Однако, ASI содержит несколько странных форм поведения, и ваш код может быть сломан, если JavaScript неверно истолкует ваш перенос строки. Эти правила станут сложнее, когда новые возможности станут частью JavaScript. Явное завершение ваших выражений и настройка вашего линтера для улавливания пропущенных точек с запятыми помогут вам предотвратить возникновение проблем.

    ```javascript
    // плохо - выбрасывает исключение
    const luke = {}
    const leia = {}
    [luke, leia].forEach((jedi) => jedi.father = 'vader')

    // плохо - выбрасывает исключение
    const reaction = "No! That’s impossible!"
    (async function meanwhileOnTheFalcon() {
      // переносимся к `leia`, `lando`, `chewie`, `r2`, `c3p0`
      // ...
    }())

    // плохо - возвращает `undefined` вместо значения на следующей строке. Так всегда происходит, когда `return` расположен сам по себе, потому что ASI (Автоматическая Вставка Точки с запятой)!
    function foo() {
      return
        'search your feelings, you know it to be foo'
    }

    // хорошо
    const luke = {};
    const leia = {};
    [luke, leia].forEach((jedi) => {
      jedi.father = 'vader';
    });

    // хорошо
    const reaction = "No! That’s impossible!";
    (async function meanwhileOnTheFalcon() {
      // переносимся к `leia`, `lando`, `chewie`, `r2`, `c3p0`
      // ...
      }());

    // хорошо
    function foo() {
      return 'search your feelings, you know it to be foo';
    }
    ```

    [Читать подробнее](https://stackoverflow.com/questions/7365172/semicolon-before-self-invoking-function/7365214#7365214).

**[⬆ к оглавлению](#Оглавление)**

## <a name="type-casting--coercion">Приведение типов</a>

  <a name="coercion--explicit"></a><a name="21.1"></a>
  - [22.1](#coercion--explicit) Выполняйте приведение типов в начале инструкции.

  <a name="coercion--strings"></a><a name="21.2"></a>
  - [22.2](#coercion--strings) Строки: eslint: [`no-new-wrappers`](https://eslint.org/docs/rules/no-new-wrappers)

    ```javascript
    // => this.reviewScore = 9;

    // плохо
    const totalScore = new String(this.reviewScore); // тип totalScore будет "object", а не "string"

    // плохо
    const totalScore = this.reviewScore + ''; // вызывает this.reviewScore.valueOf()

    // плохо
    const totalScore = this.reviewScore.toString(); // нет гарантии что вернётся строка

    // хорошо
    const totalScore = String(this.reviewScore);
    ```

  <a name="coercion--numbers"></a><a name="21.3"></a>
  - [22.3](#coercion--numbers) Числа: Используйте `Number` и `parseInt` с основанием системы счисления. eslint: [`radix`](https://eslint.org/docs/rules/radix) [`no-new-wrappers`](https://eslint.org/docs/rules/no-new-wrappers)

    ```javascript
    const inputValue = '4';

    // плохо
    const val = new Number(inputValue);

    // плохо
    const val = +inputValue;

    // плохо
    const val = inputValue >> 0;

    // плохо
    const val = parseInt(inputValue);

    // хорошо
    const val = Number(inputValue);

    // хорошо
    const val = parseInt(inputValue, 10);
    ```

  <a name="coercion--booleans"></a><a name="21.6"></a>
  - [22.6](#coercion--booleans) Логические типы: eslint: [`no-new-wrappers`](https://eslint.org/docs/rules/no-new-wrappers)

    ```javascript
    const age = 0;

    // плохо
    const hasAge = new Boolean(age);

    // хорошо
    const hasAge = Boolean(age);

    // отлично
    const hasAge = !!age;
    ```

**[⬆ к оглавлению](#Оглавление)**

## <a name="naming-conventions">Соглашение об именовании</a>

  <a name="naming--descriptive"></a><a name="22.1"></a>
  - [23.1](#naming--descriptive) Избегайте названий из одной буквы. Имя должно быть наглядным. eslint: [`id-length`](https://eslint.org/docs/rules/id-length)

    ```javascript
    // плохо
    function q() {
      // ...
    }

    // хорошо
    function query() {
      // ...
    }
    ```

  <a name="naming--camelCase"></a><a name="22.2"></a>
  - [23.2](#naming--camelCase) Используйте `camelCase` для именования объектов, функций и экземпляров. eslint: [`camelcase`](https://eslint.org/docs/rules/camelcase.html)

    ```javascript
    // плохо
    const OBJEcttsssss = {};
    const this_is_my_object = {};
    function c() {}

    // хорошо
    const thisIsMyObject = {};
    function thisIsMyFunction() {}
    ```

  <a name="naming--PascalCase"></a><a name="22.3"></a>
  - [23.3](#naming--PascalCase) Используйте `PascalCase` только для именования конструкторов и классов. eslint: [`new-cap`](https://eslint.org/docs/rules/new-cap.html)

    ```javascript
    // плохо
    function user(options) {
      this.name = options.name;
    }

    const bad = new user({
      name: 'nope',
    });

    // хорошо
    class User {
      constructor(options) {
        this.name = options.name;
      }
    }

    const good = new User({
      name: 'yup',
    });
    ```

  <a name="naming--leading-underscore"></a><a name="22.4"></a>
  - [23.4](#naming--leading-underscore) Не используйте `_` в начале или в конце названий. eslint: [`no-underscore-dangle`](https://eslint.org/docs/rules/no-underscore-dangle.html)

    > Почему? JavaScript не имеет концепции приватности свойств или методов. Хотя подчёркивание в начале имени является распространённым соглашением, которое показывает «приватность», фактически эти свойства являются такими же доступными, как и часть вашего публичного API. Это соглашение может привести к тому, что разработчики будут ошибочно думать, что изменения не приведут к поломке или что тесты не нужны. Итог: если вы хотите, чтобы что-то было «приватным», то оно не должно быть доступно извне.

    ```javascript
    // плохо
    this.__firstName__ = 'Panda';
    this.firstName_ = 'Panda';
    this._firstName = 'Panda';

    // хорошо
    this.firstName = 'Panda';

    // хорошо, в средах, где поддерживается WeakMaps
    // смотрите https://kangax.github.io/compat-table/es6/#test-WeakMap
    const firstNames = new WeakMap();
    firstNames.set(this, 'Panda');
    ```

  <a name="naming--self-this"></a><a name="22.5"></a>
  - [23.5](#naming--self-this) Не сохраняйте ссылку на `this`. Используйте стрелочные функции или [метод bind()](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Function/bind).

    ```javascript
    // плохо
    function foo() {
      const self = this;
      return function () {
        console.log(self);
      };
    }

    // плохо
    function foo() {
      const that = this;
      return function () {
        console.log(that);
      };
    }

    // хорошо
    function foo() {
      return () => {
        console.log(this);
      };
    }
    ```
  <a name="naming--PascalCase-singleton"></a><a name="22.8"></a>
  - [23.8](#naming--PascalCase-singleton) Используйте `PascalCase`, когда экспортируете конструктор / класс / синглтон / библиотечную функцию / объект.

    ```javascript
    const AirbnbStyleGuide = {
      es6: {
      },
    };

    export default AirbnbStyleGuide;
    ```

  <a name="naming--Acronyms-and-Initialisms"></a>
  - [23.9](#naming--Acronyms-and-Initialisms) Сокращения или буквенные аббревиатуры всегда должны быть в верхнем или нижнем регистре.

    > Почему? Имена предназначены для удобства чтения.

    ```javascript
    // плохо
    import SmsContainer from './containers/SmsContainer';

    // плохо
    const HttpRequests = [
      // ...
    ];

    // хорошо
    import SMSContainer from './containers/SMSContainer';

    // хорошо
    const HTTPRequests = [
      // ...
    ];

    // также хорошо
    const httpRequests = [
      // ...
    ];

    // отлично
    import TextMessageContainer from './containers/TextMessageContainer';

    // отлично
    const requests = [
      // ...
    ];
    ```
**[⬆ к оглавлению](#Оглавление)**

## <a name="testing">Тестирование</a>

  <a name="testing--yup"></a><a name="28.1"></a>
  - [30.1](#testing--yup) **Ага.**

    ```javascript
    function foo() {
      return true;
    }
    ```

  <a name="testing--for-real"></a><a name="28.2"></a>
  - [30.2](#testing--for-real) **Нет, но серьёзно**:
    - Вы должны писать тесты!
    - Стремитесь к тому, чтобы написать много маленьких чистых функций, и к тому, чтобы свести к минимуму места, где происходят мутации.
    - Будьте осторожны со стабами (stubs) и моками (mocks) — они могут сделать ваше тестирование хрупким.
    - Мы в первую очередь советуем вам использовать [`mocha`](https://www.npmjs.com/package/mocha) и [`jest`](https://www.npmjs.com/package/jest). [`tape`](https://www.npmjs.com/package/tape) также иногда используется для небольших, отдельных модулей.
    - 100% покрытие тестами — это хорошая цель, к которой надо стремиться, даже если это не всегда практично.
    - Всякий раз, когда вы исправляете ошибку, _пишите регрессионный тест_. Исправленная ошибка без регрессионного тестирования почти наверняка всплывёт в будущем.

**[⬆ к оглавлению](#Оглавление)**
