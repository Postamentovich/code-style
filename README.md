# Руководство по написанию кода

## Оглавление

1. [Объекты](#objects)
1. [Массивы](#arrays)
1. [Деструктуризация](#destructuring)
1. [Строки](#strings)
1. [Функции](#functions)
1. [Стрелочные функции](#arrow-functions)
1. [Классы и конструкторы](#classes--constructors)
1. [Модули](#modules)
1. [Свойства](#properties)
1. [Переменные](#variables)
1. [Операторы сравнения и равенства](#comparison-operators--equality)
1. [Блоки](#blocks)
1. [Управляющие операторы](#control-statements)
1. [Комментарии](#comments)
1. [Пробелы](#whitespace)
1. [Запятые](#commas)
1. [Точка с запятой](#semicolons)
1. [Приведение типов](#type-casting--coercion)
1. [Соглашение об именовании](#naming-conventions)
1. [React](#react)
1. [Typescript](#typescript)
1. [Тестирование](#test)
1. [Ассинхронность](#promise)

## <a name="objects">Объекты</a>

<a name="objects--no-new"></a>

-   [1.1](#objects--no-new) Для создания объекта используйте литеральную нотацию. eslint: [`no-new-object`](https://eslint.org/docs/rules/no-new-object.html)

    ```javascript
    // плохо
    const item = new Object();

    // хорошо
    const item = {};
    ```

<a name="objects--computed-properties"></a>

-   [1.2](#objects--computed-properties) Используйте вычисляемые имена свойств, когда создаёте объекты с динамическими именами свойств.

    > Почему? Они позволяют вам определить все свойства объекта в одном месте.

    ```javascript
    function getKey(k) {
        return `a key named ${k}`;
    }

    // плохо
    const obj = {
        id: 5,
        name: "San Francisco",
    };
    obj[getKey("enabled")] = true;

    // хорошо
    const obj = {
        id: 5,
        name: "San Francisco",
        [getKey("enabled")]: true,
    };
    ```

<a name="objects--shorthand"></a><a name="1.3"></a>

-   [1.3](#objects--shorthand) Используйте сокращённую запись метода объекта. eslint: [`object-shorthand`](https://eslint.org/docs/rules/object-shorthand.html)

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

<a name="objects--concise"></a>

-   [1.4](#objects--concise) Используйте сокращённую запись свойств объекта. eslint: [`object-shorthand`](https://eslint.org/docs/rules/object-shorthand.html)

    > Почему? Это короче и понятнее.

    ```javascript
    const lukeSkywalker = "Luke Skywalker";

    // плохо
    const obj = {
        lukeSkywalker: lukeSkywalker,
    };

    // хорошо
    const obj = {
        lukeSkywalker,
    };
    ```

<a name="objects--grouped-shorthand"></a>

-   [1.5](#objects--grouped-shorthand) Группируйте ваши сокращённые записи свойств в начале объявления объекта.

    > Почему? Так легче сказать, какие свойства используют сокращённую запись.

    ```javascript
    const anakinSkywalker = "Anakin Skywalker";
    const lukeSkywalker = "Luke Skywalker";

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

<a name="objects--rest-spread"></a>

-   [1.6](#objects--rest-spread) Используйте оператор расширения вместо [`Object.assign`](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Object/assign) для поверхностного копирования объектов. Используйте синтаксис оставшихся свойств, чтобы получить новый объект с некоторыми опущенными свойствами.


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

## <a name="arrays">Массивы</a>

<a name="arrays--literals"></a>

-   [2.1](#arrays--literals) Для создания массива используйте литеральную нотацию. eslint: [`no-array-constructor`](https://eslint.org/docs/rules/no-array-constructor.html)

    ```javascript
    // плохо
    const items = new Array();

    // хорошо
    const items = [];
    ```

<a name="arrays--push"></a>

-   [2.2](#arrays--push) Для добавления элемента в массив используйте [Array#push](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Array/push) вместо прямого присваивания.

    ```javascript
    const someStack = [];

    // плохо
    someStack[someStack.length] = "abracadabra";

    // хорошо
    someStack.push("abracadabra");
    ```

<a name="arrays--spreads"></a>

-   [2.3](#arrays--spreads) Для копирования массивов используйте оператор расширения `...`.

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

<a name="arrays--from-iterable"></a>

-   [2.4](#arrays--from-iterable) Для преобразования итерируемого объекта в массив используйте оператор расширения `...` вместо [`Array.from`](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Array/from).

    ```javascript
    const foo = document.querySelectorAll(".foo");

    // хорошо
    const nodes = Array.from(foo);

    // отлично
    const nodes = [...foo];
    ```

<a name="arrays--from-array-like"></a>

-   [2.5](#arrays--from-array-like) Используйте [`Array.from`](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Array/from) для преобразования массивоподобного объекта в массив.

    ```javascript
    const arrLike = { 0: "foo", 1: "bar", 2: "baz", length: 3 };

    // плохо
    const arr = Array.prototype.slice.call(arrLike);

    // хорошо
    const arr = Array.from(arrLike);
    ```

<a name="arrays--mapping"></a>

-   [2.6](#arrays--mapping) Используйте [`Array.from`](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Array/from) вместо оператора расширения `...` для маппинга итерируемых объектов, это позволяет избежать создания промежуточного массива.

    ```javascript
    // плохо
    const baz = [...foo].map(bar);

    // хорошо
    const baz = Array.from(foo, bar);
    ```

<a name="arrays--callback-return"></a>

-   [2.7](#arrays--callback-return) Используйте операторы `return` внутри функций обратного вызова в методах массива. Можно опустить `return`, когда тело функции состоит из одной инструкции, возвращающей выражение без побочных эффектов. [8.2](#arrows--implicit-return). eslint: [`array-callback-return`](https://eslint.org/docs/rules/array-callback-return)

    ```javascript
    // хорошо
    [1, 2, 3].map((x) => {
        const y = x + 1;
        return x * y;
    });

    // хорошо
    [1, 2, 3].map((x) => x + 1);

    // плохо - нет возвращаемого значения, следовательно, `acc` становится `undefined` после первой итерации
    [
        [0, 1],
        [2, 3],
        [4, 5],
    ].reduce((acc, item, index) => {
        const flatten = acc.concat(item);
    });

    // хорошо
    [
        [0, 1],
        [2, 3],
        [4, 5],
    ].reduce((acc, item, index) => {
        const flatten = acc.concat(item);
        return flatten;
    });

    // плохо
    inbox.filter((msg) => {
        const { subject, author } = msg;
        if (subject === "Mockingbird") {
            return author === "Harper Lee";
        } else {
            return false;
        }
    });

    // хорошо
    inbox.filter((msg) => {
        const { subject, author } = msg;
        if (subject === "Mockingbird") {
            return author === "Harper Lee";
        }

        return false;
    });
    ```

## <a name="destructuring">Деструктуризация</a>

<a name="destructuring--object"></a>

-   [3.1](#destructuring--object) При обращении к нескольким свойствам объекта используйте деструктуризацию объекта. eslint: [`prefer-destructuring`](https://eslint.org/docs/rules/prefer-destructuring)

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

<a name="destructuring--array">

-   [3.2](#destructuring--array) Используйте деструктуризацию массивов. eslint: [`prefer-destructuring`](https://eslint.org/docs/rules/prefer-destructuring)

    ```javascript
    const arr = [1, 2, 3, 4];

    // плохо
    const first = arr[0];
    const second = arr[1];

    // хорошо
    const [first, second] = arr;
    ```

<a name="destructuring--object-over-array"></a>

-   [3.3](#destructuring--object-over-array) Используйте деструктуризацию объекта для множества возвращаемых значений, но не делайте тоже самое с массивами.

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

## <a name="strings">Строки</a>

<a name="strings--line-length"></a>

-   [4.1](#strings--line-length) Строки, у которых в строчке содержится более 120 символов, не пишутся на нескольких строчках с использованием конкатенации.

    > Почему? Работать с разбитыми строками неудобно и это затрудняет поиск по коду.

    ```javascript
    // плохо
    const errorMessage =
        "This is a super long error that was thrown because \
    of Batman. When you stop to think about how Batman had anything to do \
    with this, you would get nowhere \
    fast.";

    // плохо
    const errorMessage =
        "This is a super long error that was thrown because " +
        "of Batman. When you stop to think about how Batman had anything to do " +
        "with this, you would get nowhere fast.";

    // хорошо
    const errorMessage =
        "This is a super long error that was thrown because of Batman. When you stop to think about how Batman had anything to do with this, you would get nowhere fast.";
    ```

<a name="strings--template-literals"></a>

-   [4.2](#strings--template-literals) При создании строки программным путём используйте шаблонные строки вместо конкатенации. eslint: [`prefer-template`](https://eslint.org/docs/rules/prefer-template.html) [`template-curly-spacing`](https://eslint.org/docs/rules/template-curly-spacing)

    > Почему? Шаблонные строки дают вам читабельность, лаконичный синтаксис с правильными символами перевода строк и функции интерполяции строки.

    ```javascript
    // плохо
    function sayHi(name) {
        return "How are you, " + name + "?";
    }

    // плохо
    function sayHi(name) {
        return ["How are you, ", name, "?"].join();
    }

    // плохо
    function sayHi(name) {
        return `How are you, ${name}?`;
    }

    // хорошо
    function sayHi(name) {
        return `How are you, ${name}?`;
    }
    ```

<a name="strings--escaping"></a>

-   [4.3](#strings--escaping) Не используйте в строках необязательные экранирующие символы. eslint: [`no-useless-escape`](https://eslint.org/docs/rules/no-useless-escape)

    > Почему? Обратные слеши ухудшают читабельность, поэтому они должны быть только при необходимости.

    ```javascript
    // плохо
    const foo = "'this' is \"quoted\"";

    // хорошо
    const foo = "'this' is \"quoted\"";
    const foo = `my name is '${name}'`;
    ```

## <a name="functions">Функции</a>

<a name="functions--single-responsibility"></a>

-   [5.1](#functions--single-responsibility) Функция должна решать одну задачу. Стремитесь к тому, чтобы одна функция не выполняла бы множество действий.

    ```javascript
    // плохо
    function notifyUsers(users) {
        users.forEach((user) => {
            const userRecord = database.lookup(user);
            if (userRecord.isVerified()) {
                notify(user);
            }
        });
    }

    // хорошо
    function notifyVerifiedUsers(users) {
        users.filter(isUserVerified).forEach(notify);
    }

    function isUserVerified(user) {
        const userRecord = database.lookup(user);
        return userRecord.isVerified();
    }
    ```

<a name="functions--arguments-count"></a>

-   [5.2](#functions--arguments-count) Избегайте использования длинных списков аргументов. В идеале функции следует иметь два или меньшее количество аргументов.

    > Чем меньше у функции аргументов — тем легче будет её тестировать.

    ```javascript
    // плохо
    function getUsers(fields, fromDate, toDate) {
        // реализация
    }

    // хорошо
    function getUsers({ fields, fromDate, toDate }) {
        // реализация
    }
    ```

<a name="functions--no-flags-in-arguments"></a>

-   [5.3](#functions--no-flags-in-arguments) Не используйте флаги в качестве параметров.

    > Их использование означает, что функция выполняет больше действий, чем ей следовало бы выполнять.

    ```javascript
    // плохо
    function createFile(name, isPublic) {
        if (isPublic) {
            fs.create(`./public/${name}`);
        } else {
            fs.create(name);
        }
    }

    // хорошо
    function createFile(name) {
        fs.create(name);
    }

    function createPublicFile(name) {
        createFile(`./public/${name}`);
    }
    ```

<a name="functions--rest"></a>

-   [5.4](#functions--rest) Никогда не используйте `arguments`, вместо этого используйте синтаксис оставшихся параметров `...`. eslint: [`prefer-rest-params`](https://eslint.org/docs/rules/prefer-rest-params)

    > Почему? `...` явно говорит о том, какие именно аргументы вы хотите извлечь. Кроме того, такой синтаксис создаёт настоящий массив, а не массивоподобный объект как `arguments`.

    ```javascript
    // плохо
    function concatenateAll() {
        const args = Array.prototype.slice.call(arguments);
        return args.join("");
    }

    // хорошо
    function concatenateAll(...args) {
        return args.join("");
    }
    ```

<a name="functions--default-parameters"></a><a name="7.7"></a>

-   [5.5](#functions--default-parameters) Используйте синтаксис записи аргументов по умолчанию, а не изменяйте аргументы функции.

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

-   [5.6](#functions--default-side-effects) Избегайте побочных эффектов с параметрами по умолчанию.

    > Почему? И так всё понятно.

    ```javascript
    let b = 1;
    // плохо
    function count(a = b++) {
        console.log(a);
    }
    count(); // 1
    count(); // 2
    count(3); // 3
    count(); // 3
    ```

<a name="functions--defaults-last"></a><a name="7.9"></a>

-   [5.7](#functions--defaults-last) Всегда вставляйте последними параметры по умолчанию.

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

<a name="functions--reassign-params"></a>

-   [5.8](#functions--reassign-params) Никогда не переназначайте параметры. eslint: [`no-param-reassign`](https://eslint.org/docs/rules/no-param-reassign.html)

    > Почему? Переназначенные параметры могут привести к неожиданному поведению, особенно при обращении к `arguments`. Это также может вызывать проблемы оптимизации, особенно в V8.

    ```javascript
    // плохо
    function f1(a) {
        a = 1;
        // ...
    }

    function f2(a) {
        if (!a) {
            a = 1;
        }
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

-   [5.9](#functions--spread-vs-apply) Отдавайте предпочтение использованию оператора расширения `...` при вызове вариативной функции. eslint: [`prefer-spread`](https://eslint.org/docs/rules/prefer-spread)

    > Почему? Это чище, вам не нужно предоставлять контекст, и не так просто составить `new` с `apply`.

    ```javascript
    // плохо
    const x = [1, 2, 3, 4, 5];
    console.log.apply(console, x);

    // хорошо
    const x = [1, 2, 3, 4, 5];
    console.log(...x);

    // плохо
    new (Function.prototype.bind.apply(Date, [null, 2016, 8, 5]))();

    // хорошо
    new Date(...[2016, 8, 5]);
    ```

## <a name="arrow-functions">Стрелочные функции</a>

<a name="arrows--use-them"></a>

-   [6.1](#arrows--use-them) Когда вам необходимо использовать анонимную функцию (например, при передаче встроенной функции обратного вызова), используйте стрелочную функцию. eslint: [`prefer-arrow-callback`](https://eslint.org/docs/rules/prefer-arrow-callback.html), [`arrow-spacing`](https://eslint.org/docs/rules/arrow-spacing.html)

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

<a name="arrows--implicit-return"></a>

-   [6.2](#arrows--implicit-return) Если тело функции состоит из одного оператора, возвращающего [выражение](https://developer.mozilla.org/ru/docs/Web/JavaScript/Guide/Expressions_and_Operators#Выражения) без побочных эффектов, то используйте неявное возвращение. В противном случае, используйте оператор `return`. eslint: [`arrow-parens`](https://eslint.org/docs/rules/arrow-parens.html), [`arrow-body-style`](https://eslint.org/docs/rules/arrow-body-style.html)

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
    ```

<a name="arrows--one-arg-parens"></a>

-   [6.4](#arrows--one-arg-parens) Всегда оборачивайте аргументы круглыми скобками для ясности и согласованности. eslint: [`arrow-parens`](https://eslint.org/docs/rules/arrow-parens.html)

    > Почему? Минимизирует различия при удалении или добавлении аргументов.

    ```javascript
    // плохо
    [1, 2, 3].map((x) => x * x);

    // хорошо
    [1, 2, 3].map((x) => x * x);
    ```

<a name="arrows--confusing"></a>

-   [6.5](#arrows--confusing) Избегайте схожести стрелочной функции (`=>`) с операторами сравнения (`<=`, `>=`). eslint: [`no-confusing-arrow`](https://eslint.org/docs/rules/no-confusing-arrow)

    ```javascript
    // плохо
    const itemHeight = (item) => (item.height <= 256 ? item.largeSize : item.smallSize);

    // плохо
    const itemHeight = (item) => (item.height >= 256 ? item.largeSize : item.smallSize);

    // хорошо
    const itemHeight = (item) => (item.height <= 256 ? item.largeSize : item.smallSize);

    // хорошо
    const itemHeight = (item) => {
        const { height, largeSize, smallSize } = item;
        return height <= 256 ? largeSize : smallSize;
    };
    ```

## <a name="classes--constructors">Классы и конструкторы</a>

<a name="constructors--chaining"></a>

-   [7.1](#constructors--chaining) Методы могут возвращать `this`, чтобы делать цепочки вызовов.

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

<a name="constructors--no-useless"></a>

-   [7.2](#constructors--no-useless) У классов есть конструктор по умолчанию, если он не задан явно. Можно опустить пустой конструктор или конструктор, который только делегирует выполнение родительскому классу. eslint: [`no-useless-constructor`](https://eslint.org/docs/rules/no-useless-constructor)

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
            this.name = "Rey";
        }
    }
    ```

<a name="classes--no-duplicate-members"></a>

-   [7.3](#classes--no-duplicate-members) Избегайте дублирующих членов класса. eslint: [`no-dupe-class-members`](https://eslint.org/docs/rules/no-dupe-class-members)

    > Почему? Если объявление члена класса повторяется, без предупреждения будет использовано последнее. Наличие дубликатов скорее всего приведёт к ошибке.

    ```javascript
    // плохо
    class Foo {
        bar() {
            return 1;
        }
        bar() {
            return 2;
        }
    }

    // хорошо
    class Foo {
        bar() {
            return 1;
        }
    }

    // хорошо
    class Foo {
        bar() {
            return 2;
        }
    }
    ```

<a name="classes--methods-use-this"></a>

-   [7.4](#classes--methods-use-this) Метод класса должен использовать `this` или быть преобразованным в статический метод, если только внешняя библиотека или фреймворк не требуют использования определённых нестатических методов. Будучи методом экземпляра, следует указать, что он ведёт себя по-разному в зависимости от свойств получателя. eslint: [`class-methods-use-this`](https://eslint.org/docs/rules/class-methods-use-this)

    ```javascript
    // плохо
    class Foo {
        bar() {
            console.log("bar");
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
            console.log("bar");
        }
    }
    ```

## <a name="modules">Модули</a>

<a name="modules--no-duplicate-imports"></a>

-   [8.1](#modules--no-duplicate-imports) Импортируйте из пути только один раз. eslint: [`no-duplicate-imports`](https://eslint.org/docs/rules/no-duplicate-imports)

    > Почему? Наличие нескольких строк, которые импортируют из одного и того же файла, может сделать код неподдерживаемым.

    ```javascript
    // плохо
    import foo from "foo";
    // … какие-то другие импорты … //
    import { named1, named2 } from "foo";

    // хорошо
    import foo, { named1, named2 } from "foo";

    // хорошо
    import foo, { named1, named2 } from "foo";
    ```

<a name="modules--no-mutable-exports"></a>

-   [8.2](#modules--no-mutable-exports) Не экспортируйте изменяемые переменные. eslint: [`import/no-mutable-exports`](https://github.com/benmosher/eslint-plugin-import/blob/master/docs/rules/no-mutable-exports.md)

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

-   [8.3](#modules--imports-first) Поместите все импорты выше остальных инструкций. eslint: [`import/first`](https://github.com/benmosher/eslint-plugin-import/blob/master/docs/rules/first.md)

    > Почему? Так как `import` обладает подъёмом, то хранение их всех в начале файла предотвращает от неожиданного поведения.

    ```javascript
    // плохо
    import foo from "foo";
    foo.init();

    import bar from "bar";

    // хорошо
    import foo from "foo";
    import bar from "bar";

    foo.init();
    ```

<a name="modules--no-webpack-loader-syntax"></a>

-   [8.4](#modules--no-webpack-loader-syntax) Запретите синтаксис загрузчика Webpack в импорте. eslint: [`import/no-webpack-loader-syntax`](https://github.com/benmosher/eslint-plugin-import/blob/master/docs/rules/no-webpack-loader-syntax.md)

    > Почему? Использование Webpack синтаксиса связывает код с упаковщиком модулей. Предпочтительно использовать синтаксис загрузчика в `webpack.config.js`.

    ```javascript
    // плохо
    import fooSass from "css!sass!foo.scss";
    import barCss from "style!css!bar.css";

    // хорошо
    import fooSass from "foo.scss";
    import barCss from "bar.css";
    ```

## <a name="properties">Свойства</a>

<a name="properties--dot"></a>

-   [9.1](#properties--dot) Используйте точечную нотацию для доступа к свойствам. eslint: [`dot-notation`](https://eslint.org/docs/rules/dot-notation.html)

    ```javascript
    const luke = {
        jedi: true,
        age: 28,
    };

    // плохо
    const isJedi = luke["jedi"];

    // хорошо
    const isJedi = luke.jedi;
    ```

<a name="properties--bracket"></a>

-   [9.2](#properties--bracket) Используйте скобочную нотацию `[]`, когда название свойства хранится в переменной.

    ```javascript
    const luke = {
        jedi: true,
        age: 28,
    };

    function getProp(prop) {
        return luke[prop];
    }

    const isJedi = getProp("jedi");
    ```

## <a name="variables">Переменные</a>

<a name="references--prefer-const"></a>

-   [10.1](#references--prefer-const) Используйте `const` для объявления переменных; избегайте `var`. eslint: [`prefer-const`](https://eslint.org/docs/rules/prefer-const.html), [`no-const-assign`](https://eslint.org/docs/rules/no-const-assign.html)

    > Почему? Это гарантирует, что вы не сможете переопределять значения, т.к. это может привести к ошибкам и к усложнению понимания кода.

    ```javascript
    // плохо
    var a = 1;
    var b = 2;

    // хорошо
    const a = 1;
    const b = 2;
    ```

<a name="references--disallow-var"></a>

-   [10.2](#references--disallow-var) Если вам необходимо переопределять значения, то используйте `let` вместо `var`. eslint: [`no-var`](https://eslint.org/docs/rules/no-var.html)

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

<a name="references--block-scope"></a>

-   [10.3](#references--block-scope) Помните, что у `let` и `const` блочная область видимости.

    ```javascript
    // const и let существуют только в том блоке, в котором они определены.
    {
        let a = 1;
        const b = 1;
    }
    console.log(a); // ReferenceError
    console.log(b); // ReferenceError
    ```

<a name="variables--const-let-group"></a><a name="13.3"></a>

-   [10.4](#variables--const-let-group) В первую очередь группируйте `const`, а затем `let`.

    > Почему? Это полезно, когда в будущем вам понадобится создать переменную, зависимую от предыдущих.

    ```javascript
    // плохо
    let i,
        len,
        dragonball,
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

<a name="variables--define-where-used"></a>

-   [10.5](#variables--define-where-used) Создавайте переменные там, где они вам необходимы, но помещайте их в подходящее место.

    > Почему? `let` и `const` имеют блочную область видимости, а не функциональную.

    ```javascript
    // плохо - вызов ненужной функции
    function checkName(hasName) {
        const name = getName();

        if (hasName === "test") {
            return false;
        }

        if (name === "test") {
            this.setName("");
            return false;
        }

        return name;
    }

    // хорошо
    function checkName(hasName) {
        if (hasName === "test") {
            return false;
        }

        const name = getName();

        if (name === "test") {
            this.setName("");
            return false;
        }

        return name;
    }
    ```

<a name="variables--no-unused-vars"></a>

-   [10.6](#variables--no-unused-vars) Запрещены неиспользуемые переменные. eslint: [`no-unused-vars`](https://eslint.org/docs/rules/no-unused-vars)

    > Почему? Переменные, которые объявлены и не используются в коде, скорее всего, являются ошибкой из-за незавершённого рефакторинга. Такие переменные занимают место в коде и могут привести к путанице при чтении.

## <a name="comparison-operators--equality">Операторы сравнения и равенства</a>

<a name="comparison--eqeqeq"></a>

-   [11.1](#comparison--eqeqeq) Используйте `===` и `!==` вместо `==` и `!=`. eslint: [`eqeqeq`](https://eslint.org/docs/rules/eqeqeq.html)

<a name="comparison--shortcuts"></a>

-   [11.2](#comparison--shortcuts) Используйте сокращения для булевских типов, а для строк и чисел применяйте явное сравнение.

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
    if (name !== "") {
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

<a name="comparison--switch-blocks"></a>

-   [11.3](#comparison--switch-blocks) Используйте фигурные скобки для `case` и `default`, если они содержат лексические декларации (например, `let`, `const`, `function`, и `class`). eslint: [`no-case-declarations`](https://eslint.org/docs/rules/no-case-declarations.html).

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

<a name="comparison--nested-ternaries"></a>

-   [11.4](#comparison--nested-ternaries) Тернарные операторы не должны быть вложены и в большинстве случаев должны быть расположены на одной строке. eslint: [`no-nested-ternary`](https://eslint.org/docs/rules/no-nested-ternary.html).

    ```javascript
    // плохо
    const foo = maybe1 > maybe2 ? "bar" : value1 > value2 ? "baz" : null;

    // отлично
    // разбит на два отдельных тернарных выражения
    const maybeNull = value1 > value2 ? "baz" : null;

    const foo = maybe1 > maybe2 ? "bar" : maybeNull;
    ```

<a name="comparison--unneeded-ternary"></a>

-   [11.5](#comparison--unneeded-ternary) Избегайте ненужных тернарных операторов. eslint: [`no-unneeded-ternary`](https://eslint.org/docs/rules/no-unneeded-ternary.html).

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

## <a name="blocks">Блоки</a>

<a name="blocks--braces"></a>

-   [12.1](#blocks--braces) Используйте фигурные скобки, когда блок кода занимает несколько строк. eslint: [`nonblock-statement-body-position`](https://eslint.org/docs/rules/nonblock-statement-body-position)

    ```javascript
    // prettier-ignore
    // плохо
    if (test) 
      return false;

    // хорошо
    if (test) return false;

    // хорошо
    if (test) {
        return false;
    }

    // prettier-ignore
    // плохо
    function foo() { return false; }

    // хорошо
    function bar() {
        return false;
    }
    ```

<a name="blocks--no-else-return"></a><a name="16.3"></a>

-   [12.2](#blocks--no-else-return) Если в блоке `if` всегда выполняется оператор `return`, последующий блок `else` не нужен. `return` внутри блока `else if`, следующем за блоком `if`, который содержит `return`, может быть разделён на несколько блоков `if`. eslint: [`no-else-return`](https://eslint.org/docs/rules/no-else-return)

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
    ```

## <a name="control-statements">Управляющие операторы</a>

<a name="control-naming"></a>

-   [13.1](#control-naming) Старайтесь не называть логические переменные так, чтобы в их именах присутствовало бы отрицание. То же самое касается и функций, возвращающих логические значения. Использование таких сущностей в условных конструкциях затрудняет чтение кода.

        ```javascript
        // плохо
        function isUserNotBlocked(user) {
            // реализация
        }

        if (!isUserNotBlocked(user)) {
            // реализация
        }

        // хорошо
        function isUserBlocked(user) {
            // реализация
        }

        if (isUserBlocked(user)) {
            // реализация
        }
        ```

<a name="control-return"></a>

-   [13.2](#control-return) Если продолжение выполнения функции зависит от значения, рекомендуется при невыполнении условия прекращать выполнение функции.

    > Почему? Уменьшает вложенность кода, легче читается.

    ```javascript
    // плохо
    function doSomething() {
        if (true) {
            // реализация
        }
    }

    // хорошо
    function doSomething() {
        if (false) return;
        // реализация
    }
    ```

## <a name="comments">Комментарии</a>

<a name="comments--multiline"></a>

-   [14.1](#comments--multiline) Используйте конструкцию `/** ... */` для многострочных комментариев.

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

<a name="comments--singleline"></a>

-   [14.2](#comments--singleline) Используйте двойной слеш `//` для однострочных комментариев. Располагайте такие комментарии отдельной строкой над кодом, который хотите пояснить. Если комментарий не является первой строкой блока, добавьте сверху пустую строку.

    ```javascript
    // плохо
    const active = true; // это текущая вкладка

    // хорошо
    // это текущая вкладка
    const active = true;

    // плохо
    function getType() {
        console.log("fetching type...");
        // установить по умолчанию тип 'no type'
        const type = this.type || "no type";

        return type;
    }

    // хорошо
    function getType() {
        console.log("fetching type...");

        // установить по умолчанию тип 'no type'
        const type = this.type || "no type";

        return type;
    }

    // тоже хорошо
    function getType() {
        // установить по умолчанию тип 'no type'
        const type = this.type || "no type";

        return type;
    }
    ```

<a name="comments--spaces"></a>

-   [14.3](#comments--spaces) Начинайте все комментарии с пробела, так их проще читать. eslint: [`spaced-comment`](https://eslint.org/docs/rules/spaced-comment)

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

-   [14.4](#comments--actionitems) Если комментарий начинается со слов `FIXME` или `TODO`, то это помогает другим разработчикам быстро понять, когда вы хотите указать на проблему, которую надо решить, или когда вы предлагаете решение проблемы, которое надо реализовать. Такие комментарии, в отличие от обычных, побуждают к действию: `FIXME: -- нужно разобраться с этим` или `TODO: -- нужно реализовать`.

## <a name="whitespace">Пробелы</a>

<a name="whitespace--after-blocks"></a>

-   [15.1](#whitespace--after-blocks) Оставляйте пустую строку между блоком кода и следующей инструкцией.

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
        foo() {},
        bar() {},
    };
    return obj;

    // хорошо
    const obj = {
        foo() {},

        bar() {},
    };

    return obj;
    ```

<a name="whitespace--max-len"></a>

-   [15.2](#whitespace--max-len) Старайтесь не допускать, чтобы строки были длиннее 120 символов (включая пробелы). Замечание: согласно [пункту выше](#strings--line-length), длинные строки с текстом освобождаются от этого правила и не должны разбиваться на несколько строк. eslint: [`max-len`](https://eslint.org/docs/rules/max-len.html)

    > Почему? Это обеспечивает удобство чтения и поддержки кода.

## <a name="commas">Запятые</a>

<a name="commas--leading-trailing"></a>

-   [16.1](#commas--leading-trailing) Не начинайте строку с запятой. eslint: [`comma-style`](https://eslint.org/docs/rules/comma-style.html)

    ```javascript
    // плохо
    const story = [once, upon, aTime];

    // хорошо
    const story = [once, upon, aTime];

    // плохо
    const hero = {
        firstName: "Ada",
        lastName: "Lovelace",
        birthYear: 1815,
        superPower: "computers",
    };

    // хорошо
    const hero = {
        firstName: "Ada",
        lastName: "Lovelace",
        birthYear: 1815,
        superPower: "computers",
    };
    ```

<a name="commas--dangling"></a>

-   [16.2](#commas--dangling) Добавляйте висячие запятые. eslint: [`comma-dangle`](https://eslint.org/docs/rules/comma-dangle.html)

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

## <a name="semicolons">Точка с запятой</a>

<a name="semicolons--required"></a>

-   [17.1](#semicolons--required) **Да.** eslint: [`semi`](https://eslint.org/docs/rules/semi.html)

    > Почему? Когда JavaScript встречает перенос строки без точки с запятой, он использует правило под названием [Автоматическая Вставка Точки с запятой (Automatic Semicolon Insertion)](https://tc39.github.io/ecma262/#sec-automatic-semicolon-insertion), чтобы определить, стоит ли считать этот перенос строки как конец выражения и (как следует из названия) поместить точку с запятой в вашем коде до переноса строки. Однако, ASI содержит несколько странных форм поведения, и ваш код может быть сломан, если JavaScript неверно истолкует ваш перенос строки. Эти правила станут сложнее, когда новые возможности станут частью JavaScript. Явное завершение ваших выражений и настройка вашего линтера для улавливания пропущенных точек с запятыми помогут вам предотвратить возникновение проблем.

## <a name="type-casting--coercion">Приведение типов</a>

<a name="coercion--explicit"></a>

-   [18.1](#coercion--explicit) Выполняйте приведение типов в начале инструкции.

<a name="coercion--strings"></a>

-   [18.2](#coercion--strings) Строки: eslint: [`no-new-wrappers`](https://eslint.org/docs/rules/no-new-wrappers)

    ```javascript
    // => this.reviewScore = 9;

    // плохо
    const totalScore = new String(this.reviewScore); // тип totalScore будет "object", а не "string"

    // плохо
    const totalScore = this.reviewScore + ""; // вызывает this.reviewScore.valueOf()

    // плохо
    const totalScore = this.reviewScore.toString(); // нет гарантии что вернётся строка

    // хорошо
    const totalScore = String(this.reviewScore);
    ```

<a name="coercion--numbers"></a>

-   [18.3](#coercion--numbers) Числа: Используйте `Number` и `parseInt` с основанием системы счисления. eslint: [`radix`](https://eslint.org/docs/rules/radix) [`no-new-wrappers`](https://eslint.org/docs/rules/no-new-wrappers)

    ```javascript
    const inputValue = "4";

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

<a name="coercion--booleans"></a>

-   [18.4](#coercion--booleans) Логические типы: eslint: [`no-new-wrappers`](https://eslint.org/docs/rules/no-new-wrappers)

    ```javascript
    const age = 0;

    // плохо
    const hasAge = new Boolean(age);

    // хорошо
    const hasAge = Boolean(age);

    // отлично
    const hasAge = !!age;
    ```

## <a name="naming-conventions">Соглашение об именовании</a>

<a name="naming--meaning"></a>

-   [19.1](#naming--meaning) Называйте переменные так, чтобы их имена раскрывали бы их сущность, их роль в программе. При таком подходе их удобно будет искать в коде, а тот, кто увидит этот код, легче сможет понять смысл выполняемых им действий

    ```javascript
    // плохо
    let daysSLV = 10;
    let y = new Date().getFullYear();

    let ok;
    if (user.age > 30) {
        ok = true;
    }

    // хорошо
    const MAX_AGE = 30;
    let daysSinceLastVisit = 10;
    let currentYear = new Date().getFullYear();

    ...

    const isUserOlderThanAllowed = user.age > MAX_AGE;
    ```

    Не надо добавлять в имена переменных дополнительные слова, в которых нет необходимости.

    ```javascript
    // плохо
    let nameValue;
    let theProduct;

    // хорошо
    let name;
    let product;
    ```

    Не стоит принуждать того, кто читает код, к тому, чтобы ему приходилось бы помнить о том, в каком окружении объявлена переменная.

    ```javascript
    // плохо
    const users = ["John", "Marco", "Peter"];
    users.forEach((u) => {
        doSomething();
        doSomethingElse();
        // ...
        // ...
        // ...
        // ...
        // Тут перед нами ситуация, в которой возникает WTF-вопрос "Для чего используется `u`?"
        register(u);
    });

    // хорошо
    const users = ["John", "Marco", "Peter"];
    users.forEach((user) => {
        doSomething();
        doSomethingElse();
        // ...
        // ...
        // ...
        // ...
        register(user);
    });
    ```

    Не нужно снабжать имена переменных избыточными сведениями о контексте, в котором они используются.

    ```javascript
    // плохо
    const user = {
        userName: "John",
        userSurname: "Doe",
        userAge: "28",
    };

    user.userName;
    // хорошо
    const user = {
        name: "John",
        surname: "Doe",
        age: "28",
    };

    user.name;
    ```

<a name="naming--arrays"></a>

-   [19.2](#naming--arrays") Массивы обычно содержат в себе наборы каких-то значений. В результате к имени переменной, хранящей массив, имеет смысл добавлять букву s. Например:

    ```javascript
    // плохо
    const user = [];

    // хорошо
    const users = [];
    ```

<a name="naming--boolean"></a>

-   [19.3](#naming--boolean) Имена логических переменных имеет смысл начинать с is, are или has. Это приближает их к конструкциям, которые имеются в обычном языке. Например, вот вопрос: «Is that person a teacher?». Ответом на него может служить «Yes» или «No». Аналогично можно поступать и подбирая имена для логических переменных:

    ```javascript
    // хорошо
    const isTeacher = true;
    ```

<a name="naming--functions"></a>

-   [19.4](#naming--functions) Используйте для функций длинные описательные имена. Учитывая то, что функция представляет собой описание выполнения некоего действия, её имя должно представлять собой глагол или фразу, полностью описывающую суть функции. Имена аргументов нужно подбирать так, чтобы они адекватно описывали бы представляемые ими данные. Имена функций должны сообщать читателю кода о том, что именно делают эти функции.

    ```javascript
    // плохо
    function notif(user) {
        // реализация
    }

    // хорошо
    function notifyUser(emailAddress) {
        // реализация
    }
    ```

<a name="naming--descriptive"></a>

-   [19.5](#naming--descriptive) Избегайте названий из одной буквы. Имя должно быть наглядным. eslint: [`id-length`](https://eslint.org/docs/rules/id-length)

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

<a name="naming--camelCase"></a>

-   [19.6](#naming--camelCase) Используйте `camelCase` для именования:

    -   переменных,
    -   объектов,
    -   функций,
    -   методов,
    -   экземпляров класса,
    -   свойств компонента,

    ```javascript
    // плохо
    const OBJEcttsssss = {};
    const this_is_my_object = {};
    function c() {}

    // хорошо
    const thisIsMyObject = {};
    function thisIsMyFunction() {}
    ```

<a name="naming--PascalCase"></a>

-   [19.7](#naming--PascalCase) Используйте `PascalCase` для именования:

    -   конструкторов,
    -   классов,
    -   синглтонов,
    -   типов,
    -   интерфейсов,
    -   enum,
    -   свойств enum,
    -   React компонентов

    ```javascript
    // плохо
    function user(options) {
        this.name = options.name;
    }

    const bad = new user({
        name: "nope",
    });

    // хорошо
    class User {
        constructor(options) {
            this.name = options.name;
        }
    }

    const good = new User({
        name: "yup",
    });

    // плохо
    enum color {
      red
    }

    // хорошо
    enum Color {
        Red
    }
    ```

<a name="naming--leading-underscore"></a>

-   [19.8](#naming--leading-underscore) Не используйте `_` в начале или в конце названий. eslint: [`no-underscore-dangle`](https://eslint.org/docs/rules/no-underscore-dangle.html)

    > Почему? JavaScript не имеет концепции приватности свойств или методов. Хотя подчёркивание в начале имени является распространённым соглашением, которое показывает «приватность», фактически эти свойства являются такими же доступными, как и часть вашего публичного API. Это соглашение может привести к тому, что разработчики будут ошибочно думать, что изменения не приведут к поломке или что тесты не нужны. Итог: если вы хотите, чтобы что-то было «приватным», то оно не должно быть доступно извне.

    ```javascript
    // плохо
    this.__firstName__ = "Panda";
    this.firstName_ = "Panda";
    this._firstName = "Panda";

    // хорошо
    this.firstName = "Panda";

    // хорошо, в средах, где поддерживается WeakMaps
    // смотрите https://kangax.github.io/compat-table/es6/#test-WeakMap
    const firstNames = new WeakMap();
    firstNames.set(this, "Panda");
    ```

<a name="naming--self-this"></a>

-   [19.9](#naming--self-this) Не сохраняйте ссылку на `this`. Используйте стрелочные функции или [метод bind()](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Function/bind).

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

<a name="naming--Acronyms-and-Initialisms"></a>

-   [19.10](#naming--Acronyms-and-Initialisms) Сокращения или буквенные аббревиатуры всегда должны быть в верхнем или нижнем регистре.

    > Почему? Имена предназначены для удобства чтения.

    ```javascript
    // плохо
    import SmsContainer from "./containers/SmsContainer";

    // плохо
    const HttpRequests = [
        // ...
    ];

    // хорошо
    import SMSContainer from "./containers/SMSContainer";

    // хорошо
    const HTTPRequests = [
        // ...
    ];

    // также хорошо
    const httpRequests = [
        // ...
    ];

    // отлично
    import TextMessageContainer from "./containers/TextMessageContainer";

    // отлично
    const requests = [
        // ...
    ];
    ```

## <a name="react">React</a>

<a name="react--components-per-file"></a>

-   [20.1](#react--components-per-file) Включайте только один React компонент в файл.

    > Однако, разрешается несколько небольших [компонентов без состояний (чистых)](https://facebook.github.io/react/docs/reusable-components.html#stateless-functions) в файле. eslint: [`react/no-multi-comp`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/no-multi-comp.md#ignorestateless).

<a name="react--file-extensions"></a>

-   [20.2](#react--file-extensions) Используйте расширение `.jsx | .tsx` для компонентов React. eslint: [`react/jsx-filename-extension`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-filename-extension.md)

<a name="react--jsx-style"></a>

-   [20.3](#react--jsx-style) Следуйте приведённым ниже стилям для JSX-синтаксиса. eslint: [`react/jsx-closing-bracket-location`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-closing-bracket-location.md)

    ```jsx
    // плохо
    <Foo superLongParam="bar"
            anotherSuperLongParam="baz" />

    // хорошо
    <Foo
        superLongParam="bar"
        anotherSuperLongParam="baz"
    />

    // если свойства помещаются на одну строку, оставляйте их на одной строке
    <Foo bar="bar" />

    // отступ у дочерних элементов задается как обычно
    <Foo
        superLongParam="bar"
        anotherSuperLongParam="baz"
    >
        <Quux />
    </Foo>

    // плохо
    {showButton &&
        <Button />
    }

    // плохо
    {
        showButton &&
        <Button />
    }

    // хорошо
    {showButton && (
        <Button />
    )}

    // хорошо
    {showButton && <Button />}
    ```

<a name="react--no-multi-spaces"></a>

-   [20.4](#react--no-multi-spaces) Всегда вставляйте один пробел в ваш самозакрывающийся тег. eslint: [`no-multi-spaces`](https://eslint.org/docs/rules/no-multi-spaces), [`react/jsx-tag-spacing`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-tag-spacing.md)

    ```jsx
    // плохо
    <Foo/>

    // очень плохо
    <Foo                 />

    // плохо
    <Foo
     />

    // хорошо
    <Foo />
    ```

    Не отделяйте фигурные скобки пробелами в JSX. eslint: [`react/jsx-curly-spacing`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-curly-spacing.md)

    ```jsx
    // плохо
    <Foo bar={ baz } />

    // хорошо
    <Foo bar={baz} />
    ```

<a name="react--jsx-boolean-value"></a>

-   [20.5](#react--jsx-boolean-value) Не указывайте значение свойства, когда оно явно `true`. eslint: [`react/jsx-boolean-value`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-boolean-value.md)

        ```jsx
        // плохо
        <Foo
          hidden={true}
        />

        // хорошо
        <Foo
          hidden
        />

        // хорошо
        <Foo hidden />
        ```

    <a name="react--jsx-props-spread"></a>

-   [20.6](#react--jsx-props-spread) Используйте оператор расширения для свойств осознанно.

    > Почему? В противном случае вы скорее всего будете передавать внутрь компонента лишние свойства. А для React версии 15.6.1 и старше, вы можете [передать невалидные HTML-атрибуты в DOM](https://reactjs.org/blog/2017/09/08/dom-attributes-in-react-16.html).

    Исключения:

    -   Компоненты высшего порядка, которые передают свойства внутрь дочернего компонента и поднимают propTypes.

    ```jsx
    function HOC(WrappedComponent) {
      return class Proxy extends React.Component {
        Proxy.propTypes = {
          text: PropTypes.string,
          isLoading: PropTypes.bool
        };

        render() {
          return <WrappedComponent {...this.props} />
        }
      }
    }
    ```

    -   Использование оператора расширения для известных, явно заданных свойств. Это может быть особенно полезно при тестировании компонентов React с конструкцией beforeEach из Mocha.

    ```jsx
    export default function Foo {
      const props = {
        text: '',
        isPublished: false
      }

      return <div {...props} />;
    }
    ```

    Примечания по использованию: Если возможно, отфильтруйте ненужные свойства. Кроме того, используйте [prop-types-exact](https://www.npmjs.com/package/prop-types-exact), чтобы предотвратить ошибки.

    ```jsx
    // плохо
    render() {
      const { irrelevantProp, ...relevantProps } = this.props;
      return <WrappedComponent {...this.props} />
    }

    // хорошо
    render() {
      const { irrelevantProp, ...relevantProps } = this.props;
      return <WrappedComponent {...relevantProps} />
    }
    ```

<a name="react--jsx-wrap-multiline"></a>

-   [20.7](#react--jsx-wrap-multiline) Оборачивайте в скобки JSX теги, когда они занимают больше одной строки. eslint: [`react/jsx-wrap-multilines`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-wrap-multilines.md)


    ```jsx
    // плохо
    render() {
      return <MyComponent variant="long body" foo="bar">
               <MyChild />
             </MyComponent>;
    }

    // хорошо
    render() {
      return (
        <MyComponent variant="long body" foo="bar">
          <MyChild />
        </MyComponent>
      );
    }

    // хорошо, когда одна строка
    render() {
      const body = <div>hello</div>;
      return <MyComponent>{body}</MyComponent>;
    }
    ```

<a name="react--self-closing-comp"></a>

-   [20.8](#react--self-closing-comp) Всегда используйте самозакрывающиеся теги, если у элемента нет дочерних элементов. eslint: [`react/self-closing-comp`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/self-closing-comp.md)

    ```jsx
    // плохо
    <Foo variant="stuff"></Foo>

    // хорошо
    <Foo variant="stuff" />
    ```

<a name="react--jsx-closing-bracket-location"></a>

-   [20.9](#react--jsx-closing-bracket-location) Если ваш компонент имеет множество свойств, которые располагаются на нескольких строчках, то закрывайте тег на новой строке. eslint: [`react/jsx-closing-bracket-location`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-closing-bracket-location.md) [`react/jsx-closing-tag-location`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-closing-tag-location.md)

    ```jsx
    // плохо
    <Foo
      bar="bar"
      baz="baz" />

    // хорошо
    <Foo
      bar="bar"
      baz="baz"
    />
    ```

## <a name="react">Typescript</a>

<a name="typescript--use-enum"></a>

-   [21.1](#typescript--use-enum) Используйте enum для документирования

        > Почему? Enam'ы могут помочь документированию вашего кода. Например когда мы обеспокоены тем, что наши переменные отличаются от значений.

        ```javascript
        // плохо
        const GENRE = {
          ROMANTIC: 'romantic',
          DRAMA: 'drama',
          COMEDY: 'comedy',
          DOCUMENTARY: 'documentary',
        }

        // хорошо
        enum Genre {
          Romantic,
          Drama,
          Comedy,
          Documentary,
        }
        ```

<a name="typescript--dont-check-types"></a>

-   [21.2](#typescript--dont-check-types) Избегайте дополнительных проверок типов

    > TypeScript является надмножеством синтаксиса JavaScript и добавляют дополнительные статические проверки типов для языка. Всегда предпочитайте указывать типы переменных, параметров и возвращаемых значений, чтобы использовать всю мощь TypeScript. Это делает будущий рефакторинг более легким.

    ```javascript
    // плохо
    function travelToTexas(vehicle: Bicycle | Car) {
        if (vehicle instanceof Bicycle) {
            vehicle.pedal(currentLocation, new Location("texas"));
        } else if (vehicle instanceof Car) {
            vehicle.drive(currentLocation, new Location("texas"));
        }
    }

    // хорошо
    type Vehicle = Bicycle | Car;

    function travelToTexas(vehicle: Vehicle) {
        vehicle.move(currentLocation, new Location("texas"));
    }
    ```

<a name="typescript--use-private-types"></a>

-   [21.3](#typescript--use-private-types) Создавайте объекты с приватными/защищенными полями

    > TypeScript поддерживает public (по умолчанию), protected и private средства доступа к свойствам класса.

<a name="typescript--types-vs-interface"></a>

-   [21.4](#typescript--types-vs-interface) Типы vs. интерфейсы

    > Используйте типы, когда вам может понадобиться объединение или пересечение. Используйте интерфейс, когда хотите использовать extends или implements. Однако строгого правила не существует, используйте то, что работает у вас.

    ```javascript
    // плохо
    interface EmailConfig {
        // ...
    }

    interface DbConfig {
        // ...
    }

    interface Config {
        // ...
    }

    //...

    type Shape = {
        // ...
    };

    // хорошо
    type EmailConfig = {
        // ...
    };

    type DbConfig = {
        // ...
    };

    type Config = EmailConfig | DbConfig;

    // ...

    interface Shape {
        // ...
    }

    class Circle implements Shape {
        // ...
    }

    class Square implements Shape {
        // ...
    }
    ```

<a name="typescript--naming-prefix"></a>

-   [21.5](#typescript--naming-prefix) Префикс в именовании типов и интерфейсов

    > Использование префиксов не является обязательным. Используйте на свое усмотрение.

    ```javascript
    // хорошо
    interface IFacility {}

    // хорошо
    interface Facility {}
    ```

<a name="typescript--no-implicit-undefined"></a>

-   [1.1](#typescript--no-implicit-undefined) Не объявляйте явно undefined

    > Если необходимо объявить неизвестное значение используйте null. В интерфейсах используйте необязательные параметры

    ```javascript
    // плохо
    let user = undefined;

    // хорошо
    let user = null;

    // плохо
    let foo = { x: 123, y: undefined };

    // хорошо
    let foo: { x: number, y?: number } = { x: 123 };
    ```

## <a name="test">Тестирование</a>

<a name="test--spp"></a>

-   [22.1](#test--spp) Один кейс на тест

    > Тесты также должны соответствовать Принципу единой ответственности(SPP). Делайте только одно утверждение за единицу теста

    ```javascript
    // плохо
    describe("AwesomeDate", () => {
        it("handles date boundaries", () => {
            let date: AwesomeDate;

            date = new AwesomeDate("1/1/2015");
            assert.equal("1/31/2015", date.addDays(30));

            date = new AwesomeDate("2/1/2016");
            assert.equal("2/29/2016", date.addDays(28));

            date = new AwesomeDate("2/1/2015");
            assert.equal("3/1/2015", date.addDays(28));
        });
    });

    // хорошо
    describe("AwesomeDate", () => {
        it("handles 30-day months", () => {
            const date = new AwesomeDate("1/1/2015");
            assert.equal("1/31/2015", date.addDays(30));
        });

        it("handles leap year", () => {
            const date = new AwesomeDate("2/1/2016");
            assert.equal("2/29/2016", date.addDays(28));
        });

        it("handles non-leap year", () => {
            const date = new AwesomeDate("2/1/2015");
            assert.equal("3/1/2015", date.addDays(28));
        });
    });
    ```

## <a name="promise">Асинхронность</a>

<a name="promise--use-async-await"></a>

-   [23.1](#promise--use-async-await) Используйте там, где это возможно, конструкцию async/await

    > Callback-функции ухудшают читаемость и приводят к чрезмерному количеству вложенности (ад обратных вызовов(callback hell)).
