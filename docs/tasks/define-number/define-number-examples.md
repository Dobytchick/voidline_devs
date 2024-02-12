---
sidebar_position: 2
---

# Примеры реализации

Если ты всё ещё в процессе создания своей версии игры и хочешь создать её без подсказок, не открывай эти блоки.

В любом другом случае, можешь набираться опыта и подсматривать решения 🐌

:::info
Код из сносок ниже в большинстве своём написан новичками, и его реализация может быть далека от идеала.
Чтобы добавить свой код, воспользуйся кнопкой "Отредактировать эту страницу" внизу страницы
:::

----

<details>
  <summary>Lina</summary>
  ```lua
-- создаем функцию генерирующую случайное число в заданном диапазоне
local function GenerateRandomInRange(min, max)
    math.randomseed(os.time())
    local r = math.random(min, max)
    return r
end

-- создаем функцию проверяющую корректность введенного значения
local function IsNoNumber(a)
    local b = tonumber(a)
    if not b then
        repeat
            print("Не является числом. Введите число")
            local c = io.read()
            a = IsNoNumber(c)
        until a ~= nil
        return a
    else
        return b
    end
end

-- создаем функцию сравнения значения компьютера и игрока
function GenerateResult(a, b, l)
    local c = a - b
    local response = {} -- объявляю таблицу и в дальнейшем заполняю в зависимости от условия

    if c == 0 then
        response = {
            answer = string.format("Вы отгадали! Загаданное число: %d", a),
            isWinner = true
        }
    elseif c > 0 then
        response = {
            answer = string.format("Загаданное число больше. Попыток осталось: %d", l - 1),
            isWinner = false,
            life = l - 1
        }
    elseif c < 0 then
        response = {
            answer = string.format("Загаданное число меньше. Попыток осталось: %d", l - 1),
            isWinner = false,
            life = l - 1
        }
    end

    -- тут я беру из таблицы значения, можно конечно и отправлять целую таблицу и это норм, но мне так нравится
    return response.isWinner, response.answer, response.life -- возвращаю 2 значения, первое - победа или нет, второе - ответ программы
    -- [[добавила третье значение, которое возвращает количество жизней игрока с учетом проигрыша]]
end

-- устанавливаем количество жизней
local lives = 3

-- задаем начало диапазона
print("Задайте начало диапазона. Введите число")
local min = io.read()
range_start = IsNoNumber(min)

-- задаем конец диапазона
print("Задайте конец диапазона. Введите число")
local max = io.read()
range_end = IsNoNumber(max)

CompVal = GenerateRandomInRange(range_start, range_end)
print(string.format("Рандомное число в диапазоне %d - %d загадано. Попробуй отгадать!", range_start, range_end))

while lives > 0 do
    local Player = io.read()
    PlayerVal = IsNoNumber(Player)
    local isWinner, response, life = GenerateResult(CompVal, PlayerVal, lives) -- итого, тут мы получаем 2 возвращенных с функции переменных со всем необходимым. Также, в GenerateResult я посылаю количество жизней как 3 аргумент, дабы там их посчтитать.
  -- [[получаем соответсвенно 3 переменных]]

    if isWinner then -- (если в таблице ответа пришло, что игрок победил)
        print(response)
        break
    else
      lives = life -- [[присваиваем жизни согласно расчету]]
        if lives == 0 then
            print(string.format("Вы проиграли. Загаданное число: %d", CompVal))
            return -- после return всё что ниже не будет исполнено, это я для того чтобы убрать else и сократить код
        end
        print(response)
    end
end
  ```
</details>
