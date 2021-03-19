---
title: Web Vitals
slug: web-vitals
metatags:
  description: Web Vitals — это набор руководств, рекомендаций и инструментов, которые позволяют произвести оценку качества и удобства конкретной страницы сайта.
---

**Web Vitals** — это инициатива Google, направленная на обеспечение единого руководства по оценке качества веб сайтов.

## Введение

Оптимизация сайта для предоставления качественного пользовательского опыта — залог к долгосрочному успеху любого сайта.

Web Vitals — это набор руководств, рекомендаций и инструментов, которые позволяют произвести оценку качества и удобства конкретной страницы сайта.

Для проверки на соответствие рекомендациям Web Vitals существуют различные сервисы. Самый популярный и известный из них — [PageSpeed](https://developers.google.com/speed/pagespeed/insights), который также встроен в браузер Google Chrome и доступен в инструментах для разработчиков на вкладке [Lighthouse](https://github.com/GoogleChrome/lighthouse).

Оценка проводится по многим параметрам и в различных условиях. На основе полученных результатов формируется сводка, рекомендации и оценки по 100-бальной шкале.

Web Vitals старается преподнести информацию в максимально доступном и понятном формате, который поймут не только разработчики, но и владельцы сайтов. Это позволяет производить данные проверки без технического опыта.

## Core Web Vitals

**Core Web Vitals** — это подмножество Web Vitals с самыми приоритетными и важными параметрами, которые необходимо измерять для каждой страницы и производить улучшения, если обнаружены проблемы.

> [!NOTE]
> Начиная с мая 2021 г., [Google начнёт учитывать](https://developers.google.com/search/blog/2020/11/timing-for-page-experience) показатели **Core Web Vitals** при ранжировании поисковой выдачи.

Параметры, которые включены в Core Web Vitals могут меняться и совершенствоваться с ходом времени. На 2020 год, данные проверки производят оценку пользовательского опыта — _скорость загрузки_, _отзывчивость_, и _визуальную стабильность_. Данные параметры представлены в виде трёх значений:

![Core Web Vitals](https://i.imgur.com/kVV0DAd.png)

- **Largest Contentful Paint** (LCP): измеряет _скорость загрузки_. Для предоставления хорошего пользовательского опыта, LCP должно быть в пределах **2.5 секунды** с момента начала загрузки страницы.
- **First Input Delay** (FID): измеряет _отзывчивость_. Для предоставления хорошего пользовательского опыта, значение FID должно быть менее **100 миллисекунд**.
- **Cumulative Layout Shift** (CLS): измеряет _визуальную стабильность_. Для предоставления хорошего пользовательского опыта, значение CLS должно быть менее **0.1**.

## Общие рекомендации

В данном разделе собраны общие рекомендации как улучшить показатели Web Vitals.

Drupal из коробки проходит все аудиты и имеет оценку 90+ на любом устройстве в любом [установочном профиле](../drupal/9/distributions/index.md). Это хорошие примеры того, как нужно использовать возможности Drupal для достижения хороших результатов.

### Сократите время до первого байта

Старайтесь держать Time To First Byte (TTFB) ниже 200мс — [рекомендованное значение](https://developers.google.com/web/tools/chrome-devtools/network/understanding-resource-timing). Значения выше 600мс считаются провальными и сильно влияют и искажают все прочие проверки.

* Используйте встроенное в Drupal кеширование: [Internal Dynamic Page Cache](../drupal/9/dynamic-page-cache/index.md), [Internal Page Cache](../drupal/9/page-cache/index.md). Для большинства маленьких и средних сайтов будет достаточно включить Internal Page Cache для достижения рекомендуемых значений.
* Если нет возможности использовать Internal Page Cache, сократите количество выполняемого кода в рантайме и оптимизируйте его, храните значения в cache bin.
* Если данные персональные и не кешируемые — загружайте их лениво. Вы можете использовать [ленивые строители](../drupal/9/lazy-builder/index.md) и «стратегии рендера плейсхолдеров» для различных подходов. Попробуйте использовать поставляемый в комплекте модуль BigPipe.
* Если инвалидация кеша происходит часто или в больших объёмах, используйте инструменты «разогрева» кеша для самых важных страниц. Например, модуль [Warmer](https://www.drupal.org/project/warmer) позволяет производить разогрев с использованием [очередей](../drupal/9/queues/index.md).

### Загружайте только необходимый CSS / JavaScript

Большие объёмы загружаемых CSS и JavaScript сильно сказываются на параметре FID (TTI, TBT), особенно на мобильных тестах.

Вы можете проверить, какой объём данных и что именно используется на странице. Для этого в Chrome Dev Tools перейдите на вкладку Coverage и запустите аудит. Если суммарный процент использования стилей и скриптов менее 50% - это отличная возможность внести улучшения, которые положительно скажутся на FID.

* Используйте [Libraries API](../drupal/9/libraries/index.md) для оптимальной загрузки необходимых CSS и JS.
* Не указывайте все библиотеки в свойстве `libraries` **.info.yml** [темы оформления](../drupal/9/themes/index.md). Изучите [возможные способы подключения библиотек](../drupal/9/libraries/attach/index.md) и используйте наиболее подходящий вариант.

### Оптимизируйте изображения

Изображения являются одним из главных источником проблем. Им следует уделить особое внимание.

* Используйте предоставляемый Drupal механизм создания стилей изображений.
* Старайтесь готовить стили изображений тех размеров, в которых они будут использованы на страницах. В этом помогут адаптивные стили изображений.
* Убедитесь что у всех изображений на сайте имеется аттрибут `width` и `height`. Все изображения, проходящие через Drupal, автоматически получают данные значения, но не забудьте проверить у тех, что добавлены вручную.
* Не завышайте качество обработки картинок. По умолчанию в Drupal используется значение 75% — это оптимальное значение.
* Попробуйте использовать сторонние наборы инструментов для обработки изображений, например [ImageMagick](https://www.drupal.org/project/imagemagick). Как правило, они эффективнее стандартного в PHP инструмента — [GD](https://www.php.net/manual/ru/book.image.php), который используется по молчанию.
* Удалите метаданные (EXIF) из изображений, если они вам не нужны. Они могут существенно увеличить объем изображения, иногда они весят больше чем само изображение. Для их очистки добавьте нужным стилям изображений эффект «Strip metadata», предоставляемый модулем [Image Effects](https://www.drupal.org/project/image_effects) (поддерживает GD и ImageMagick), либо, если используйте ImageMagick с аргументом `-strip`.
* Если есть возможность, используйте более современные форматы, такие как WebP (в поставке с [Drupal 9.2.0](../drupal/9/releases/9.2.x/9.2.0/index.md)). Как показывает опыт, это не обязательно для достижения 100 баллов на мобильных устройствах.
* Загружайте изображения лениво. В этом могут помочь библиотеки типа [lazysizes](https://github.com/aFarkas/lazysizes). Если вам не нужно поддерживать очень старые устройства и IE11, используйте нативные ленивые загрузки — поставляется с [Drupal 9.2.0](../drupal/9/releases/9.2.x/9.2.0/index.md) и работает автоматически для всех.

### Оптимизируйте внешние скрипты

Внешние скрипты оказывают серьезное влияние на показатели, но в отличие от предыдущих рекомендаций и советов, у разработчиков нет возможности влиять на данные скрипты напрямую. Тем не менее есть способы сгладить данные проблемы.

* Проведите аудит внешних библиотек, скриптов, айфреймов и т.д. Постарайтесь полностью отключить то, что не критично для работы сайта.
* Внешние скрипты не первой важности, такие как чаты, загружайте по требованию. Например, создайте статичную кнопку, с простым JS, который при клике на эту кнопку будет загружать скрипт и активировать чат.
* Если вы загружаете внешние скрипты при помощи [Libraries API](../drupal/9/libraries/index.md), укажите чтобы они загружались асинхронно и только после загрузки страницы. Пример:

```yaml
some-js:
  js:
    //example.com/script.js:
      type: external
      attributes:
        async: true

google.fonts:
  css:
    theme:
      //fonts.googleapis.com/css?family=Roboto:400,400i,500,500i,700,700i&subset=cyrillic&display=swap:
        type: external
        attributes:
          media: 'print'
          onload: "this.media='all'"
```

* Те скрипты и стили, что гарантированно загружаются на каждой странице с самого начала, добавьте в `preload`.

Пример preload с Twig в [теме оформления](../drupal/9/themes/index.md):

**html.html.twig**:

```twig
...
<!DOCTYPE html>
<html{{ html_attributes }}>
  <head>
    <head-placeholder token="{{ placeholder_token|raw }}">
    <title>{{ head_title|safe_join(' | ') }}</title>
    <css-placeholder token="{{ placeholder_token|raw }}">
    <js-placeholder token="{{ placeholder_token|raw }}">
    {% include '@THEMENAME/includes/preload.twig' only %}
...
```

**THEMENAME/includes/preload.twig**:

```twig
{#
/**
 * @file
 * Preload important third-party resources.
 */
#}
<link rel="preload" href="https://mc.yandex.ru/metrika/watch.js" as="script">
<link rel="preload" href="https://fonts.googleapis.com/css?family=Roboto:400,400i,500,500i,700,700i&subset=cyrillic&display=swap" as="style">
```

Пример preload через [хуки](../drupal/9/hooks/index.md):

```php
/**
 * Implements hook_preprocess_page().
 */
function THEMENAME_preprocess_page(array &$variables): void {
  $google_font_styles_preload = [
    '#tag' => 'link',
    '#attributes' => [
      'rel' => 'preload',
      'href' => 'https://fonts.googleapis.com/css?family=Roboto:400,400i,500,500i,700,700i&subset=cyrillic&display=swap',
      'as' => 'style',
    ],
  ];
  $variables['#attached']['html_head'][] = [$google_font_styles_preload, 'google_font_styles_preload'];
}
```

## Ссылки

- [Web Vitals](https://web.dev/vitals/), Google, Philip Walton, 2020
- [PageSpeed Insights](https://developers.google.com/speed/pagespeed/insights/?hl=ru), Google
- [Lighthouse](https://github.com/GoogleChrome/lighthouse), GitHub
- [Lighthouse Scoring Calculator](https://googlechrome.github.io/lighthouse/scorecalc), GitHub