---
id: release-8.8.0
title: 'Drupal 8.8.0'
path: /8/releases/8.8.0
core: 8
metatags:
  title: 'Drupal 8.8.0: Список изменений'
  description: 'Актуальный список изменений Drupal 8.8.0, релиз которого запланирован на 4 декабря 2019 г.'
---

> [!IMPORTANT]
> Будущий релиз Drupal. Не предназначен для использования на продакшен сайтах. Список изменений неполный и многое может измениться.

**Дата релиза**: 4 декабря 2019 г.\
**Перевод на поддержку безопасности**: 3 июня 2020 г.\
**Окончание поддержки**: +- полгода после перевода на поддержку безопасности.

## Обновление с 8.7.x

На данный момент информация отсутствует. Она появится ближе к релизу или в день релиза.

## Список изменений

### Общие изменения

#### Путь до конфигураций синхронизации теперь хранится в $settings вместо $config_directories

- [#3018145](https://www.drupal.org/node/3018145)

Путь до конфигураций синхронизации теперь хранится в `$settings['config_sync_directory']` файла **settings.php**.

Возможность указания нескольких конфигурационных директорий в `$config_directories` помечена устаревшим. Если ваш собственный или контрибный код опирается на данную особенность, вам необходимо перенести настройки либо в `$settings`, либо в другое хранилище, например, в State или конфигурации напрямую.

Как результат упрощения настройки пути до директории, были также помечены устаревшими функции `config_get_config_directory()` и `drupal_install_config_directories()`, а также константа `CONFIG_SYNC_DIRECTORY`.

Для получения пути до данной директории, вместо старого варианта `config_get_config_directory(CONFIG_SYNC_DIRECTORY)` используйте `Settings::get('config_sync_directory')`.

##### Обновление settings.php

**Было**

```php
$config_directories['sync'] = 'sites/default/files/config_YLZJmmpOqc_KBWbMc2I58ky3-8c7qtg4G-OpSqFClHs5E0NL9YMFgyF4RRTv8IFdl_kAMs_Bdw/sync';
````

**Стало**

```php
$settings['config_sync_directory'] = 'sites/default/files/config_YLZJmmpOqc_KBWbMc2I58ky3-8c7qtg4G-OpSqFClHs5E0NL9YMFgyF4RRTv8IFdl_kAMs_Bdw/sync';
```

### Оформление и темизация

- [#3060703](https://www.drupal.org/node/3060703) Добавленна новая переменная `file_size` для шаблона **file-link.html.twig**.

#### Классы для визуализации были удалены для filter модуля

- [#3061520](https://www.drupal.org/node/3061520)

Все CSS классы (`filter-wrapper`, `filter-guidelines`, `filter-list` и `filter-help`), которые использовались для справочного раздела о текстовых форматах ввода, были удалены. Также были удалены все стили для данных классов из **filter.admin.css**.

> [!NOTE]
> Данное изменение не повлияет на темы, которые наследуются от Stable или Classy.

Если вы хотите вернуть данные классы обратно, можете воспользоваться следующим кодом в своей теме:

```php
/**
 * Implements hook_element_info_alter().
 */
function THEMENAME_element_info_alter(array &$info) {
  if (array_key_exists('text_format', $info)) {
    $info['text_format']['#process'][] = 'THEMENAME_process_text_format';
  }
}

/**
 * #process callback, for adding classes to filter components.
 *
 * @param array $element
 *   Render array for the text_format element.
 *
 * @return array
 *   Text_format element with the filter classes added.
 */
function THEMENAME_process_text_format(array $element) {
  $element['format']['#attributes']['class'][] = 'filter-wrapper';
  $element['format']['guidelines']['#attributes']['class'][] = 'filter-guidelines';
  $element['format']['format']['#attributes']['class'][] = 'filter-list';
  $element['format']['help']['#attributes']['class'][] = 'filter-help';

  return $element;
}
```

### Core API

- [#3056639](https://www.drupal.org/node/3056639) `MailManagerInterface::mail()` теперь позволяет переопределять сообщение об ошибке.
- [#3035273](https://www.drupal.org/node/3035273) Несколько функций относящихся к uri и scheme файлов помечены устаревшими и перенесены в `\Drupal\Core\StreamWrapper\StreamWrapperManagerInterface`.
- [#3059344](https://www.drupal.org/node/3059344) Добавлен подкласс `\Symfony\Component\Validator\ConstraintViolation` в Drupal `\Drupal\Core\Validation\ConstraintViolation` для использования в `\Drupal\Core\TypedData\Validation\ExecutionContext::addViolation()`.
- [#3054692](https://www.drupal.org/node/3054692) `\Drupal\system\SystemRequirements::phpVersionWithPdoDisallowMultipleStatements()` помечен устаревшим.

### Entity API

- [#2835616](https://www.drupal.org/node/2835616) Функции `entity_get_display()` и `entity_get_form_display()` получили замену в виде сервиса `entity_display.repository`.
- [#3033656](https://www.drupal.org/node/3033656) Функции для просмотра сущностней помечены устаревшими.

### Migrate API

- [#3043694](https://www.drupal.org/node/3043694) Возможность передачи массива для `link_uri` плагина источника Migrate API помечено устаревшим.

### User

- [#3038171](https://www.drupal.org/node/3038171) Модуль `user` теперь предоставляет новый маршрут поддерживающий стандарт [RFC5785](https://github.com/WICG/change-password-url), позволяющий менять пароль по well-known пути `/.well-known/change-password` (произведет 301-й редирект на форму восстановления пароля Drupal).
- [#3051463](https://www.drupal.org/node/3051463) Функции `user_delete()` и `user_delete_multiple()` помечены устаревшими.

### Views

- [#3040204](https://www.drupal.org/node/3040204) Установка идентификатора раскрытого фильтра Views теперь обрабатываются корректно.
- [#3047897](https://www.drupal.org/node/3047897) Конструктор `NodeNewComments` изменен. В него добавили два новых параметра, которые ожидают экземпляры объектов `EntityTypeManagerInterface` и `EntityFieldManagerInterface`, соответственно. Для старого кода будет выводиться предупреждение об ошибке и сервисы будут автоматически получены из глобального контейнера.
- [#2869168](https://www.drupal.org/node/2869168) Добавлена возможность ограничивать доступные операторы для раскрытых фильтров.

### Тестирование

- [#3030340](https://www.drupal.org/node/3030340) Объект `WebTestBase` помечен устаревшим.
- [#3056869](https://www.drupal.org/node/3056869) Поддержка PHPUnit 4.x прекращена, теперь будет запрашиваться версия ^6.5. В соответствии с данным изменением, [Drush](../../drush.md) команды `drupal-phpunit-upgrade` и `drupal-phpunit-upgrade-check` больше не нужны и были удалены.
- [#3057326](https://www.drupal.org/node/3057326) Передача File сущности в качестве первого аргумента в `assertFileExists` и `assertFileNotExists` помечена устаревшей.

### Конфигурации

- [#3037022](https://www.drupal.org/node/3037022) Новый сервис `ExportStorage` для помощи при экспортировании конфигураций.
- [#2943918](https://www.drupal.org/node/2943918) `ConfigImporter` теперь также получает сервис `extension.list.module` в качестве аргумента.

### Стандарты

- [#3041002](https://www.drupal.org/node/3041002) CSS стандарты для сортировки теперь обрабатываются с использованием styleling-order плагином.

### Прочие изменения

- [#3033540](https://www.drupal.org/node/3033540) Формы модуля `action` были перенесены в `src/Form`.
- [#2853355](https://www.drupal.org/node/2853355) Добавлен новый трейт `ConfigurableTrait` для уменьшения кода плагинов.
- [#3050078](https://www.drupal.org/node/3050078) Third party settings для форматтеров полей теперь передаются вместе с рендер элементом и доступы по ключу `#third_party_settings`.
- [#3051983](https://www.drupal.org/node/3051983) Функция `drupal_schema_get_field_value()` помечена устаревшей.
- [#2769027](https://www.drupal.org/node/2769027) Для SQLite включена опция [WAL](https://www.sqlite.org/wal.html) (Write-Ahead Logging) по умолчанию.

## Ссылки

- [Спиок изменений Drupal](https://www.drupal.org/list-changes/drupal) (англ.)