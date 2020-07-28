---
layout: post
title: Пробуем Deno
date: "2020-07-21T13:02:00Z"
tags: ["Deno"]
--- 

Джаваскрипт и бекенд, как по мне, понятия не совместимые, но все же, встречаем дено.

<p style="text-align: center">
<img src="/assets/denoLogo.svg" alt="drawing" width="200"/>
</p>

Node и npm экосистема - главный образец показывающий на сколько джаваскрипт крут и то, как нужно делать качественный софт.
Вот только, создатель посмотрел, почесал репу, и подумал здесь уже ничего не исправить, жги. И отжог, видимо как-то так и появился дено, который теперь убийца ноды, а npm вообще выбросили. 

#### В чём принципиальные отличия? 
Ключевых на самом то деле не так уж и много:

- Переписали все нативные байдинги на раст
- Выбросили пакетный менеджер (теперь можно просто воткнуть что угодно ссылкой) 
- Из коробки можно запускать тайпскрипт

Там, еще куча всего, но ценность этого всего весьма сомнительная:
- теперь по умолчанию нет доступа ни к чему пока явно не укажешь (сеть, файловая система и тп.)
- поставляется всё одним бинарником 
- есть инспектор зависимостей и встроенный код форматер, КАРЛ!

Вот это я понимаю 21 век. Я не особо нод разработчик, но прямо сказать, что продукт решает какую нибудь конкретную проблему весьма сложно. Больше всего это похоже на релиз "Node перезагрузка" чем на что-то действительно особенное. 

#### Давай уже там писать, чего тянуть то.

На момент написания статьи версия дено уже 1 с чем то там, что, как я считаю должно быть показателем довольно стабильного продукта. 

Набросаем простую CLI app. Суть такая, запускаем с параметром -c “Страна”, софтина дергает апишку и возвращает количество covid кейсов за прошлые сутки для введенной станы. 

Ставим дено на винду:
```
choco install deno
```

#### C чего начинается разработка?

Правильная (у нас только такая) конечно же с тестирования, подумал я, и создал файлик с простым тестом.

```typescript
import { assertEquals } from "https://deno.land/std/testing/asserts.ts";

Deno.test("Should correctly parse country argument", () => {
    assertEquals(1,1);
});
```

Давайте запустем и вн....имание, в ответ получаем:

```
error: TS2345 [ERROR]: Argument of type '{ depth: number; sorted: boolean; trailingComma: boolean; compact: boolean; iterableLimit: number; }' is not assignable to parameter of type 'InspectOptions'.
```

Ну что ж, бывает. А в голове промелькнуло: интересно, а как на этом писать интеграционные тесты, бдд и вот это вот все. А как апишечку замокать для тестов? (Да и хер с этим всем, теперь везде лямбды на три сточки, серверлесс, может этих всех тестов и не нужно).

Раздосадованный я забиваю на тесты (это же джаваскрипт детка) и решаю писать так.
Вот что получилось (не буду вдаваться в детали и так всё понятно):

```typescript
import clc from "https://deno.land/x/color/index.ts";

interface ICovidReportApp {
  country: string | null;
}

interface ICovidResponse {
  Country: string;
  CountryCode: string;
  Cases: number;
  Status: string;
}

function getArg(param: string): string | null {
  const parameterIndex = Deno.args.indexOf(param);

  if (parameterIndex !== -1 && parameterIndex + 1 < Deno.args.length) {
    // the parameter array will looks like ['-c', 'poland'] 
    // so we need to take the next array item to get actual param value
    return Deno.args[parameterIndex + 1];
  }

  return null;
}

async function main({ country = "" }: ICovidReportApp) {
  // Check if country is set
  if (!country) {
    throw new Error("County is not specified!");
  }

  // Make the country name acceptable for api
  // The api accept the country in specific format like 'south-africa'
  const countrySlug = country.toLowerCase().replaceAll(
    " ",
    "-",
  );

  // API Need dates so put today and yesterday as dates to get ~24h of data
  let startDate = new Date();
  let endDate = new Date();

  // Since the api doesn't bother a lot with time we just set it to 0 
  startDate.setHours(0, 0, 0, 0);
  endDate.setHours(0, 0, 0, 0);
  // And our end date will be the today - 1 day, this allows to get results for ~24h
  startDate.setDate(endDate.getDate() - 1);

  const queryString =
    `https://api.covid19api.com/country/${countrySlug}/status/confirmed?from=${startDate.toISOString()}&to=${endDate.toISOString()}`;

  try {

    // Fetch the data
    const res = await fetch(queryString);
    // Parse the data
    const resData: ICovidResponse[] = await res.json();

    // Check if everything is ok
    if (!resData || res.status !== 200 || resData.length < 0) { 
      console.error("No data was found!");
      return;
    }

    const covidCases = resData.reduce((cases, curr) => (cases + curr.Cases), 0);
    const covidCasesRed = clc.bgRed.text(covidCases);
    const contryBlue = clc.bgBlue.text(country);

    // Nice color output
    console.log(`There are ${covidCasesRed}${clc.reset.text("")} confirmed cases for last 24h in ${contryBlue}${clc.reset.text(".")}`);
  } catch (e) {
    console.error(e);
  }
}

// Main entry point, parse the input args and pass into the function
main({country: getArg("-c")});
```

https://gist.github.com/slavik925/5dacf1e6865409ea5eb4a5a39cdad42a

Для запуска:
```
deno run --allow-net covid24Report.ts -c “Germany”
```

Output:

![Corona report](/assets/coronaReport.png "Corona report")

 
#### Какой вывод можно сделать? 

- Хреновый язык как был так и остался :)
- Никакого повода для миграции хотя бы с того же nodejs. Выглядит как шаг немного вперёд и в сторону, но всё равно выстрелит, ведь джаваскрипт это комьюнити построенное на хайпе и смене библиотек каждые 2 мясяца (А нода залежалась уже, да).
- Все также не хватает простого и стандартного функционала. Например для CLI аргументов, дат или ещё простых базовых вещей. Все нужно делать тонной сторонних библиотек и по концове в коробке из 1000 пазлов один какой-нибудь, как обычно, возьмёт да и потеряется.

Для сравнения можно взять такой же 25 летний руби и напедалить все тоже:

```ruby
require 'getoptlong'
require 'net/http'
require 'json'
require 'time'  
require 'date'

opts = GetoptLong.new(['--country', '-c', GetoptLong::REQUIRED_ARGUMENT])

country_slug = nil
original_contry = nil

opts.each do |opt, arg|
    case opt
        when '--country'
            original_contry = arg
            country_slug = arg.downcase.gsub(/\s/, "-")
    end
end

today_date = Date.today.to_time.utc.iso8601
yesterday_date = Date.today.prev_day.to_time.utc.iso8601

url = "https://api.covid19api.com/country/#{country_slug}/status/confirmed?from=#{yesterday_date}&to=#{today_date}"

uri = URI(url)
response = Net::HTTP.get(uri)

response_json = JSON.parse(response)
total_cases = response_json.reduce(0) { | casesSum, response | casesSum + response['Cases'] }

puts "There are \e[41m#{total_cases}\e[0m confirmed cases for last 24h in  \e[44m#{original_contry.capitalize}\e[0m"
```

https://gist.github.com/slavik925/778cd1f9c25013efb5832a5bd8a881d9

Все библиотеки встроенные, вышло проще, качественнее и красивее. При том что я вообще не рубист, ни разу :)

Скорость? Ха смешно, все ноют о какой-то иллюзорной скорости в мутных бенчмарках. Где она - я не знаю и никогда не видел.

#### Итого

Если вы сприптодрочер и js/ts захватит мир - однозначно брать, пилить новые либы каждые 2 месяца, плакать, радоваться, всё как вы привыкли.

Если вы вменяем разраб и вам нужны инструменты для быстрой и качественной работы посмотрите в сторону лучше уж раста ну и всегда есть Ruby/Python/Java/C#/etc. 

Сюда можно заглянуть только, так из интереса.

> Все вышеизложенное это конечно же мои личные субъективные впечатления, возможно всё на самом деле не так :)

