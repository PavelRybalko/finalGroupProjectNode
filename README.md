### Команды:

- `npm start` &mdash; старт сервера в режиме production
- `npm run start:dev` &mdash; старт сервера в режиме разработки (development)
- `npm run lint` &mdash; запустить выполнение проверки кода с eslint, необходимо выполнять перед каждым PR и исправлять все ошибки линтера
- `npm run lint:fix` &mdash; та же проверка линтера, но с автоматическими исправлениями простых ошибок
- `npm run test` &mdash; запустить тесты
- `npm run test:watch` &mdash; запустить тесты в режиме WATCH
- `npm run test:coverage` &mdash; запустить покрытие тестами

### @ POST /auth/register

- Получает `email` и `password`
- Делает валидацию с помощью Joi
- В случае успешной валидации в модели User создает пользователя по данным которые прошли валидацию и отправляет письмо для верификации почты на введённую почту.
- Если почта уже используется кем-то другим, возвращает ошибку `Conflict`.

### @ POST /auth/login

- Получает `email` и `password`
- Делает валидацию с помощью Joi
- Без перехода по ссылке верификации из письма не авторизирует пользователя.
- Если пароли совпадают и верификация пройдена: создает токен, сохраняет в текущем юзере и возвращает Успешный ответ.
- Если пароль или имейл неверный, возвращает ошибку `'Email or password is wrong'`.
- Если верификация почты не сделана, возвращает ошибку `'Please verify your email'`.

### @ POST /auth/logout

- Не получает body
- Если пользователя не сущестует возвращает `Ошибку Unauthorized`.
- В противном случае, удалит токен в текущем юзере и возвращает Успешный ответ.

### @ PATCH /users/avatars

- Получает картинку файлом(до 2Мб) из формы
- Делает валидацию с помощью Joi
- Cохраняет картинку во временную папку tmp
- Обрезает её до 250х250 и загружает на cloudinary
- Удаляет старую картинку и файл из временной папки
- Возвращает обьект со ссылкой в json-формате со статусом 200

### @ GET /users/current

- Не получает body
- Ищет информацию по токену из заголовка
- Если пользователя не сущестует возвращает Ошибку `Unauthorized`.
- В противном случае возвращает Успешный ответ

### @ GET /users/verify/:verificationToken

- Получает параметр verificationToken
- Обрабатывает переход по ссылке верификации из письма
- Сбрасывает verificationToken
- При положительном результате возвращает ответ `Status: 200 OK, "Verification successful!"`
- В противном случае возвращает `Status: 400 BAD_REQUEST, "Your verification token is not valid"`
