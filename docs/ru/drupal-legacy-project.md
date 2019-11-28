---
id: drupal-legacy-project
title: Composer Drupal Legacy Project
search-keywords:
  - Установка при помощи композера
metatags:
  description: 'Альтернативный способ установки Drupal 8 при помощи Composer.'
---

**drupal/legacy-project** — данный шаблон создает новый сайт со структурой как она была в [Drupal 8.7.0](./release-8.7.0.md) и ранее. 

Файл "index.php" , "core" директория и т.д. расположене в корне проекте рядом с "composer.json" и "vendor" директорией. Используйте данный шаблон только если у вас нет возможности использовать рекомендуемый шаблон.

> [!NOTE]
> Рекомендуется использовать [drupal/recommended-project](drupal-recommended-project.md).

**Пример структуры проекта:**

```bash
  project/
  ├─ core/
  ├─ libraries/
  ├─ modules/
  ├─ profiles/
  ├─ themes/
  ├─ vendor/
  ├─ index.php
  └─ composer.json
```

## Создание проекта с использованием данного шаблона

```bash
composer -n create-project drupal/legacy-project:^8.8@dev my_new_site 
```

## См. также

- [drupal/recommended-project](drupal-recommended-project.md) — рекомендуемый шаблон для всех сайтов.
- [Composer](composer.md)

## Ссылки

- [drupal/legacy-project](https://github.com/drupal/legacy-project) (англ.)