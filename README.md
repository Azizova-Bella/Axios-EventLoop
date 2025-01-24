# Axios и Event Loop в JavaScript

В этом документе рассмотрены две важные темы в JavaScript: работа с библиотекой Axios для выполнения HTTP-запросов и понятие Event Loop (цикла событий), которое лежит в основе обработки асинхронного кода.

---
![image](https://github.com/user-attachments/assets/8f83c61b-6c18-4bf6-96b5-4d6fbaaef963)

## Axios
Axios — это популярная библиотека для выполнения HTTP-запросов в JavaScript, поддерживающая как браузеры, так и Node.js. Она позволяет отправлять запросы на сервер и обрабатывать ответы, упрощая взаимодействие с API.

### Установка

Для начала необходимо установить Axios. Вы можете использовать npm или yarn:

```bash
npm install axios
```

Или:

```bash
yarn add axios
```

### Основные возможности Axios

1. **GET-запрос:**
   ```javascript
   const axios = require('axios');

   axios.get('https://api.example.com/data')
     .then(response => {
       console.log(response.data);
     })
     .catch(error => {
       console.error('Ошибка:', error);
     });
   ```

2. **POST-запрос:**
   ```javascript
   const axios = require('axios');

   axios.post('https://api.example.com/data', {
       key1: 'value1',
       key2: 'value2'
     })
     .then(response => {
       console.log(response.data);
     })
     .catch(error => {
       console.error('Ошибка:', error);
     });
   ```

3. **Обработка заголовков:**
   ```javascript
   axios.get('https://api.example.com/data', {
     headers: {
       'Authorization': 'Bearer YOUR_TOKEN'
     }
   })
   .then(response => {
     console.log(response.data);
   })
   .catch(error => {
     console.error('Ошибка:', error);
   });
   ```

4. **Настройка клиента Axios:**
   ```javascript
   const client = axios.create({
     baseURL: 'https://api.example.com',
     timeout: 5000,
     headers: { 'Content-Type': 'application/json' }
   });

   client.get('/data')
     .then(response => {
       console.log(response.data);
     })
     .catch(error => {
       console.error('Ошибка:', error);
     });
   ```

### Преимущества Axios
- Простота использования.
- Поддержка автоматической обработки JSON.
- Встроенная обработка ошибок.
- Возможность настройки параметров запроса и базового URL.

---

![image](https://github.com/user-attachments/assets/bdb5ca49-1150-453a-9538-255552005e97)


## Event Loop

**Event Loop** (“Цикл событий”) — это механизм в JavaScript, который обеспечивает выполнение операций ввода/вывода и асинхронного кода, несмотря на однопоточность языка.

### Как работает Event Loop
JavaScript — это однопоточный язык. Это означает, что одновременно может выполняться только одна операция. Однако благодаря Event Loop JavaScript может обрабатывать асинхронный код, такой как таймеры, обработка HTTP-запросов, событий DOM и т. д.

Event Loop состоит из следующих компонентов:
![image](https://github.com/user-attachments/assets/05a08484-a286-43a8-abdd-b3c381c326e3)

1. **Call Stack (Стек вызовов):** Содержит функции, которые выполняются последовательно. Когда функция завершается, она удаляется из стека.
2. **Task Queue (Очередь задач):** Здесь хранятся функции обратного вызова (например, из `setTimeout` или HTTP-запросов), ожидающие выполнения.
3. **Web APIs:** Отвечают за выполнение асинхронных операций, таких как HTTP-запросы, таймеры или события DOM.

### Пример работы Event Loop

```javascript
console.log('Начало');

setTimeout(() => {
  console.log('Асинхронная операция');
}, 2000);

console.log('Конец');
```

**Результат выполнения:**
```
Начало
Конец
Асинхронная операция
```


### Стадии Event Loop

1. **Timers:** Выполняются обратные вызовы из `setTimeout` и `setInterval`.
2. **Pending Callbacks:** Обрабатываются операции ввода/вывода.
3. **Idle, Prepare:** Внутренние процессы движка.
4. **Poll:** Ожидание новых событий ввода/вывода.
5. **Check:** Выполнение обратных вызовов из `setImmediate`.
6. **Close Callbacks:** Обработка закрывающих обратных вызовов (например, `socket.on('close')`).

### Пример с Promise

```javascript
console.log('Начало');

setTimeout(() => console.log('setTimeout'), 0);

Promise.resolve().then(() => console.log('Promise'));

console.log('Конец');
```

**Результат выполнения:**
```
Начало
Конец
Promise
setTimeout
```

### Объяснение
1. `console.log('Начало')` и `console.log('Конец')` выполняются сразу, так как это синхронный код.
2. `Promise.resolve().then(...)` добавляет функцию в очередь микрозадач (microtasks).
3. `setTimeout` добавляет функцию в очередь задач (macrotasks).
4. Микрозадачи выполняются до макрозадач, поэтому `Promise` выводится перед `setTimeout`.

---

## Заключение
Axios облегчает взаимодействие с внешними API, а понимание Event Loop помогает эффективно работать с асинхронным кодом. Овладение этими инструментами делает разработчика более уверенным и продуктивным в JavaScript.

