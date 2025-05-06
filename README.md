# Сборка и установка прошивки для клавиатуры на ZMK

## 1. Установка зависимостей

### macOS

#### 1.1 Проверка Python
```sh
which python3 || echo 'Python3 не найден'
python3 --version || echo 'Python3 не найден'
```
Если python3 не найден:
```sh
brew install python
```

#### 1.2 Проверка pipx
```sh
which pipx || echo 'pipx не найден'
pipx --version || echo 'pipx не найден'
```
Если pipx не найден:
```sh
brew install pipx
```

#### 1.3 Проверка наличия в PATH (если после установки не работает)
```sh
# Найти путь к python3 и pipx
find /usr/local/bin /opt/homebrew/bin /usr/bin -name python3 2>/dev/null
find /usr/local/bin /opt/homebrew/bin /usr/bin -name pipx 2>/dev/null

# Добавить в PATH (пример для bash/zsh)
export PATH="/usr/local/bin:$PATH"
# или
export PATH="/opt/homebrew/bin:$PATH"
```

#### 1.4 Установка west и cmake
```sh
pipx install west
brew install cmake
pipx ensurepath
```

### Linux (Ubuntu/Debian)

```sh
sudo apt update
sudo apt install python3 python3-pip cmake
python3 -m pip install --user pipx
python3 -m pipx ensurepath
pipx install west
```

### Windows

1. Установите Python с https://www.python.org/downloads/
2. Установите pipx: `python -m pip install --user pipx`
3. Добавьте pipx в PATH (см. вывод команды установки)
4. Установите west: `pipx install west`
5. Установите CMake: https://cmake.org/download/
6. Добавьте CMake в PATH при установке

---

## 2. Настройка окружения

```sh
# Инициализация проекта (один раз)
west init -l config
west update
west zephyr-export
```

> Если команда `west` не найдена — убедитесь, что pipx и Python добавлены в PATH.

---

## 3. Сборка прошивки

### Левая половина:
```sh
west build -s zmk/app -b nice_nano_v2 -- -DSHIELD="sofle_left nice_view_adapter nice_view"
```

### Правая половина:
```sh
west build -s zmk/app -b nice_nano_v2 -- -DSHIELD="sofle_right nice_view_adapter nice_view"
```

- После сборки файл прошивки будет лежать в `build/zephyr/zmk.uf2`.
- Скопируйте его на устройство (обычно через USB, как на флешку).

---

## 4. .gitignore

Добавьте в `.gitignore`:
```
build/
zephyr/
modules/
bootloader/
tools/
.ninja_deps
.ninja_log
.cmake/
CMakeFiles/
CMakeCache.txt
*.pyc
*.o
*.elf
*.bin
*.hex
*.uf2
```

---

## 5. Полезные ссылки
- Документация ZMK: https://zmk.dev/docs/development/setup/
- Документация Zephyr: https://docs.zephyrproject.org/latest/getting_started/index.html 