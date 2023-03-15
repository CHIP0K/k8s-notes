# Запуск раннера в Kubernetes

## 1. Додаємо репозиторій helm собі на ПК

```bash
helm repo add gitlab https://charts.gitlab.io
helm repo update gitlab
helm search repo -l gitlab/gitlab-runner
```

## 2. Встановлення gitlab-runner в кластер Kubernetes

Перед встановленням відредагуємо файл з налаштуваннями ```values.yaml```

Якщо будемо використовувати спільний кеш, встановимо секрети
```bash
kubectl create secret generic s3accesscache \
    --from-literal=accesskey="YourAccessKey" \
    --from-literal=secretkey="YourSecretKey" \
    -n gitlab-runner
```

* необхідно вказати токен (токен може бути як груповий так і на репозиторій) змінна `runnerRegistrationToken`
  * Якщо груповий то переходимо в групу ``CI\CD => Runners => Register a group runner``
  * В другому випадку переходимо в репозиторій ``Settings => CI/CD => Runners => Project runners => And this registration token``
* Якщо свій гітлаб то і `gitlabUrl` потрібно змінити

Встановлювати раннер будемо за допомогою Helm:

```bash
helm upgrade -i gitlab-runner gitlab/gitlab-runner -f values.yaml -n gitlab-runner --create-namespace
```

Якщо все зроблено вірно, ви побачите свій раннер в секції `Available specific runners`
