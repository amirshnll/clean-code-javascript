<div dir="rtl">

# مفهوم Clean Code در javascript

## فهرست

1. [مقدمه](#introduction)
2. [متغیرها](#variables)
3. [توابع](#functions)
4. [اشیا و ساختارهای داده](#objects-and-data-structures)
5. [کلاس ها](#classes)
6. [SOLID](#solid)
7. [تست کردن](#testing)
8. [همزمانی](#concurrency)
9. [مدیریت خطا](#error-handling)
10. [قالب بندی](#formatting)
11. [کامنت ها](#comments)
12. [ترجمه ها](#translation)

## مقدمه

![Humorous image of software quality estimation as a count of how many expletives
you shout when reading code](https://www.osnews.com/images/comics/wtfm.jpg)

اصول مهندسی نرم افزار ، از کتاب Robert Code Martin [_Clean Code_](https://www.amazon.com/Clean-Code-Handbook-Software-Craftsmanship/dp/0132350882) ، اقتباس گرفته شده است تا در این نوشته اصول آن برای زبان برنامه نویسی javascript را بررسی کنیم. این یک راهنمای ساده نیست. این یک راهنما برای تولید نرم افزارهای قابل خواندن با خوانایی بالا، قابل استفاده مجدد در زبان برنامه نویسی جاوااسکریپت است.

لازم نیست که هر اصلی که گفته شد دقیقاً رعایت شود، و حتی تعداد کمتر مورد توافق همه توسعه دهدگان قرار خواهند گرفت. این ها رهنمودهایی هستند و چیز دیگری فراتر از این نیستند، اما مواردی هستند که در طول چندین سال تجربه جمعی توسط نویسندگان Clean Code نوشته شده است.

صنعت مهندسی نرم افزار ما کمی بیش از 50 سال قدمت دارد و ما هنوز چیزهای زیادی باید بیاموزیم. وقتی معماری نرم افزار به اندازه خود معماری قدیمی باشد، شاید در این صورت قوانین سخت تری برای پیروی از آن داشته باشیم. در حال حاضر، اجازه دهید این دستورالعمل ها به عنوان یک موضوع مهم برای ارزیابی کیفیت کد در زبان برنامه نویسی JavaScript که شما و تیم خود تولید می کنید، باشد.

یک نکته مهم دیگر: دانستن این موارد بلافاصله شما را به یک توسعه دهنده نرم افزار بهتر تبدیل نمی کند و سال ها کار با این نکات به این معنی نیست که اشتباه نخواهید کرد. هر قطعه از کد به عنوان اولین پیش نویس شروع می شود، مثل اینکه خاک رس مرطوب را به شکل نهایی خود که مثلا می تواند کوزه باشد درآید. سرانجام، وقتی با  این نوشته را می خوانید مشکلات خود را هم ببنید. خودتان را برای پیش نویس های اولیه که نیاز به پیشرفت دارند، اذیت نکنید و به جای آن سعی کنید بیشتر کد بزنید.


## **متغیرها**

### از نام های معنا دار برای متغیرهای خود استفاده کنید

**بد:**

```javascript
const yyyymmdstr = moment().format("YYYY/MM/DD");
```

**خوب:**

```javascript
const currentDate = moment().format("YYYY/MM/DD");
```

**[⬆ برگشت به بالا](#table-of-contents)**

### از کلمات مشابه برای همان نوع متغیر استفاده کنید

**بد:**

```javascript
getUserInfo();
getClientData();
getCustomerRecord();
```

**خوب:**

```javascript
getUser();
```

**[⬆ برگشت به بالا](#table-of-contents)**

### از نام های جستجو شدنی استفاده کنید

ما بیشتر از آنچه را که کد می نویسیم کدها را می خوانیم. مهم است که کدی که می نویسیم قابل خواندن و جستجو کردن باشد. با نام گذاری متغیرها که در نهایت برای درک برنامه ما معنی دار هستند، ما برای خوانندگان کد خود آسیب نمی رسانیم. نام های خود را جستجو کردنی انتخاب کنید. ابزارهایی مانند [buddy.js](https://github.com/danielstjules/buddy.js) و [ESLint](https://github.com/eslint/eslint/blob/660e0918933e6e7fede26bc675a0763a6b357c94/docs/rules/no-magic-numbers.md) می توانند به شناسایی ثابت های بدون نام کمک کنند.

**بد:**

```javascript
// What the heck is 86400000 for?
setTimeout(blastOff, 86400000);
```

**خوب:**

```javascript
// Declare them as capitalized named constants.
const MILLISECONDS_IN_A_DAY = 60 * 60 * 24 * 1000; //86400000;

setTimeout(blastOff, MILLISECONDS_IN_A_DAY);
```

**[⬆ برگشت به بالا](#table-of-contents)**

### از متغیرهای توضیحی استفاده کنید

**بد:**

```javascript
const address = "One Infinite Loop, Cupertino 95014";
const cityZipCodeRegex = /^[^,\\]+[,\\\s]+(.+?)\s*(\d{5})?$/;
saveCityZipCode(
  address.match(cityZipCodeRegex)[1],
  address.match(cityZipCodeRegex)[2]
);
```

**خوب:**

```javascript
const address = "One Infinite Loop, Cupertino 95014";
const cityZipCodeRegex = /^[^,\\]+[,\\\s]+(.+?)\s*(\d{5})?$/;
const [_, city, zipCode] = address.match(cityZipCodeRegex) || [];
saveCityZipCode(city, zipCode);
```

**[⬆ برگشت به بالا](#table-of-contents)**

### از نگاشت ذهنی خودداری کنید

متغیرهای صریح بهتر از متغیرهای ضمنی است.

**بد:**

```javascript
const locations = ["Austin", "New York", "San Francisco"];
locations.forEach(l => {
  doStuff();
  doSomeOtherStuff();
  // ...
  // ...
  // ...
  // Wait, what is `l` for again?
  dispatch(l);
});
```

**خوب:**

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

**[⬆ برگشت به بالا](#table-of-contents)**

### نیاز به تکرار نام شی در متغیرها نیست

برای نام گذاری اعضای یک کلاس نیاز به استفاده از نام کلاس در نام آن ها نیست.

**بد:**

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

**خوب:**

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

**[⬆ برگشت به بالا](#table-of-contents)**

### به جای اتصال کوتاه یا شرطی، از آرگومان های پیش فرض استفاده کنید

بحث های پیش فرض اغلب تمیزتر از اتصال کوتاه است. توجه داشته باشید که اگر از آنها استفاده کنید، عملکرد شما فقط مقادیر پیش فرض آرگومان های تعریف نشده را ارائه می دهد. سایر مقادیر `falsy` مانند `''` ، `""` ، `false` ، `null` ، `0` و `NaN` ، با مقدار پیش فرض جایگزین نخواهند شد.

**بد:**

```javascript
function createMicrobrewery(name) {
  const breweryName = name || "Hipster Brew Co.";
  // ...
}
```

**خوب:**

```javascript
function createMicrobrewery(name = "Hipster Brew Co.") {
  // ...
}
```

**[⬆ برگشت به بالا](#table-of-contents)**

## **توابع**

### آرگومان های توابع (در حالت ایده آل 2 تا یا کمتر از آن)

محدود کردن تعداد پارامترهای توابع بسیار مهم است زیرا تست کردن توابع برای شما آسان تر می شود. داشتن بیش از سه مورد آرگومان منجر به این می شود که تست کردن کدها سخت تر شود.

یک یا دو آرگومان ایده آل است و در صورت امکان باید از سه آرگومان بیشتر جلوگیری کرد. هر چیزی بیش از این باید ادغام شود. معمولاً، اگر بیش از دو آرگومان دارید، تابع شما تلاش می کند کارهای زیادی (بیش از یک کار) را انجام دهد.

از آنجا که جاوا اسکریپت به شما امکان می دهد اشیا را بسازید می توانید از آن چندین نمونه بسازید و استفاده کنید.

برای اینکه مشخص شود تابع مورد نظر چه ویژگی هایی دارد، می توانید از destructuring syntax در ES2015 / ES6 استفاده کنید. این چند مزیت اصلی دارد:

- وقتی کسی به امضای تابع نگاه می کند، بلافاصله مشخص شود که وظیفه ی انجام آن چیست
- می توان از آن برای شبیه سازی پارامترهای نام برده استفاده کرد
- با کمک آن ها می توان از side effectsها جلوگیری کرد و اشیا بدون آرگومان ها اصلا ساخته نمی شوند و برای کلون کردن توابع نیاز هستند
- استفاده از توابع بدون آرگومان ها نمی تواند امکان پذیر باشد

**بد:**

```javascript
function createMenu(title, body, buttonText, cancellable) {
  // ...
}

createMenu("Foo", "Bar", "Baz", true);

```

**خوب:**

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

**[⬆ برگشت به بالا](#table-of-contents)**

### توابع باید یک کار را انجام دهند

این مهمترین قانون در مهندسی نرم افزار است. وقتی توابع بیش از یک کار انجام می دهند، نوشتن، تست و استدلال آن ها دشوارتر است. وقتی می توانید یک تابع را فقط برای یک کار بنویسید، می توان به راحتی آنرا تغییر داد و کد شما بسیار تمیزتر خواهد شد. اگر از کل این راهنما فقط همین یک مورد را متوجه شوید شما از تعداد زیادی از توسعه دهندگان پیشی خواهید گرفت.

**بد:**

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

**خوب:**

```javascript
function emailActiveClients(clients) {
  clients.filter(isActiveClient).forEach(email);
}

function isActiveClient(client) {
  const clientRecord = database.lookup(client);
  return clientRecord.isActive();
}
```

**[⬆ برگشت به بالا](#table-of-contents)**

### نام تابع باید بگوید که چه کاری انجام می دهد

**بد:**

```javascript
function addToDate(date, month) {
  // ...
}

const date = new Date();

// It's hard to tell from the function name what is added
addToDate(date, 1);
```

**خوب:**

```javascript
function addMonthToDate(month, date) {
  // ...
}

const date = new Date();
addMonthToDate(1, date);
```

**[⬆ برگشت به بالا](#table-of-contents)**

### توابع فقط باید یک سطح انتزاع باشند

وقتی بیش از یک سطح انتزاع داشته باشید، تابع شما معمولاً بیش از حد استفاده می شود. تقسیم توابع منجر به استفاده مجدد و تست آسان تر می شود.

**بد:**

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

**خوب:**

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

**[⬆ برگشت به بالا](#table-of-contents)**

### کد تکراری را حذف کنید

تمام تلاش خود را انجام دهید تا از نوشتن کدهای تکراری جلوگیری کنید. کد تکراری بد است زیرا به این معنی است که در صورت نیاز به تغییر منطق در ساختار برنامه، بیش از یک مکان برای تغییر دادن چیزی وجود دارد که باید آن را انجام دهید.

تصور کنید اگر یک رستوران اداره می کنید و موجودی خود را پیگیری می کنید: تمام گوجه فرنگی ، پیاز ، سیر ، ادویه جات و … را در لیست نوشته باشید. اگر چندین لیست دارید که این مورد را در آن نگه می دارید ، پس هنگام تهیه یک غذا با گوجه فرنگی همه ی لیست ها باید به روز شوند ولی اگر فقط یک لیست موجودی دارید ، فقط یک جا برای به روزرسانی وجود دارد که باید آنرا تغییر دهید!!

اگر غالباً کد تکراری دارید علت آن این است که دو یا چند چیز کمی متفاوت دارید که اشتراکات زیادی با هم دارند، اما تفاوت آن ها شما را مجبور می کند که دو یا چند عملکرد جداگانه برای آن ها داشته باشید که بیشتر کارهای مشابه را انجام می دهند. حذف کد تکراری به معنای ایجاد انتزاعی است که می تواند مجموعه ای از موارد مختلف را فقط با یک تابع / ماژول / کلاس مدیریت کند.

درست گرفتن انتزاع بسیار مهم است، به همین دلیل باید از اصول SOLID مندرج در بخش Class ها پیروی کنید. انتزاعات بد ممکن است از کد تکراری بدتر باشد، بنابراین مراقب باشید! با گفتن این، اگر می توانید انتزاع خوبی انجام دهید، آن را انجام دهید! کدتان را مجددا تکرار نکنید، در غیر این صورت هر زمان که بخواهید یک چیز را تغییر دهید باید بخش های مختلفی را ویرایش کنید.

**بد:**

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

**خوب:**

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

**[⬆ برگشت به بالا](#table-of-contents)**

### اشیا پیش فرض را با Object.assign تنظیم کنید

**بد:**

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

**خوب:**

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
  // config now equals: {title: "Order", body: "Bar", buttonText: "Send", cancellable: true}
  // ...
}

createMenu(menuConfig);
```

**[⬆ برگشت به بالا](#table-of-contents)**

### از flagها به عنوان پارامترهای توابع استفاده نکنید

flagها به کاربر شما می گویند که این تابع بیش از یک کار انجام می دهد. توابع باید یک کار انجام دهند. اگر توابع شما براساس کدهای boolean دنبال می شوند، توابع خود را تجزیه کنید.

**بد:**

```javascript
function createFile(name, temp) {
  if (temp) {
    fs.create(`./temp/${name}`);
  } else {
    fs.create(name);
  }
}
```

**خوب:**

```javascript
function createFile(name) {
  fs.create(name);
}

function createTempFile(name) {
  createFile(`./temp/${name}`);
}
```

**[⬆ برگشت به بالا](#table-of-contents)**

### از عوارض جانبی یا Side Effects خودداری کنید (بخش 1)

به جای اینکه مقدار یک تابع در یک متغیر global نوشته شود به صورت غیر void آنرا تعریف کنید و مقدار را برگردانید.

**بد:**

```javascript
// Global variable referenced by following function.
// If we had another function that used this name, now it'd be an array and it could break it.
let name = "Ryan McDermott";

function splitIntoFirstAndLastName() {
  name = name.split(" ");
}

splitIntoFirstAndLastName();

console.log(name); // ['Ryan', 'McDermott'];
```

**خوب:**

```javascript
function splitIntoFirstAndLastName(name) {
  return name.split(" ");
}

const name = "Ryan McDermott";
const newName = splitIntoFirstAndLastName(name);

console.log(name); // 'Ryan McDermott';
console.log(newName); // ['Ryan', 'McDermott'];
```

**[⬆ برگشت به بالا](#table-of-contents)**

### از عوارض جانبی یا Side Effects خودداری کنید (بخش 2)

در JavaScript برخی مقادیر غیر قابل تغییر (تغییرناپذیر) و برخی قابل تغییر (تغییر پذیر) هستند. اشیا و آرایه ها دو نوع مقادیر قابل تغییر هستند بنابراین مهم است که هنگام انتقال به عنوان پارامترهای یک تابع، آنها را با دقت کنترل کنید. یک تابع جاوا اسکریپت می تواند خصوصیات یک شی را تغییر دهد یا محتوای یک آرایه را تغییر دهد که به راحتی باعث اشکال در جای دیگر شود.

Two caveats to mention to this approach:

1. There might be cases where you actually want to modify the input object,
   but when you adopt this programming practice you will find that those cases
   are pretty rare. Most things can be refactored to have no side effects!

2. Cloning big objects can be very expensive in terms of performance. Luckily,
   this isn't a big issue in practice because there are
   [great libraries](https://facebook.github.io/immutable-js/) that allow
   this kind of programming approach to be fast and not as memory intensive as
   it would be for you to manually clone objects and arrays.

**بد:**

```javascript
const addItemToCart = (cart, item) => {
  cart.push({ item, date: Date.now() });
};
```

**خوب:**

```javascript
const addItemToCart = (cart, item) => {
  return [...cart, { item, date: Date.now() }];
};
```

**[⬆ برگشت به بالا](#table-of-contents)**

### توابع عمومی یا global ننویسید

توابع global در JavaScript یک عمل نامناسب است زیرا شما می توانید با کتابخانه یا API دیگری این کار را انجام دهید.

به طور مثال: اگر می خواهید یک `Array` در جاوا اسکریپت را گسترش دهید تا یک متد `diff` داشته باشد که می تواند تفاوت بین دو آرایه را نشان دهد، چه می کنید؟ شما می توانید تابع جدید خود را در `Array.prototype` بنویسید، اما این می تواند با کتابخانه دیگری که سعی در انجام همان کار دارد تداخل پیدا کند. اگر آن کتابخانه دیگر فقط برای پیدا کردن تفاوت بین اولین و آخرین عناصر یک آرایه از تفاوت استفاده می کرد چه؟ به همین دلیل بهتر است فقط از کلاس های ES2015 / ES6 استفاده کنید و Array global را به سادگی گسترش دهید.

**بد:**

```javascript
Array.prototype.diff = function diff(comparisonArray) {
  const hash = new Set(comparisonArray);
  return this.filter(elem => !hash.has(elem));
};
```

**خوب:**

```javascript
class SuperArray extends Array {
  diff(comparisonArray) {
    const hash = new Set(comparisonArray);
    return this.filter(elem => !hash.has(elem));
  }
}
```

**[⬆ برگشت به بالا](#table-of-contents)**

### برنامه نویسی تابعی را ترجیح دهید

جاوا اسکریپت مثل زبان Haskell یک زبان کاربردی نیست، اما یک زبان تابع محور است. زبان های تابعی می توانند تمیزتر و تست کردن آن ها آسان تر هم باشد. هر وقت می توانید این سبک برنامه نویسی را ترجیح دهید جاوااسکریپت را انتخاب کنید.

**بد:**

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

**خوب:**

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

**[⬆ برگشت به بالا](#table-of-contents)**

### کپسوله سازی دستورات شرطی

**بد:**

```javascript
if (fsm.state === "fetching" && isEmpty(listNode)) {
  // ...
}
```

**خوب:**

```javascript
function shouldShowSpinner(fsm, listNode) {
  return fsm.state === "fetching" && isEmpty(listNode);
}

if (shouldShowSpinner(fsmInstance, listNodeInstance)) {
  // ...
}
```

**[⬆ برگشت به بالا](#table-of-contents)**

### از شروط منفی استفاده نکنید

**بد:**

```javascript
function isDOMNodeNotPresent(node) {
  // ...
}

if (!isDOMNodeNotPresent(node)) {
  // ...
}
```

**خوب:**

```javascript
function isDOMNodePresent(node) {
  // ...
}

if (isDOMNodePresent(node)) {
  // ...
}
```

**[⬆ برگشت به بالا](#table-of-contents)**

### از شرطی شدن اجتناب کنید

به نظر می رسد این یک کار غیرممکن است. با شنیدن این موضوع احتمالا همه ی برنامه نویسان عزیز می گویند “چگونه قرار است بدون `if` ، کاری انجام دهیم؟” پاسخ این است که شما می توانید برای رسیدن به همان کار در بسیاری از موارد از چند case استفاده کنید. سوال دوم معمولاً این است ، “خوب این عالی است اما چرا من می خواهم این کار را انجام دهم؟” پاسخ این است که مفهوم قبلی که در ارتباط با clean code خواندیم این است که یک تابع فقط باید یک کار واحد انجام دهد. وقتی کلاس ها و توابعی را دارید که دستور `if` دارند ، به خواننده ی کد خود می گویید عملکرد این بخش شما بیش از یک کار را انجام می دهد. به یاد داشته باشید ، فقط یک کار را باید انجام دهید.

**بد:**

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

**خوب:**

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

**[⬆ برگشت به بالا](#table-of-contents)**

### از بررسی نوع خودداری کنید (بخش 1)

در زبان برنامه نویسی javascript بررسی نوع نداریم اگر برای این موضوع وسواس دارید باید با یک تابع استفاده کنید که بهتر است این کار را انجام ندهید. این موضوع که نوع خاصی برای متغیرهای جاوااسکریپت در نظر گرفته می شود گاهی خوب است و گاهی بد اگر نیاز به جلوگیری از این موضوع دارید باید از APIهای سازگار استفاده کنید.

**بد:**

```javascript
function travelToTexas(vehicle) {
  if (vehicle instanceof Bicycle) {
    vehicle.pedal(this.currentLocation, new Location("texas"));
  } else if (vehicle instanceof Car) {
    vehicle.drive(this.currentLocation, new Location("texas"));
  }
}
```

**خوب:**

```javascript
function travelToTexas(vehicle) {
  vehicle.move(this.currentLocation, new Location("texas"));
}
```

**[⬆ برگشت به بالا](#table-of-contents)**

### از بررسی نوع خودداری کنید (بخش 2)

اگر با مقادیر موجود در زبان جاوااسکریپت مانند رشته ها و عدد های صحیح کار می کنید و نمی توانید از polymorphism استفاده کنید اما هنوز هم نیاز به بررسی نوع دارید ، باید از TypeScript استفاده کنید. این یک گزینه ی عالی برای جاوا اسکریپت در حالت معمول است، زیرا TypeScript را در یک syntax استاندارد JavaScript را برای شما فراهم می کند. مشکلی که در بررسی  دستی نوع ها در جاوا اسکریپت به وجود دارد این است که انجام آن به خوبی به توابع و کدهای اضافی احتیاج دارد به طوری که type-safety ساختگی شما خوانایی از دست رفته را جبران نمی کند. کدهای جاوا اسکریپت خود را تمیز نگه دارید، تست های خوبی بنویسید و آنها را تست کنید. در غیر این صورت با TypeScript که یک گزینه ی خوب می باشد همه ی این کارها را انجام دهید.

**بد:**

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

**خوب:**

```javascript
function combine(val1, val2) {
  return val1 + val2;
}
```

**[⬆ برگشت به بالا](#table-of-contents)**

### بیش از حد بهینه نکنید

Modern browsers do a lot of optimization under-the-hood at runtime. A lot of
times, if you are optimizing then you are just wasting your time. [There are good
resources](https://github.com/petkaantonov/bluebird/wiki/Optimization-killers)
for seeing where optimization is lacking. Target those in the meantime, until
they are fixed if they can be.

مرورگرهای مدرن هنگام اجرای کدها بهینه سازی زیادی را انجام می دهند. بسیاری از اوقات، اگر در حال بهینه سازی هستید، فقط وقت خود را تلف می کنید. منابع خوبی برای دیدن این بهینه سازی سازی ها وجود دارد که در [این لینک](https://github.com/petkaantonov/bluebird/wiki/Optimization-killers) می توانید آن ها را مطالعه کنید.

**بد:**

```javascript
// On old browsers, each iteration with uncached `list.length` would be costly
// because of `list.length` recomputation. In modern browsers, this is optimized.
for (let i = 0, len = list.length; i < len; i++) {
  // ...
}
```

**خوب:**

```javascript
for (let i = 0; i < list.length; i++) {
  // ...
}
```

**[⬆ برگشت به بالا](#table-of-contents)**

### کد مرده را حذف کنید

کد مرده به همان اندازه ای که کدهای تکراری بد هستند نامناسب است. دلیلی برای عدم حذف کدهای مرده نیست و اگر کدی هیچ وقت فراخوانی نمی شود، از دستش خلاص شوید!

**بد:**

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

**خوب:**

```javascript
function newRequestModule(url) {
  // ...
}

const req = newRequestModule;
inventoryTracker("apples", req, "www.inventory-awesome.io");
```

**[⬆ برگشت به بالا](#table-of-contents)**

## **اشیا و ساختارهای داده**

### از getters و setters استفاده کنید

استفاده از getters و setters برای دسترسی به داده های داخل اشیا می تواند بهتر از دسترسی مستقیم به اعضای آن شی باشد؛ دلایل زیر را برای آن داریم:

- وقتی می خواهید کارهایی فراتر از به دست آوردن یک property در شی را دارید، نیازی نیست به جستجو و تغییر دسترسی نیست.
- امکان افزودن اعتبار سنجی را هنگام set کردن دارید.
- امکان کپسوله سازی به وجود می آید.
- برای افزودن error handling و logging مشکلی نخواهید داشت.
- می توانید ویژگی های شی object خود را به صورت lazy بارگذاری کنید و سرعت لود خود را سریعتر کنید.

**بد:**

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

**خوب:**

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

**[⬆ برگشت به بالا](#table-of-contents)**

### اعضای داخل اشیا را private کنید

این را می توان از طریق بستارها (برای ES5 و نسخه های پایین تر) داشت.

**بد:**

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

**خوب:**

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

**[⬆ برگشت به بالا](#table-of-contents)**

## **کلاس ها**

### کلاس های ES2015 / ES6 را به توابع ساده ES5 ترجیح دهید

بدست آوردن تعاریف، ساختار و روش تعریف قابل خواندن برای کلاس های ES5 کلاسیک بسیار دشوار است. اگر به ارث بری نیاز دارید (و توجه داشته باشید که ممکن است این کار را نکنید) ، کلاس های ES2015 / ES6 را ترجیح دهید. با این وجود، تابع های کوچک را به کلاس ها ترجیح دهید تا زمانی که که به اشیا بزرگتر و پیچیده تری احتیاج داشته باشید.

**بد:**

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

**خوب:**

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

**[⬆ برگشت به بالا](#table-of-contents)**

### از متدهای زنجیره گذاری یا chaining استفاده کنید

این الگو در JavaScript بسیار مفید است و شما آن را در بسیاری از کتابخانه ها مانند jQuery و Lodash مشاهده می کنید. این کار به شما اجازه می دهد تا کد خوانا با حجم کمتری داشته باشید. به همین دلیل، می گوییم، از توابع زنجیره گذاری استفاده کنید و نگاهی به تمیز بودن کدهای خود بیندازید. در توابع کلاس خود به راحتی دستور `this`  را می توانید پایان هر کلاس بگذارید و زنجیره را برای هر متدهای کلاس بزرگتر کنید.

**بد:**

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

**خوب:**

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

**[⬆ برگشت به بالا](#table-of-contents)**

### ترکیب را بر ارث بری ترجیح دهید

ابتدا بهتر است کمی در ارتباط با [_Design Patterns_](https://en.wikipedia.org/wiki/Design_Patterns)ها بخوانید.

**بد:**

```javascript
class Employee {
  constructor(name, email) {
    this.name = name;
    this.email = email;
  }

  // ...
}

// Bad because Employees "have" tax data. EmployeeTaxData is not a type of Employee
class EmployeeTaxData extends Employee {
  constructor(ssn, salary) {
    super();
    this.ssn = ssn;
    this.salary = salary;
  }

  // ...
}
```

**خوب:**

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

**[⬆ برگشت به بالا](#table-of-contents)**

## **SOLID**

SOLID مخففی است که توسط مایکل پرز برای پنج اصل اول به نام رابرت مارتین معرفی شده است، که به معنی پنج اصل اساسی برنامه نویسی و طراحی شی گرا است.

- S: Single Responsibility Principle (SRP)
- O: Open/Closed Principle (OCP)
- L: Liskov Substitution Principle (LSP)
- I: Interface Segregation Principle (ISP)
- D: Dependency Inversion Principle (DIP)

### Single Responsibility Principle یا SRP

همانطور که در Clean Code بیان شد ، “هرگز نباید بیش از یک کار برای یک کلاس وجود داشته باشد”. بسته بندی کلاس هایی که قابلیت های زیادی دارند ، وسوسه انگیز است ، مانند زمانی که فقط اجازه دارید یک چمدان را در پرواز با خود حمل کنید. مسئله این است که کلاس شما از نظر مفهومی منسجم نخواهد بود و دلایل زیادی برای تغییر در آن ایجاد می کند. به حداقل رساندن تعداد دفعات لازم برای تغییر کلاس مهم است. این مهم است زیرا اگر کارایی بیش از حد در یک کلاس وجود دارد و شما بخشی از آن را اصلاح می کنید ، درک اینکه چگونه این امر بر سایر ماژول های وابسته در کد شما تأثیر می گذارد دشوار است.

**بد:**

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

**خوب:**

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

**[⬆ برگشت به بالا](#table-of-contents)**

### Open/Closed Principle یا OCP

همانطور که برتراند مایر اظهار داشت ، “نهادهای نرم افزاری (کلاسها ، ماژولها ، توابع و …) باید برای پسوند باز باشند، اما برای اصلاح بسته هستند.”

این به چه معناست؟ این اصل در اصل بیان می کند که شما باید به افرادی که کد شما را می خوانند اجازه دهید ویژگی های جدید را بدون تغییر کد موجود اضافه کنند.

**بد:**

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

**خوب:**

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

**[⬆ برگشت به بالا](#table-of-contents)**

### Liskov Substitution Principle یا LSP

**بد:**

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
    const area = rectangle.getArea(); // BAD: Returns 25 for Square. Should be 20.
    rectangle.render(area);
  });
}

const rectangles = [new Rectangle(), new Rectangle(), new Square()];
renderLargeRectangles(rectangles);
```

**خوب:**

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

**[⬆ برگشت به بالا](#table-of-contents)**

### Interface Segregation Principle یا ISP

**بد:**

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

**خوب:**

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

**[⬆ برگشت به بالا](#table-of-contents)**

### Dependency Inversion Principle یا DIP

این اصل دو چیز اساسی را بیان می کند:

- ماژول های سطح بالا نباید به ماژول های سطح پایین وابسته باشند. هر دو باید به Abstractionها بستگی داشته باشند.
- Abstractionها نباید به جزئیات بستگی داشته باشد. جزئیات باید به Abstractionها بستگی داشته باشد.

**بد:**

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

    // BAD: We have created a dependency on a specific request implementation.
    // We should just have requestItems depend on a request method: `request`
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

**خوب:**

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

// By constructing our dependencies externally and injecting them, we can easily
// substitute our request module for a fancy new one that uses WebSockets.
const inventoryTracker = new InventoryTracker(
  ["apples", "bananas"],
  new InventoryRequesterV2()
);
inventoryTracker.requestItems();
```

**[⬆ برگشت به بالا](#table-of-contents)**

## **تست کردن**

بهانه ای برای نوشتن تست وجود ندارد. بسیاری از فریم ورک های تست جاوااسکریپت می توانند استفاده شوند. اگر روش ترجیحی شما Test Driven Development یا TDD است، بسیار عالی است، اما نکته اصلی این است که قبل انجام این اتفاق مطمئن شوید این کاراهداف شما را پوشش داده است یا خیر؟!

### مفهوم واحد در هر تست

**بد:**

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

**خوب:**

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

**[⬆ برگشت به بالا](#table-of-contents)**

## **همزمانی**

### Use Promises, not callbacks

Callbacks اصلا کد تمیز نیست و باعث ایجاد تو در تویی بیش از حد می شود. با ES2015  و ES6 می توانید که نوع built-in عمومی یا GLOBAL دارید؛ از آن استفاده کنید 🙂

**بد:**

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

**خوب:**

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

**[⬆ برگشت به بالا](#table-of-contents)**

### استفاده از Async / Await حتی تمیزتر از Promisesها هستند

Promisesها یک گزینه ی بسیار تمیز برای callbacksها هستند، اما ES2017 / ES8 async و await را به ارمغان می آورد که یک راه حل حتی تمیزتر را ارائه می دهد. تمام آنچه شما نیاز دارید یک تابعی است که در یک کلمه کلیدی async پیشوند آ.ن قرار گرفته است، و سپس می توانید منطق خود را بدون یک زنجیره از توابع ضروری بنویسید اگر امروز می توانید از ویژگی های ES2017 / ES8 استفاده کنید، از این استفاده کنید!

**بد:**

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

**خوب:**

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

**[⬆ برگشت به بالا](#table-of-contents)**

## **مدیریت خطا**

خطاهای پرتاب شده (Thrown errors) چیز خوبی است! این به این معنی است که زمان اجرا با موفقیت مشخص شده است که مشکلی در برنامه شما رخ داده است و این امر با متوقف کردن اجرای عملکرد روی پشته فعلی، کشتن روند کار (در Node) و اطلاع دادن به شما در کنسول با ردیابی پشته، به شما اطلاع می دهد.

### caught errorsها را نادیده نگیرید

هیچ کاری با caught error به شما این توانایی را نمی دهد که هرگز خطای گفته شده را برطرف یا واکنش نشان دهید. ورود خطا به کنسول (console.log) خیلی خوب نیست، زیرا اغلب اوقات ممکن است در دریایی از چیزهای چاپ شده در کنسول گم شود. اگر هر بیتی از کد را در try / catch قرار دهید، به این معنی است که فکر می کنید ممکن است در آنجا خطایی رخ دهد و بنابراین باید برنامه ای تنظیم کنید یا یک مسیر کد برای زمان بروز آن ایجاد کنید.

**بد:**

```javascript
try {
  functionThatMightThrow();
} catch (error) {
  console.log(error);
}
```

**خوب:**

```javascript
try {
  functionThatMightThrow();
} catch (error) {
  // One option (more noisy than console.log):
  console.error(error);
  // Another option:
  notifyUserOfError(error);
  // Another option:
  reportErrorToService(error);
  // OR do all three!
}
```

### promisesهای رد شده را نادیده نگیرید

به همین دلیل نباید از caught errors با `try / catch` چشم پوشی کنید.

**بد:**

```javascript
getdata()
  .then(data => {
    functionThatMightThrow(data);
  })
  .catch(error => {
    console.log(error);
  });
```

**خوب:**

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

**[⬆ برگشت به بالا](#table-of-contents)**

## **قالب بندی**

قالب بندی ذهنی است. مانند بسیاری از قوانین در اینجا، هیچ قانون سخت و سریعی وجود ندارد که شما باید از آن پیروی کنید. نکته اصلی این است که در مورد قالب بندی بحث نکنید. ابزارهای زیادی برای خودکار کردن این کار وجود دارد. یکی از آن ها را استفاده کنید! بحث و جدال مهندسان درباره قالب بندی، اتلاف وقت و هزینه است.

برای مواردی که تحت عنوان قالب بندی خودکار قرار نمی گیرند (تورفتگی، `tab`ها در مقابل `space`ها، ” یا ‘ و …).

### از حروف بزرگ استفاده کنید

جاوا اسکریپت دارای نوع های مختلف نیست، بنابراین بزرگ نویسی در مورد متغیرها، توابع و … چیزهای زیادی به شما می گوید. این قوانین ذهنی هستند، بنابراین تیم شما می تواند هر آنچه را که می خواهد انتخاب کند. نکته این است، مهم نیست که همه شما چه چیزی را انتخاب می کنید، فقط باید در این موضوع ثابت قدم باشید.

**بد:**

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

**خوب:**

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

**[⬆ برگشت به بالا](#table-of-contents)**

### توابع تماس گیرنده ها (callers) و تماس گیرنده ها (callees) باید نزدیک باشند

اگر یک تابع، تابع دیگری را فراخوانی می کند، آن توابع را اگر در یک فایل هستند نزدیک هم نگه دارید. در حالت ایده آل، تماس گیرنده را دقیقاً بالاتر از تماس گیرنده قرار دهید. ما تمایل داریم کد را مانند روزنامه از بالا به پایین بخوانیم. به همین دلیل، کد خود را به این روش بنویسید.

**بد:**

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

**خوب:**

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

**[⬆ برگشت به بالا](#table-of-contents)**

## **کامنت ها**

### فقط برای مواردی را کامنت قرار دهید که دارای پیچیدگی بالایی هستند.

نظرات یک موضوع دلخواه است، نه یک الزام. یک کد خوب بیشتر دارای داکیومنت خوب است نه کامنت های زیاد و کامل در بین برنامه.

**بد:**

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

**خوب:**

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

**[⬆ برگشت های بالا](#table-of-contents)**

### کد خارج از کامنت را در پایگاه کد خود رها نکنید

Version control به یک دلیل وجود دارد که کد قدیمی را در history خود بگذارید.

**بد:**

```javascript
doStuff();
// doOtherStuff();
// doSomeMoreStuff();
// doSoMuchStuff();
```

**خوب:**

```javascript
doStuff();
```

**[⬆ برگشت به بالا](#table-of-contents)**

### کامنت های طولانی و ژورنالی نداشته باشید

به یاد داشته باشید که از version control استفاده کنید! با همین موضوع دیگر نیازی به کد مرده، کامنت های طولانی نیست. برای به دست آوردن تاریخچه ی فعالیت های خود از `git log` استفاده کنید!

**بد:**

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

**خوب:**

```javascript
function combine(a, b) {
  return a + b;
}
```

**[⬆ برگشت به بالا](#table-of-contents)**

### از نشانگرهای موقعیتی خودداری کنید

به جای نشانگرهای طولانی که با کاراکترهای خاص در کامنت ها می گذارید از indentها و فرورفتگی ها و از قالب ها استفاده کنید.

**بد:**

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

**خوب:**

```javascript
$scope.model = {
  menu: "foo",
  nav: "bar"
};

const actions = function() {
  // ...
};
```

**[⬆ برگشت به بالا](#table-of-contents)**

## ترجمه ها

ترجمه ی این مقاله به زبان های دیگر نیز موجود است:

- ![am](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Armenia.png) **ارمنی**: [hanumanum/clean-code-javascript/](https://github.com/hanumanum/clean-code-javascript)
- ![bd](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Bangladesh.png) **بنگلادشی (বাংলা)**: [InsomniacSabbir/clean-code-javascript/](https://github.com/InsomniacSabbir/clean-code-javascript/)
- ![br](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Brazil.png) **برزیلی پرتغالی**: [fesnt/clean-code-javascript](https://github.com/fesnt/clean-code-javascript)
- ![cn](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/China.png) **چینی**:
  - [alivebao/clean-code-js](https://github.com/alivebao/clean-code-js)
  - [beginor/clean-code-javascript](https://github.com/beginor/clean-code-javascript)
- ![tw](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Taiwan.png) **چینی قدیمی سنتی**: [AllJointTW/clean-code-javascript](https://github.com/AllJointTW/clean-code-javascript)
- ![fr](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/France.png) **فرانسوی**: [GavBaros/clean-code-javascript-fr](https://github.com/GavBaros/clean-code-javascript-fr)
- ![de](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Germany.png) **آلمانی**: [marcbruederlin/clean-code-javascript](https://github.com/marcbruederlin/clean-code-javascript)
- ![id](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Indonesia.png) **اندونزی**: [andirkh/clean-code-javascript/](https://github.com/andirkh/clean-code-javascript/)
- ![it](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Italy.png) **ایتالیایی**: [frappacchio/clean-code-javascript/](https://github.com/frappacchio/clean-code-javascript/)
- ![ja](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Japan.png) **ژاپنی**: [mitsuruog/clean-code-javascript/](https://github.com/mitsuruog/clean-code-javascript/)
- ![kr](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/South-Korea.png) **کره ای**: [qkraudghgh/clean-code-javascript-ko](https://github.com/qkraudghgh/clean-code-javascript-ko)
- ![pl](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Poland.png) **لهستانی**: [greg-dev/clean-code-javascript-pl](https://github.com/greg-dev/clean-code-javascript-pl)
- ![ru](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Russia.png) **روسی**:
  - [BoryaMogila/clean-code-javascript-ru/](https://github.com/BoryaMogila/clean-code-javascript-ru/)
  - [maksugr/clean-code-javascript](https://github.com/maksugr/clean-code-javascript)
- ![es](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Spain.png) **اسپانیایی**: [tureey/clean-code-javascript](https://github.com/tureey/clean-code-javascript)
- ![es](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Uruguay.png) **اسپانیایی**: [andersontr15/clean-code-javascript](https://github.com/andersontr15/clean-code-javascript-es)
- ![tr](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Turkey.png) **ترکی**: [bsonmez/clean-code-javascript](https://github.com/bsonmez/clean-code-javascript/tree/turkish-translation)
- ![ua](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Ukraine.png) **اوکراینی**: [mindfr1k/clean-code-javascript-ua](https://github.com/mindfr1k/clean-code-javascript-ua)
- ![vi](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Vietnam.png) **ویتنامی**: [hienvd/clean-code-javascript/](https://github.com/hienvd/clean-code-javascript/)
- ![ir](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Iran.png) **فارسی**: [amirshnll/clean-code-javascript/](https://github.com/amirshnll/clean-code-javascript/)

**[⬆ برگشت به بالا](#table-of-contents)**

</div>
