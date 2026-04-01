
# Отчет по лабораторной работе №3
## Тема: «Разработка простой темы для CMS WordPress»

**Студент:** Кристева Ирина  
**Предмет:** CMS  
 

---

## 1. Цель работы
Научиться создавать собственную тему WordPress, разобраться в её минимальной структуре и принципах работы шаблонов.

---

## 2. Среда выполнения
* **ОС:** Windows 10 (XAMPP / OpenServer)
* **Редактор:** VS Code
* **CMS:** WordPress (локальная установка)

---

## 3. Ход работы

### Шаг 1. Создание структуры папок и файлов
В каталоге `/wp-content/themes/` создана папка темы `usm-theme`. В ней созданы файлы, согласно требованиям иерархии шаблонов WP:

> **📸 МЕСТО ДЛЯ СКРИНШОТА №1** > *(Скриншот открытой папки темы в VS Code со всеми файлами)*

### Шаг 2. Описание темы (style.css)
Файл необходим для идентификации темы системой.

```css
/*
Theme Name: USM Theme
Author: Irina Cristeva
Description: Лабораторная работа по разработке темы WP
Version: 1.0
*/

body { font-family: Segoe UI, sans-serif; line-height: 1.5; color: #333; }
.wrapper { max-width: 1000px; margin: 0 auto; display: flex; }
header, footer { background: #222; color: #fff; padding: 1.5rem; text-align: center; }
article { margin-bottom: 2rem; border-bottom: 1px solid #ccc; padding-bottom: 1rem; }
aside { width: 30%; background: #f4f4f4; padding: 20px; }
main { width: 70%; padding: 20px; }
```

### Шаг 3. Подключение функций (functions.php)
Регистрация стилей и поддержка меню/виджетов.

```php
<?php
function usm_theme_scripts() {
    wp_enqueue_style('main-styles', get_stylesheet_uri());
}
add_action('wp_enqueue_scripts', 'usm_theme_scripts');

add_action('after_setup_theme', function() {
    add_theme_support('title-tag');
    add_theme_support('post-thumbnails');
});
```

### Шаг 4. Шаблоны Header и Footer
Разделение страницы на логические части.

**header.php:**
```php
<!DOCTYPE html>
<html <?php language_attributes(); ?>>
<head>
    <meta charset="<?php bloginfo('charset'); ?>">
    <?php wp_head(); ?>
</head>
<body <?php body_class(); ?>>
<header>
    <h1><?php bloginfo('name'); ?></h1>
    <p><?php bloginfo('description'); ?></p>
</header>
```

**footer.php:**
```php
<footer>
    <p>&copy; <?php echo date('Y'); ?> <?php bloginfo('name'); ?> - Irina Cristeva</p>
</footer>
<?php wp_footer(); ?>
</body>
</html>
```

### Шаг 5. Главный шаблон (index.php)
Реализация "Цикла WordPress" (The Loop) для вывода контента.

```php
<?php get_header(); ?>
<div class="wrapper">
    <main>
        <?php if (have_posts()) : while (have_posts()) : the_post(); ?>
            <article>
                <h2><a href="<?php the_permalink(); ?>"><?php the_title(); ?></a></h2>
                <small>Опубликовано: <?php the_time('F jS, Y'); ?> в категории <?php the_category(', '); ?></small>
                <div><?php the_excerpt(); ?></div>
            </article>
        <?php endwhile; else : ?>
            <p>Контент не найден.</p>
        <?php endif; ?>
    </main>
    <?php get_sidebar(); ?>
</div>
<?php get_footer(); ?>
```

---

## 4. Активация темы
После создания всех файлов тема появилась в панели управления WordPress. Я добавила файл `screenshot.png` (1200x900) для корректного превью.

> **📸 МЕСТО ДЛЯ СКРИНШОТА №2** > *(Скриншот из "Внешний вид -> Темы", где видна ваша тема с картинкой-превью)*

---

## 5. Результат работы
Тема корректно отображает записи из базы данных, подключает стили и разделяет контент на колонки.

> **📸 МЕСТО ДЛЯ СКРИНШОТА №3** > *(Скриншот главной страницы сайта в браузере)*

---

## 6. Контрольные вопросы
1. **Какие файлы обязательны?** `style.css` и `index.php`.
2. **Как вывести название сайта?** С помощью функции `bloginfo('name')`.
3. **Для чего нужен functions.php?** Для регистрации функций, стилей и скриптов темы.

---

## Вывод
В ходе работы была разработана тема WordPress по стандарту иерархии шаблонов. Изучена работа функций `wp_head()`, `wp_footer()` и основного цикла вывода записей.
```

### Что тебе нужно сделать (мини-чеклист):
1. **Скриншот 1:** Папка с файлами в VS Code.
2. **Скриншот 2:** Список тем в админке WordPress.
3. **Скриншот 3:** Твой сайт в браузере (как он выглядит с этой темой).
4. **Важно:** Если хочешь, чтобы в админке была красивая картинка у темы (как в примере), закинь в папку любой файл `screenshot.png`.
