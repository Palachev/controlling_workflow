# GitHub Actions CI/CD Workflows (DevOps Practice)

Этот репозиторий содержит набор **GitHub Actions workflow**, демонстрирующих практические подходы к построению CI/CD пайплайнов: управление порядком выполнения, работа с артефактами, reusable workflows, matrix strategy, контейнеризированные job’ы и безопасную работу с secrets.

Репозиторий используется как **портфель DevOps-практики** и не привязан к конкретному приложению.

---

## Что демонстрируется

В репозитории реализованы следующие DevOps-практики:

* CI/CD пайплайны с этапами **lint → test → build → deploy**
* Управление порядком выполнения job’ов через `needs`
* Кэширование зависимостей для ускорения сборок
* Передача артефактов между job’ами (`upload-artifact` / `download-artifact`)
* Обработка ошибок и отдельные job’ы для отчётов при падениях
* **Reusable workflows** (`workflow_call`) с входными параметрами и выходными результатами
* Matrix strategy (несколько версий Node.js и операционных систем)
* Запуск job’ов в Docker container
* Использование GitHub Environments и Secrets
* E2E-проверки: запуск сервера в CI и ожидание readiness перед тестами
* Аутентификация и публикация образа в Yandex Container Registry (YCR)
* Автоматический деплой на удалённый сервер по SSH
* Обновление сервисов через Docker Compose (docker compose pull и docker compose up -d).

---

## Структура workflow

### continio.yml

**Базовый CI/CD pipeline**

* lint → test → build → deploy
* Кэширование npm-зависимостей
* Сборка и передача артефактов
* Отдельный job для обработки ошибок (`if: failure()`)

Используется как пример классического CI-пайплайна.

---

### execution-flow.yml

**Управление порядком выполнения**

* Демонстрация зависимости job’ов через `needs`
* Передача артефактов между этапами
* Контроль последовательности выполнения CI/CD

---
## ya_deploy.yaml

Workflow реализует end-to-end CI/CD:
- запуск тестов;
- сборка Docker-образа и публикация в Yandex Container Registry;
- деплой на удалённый сервер по SSH с обновлением сервисов через Docker Compose.

Все секреты (OAuth, SSH-ключи) хранятся в GitHub Secrets и не выводятся в логи.

---

### matrix.yml

**Matrix strategy**

* Запуск сборок на разных версиях Node.js и ОС
* Использование `include` и `exclude` для управления комбинациями
* Демонстрация кросс-платформенного CI

---

### deploy.yaml

**Containerized CI job + environments**

* Запуск job’а внутри Docker container (`container: node`)
* Использование GitHub Environments
* Работа с secrets и переменными окружения
* E2E-подход: запуск сервера, ожидание readiness (`wait-on`), затем тесты

---

### reusable.yml

**Reusable Deploy Workflow**

* Переиспользуемый workflow через `workflow_call`
* Приём входных параметров (`inputs`)
* Возврат результата деплоя через `outputs`
* Используется для стандартизации деплоя без копипаста

---

### use-reuse.yml

**Использование reusable workflow**

* Сборка и публикация артефактов
* Вызов `reusable.yml` как шага деплоя
* Чтение результата деплоя через `needs.<job>.outputs`

Демонстрирует связку CI → reusable deploy → результат выполнения.

---

## Безопасность

* Секреты хранятся в **GitHub Secrets / Environment Secrets**
* Значения секретов не логируются
* Используется принцип минимально необходимого доступа

---

## Назначение репозитория

Этот репозиторий предназначен для:

* демонстрации практических навыков DevOps
* изучения возможностей GitHub Actions
* использования в качестве портфеля для Junior DevOps-позиций

---

## Используемые технологии

* GitHub Actions
* Docker (container jobs)
* Node.js / npm
* Git
* Linux
