# gitlab flow

Gitlab Flow — подходит для непрерывной интеграции и деплоя (Continuous Integration/Continuous Deployment, CI/CD). Основные принципы:
  - используются `main` ветка для хранения production-ready кода и ветки-окружения для выпуска релизов
  - коммиты проходят через ветки в строго определенном порядке для гарантии тестирования во всех средах
  - существует два варианта стратегии ветвления gitlab flow:
    - gitlab flow с ветками окружения
    - gitlab flow  вариант с релизными ветками
---

## gitlab flow с ветками окружения:
1. **`production`** (ветка окружения):
   - Используется для выпуска релиза и доставки клиенту (CI/CD).

2. **`pre-production`**(опция):
   - Промежуточная ветка, может использоваться для финальных проверок.

3. **`staging`**(опция):
   - Промежуточная ветка, может использоваться для начальных проверок.

4. **`main`**:
   - Хранит стабильную версию кода, которая всегда готова для деплоя в продакшн.
   - Любые изменения, попадающие в эту ветку должны быть протестированы и проверены.

5. **`feature`-ветки** (фичи):
   - Используются для разработки новой функциональности.
   - Ответвляются от `main`.
   - После завершения разработки создается `pull-request` в `main`.
   - Изменения из Pull Request должны пройти код-ревью и автоматические тесты новой функциональнсоти.
   - Если все проверки пройдены, можно выполнять слияние Pull Request. После этого изменения попадают в ветку main.
   - Название веток: `feature/имя-фичи`.

6. **`fix`-ветки** (фиксы):
   - Используются для исправления ошибок.
   - Ответвляются от main.
   - После исправления создается `pull-request` перед слиянием в `main`.
   - Изменения из Pull Request должны пройти код-ревью и автоматические тесты (если настроены CI-инструменты).
   - Если все проверки пройдены, можно выполнять слияние Pull Request. После этого изменения попадают в ветку main.
   - Название веток: `fix/описание-исправление`.

**Пример `gitlab flow` с ветками окружения**
```mermaid
  gitGraph%%{init: { 'gitGraph': {'mainBranchName': 'production'}} }%%
   commit id: "initial commit"
   branch pre-production
   branch main
   commit
   branch feature/project-setup
   commit
   checkout main
   merge feature/project-setup id: "pull-request feature/project-setup to main"
   branch feature/profile-settings
   branch feature/user-authentication
   commit
   checkout feature/profile-settings
   commit
   checkout feature/user-authentication
   commit
   checkout main
   merge feature/user-authentication id: "pull-request user-authentication to main"
   checkout feature/profile-settings
   commit
   commit
   checkout main
   merge feature/profile-settings id: "pull-request profile-settings to main"
   checkout pre-production
   merge main id: "merge to pre-production"
   checkout production
   merge pre-production tag: "v1.0.0" id: "merge to production"
   checkout main
   branch fix/user-authentication-bug
   commit
   checkout main
   merge fix/user-authentication-bug id: "pull-request user-authentication-bug to main"
   checkout pre-production
   merge main
   checkout production
   merge pre-production tag: "v1.0.1"
```

Ссылки:

- [Github flow tutorial]([https://www.youtube.com/watch?v=ZJuUz5jWb44](https://youtu.be/ZJuUz5jWb44?feature=shared))
- [Сравнение процесов](https://yapro.ru/article/6172)
