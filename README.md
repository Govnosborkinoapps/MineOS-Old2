Инструкция по установке MineOS:
-----------------------------------------------------------

Перед установкой убедитесь, что ваш компьютер соответствует минимальной конфигурации:



# Установка
Команда Установки

    wget https://raw.githubusercontent.com/Govnosborkinoapps/MineOS-Old2/master/Installer.lua

Компьютер будет проанализирован на соответствие минимальным требованиям, после чего перед вами откроется симпатичный установщик. Вы можете изменить некоторые системные опции на свой вкус, и, согласившись с лицензионным соглашением, установить MineOS:

![](https://i.imgur.com/tN9ua0J.gif)

Как создавать приложения MineOS
-----------------------------------------------------------

Каждое приложение MineOS - это директория с расширением **.app**, имеющая следующее содержимое:

![](https://i.imgur.com/o6uiNBJ.png)

Файл **Main.lua** исполняется при запуске приложения, а **Icon.pic** используется для отображения иконки на рабочем столе и в проводнике. Самый простой способ создать приложение - это кликнуть на соответствующую опцию в контекстом меню:

![](https://i.imgur.com/S16oFce.png)

Вам будет предложено выбрать имя вашего приложения, а также его иконку. Если иконка не выбрана, то будет использована системная. Для изменения исходного кода приложения достаточно отредактировать файл **Main.lua**. В примере ниже мы будем использовать методы системных библиотек **[GUI](https://github.com/IgorTimofeev/OpenComputers/blob/master/Documentation/GUI.md)**, **[MineOSInterface](https://github.com/IgorTimofeev/OpenComputers/blob/master/Documentation/MineOSInterface.md)** и **[doubleBuffering](https://github.com/IgorTimofeev/OpenComputers/blob/master/Documentation/doubleBuffering.md)**, поэтому базовое ознакомление с ними крайне рекомендуется:

```lua

local GUI = require("GUI")
local buffer = require("doubleBuffering")
local MineOSInterface = require("MineOSInterface")

------------------------------------------------------------------------------------------------------

local mainContainer, window = MineOSInterface.addWindow(MineOSInterface.filledWindow(1, 1, 88, 26, 0xF0F0F0))

-- Создаем лейаут для автоматического расположения элементов интерфейса
local layout = window:addChild(GUI.layout(1, 1, window.width, window.height, 1, 1))
-- Создаем текстовый лейбл, в котором будем отображать количество нажатий на кнопку
local label = layout:addChild(GUI.label(1, 1, window.width, 1, 0x0, "")):setAlignment(GUI.alignment.horizontal.center, GUI.alignment.vertical.top)
-- Создаем саму кнопку
local button = layout:addChild(GUI.roundedButton(1, 1, 32, 3, 0xBBBBBB, 0xFFFFFF, 0x999999, 0xFFFFFF, "Нажми на меня"))

-- Создаем callback-функцию, вызываемую при нажатии на кнопку
local counter = 0
button.onTouch = function()
	label.text = "Число нажатий: " .. counter
	counter = counter + 1

	mainContainer:draw()
	buffer.draw()
end

-- Разово обновляем текстовые данные лейбла
button.onTouch()
```

Вуаля! Ваше первое оконное приложение готово:

![](https://i.imgur.com/vNhLcbX.gif)
