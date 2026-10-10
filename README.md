# 💪 Muscle Legends Script

A customizable **Muscle Legends script for Roblox**, designed to make training and rebirth management easier with a simple, draggable GUI.

## ✨ Features

- 🏋️ **Auto Train** — automatically trains your character.
- ⚡ **Train Speed Controller** — adjust the training interval from 0.05 to 2 seconds.
- 🔄 **Auto Rebirth** — automatically sends rebirth requests.
- 📍 **Auto Barbell** — teleports your character to a configurable target position and attempts to return after respawning.
- ⌨️ **Auto Press E** — repeatedly attempts to simulate the E key.
- 🖥️ **Custom GUI** — draggable menu with individual feature toggles.
- ❌ **Close Button** — closes the menu and stops the script's loops.

## 🚀 How to Use

1. Open the script using a compatible Lua executor.
2. Enable or disable features using the GUI.
3. Adjust the training interval to your preference.
4. Enable Auto Barbell to use the configured target position.
5. Enable Auto Press E to attempt repeated E-key input.

## ⚙️ Configuration

The script includes a configurable target position and training interval.

- Default training interval: `0.30` seconds
- Minimum training interval: `0.05` seconds
- Maximum training interval: `2.00` seconds

**Note:** Executor compatibility may vary. Teleportation, remote calls, and simulated key input depend on the game and executor environment. Very short intervals may increase performance load.

## ⚠️ Disclaimer

This is an unofficial community script and is not affiliated with or endorsed by Roblox or the developers of Muscle Legends. Use it responsibly and at your own risk.

## 📢 Об этом обновлении

В этой версии скрипта **Muscle Legends** добавлена система регулировки интервала Auto Rebirth и улучшено управление автоматическими функциями.

### ✨ Что нового

- ⏱️ **Rebirth Interval Controller** — регулировка интервала перерождения от 2 до 10 секунд с шагом 1 секунда.
- 🎨 **Обновлённое меню** — переключатели функций и удобные регуляторы интервалов.
- 🛠️ **Обработка ошибок** — добавлена обработка ошибок при выполнении запроса Rebirth.

### ⏱️ Настройки Auto Rebirth

- Начальный интервал: **3 секунды**
- Минимальный интервал: **2 секунды**
- Максимальный интервал: **10 секунд**
- Шаг изменения: **1 секунда**

Кнопка **«+ Быстрее»** уменьшает интервал, а **«− Медленнее»** увеличивает его.

### 📝 Примечания

- Некоторые функции зависят от текущей версии игры.
- Слишком частые запросы могут ограничиваться самой игрой.
- Если возникают ошибки, проверьте совместимость используемого инструмента запуска.

---

**Версия:** обновлённая система Auto Rebirth
**Проект:** Muscle Legends Script
