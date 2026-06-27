# Multi-K8s

[![JavaScript](https://img.shields.io/badge/JavaScript-ES6-yellow.svg)](https://www.javascript.com/)
[![Docker](https://img.shields.io/badge/Docker-20.10+-blue.svg)](https://www.docker.com/)
[![Kubernetes](https://img.shields.io/badge/Kubernetes-1.24+-blue.svg)](https://kubernetes.io/)

## Описание

Этот проект является учебным примером по развертыванию распределенного приложения в среде Docker и Kubernetes. Создан для демонстрации ключевых концепций оркестрации контейнеров и управления микросервисами в кластере.

Проект демонстрирует:
- Контейнеризацию приложений с помощью Docker.
- Развертывание многокомпонентного приложения в Kubernetes.
- Взаимодействие между сервисами (клиент, сервер, воркер).
- Использование конфигурационных файлов (манифестов) для управления ресурсами K8s.
- Основы CI/CD с использованием GitHub Actions.

## Архитектура и технологии

Проект состоит из следующих компонентов:

| Компонент | Описание | Технологии |
| :--- | :--- | :--- |
| **Client** | Пользовательский интерфейс (фронтенд). Отображает данные, полученные от сервера. | JavaScript, HTML, CSS |
| **Server** | Бэкенд-сервис, обрабатывающий запросы от клиента и взаимодействующий с воркером. | JavaScript (Node.js) |
| **Worker** | Сервис для выполнения фоновых задач (например, обработка данных, вычисления). | JavaScript (Node.js) |
| **Kubernetes (k8s)** | Оркестратор контейнеров для автоматизации развертывания, масштабирования и управления. | Kubernetes, Minikube (для локальной разработки) |
| **GitHub Actions** | CI/CD пайплайн для автоматической сборки и деплоя. | GitHub Actions, Docker |

**Основные технологии:**
- **Язык:** JavaScript, HTML, CSS
- **Контейнеризация:** Docker
- **Оркестрация:** Kubernetes (манифесты в папке `/k8s`)
- **Взаимодействие:** REST API (между клиентом, сервером и воркером)
- **CI/CD:** GitHub Actions (`.github/workflows`)

## Установка и запуск

### Предварительные требования
- Установленные **Docker** и **Kubernetes** (например, [Minikube](https://minikube.sigs.k8s.io/docs/start/) или [Docker Desktop](https://www.docker.com/products/docker-desktop/) с включенным Kubernetes).
- Установленный `kubectl` для управления кластером.

### Инструкция по запуску

1.  **Клонируйте репозиторий:**
    ```bash
    git clone https://github.com/TheGreenUp/multi-k8s.git
    cd multi-k8s
    ```

2.  **Соберите Docker-образы для каждого сервиса (опционально, если они не собраны автоматически):**
    ```bash
    docker build -t multi-k8s-client ./client
    docker build -t multi-k8s-server ./server
    docker build -t multi-k8s-worker ./worker
    ```
    > **Примечание:** Если используются уже опубликованные образы, этот шаг можно пропустить.

3.  **Примените манифесты Kubernetes для развертывания всех компонентов:**
    ```bash
    kubectl apply -f k8s/
    ```
    Эта команда создаст все необходимые Deployment, Service, ConfigMap и другие ресурсы, описанные в файлах внутри папки `k8s`.

4.  **Проверьте состояние подов:**
    ```bash
    kubectl get pods
    ```
    Дождитесь, пока все поды перейдут в состояние `Running` или `Completed`.

5.  **Получите доступ к приложению:**
    - Если вы используете Minikube, выполните:
      ```bash
      minikube service client-service
      ```
    - Если используете другой кластер, пробросьте порт для сервиса `client-service` (например, с помощью `kubectl port-forward`) или используйте NodePort/LoadBalancer, настроенный в манифесте.

## Примеры использования

После успешного развертывания:

1.  Откройте в браузере адрес, по которому доступен `client-service`.
2.  Вы увидите веб-интерфейс, который отправляет запросы к `server-service`.
3.  `server-service` взаимодействует с `worker-service` для выполнения фоновых задач.
4.  Результат обработки отображается на странице клиента.

Это демонстрирует базовую цепочку взаимодействия между микросервисами в кластере.

## Структура проекта
```
multi-k8s/
├── client/                  # Фронтенд-приложение (HTML, CSS, JS)
├── server/                  # Бэкенд-сервер (Node.js)
├── worker/                  # Воркер для фоновых задач (Node.js)
├── k8s/                     # Манифесты Kubernetes (Deployment, Service, etc.)
├── .github/                 # Конфигурация GitHub Actions для CI/CD
│   └── workflows/           # Пайплайны автоматизации
├── .github/                 # Пайплайны автоматизации
└── README.md                # Этот файл
```
