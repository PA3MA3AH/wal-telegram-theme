# wal-ayugram-theme

Динамическая тема для Telegram Desktop и AyuGram, которая автоматически подстраивается под цвета твоих обоев через `pywal`.

После смены обоев тема обновляется сама — перезапускать Telegram не нужно, всё меняется «на лету».

---

## Зависимости

| Пакет | Зачем нужен |
| :--- | :--- |
| `python-pywal` | Генерирует цветовую схему из обоев (`colors.json`) |
| `imagemagick` | *Опционально:* Конвертация GIF-обоев в PNG для фона темы |

### Установка зависимостей:

* **Arch Linux / Manjaro:**
    ```bash
    sudo pacman -S python-pywal imagemagick
    ```
* **Ubuntu / Debian:**
    ```bash
    sudo apt install imagemagick
    pip install pywal --break-system-packages
    ```
* **Fedora:**
    ```bash
    sudo dnf install imagemagick
    pip install pywal
    ```

---

## Установка и настройка

### 1. Скопируй скрипт
Помести скрипт в свою локальную папку бинарников и сделай его исполняемым:
```bash
cp wal-ayugram-theme ~/.local/bin/wal-ayugram-theme
chmod +x ~/.local/bin/wal-ayugram-theme

```

### 2. Настрой автозапуск при смене обоев

Создай файл хука `pywal`, который будет срабатывать автоматически каждый раз при обновлении палитры:

```bash
mkdir -p ~/.config/wal
nano ~/.config/wal/postrun

```

Вставь туда следующий код:

```bash
#!/usr/bin/env bash
~/.local/bin/wal-ayugram-theme

```

Сделай хук исполняемым:

```bash
chmod +x ~/.config/wal/postrun

```

*Теперь скрипт будет перезапускаться автоматически при любом вызове `wal -i ...` (из терминала, ваших скриптов, оконного менеджера и т.д.).*

### 3. Сгенерируй тему в первый раз

Если цвета уже сгенерированы — просто запусти скрипт напрямую:

```bash
~/.local/bin/wal-ayugram-theme

```

Или обнови обои через `pywal`:

```bash
wal -i /путь/к/твоим/обоям.jpg

```

---

## Как применить тему (Только один раз)

> ⚠️ **Важно:** Это действие нужно выполнить всего один раз, чтобы связать Telegram с файлом темы. В будущем всё будет обновляться автоматически.

1. Открой файловый менеджер и перейди в папку темы: `~/.local/share/ayugram-theme/`.
2. Перетащи файл `wal.tdesktop-theme` мышкой в Telegram/AyuGram (отправь его **самому себе в Избранное / Saved Messages**).
3. Кликни по отправленному файлу темы прямо внутри чата Telegram и нажмите **«Применить тему»** (Apply theme).

---

## Кастомизация путей

Скрипт по умолчанию ищет текущие обои по симлинку `~/wallpapers/current`. Если у тебя обои лежат в другом месте или ты используешь сторонние форки Telegram (64gram, Kotatogram и т.д.), просто открой скрипт текстовым редактором и измени пути в самом начале файла в секции ` Пути`:

```bash
# Путь к текущим обоям
WALLPAPER_LINK="${HOME}/wallpapers/current"

# Пути к папкам приложений
AYUGRAM_TDATA="${HOME}/.local/share/AyuGram/tdata"
TELEGRAM_TDATA="${HOME}/.local/share/TelegramDesktop/tdata"

```

*Поддерживаемые форматы обоев: `.jpg`, `.png`, `.gif` (для гифок берётся первый кадр).*

---

## Как это работает

1. `wal -i <обои>` анализирует цвета картинки и сохраняет палитру в `~/.cache/wal/colors.json`.
2. Срабатывает скрипт `postrun`, который запускает `wal-ayugram-theme`.
3. Скрипт читает палитру, упаковывает её вместе с обоями в zip-архив формата `.tdesktop-theme` и перезаписывает файлы в папках `tdata`.
4. Telegram/AyuGram мгновенно подхватывают обновлённый файл темы без перезапуска приложения.

---

## Лицензия

Проект распространяется под лицензией [MIT](https://www.google.com/search?q=LICENSE).
