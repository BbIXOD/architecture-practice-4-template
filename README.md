### Лабораторна робота №4 з архітектури ПЗ

---

#### Тема: Горизонтальне масштабування програмних систем.

Дана лабораторна робота складається з 3-х частин:
- [реалізація алгоритму балансування навантаження + покриття його юніт-тестами;](https://github.com/roman-mazur/architecture-practice-4-template/commit/0e956c9aa175ca7bbdf379282035dd13d527fb21)
- [реалізація інтеграційних тестів для балансувальника;](https://github.com/roman-mazur/architecture-practice-4-template/commit/67adc26b27852df5cbe292d670168e61a2d4746f)
- [налаштування безперервної інтеграції із запуском інтеграційних тестів.](https://github.com/roman-mazur/architecture-practice-4-template/commit/1aa58166de209b4b27977001c9fd5cebd79a03dc)

---
#### Реалізація алгоритму балансування навантаження + покриття його юніт-тестами:
Реалізація балансувальника в репозиторії відправляє всі запити (за допомогою функції
`forward`) на перший сервер зі списку доступних.

Реалізація балансувальника повинна використувати алгоритм `"Найменша кількість з’єднань"` для вибору сервера зі списку “здорових” серверів. 
Сервер називається “здоровим”, якщо він успішно відповідає на HTTP запит /health (для визначення
“здоров’я” сервера ви можете користуватися вже імплементованою функцією `health`).

---
#### Реалізація інтеграційних тестів для балансувальника
Завданням інтеграційного теста є перевірка того, що вже зібраний компонент (а саме балансувальник) може успішно бути запущеним та використовує потрібний алгоритм.

Головним завданням інтеграційного теста у даній роботі є відправка певної кількості запитів на балансувальник та перевірка, що вони були доставлені на різні сервера.
З даною метою, балансувальник запускається у режимі, в якому він додає інформацію про те, від якого сервера прийшла відповідь у вигляді додаткового заголовка `(“lb-from”)`. 

Для даного завдання, на додачу до запуску серверів та балансувальника нампотрібно також запустити тести. 
Для цього `docker-compose.test.yaml` доповнює визначення сервісів тестовим контейнером та змінює параметри запуска балансувальника.

---
#### Налаштування безперервної інтеграції із запуском інтеграційних тестів
Запуск збірки та інтеграційних тестів був реалізований за допомогою `Github Actions`. 
CI сервер має запускати docker-compose точно так, як вручну у 2-гому завданні.

`docker-compose` буде запускати збірку образів відповідних контейнерів, ті, в свою чергу
запускатимуть `go build` та `go test`, який збиратиме бінарники компонентів та запускатиме
юніт-тести).
