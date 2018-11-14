# API для підключення Asterisk / FreePBX до Clinica Web

Для підключення власної АТС побудованої на [Asterisk](https://www.asterisk.org/) / [FreePBX](https://www.freepbx.org/) необхідно провести наступні налаштування.

## Створити методи POST для повідомлення про наступні події

- Вхідний дзвінок
- Вихідний дзвінок
- Підняли трубку
- Поклали трубку

### Всі повідомлення повинні відправляти запити на наступну адресу

```markdown
https://example.com/api/Binotel/Push
```

Замість _example.com_ необхідно вказати адресу вашої _CRM Clinica Web_

## Опис методів

### Вхідний дзвінок

Приклад вихідного коду на PHP для відправлення POST запиту

```markdown
$postData = array(
    'didNumber' => '0443334000',
    'externalNumber' => '0670219424',
    'internalNumber' => '801',
    'generalCallID' => '2744830',
    'callType' => '0',
    'companyID' => '3041',
    'requestType' => 'receivedTheCall'
);
```

### Вихідний дзвінок

Приклад вихідного коду на PHP для відправлення POST запиту

```markdown
$postData = array(
    'externalNumber' => '0670219424',
    'internalNumber' => '904',
    'generalCallID' => '2744830',
    'callType' => '1',
    'companyID' => '3041',
    'requestType' => 'receivedTheCall'
);
```

### Підняли трубку

Приклад вихідного коду на PHP для відправлення POST запиту

```markdown
$postData = array(
    'didNumber' => '0442334000',
    'externalNumber' => '0670219424',
    'internalNumber' => '904',
    'generalCallID' => '2744830',
    'callType' => '0',
    'companyID' => '3041',
    'requestType' => 'answeredTheCall'
);
```

### Поклали трубку

Приклад вихідного коду на PHP для відправлення POST запиту

```markdown
$postData = array(
    'generalCallID' => '2744830',
    'billsec' => '35',
    'companyID' => '3041',
    'requestType' => 'hangupTheCall'
);
```

## Опис параметрів

```markdown
- externalNumber - телефонний номер клієнта
- internalNumber - внутрішній номер співробітника / групи в віртуальної АТС
- requestType - тип PUSH запиту (receivedTheCall-надходження дзвінка, answeredTheCall-підняття трубки (відповідь на дзвінок), hangupTheCall- завершення дзвінка)
- generalCallID - головний ідентифікатор дзвінка (унікальне для одного дзвінка)
- callType - тип дзвінка: вхідний - 0, вихідний - 1
- companyID - ідентифікатор компанії (не обов'язково)
- billsec - тривалість розмови
```