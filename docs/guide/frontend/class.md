# Класс FetchIt

Данный класс отвечает за обработку форм и объявляется в скрипте, который идёт в составе компонента и который регистрируется в теге `<head>`, но с атрибутом `defer`, что позволяет откладывать его выполнение до тех пор, пока вся страница не будет загружена. Это позволяет скрипту не препятствовать загрузке страницы.

Анализаторы скорости загрузки сайта, например PageSpeed Insights, будут довольны. Привет SEO-шникам :wink:.

## Свойства и методы класса

У класса есть несколько статических свойств. Важно! Не у экземпляра, а у самого класса.

## FetchIt.forms

- Тип: `HTMLFormElement[]`

Данное свойство хранит массив `HTMLFormElement` всех обрабатываемых форм.

## FetchIt.instances

- Тип: `Map`

Данное свойство вернёт коллекцию `Map` где храняться экземляры класса `FetchIt`. Получить доступ к классу можно через форму.

- Пример:

```js
const form = document.querySelector('#form');
const fetchit = FetchIt.instances.get(form);
```

## FetchIt.Message

- Тип: `object`

Данное свойство не объявлено, но все экземпляры класса будут пытаться вызвать её методы: `before`, `success`, `error`, `after` и `reset`. Это сделано для удобства встраивания скрипта в вашу вёрстку. Т.е. вы можете объявить данное свойство в своем скрипте, например так:

```js
FetchIt.Message = {
  before() {
    // Показать сообщение до отправки формы
  },
  success(message) {
    // Показать сообщение в случае успешной отправки
  },
  error(message) {
    // Показать сообщение в случае ошибки при отправке
  },
  after(message) {
    // Показать сообщение в любом случае
  },
  reset() {
    // Показать сообщение после сбрасывания данных формы
  },
}
```

Как вы уже заметили, методы `success`, `error` и `after` получают в качестве аргумента сообщение, которое будет возвращено вызываемым сниппетом.

## FetchIt.sanitizeHTML()

- Тип: `function (str: string): string`

Метод класса, который возвращает очищенную от HTML тегов строку переданную в качестве единственного аргумента.

## FetchIt.create()

- Тип: `function (config: object): undefined`

Фабричный метод, который создает экземляры класса FetchIt. Каждый экземпляр класса отвечает за свою форму.

## FetchIt.events

- Тип: `object`

Объект с событиями и их названиями. Может пригодиться при прототипном наследовании.

## Доступ к классу

::: warning Важно!
Надо помнить, для доступа к данному классу нужно дождаться выполнения скрипта в котором он объявлен.
:::

Если у вас файловый скрипт, то достаточно указать атрибут `defer` при его подключении (Напоминаем, что компонент регистрирует скрипт в теге `<head>`).

А в случае с инлайновым скриптом, вам нужно дождаться выполнения скрипта, а это возможно в обработчике события `DOMContentLoaded`. Пример:

```js
window.addEventListener('DOMContentLoaded', () => {
  console.log(FetchIt);
});
```