1. Опис сценарію інтеграції
Сценарій:
CSV-файл заявок потрібно перетворити у JSON для внутрішнього API.
Джерело даних:
FTP-сервер, куди щогодини надходять CSV-файли із заявками.
Формат вхідних даних:
CSV
 Приклад:
id,name,email,amount,currency,date
1,Ivan Ivanov,ivan@test.com,100,USD,2026-05-01
2,Petro Petrov,,200,EUR,2026-05-02
Цільова система:
Внутрішній REST API (Orders Service)
Формат вихідних даних:
JSON
Бізнес-логіка:
Перетворити CSV → JSON
Відфільтрувати записи без email
Додати поле status = "NEW"
Конвертувати amount у число
Додати createdAt у форматі ISO
Об’єднати currency + amount у об’єкт price

2. Проєктування трансформації
Вхідна структура:
{
 id: String,
 name: String,
 email: String (optional),
 amount: String,
 currency: String,
 date: String
}
Цільова структура:
{
 orderId: Number,
 customerName: String,
 email: String,
 price: {
   value: Number,
   currency: String
 },
 status: String,
 createdAt: String
}
Мапінг полів: 
JSON поле
orderId 
email
price.value 
price.currency 
createdAt 
CSV поле
id
name
email
amount
curency
date

Обов’язкові поля:
id
name
amount
currency
Необов’язкові:
email
Значення за замовчуванням:
status = "NEW"
email = "unknown@test.com" (якщо потрібно не фільтрувати)
Перетворення типів:
id → Number
amount → Number
date → ISO DateTime

6. Висновок
Які ризики знімає тестування:
некоректна трансформація даних
помилки типів
втрата обов’язкових полів
неправильна логіка фільтрації
Які дефекти залишаються:
проблеми продуктивності
помилки інтеграції з реальним API
некоректні дані у джерелі
Що ще варто перевіряти:
навантажувальні тести
edge cases (великі файли)
retry / error handling
безпека (валідація даних)
