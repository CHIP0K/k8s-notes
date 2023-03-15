# Running gitlab-runner in Kubernetes

## 🇺🇸

### 1. Add the Helm repository to your PC

```bash
helm repo add gitlab https://charts.gitlab.io
helm repo update gitlab
helm search repo -l gitlab/gitlab-runner
```

### 2. Installing GitLab Runner in a Kubernetes cluster

- Before installation, edit the **`values.yaml`** config file.
- If you're using a shared cache, set the secrets:

```bash
kubectl create secret generic s3accesscache \
    --from-literal=accesskey="YourAccessKey" \
    --from-literal=secretkey="YourSecretKey" \
    -n gitlab-runner
```

- Set the token as the variable `runnerRegistrationToken` in the **`values.yaml`** file
- If GitLab is a standalone installation, also change the variable `gitlabUrl`

Install the runner using Helm:

```bash
helm upgrade -i gitlab-runner gitlab/gitlab-runner -f values.yaml -n gitlab-runner --create-namespace
```

If everything is done correctly, you will see your runner in the `Available specific runners` section.

## 🇺🇦

### 1. Додаємо репозиторій helm собі на ПК

```bash
helm repo add gitlab https://charts.gitlab.io
helm repo update gitlab
helm search repo -l gitlab/gitlab-runner
```

### 2. Встановлення gitlab-runner в кластер Kubernetes

- Перед встановленням відредагуємо файл з налаштуваннями **`values.yaml`**
- Якщо будемо використовувати спільний кеш, встановимо секрети

```bash
kubectl create secret generic s3accesscache \
    --from-literal=accesskey="YourAccessKey" \
    --from-literal=secretkey="YourSecretKey" \
    -n gitlab-runner
```

- необхідно вказати токен (токен може бути як груповий так і на репозиторій) змінна `runnerRegistrationToken`
  - Якщо груповий то переходимо в групу ``CI\CD => Runners => Register a group runner``
  - В другому випадку переходимо в репозиторій ``Settings => CI/CD => Runners => Project runners => And this registration token``
- Якщо свій гітлаб то і `gitlabUrl` потрібно змінити

Встановлювати раннер будемо за допомогою Helm:

```bash
helm upgrade -i gitlab-runner gitlab/gitlab-runner -f values.yaml -n gitlab-runner --create-namespace
```

Якщо все зроблено вірно, ви побачите свій раннер в секції `Available specific runners`

✨ Magic ✨
