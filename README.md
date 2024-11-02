# Руководство по установке и настройке Allora Base Topic

Подробное руководство по установке и настройке Allora Base Topic на вашем сервере.

## Содержание
- [1. Подготовка сервера](#1-подготовка-сервера)
- [2. Установка проекта](#2-установка-проекта)
- [3. Настройка Docker и кошельков](#3-настройка-docker-и-кошельков)
- [4. Запуск проекта](#4-запуск-проекта)

## 1. Подготовка сервера

Сначала обновим систему и установим необходимые пакеты:

```bash
# Обновление системы
sudo apt update && sudo apt upgrade -y

# Установка базовых компонентов
sudo apt install -y python3 python3-pip python3-venv git curl nano apt-transport-https ca-certificates software-properties-common

# Установка Docker
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
sudo apt update
sudo apt install -y docker-ce

# Установка Docker Compose
sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
sudo usermod -aG docker ${USER}
```

## 2. Установка проекта

1. Скачайте и распакуйте архив `.rar` на свой компьютер
2. Загрузите распакованную папку на сервер
3. Перейдите в папку проекта:
   ```bash
   cd allora-base-topic
   ```

4. Установите необходимые Python-пакеты:
   ```bash
   pip3 install --upgrade pip setuptools
   pip3 install wheel pyyaml python-dotenv==0.21.0
   pip install --ignore-installed blinker
   pip install --ignore-installed -r requirements.txt
   ```

## 3. Настройка Docker и кошельков

1. Авторизуйтесь в Docker:
   ```bash
   docker login
   ```
   Следуйте инструкциям: перейдите по полученной ссылке в браузере, создайте аккаунт и введите код подтверждения на сервере.

2. Подготовьте seed-фразы кошельков:
   - Получите тестовые токены через кран
   - Информацию о получении seed-фраз и тестовых токенов можно найти здесь: [Allora Node Guide](https://teletype.in/@cryptoforto/allora-node-guide-vDEhWh-kBP0)
   - Альтернативно можно сгенерировать кошельки через allora-chain

3. Создайте файл с seed-фразами:
   ```bash
   nano seed_phrases.txt
   ```
   Вставьте seed-фразы построчно (одна фраза на строку)  
   Сохраните: `CTRL + S`, выйдите: `CTRL + X`

4. Настройте конфигурацию воркеров:
   ```bash
   nano setup_workers.py
   ```
   Основные параметры:
   - `RPC_URL = "https://rpc.ankr.com/allora_testnet"`
   - `COINGECKO_API_KEY = ""` (можно оставить пустым)
   - Настройте другие параметры под ваши требования

## 4. Запуск проекта

Выполните последовательно:
```bash
python3 setup_workers.py
docker compose build
docker compose up -d
```

Для просмотра логов:
```bash
docker-compose logs -f
```

**Примечание**: Успешный запуск подтверждается появлением логов от воркеров в течение 30 минут.

## Поддержка

Если у вас возникли вопросы или проблемы, создайте [Issue](../../issues) в этом репозитории.

