# Проект YaMDb

[![api_yamdb workflow](https://github.com/NickLanden/yamdb_final/actions/workflows/yamdb_workflow.yml/badge.svg)](https://github.com/NickLanden/yamdb_final/actions/workflows/yamdb_workflow.yml)

## Доступные адреса: 
* [http://51.250.22.129](http://51.250.22.129) - API
* [http://51.250.22.129/admin](http://51.250.22.129/admin) - Админка
* [http://51.250.22.129/redoc](http://51.250.22.129/redoc) - Документация

* * *
## Описание
- Проект YaMDb собирает отзывы (Review) пользователей на произведения (Titles). Произведения делятся на категории: «Книги», «Фильмы», «Музыка». Список категорий (Category) может быть расширен (например, можно добавить категорию «Изобразительное искусство» или «Ювелирка»).

- Сами произведения в YaMDb не хранятся, здесь нельзя посмотреть фильм или послушать музыку.

- В каждой категории есть произведения: книги, фильмы или музыка. Например, в категории «Книги» могут быть произведения «Винни Пух и все-все-все» и «Марсианские хроники», а в категории «Музыка» — песня «Давеча» группы «Насекомые» и вторая сюита Баха. Произведению может быть присвоен жанр (Genre) из списка предустановленных (например, «Сказка», «Рок» или «Артхаус»). Новые жанры может создавать только администратор.

- Благодарные или возмущённые читатели оставляют к произведениям текстовые отзывы (Review) и выставляют произведению рейтинг (оценку в диапазоне от одного до десяти). Из множества оценок высчитывается средняя оценка произведения.
* * *
## AUTH
- Отправление confirmation_code на переданный email
- Получение JWT-токена в обмен на email и confirmation_code
* * *
## USERS
- Получить список всех пользователей
- Создание пользователя
- Получить пользователя по username
- Изменить данные пользователя по username
- Удалить пользователя по username
- Получить данные своей учетной записи
- Изменить данные своей учетной записи
* * *
## CATEGORIES
- Получить список всех категорий
- Создать категорию
- Удалить категорию
* * *
## GENRES
- Получить список всех жанров
- Создать жанр
- Удалить жанр
* * *
## TITLES
- Получить список всех объектов
- Создать произведение для отзывов
- Информация об объекте
- Обновить информацию об объекте
- Удалить произведение
* * *
## REVIEWS
- Получить все отзывы
- Получить отзыв по id
- Добавить отзыв
- Изменить отзыв
- Удалить отзыв
* * *
## COMMENTS
Получить все комментарии к отзыву
- Получить комментарий к отзыву
- Добавить комментарий
- Обновить комментарий
- Удалить комментарий
## Пользовательские роли
- **Аноним** — может просматривать описания произведений, читать отзывы и комментарии.
- **Аутентифицированный пользователь (user)** — может читать всё, как и Аноним, может публиковать отзывы и ставить оценки произведениям (фильмам/книгам/песенкам), может комментировать отзывы; может редактировать и удалять свои отзывы и комментарии, редактировать свои оценки произведений. Эта роль присваивается по умолчанию каждому новому пользователю.
- **Модератор (moderator)** — те же права, что и у Аутентифицированного пользователя, плюс право удалять и редактировать любые отзывы и комментарии.
- **Администратор (admin)** — полные права на управление всем контентом проекта. Может создавать и удалять произведения, категории и жанры. Может назначать роли пользователям.
- **Суперюзер Django** должен всегда обладать правами администратора, пользователя с правами admin. Даже если изменить пользовательскую роль суперюзера — это не лишит его прав администратора. Суперюзер — всегда администратор, но администратор — не обязательно суперюзер.
* * *
## Самостоятельная регистрация новых пользователей
- Пользователь отправляет POST-запрос с параметрами email и username на эндпоинт /api/v1/auth/signup/.
![979270f9037675502d33cf38137d510e.png](:/33a29bdae67544dd98def3937d0a7312)
- Сервис YaMDB отправляет письмо с кодом подтверждения (confirmation_code) на указанный адрес email.
![b5719e72ff578a13ad099a1cca24aca8.png](:/f57d16d2dede4a92ac1d738adc4b7135)
- Пользователь отправляет POST-запрос с параметрами username и confirmation_code на эндпоинт /api/v1/auth/token/, в ответе на запрос ему приходит token (JWT-токен).
- В результате пользователь получает токен и может работать с API проекта, отправляя этот токен с каждым запросом.
![006e25c9cf08f51383cb0e021a6ec350.png](:/c15614cc09b54eaea81f1038ad57b5c4)
- После регистрации и получения токена пользователь может отправить PATCH-запрос на эндпоинт /api/v1/users/me/ и заполнить поля в своём профайле (описание полей — в документации).
![1131203cb16675b9f0c9f69dccfd9bb0.png](:/4b659ed0849a44359bd3ac2bf7454f52)
* * *
## Создание пользователя администратором
- Пользователя может создать администратор — через админ-зону сайта или через POST-запрос на специальный эндпоинт api/v1/users/ (описание полей запроса для этого случая — в документации). В этот момент письмо с кодом подтверждения пользователю отправлять не нужно.
![5905f89eb38cc2ca222cc41efaa64831.png](:/ab1a1171cbe94960b9b00965828813c9)
- После этого пользователь должен самостоятельно отправить свой email и username на эндпоинт /api/v1/auth/signup/ , в ответ ему должно прийти письмо с кодом подтверждения.
- Далее пользователь отправляет POST-запрос с параметрами username и confirmation_code на эндпоинт /api/v1/auth/token/, в ответе на запрос ему приходит token (JWT-токен), как и при самостоятельной регистрации.
* * *
## Ресурсы API YaMDb
- Ресурс auth: аутентификация.
- Ресурс users: пользователи.
- Ресурс titles: произведения, к которым пишут отзывы (определённый фильм, книга или песенка).
- Ресурс categories: категории (типы) произведений («Фильмы», «Книги», «Музыка»).
- Ресурс genres: жанры произведений. Одно произведение может быть привязано к нескольким жанрам.
- Ресурс reviews: отзывы на произведения. Отзыв привязан к определённому произведению.
- Ресурс comments: комментарии к отзывам. Комментарий привязан к определённому отзыву.

Каждый ресурс описан в документации: указаны эндпоинты (адреса, по которым можно сделать запрос), разрешённые типы запросов, права доступа и дополнительные параметры, если это необходимо.
