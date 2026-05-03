4. Тестовий контур (MUnit) 
Тест 1 — Happy Path
Мета: перевірити коректну трансформацію
Вхід:
1,Ivan Ivanov,ivan@test.com,100,USD,2026-05-01
Перевірка:
всі поля присутні
amount → number
status = NEW
MUnit механізм:
assert-equals
Очікувано:
orderId = 1
price.value = 100
Тест 2 — Відсутнє необов’язкове поле
Мета: перевірити фільтрацію
Вхід:
 email = null

Перевірка:
запис НЕ потрапляє у вихід
MUnit:
assert-that (size = 0)
Тест 3 — Некоректний формат поля
Мета: перевірити обробку amount
Вхід:
 amount = "abc"
Перевірка:
значення = 0
MUnit:
assert-equals
Тест 4 — Перевірка моків
Мета: перевірити, що зовнішній API не викликається реально
Перевірка:
HTTP request замоканий
MUnit:
mock-when
verify-call

5. AAA-структура тесту
<munit:test name="test-happy-path">

    <!-- Arrange -->
    <munit:set-event payload="test-input.csv"/>

    <!-- Act -->
    <flow-ref name="csv-to-json-flow"/>

    <!-- Assert -->
    <munit-tools:assert-that 
        expression="#[payload[0].orderId]" 
        is="#[equalTo(1)]"/>

</munit:test>
