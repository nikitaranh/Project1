Создание телеграмм бота через уже созданного бота - BotFather.
Остается написание кода.

Этот код — это простой Telegram-бот, который принимает от пользователя число и возвращает указанное количество простых чисел. Давайте рассмотрим его более подробно.

Основные компоненты кода
Импорт модулей и настройка бота

```
import asyncio
from aiogram import Bot, Dispatcher
from aiogram.filters import Command
from aiogram.types import Message
Здесь импортируются модули, необходимые для работы с Telegram API: 
```
asyncio — для асинхронного выполнения.
Bot, Dispatcher, Command, Message из aiogram, которая помогает создавать ботов для Telegram.
Инициализация бота и диспетчера
```
Функция
bot = Bot(token='8166318669:AAFLayQZfpr-c1n34I2i6J6OEaL--WpBfqg')
dp = Dispatcher()
bot — экземпляр бота с токеном, который предоставляет доступ к Telegram API.
dp — Dispatcher используется для обработки входящих сообщений и маршрутизации команд, чтобы бот реагировал на них.
Функция main() для запуска бота
```
```
async def main():
    await dp.start_polling(bot)
```
Функция main() начинает "опрос" сервера Telegram (polling) для получения новых сообщений и команд.
Функция is_number для проверки, является ли ввод числом
```
Функция проверки
def is_number(s):
    try:
        float(s)
        return True
    except ValueError:
        return False
```
Эта функция проверяет, является ли s числом. Если введённое пользователем значение можно преобразовать в float, функция возвращает True, иначе — False.

Обработчик команды /start
```
Обработка
@dp.message(Command('start'))
async def start1(message: Message):
    await message.answer('Здравствуйте! Введите число для получения списка простых чисел:')
```
Этот обработчик отвечает на команду /start. При вводе этой команды бот отправляет приветственное сообщение и просит пользователя ввести число, чтобы получить список простых чисел.

Обработчик сообщений от пользователя
```
Сообщение
@dp.message()
async def echo_handler(message: Message):
    if is_number(message.text):
        llist = []
        i = 0
        chisl = 2
        limit = int(message.text)
        while i < limit:
            if is_prime(chisl):
                llist.append(chisl)
                i += 1
            chisl += 1
        await message.answer(", ".join(map(str, llist)))
    else:
        await message.answer("Введите число.")
```
Если пользователь вводит число, код начинает находить простые числа до указанного количества limit (которое указывает пользователь).
while-цикл:
В каждом цикле проверяется, является ли chisl простым с помощью функции is_prime.
Если chisl простое, оно добавляется в llist, и счётчик i увеличивается на 1.
chisl увеличивается на 1 в каждой итерации.
После нахождения нужного количества простых чисел бот отправляет пользователю список, разделённый запятыми.
Функция is_prime для проверки числа на простоту
```
Функция:
def is_prime(num):
    if num < 2:
        return False
    for i in range(2, int(num ** 0.5) + 1):
        if num % i == 0:
            return False
    return True
```
Эта функция определяет, является ли num простым числом:
Если num меньше 2, оно не является простым.
Иначе проверка делимости числа num на все числа от 2 до sqrt(num) + 1. Если число делится, оно не является простым.
Запуск бота
```
Заключение
if name == 'main':
    try:
        asyncio.run(main())
    except KeyboardInterrupt:
        print('Exit')
```
Этот код запускает бота, вызывая asyncio.run(main()), если файл выполняется напрямую. KeyboardInterrupt позволяет завершить работу бота через Ctrl + C.
