#  Лабораторная работа: CMS WordPress

---


##  Цель работы

Изучение архитектуры CMS WordPress и получение практических навыков создания и настройки темы, страниц, меню и функциональности сайта.

---

##  Используемое окружение

- CMS: WordPress (локальный сервер XAMPP)
- Редактор кода: VS Code
- Тема: кастомная (Movie Theme)
- Язык: PHP, HTML, CSS
- Плагины: Classic Editor, Contact Form 7

---

##  Этапы выполнения работы

---

### Шаг 1 — Подготовка среды

- Установлена локальная среда WordPress (XAMPP)
- Проект размещён по пути: /wp_lab2/

- Включён режим отладки:

```php
define('WP_DEBUG', true);
```

### Шаг 2 — Создание темы оформления
Создана пользовательская тема:

wp-content/themes/usm-theme/

Структура темы
usm-theme/
├── style.css
├── index.php
├── functions.php
├── header.php
├── footer.php
├── sidebar.php
├── single.php
├── page.php
└── screenshot.png

<img width="188" height="371" alt="image" src="https://github.com/user-attachments/assets/b10628f5-3958-42bb-89ee-ca6da9434466" />

### Шаг 3 — Стили темы (style.css)

Файл style.css содержит:

метаданные темы (обязательный блок WordPress)
основные стили оформления интерфейса
адаптивную сетку карточек фильмов
Основные особенности дизайна:
светлая цветовая схема
карточный интерфейс (movie cards)
CSS Grid для отображения фильмов
hover-анимации
адаптивная верстка

```
/*
Theme Name: USM Theme
Author: Irina Cristeva
Version: 1.0
Description: Simple WordPress theme for lab work
*/

html, body {
    height: 100%;
    margin: 0;
    font-family: Arial, sans-serif;
    background: #f5f5f5;
}

body {
    display: flex;
    flex-direction: column;
}

header nav ul {
    list-style: none;
    display: flex;
    gap: 15px;
    padding: 0;
}

header nav a {
    text-decoration: none;
    color: #222;
    font-weight: bold;
}

header nav a:hover {
    color: #4caf50;
}

.container {
    width: 80%;
    margin: auto;
    flex: 1;
}

header {
    background: #e8f5e9;
    padding: 15px 0;
}

header .container {
    display: flex;
    justify-content: space-between;
    align-items: center;
}

nav ul {
    list-style: none;
    display: flex;
    gap: 20px;
    padding: 0;
    margin: 0;
}

nav ul li a {
    text-decoration: none;
    color: #222;
    font-weight: bold;
}

nav ul li a:hover {
    opacity: 0.7;
}

.post {
    background: white;
    padding: 15px;
    margin: 10px 0;
    border-radius: 8px;
}

.movies-grid {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 20px;
}

.movie-card {
    background: white;
    padding: 10px;
    border-radius: 10px;
    text-align: center;
    transition: transform 0.2s ease, box-shadow 0.2s ease;
}

.movie-card:hover {
    transform: translateY(-5px);
    box-shadow: 0 5px 15px rgba(0,0,0,0.1);
}

.movie-card img {
    width: 100%;
    border-radius: 8px;
}

.movie-card a {
    text-decoration: none;
    color: black;
}

.movie-page {
    text-align: center;
}

.movie-big img {
    width: 300px;
    border-radius: 10px;
    margin-bottom: 20px;
}

.movie-content {
    max-width: 600px;
    margin: auto;
}

.layout {
    display: block;
    gap: 20px;
    align-items: flex-start;
}

.movies-grid {
    flex: 3;
}

.sidebar {
    flex: 1;
    background: white;
    padding: 15px;
    border-radius: 10px;
    margin-top: 20px;
}

footer {
    background: #e8f5e9;
    text-align: center;
    padding: 15px;
    margin-top: 30px;
}
```
<img width="2408" height="1310" alt="Снимок экрана 2026-04-01 221442" src="https://github.com/user-attachments/assets/a416b387-6b64-47b3-bc51-086dab6b51c1" />


### Шаг 4 — functions.php

Файл отвечает за подключение и расширение возможностей темы.

```php
<?php

function theme_setup() {
    add_theme_support('title-tag');
    add_theme_support('post-thumbnails');

    register_nav_menus([
        'primary' => 'Primary Menu'
    ]);
}
add_action('after_setup_theme', 'theme_setup');

function theme_styles() {
    wp_enqueue_style('style', get_stylesheet_uri());
}
add_action('wp_enqueue_scripts', 'theme_styles');

function modify_query($query) {
    if (!is_admin() && $query->is_main_query()) {
        $query->set('orderby', 'title');
        $query->set('order', 'ASC');
    }
}
add_action('pre_get_posts', 'modify_query');

?>
```

Реализовано:
подключение стилей темы
регистрация меню навигации
поддержка миниатюр записей
изменение сортировки записей

<img width="613" height="779" alt="Снимок экрана 2026-04-01 221420" src="https://github.com/user-attachments/assets/2285168c-0d4d-4f2c-9199-38cb7ac89559" />


### Шаг 5 — header.php
Файл отвечает за верхнюю часть сайта (шапку и меню).

```php
<!DOCTYPE html>
<html>
<head>
    <title><?php wp_title(); ?></title>
    <?php wp_head(); ?>
</head>

<body>

<header>
    <div class="container">

        <h2>🎬 My Movie Theme</h2>

        <nav>
            <?php
            wp_nav_menu([
                'theme_location' => 'primary',
                'container' => false
            ]);
            ?>
        </nav>

    </div>
</header>
```
<img width="1641" height="113" alt="image" src="https://github.com/user-attachments/assets/03f8d741-17a0-4cca-8214-4d599915052a" />


### Шаг 6 — Основная логика WordPress
В теме используются стандартные шаблоны:

index.php — основной fallback-шаблон
single.php — отдельная запись
page.php — статическая страница
header.php — шапка сайта
footer.php — подвал сайта
Подключение частей шаблона:
```
get_header();
get_footer();
get_sidebar();
```


### Шаг 7 — Структура сайта

В рамках работы создан сайт — мини-каталог фильмов.

Реализовано:

главная страница с сеткой фильмов
страницы "О нас"
карточки фильмов с изображениями и описанием
единый стиль оформления

<img width="1984" height="1300" alt="Снимок экрана 2026-04-01 220028" src="https://github.com/user-attachments/assets/25a4e252-e7d8-4434-bf43-08d9ecd80ef0" />
<img width="1927" height="1305" alt="Снимок экрана 2026-04-01 220015" src="https://github.com/user-attachments/assets/14beb41c-ef85-4569-9169-03a3ad11d8bc" />
<img width="1968" height="1302" alt="Снимок экрана 2026-04-01 215937" src="https://github.com/user-attachments/assets/5ce8fb24-062b-4101-bf1d-05fdde7675fc" />
<img width="2046" height="1318" alt="Снимок экрана 2026-04-01 215952" src="https://github.com/user-attachments/assets/ecdf32e9-830d-4561-bbae-6f7f79117898" />
<img width="1960" height="1304" alt="Снимок экрана 2026-04-01 220003" src="https://github.com/user-attachments/assets/57095f8d-6dd1-43ca-84be-ddf62c313a49" />


### Шаг 8 — Навигация

Создано меню:

Главная
О нас

Меню назначено через админ-панель WordPress:

Внешний вид → Меню

<img width="613" height="779" alt="Снимок экрана 2026-04-01 221420" src="https://github.com/user-attachments/assets/3168bd88-c494-4663-aa88-35aefb5642e8" />


### Шаг 9 — Плагины

Установлены и протестированы:

Classic Editor — классический редактор записей
Contact Form 7 — форма обратной связи

### В ходе лабораторной работы была изучена архитектура CMS WordPress и реализована кастомная тема оформления.

В результате:

создана структура темы WordPress
подключены стили и функциональность через functions.php
реализована навигация и страницы сайта
установлены и протестированы плагины
проверена работоспособность сайта



