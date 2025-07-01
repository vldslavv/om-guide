# 🚀 Руководство по развертыванию MkDocs Material сайта

## 🎯 Варианты развертывания

### 1. GitHub Pages (Рекомендуется) 

#### Автоматический деплой:
```bash
# Из папки mkdocs-site
source mkdocs-env/bin/activate
mkdocs gh-deploy
```

#### Ручной деплой:
1. Создайте репозиторий на GitHub
2. Загрузите все файлы проекта
3. В настройках репозитория включите GitHub Pages
4. Выберите источник: `gh-pages` branch

### 2. Netlify (Очень просто)

1. Зарегистрируйтесь на [netlify.com](https://netlify.com)
2. Подключите ваш GitHub репозиторий
3. Настройки сборки:
   ```
   Build command: pip install -r requirements.txt && mkdocs build
   Publish directory: site
   ```

### 3. Vercel

1. Установите Vercel CLI: `npm i -g vercel`
2. Из папки mkdocs-site выполните:
   ```bash
   mkdocs build
   vercel --prod ./site
   ```

### 4. Собственный сервер

```bash
# Сборка сайта
mkdocs build

# Содержимое папки site/ загрузите на ваш сервер
```

## ⚙️ Конфигурация для хостинга

### Файл `netlify.toml` (для Netlify):
```toml
[build]
  command = "pip install -r requirements.txt && mkdocs build"
  publish = "site"

[build.environment]
  PYTHON_VERSION = "3.9"

[[redirects]]
  from = "/*"
  to = "/index.html"
  status = 404
```

### Файл `vercel.json` (для Vercel):
```json
{
  "builds": [
    {
      "src": "mkdocs.yml",
      "use": "@vercel/static-build",
      "config": {
        "distDir": "site"
      }
    }
  ],
  "routes": [
    { "src": "/(.*)", "dest": "/404.html", "status": 404 }
  ]
}
```

## 🔧 GitHub Actions (CI/CD)

Создайте файл `.github/workflows/deploy.yml`:

```yaml
name: Deploy MkDocs
on:
  push:
    branches: [ main ]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v3
    
    - name: Setup Python
      uses: actions/setup-python@v4
      with:
        python-version: '3.9'
        
    - name: Install dependencies
      run: |
        cd mkdocs-site
        pip install -r requirements.txt
        
    - name: Deploy to GitHub Pages
      run: |
        cd mkdocs-site
        mkdocs gh-deploy --force
```

## 🌐 Собственный домен

### Для GitHub Pages:
1. Добавьте файл `CNAME` в папку `docs/`:
   ```
   yourdomain.com
   ```

2. В настройках репозитория укажите ваш домен

### Для других хостингов:
Настройте DNS записи согласно инструкциям хостинга.

## 📊 Аналитика

### Google Analytics:
Добавьте в `mkdocs.yml`:
```yaml
extra:
  analytics:
    provider: google
    property: G-XXXXXXXXXX
```

### Yandex Metrica:
```yaml
extra:
  analytics:
    provider: custom
    gtag: |
      <!-- Yandex.Metrica counter -->
      <script type="text/javascript">
      // Ваш код Yandex Metrica
      </script>
```

## 🔍 SEO Оптимизация

### Добавьте в `mkdocs.yml`:
```yaml
plugins:
  - search
  - sitemap
  - meta

extra:
  social:
    - icon: fontawesome/brands/telegram
      link: https://t.me/open_market_chat
      name: Сообщество ОМ
      
site_description: >-
  Практическое руководство по защите от принуждения к увольнению 
  на основе опыта сообщества ОМ

repo_url: https://github.com/username/repo
repo_name: GitHub
```

## 🚨 Проблемы и решения

### Ошибка: "No module named 'material'"
```bash
pip install mkdocs-material
```

### Ошибка: "Config file 'mkdocs.yml' does not exist"
Убедитесь, что вы находитесь в папке `mkdocs-site`

### Медленная сборка:
Исключите ненужные файлы в `mkdocs.yml`:
```yaml
watch:
  - docs/
exclude_docs: |
  drafts/
  .tmp/
```

## 📱 Тестирование

### Локальное тестирование:
```bash
mkdocs serve --dev-addr=127.0.0.1:8000
```

### Проверка ссылок:
```bash
pip install mkdocs-linkcheck
mkdocs serve --plugin linkcheck
```

### Валидация разметки:
```bash
mkdocs build --verbose
```

## 🔄 Обновление контента

1. Обновите данные:
   ```bash
   cd ../scripts
   python3 extract_data.py
   ```

2. Синхронизируйте изменения:
   ```bash
   cd ../mkdocs-site
   mkdocs gh-deploy
   ```

---

**🎉 Поздравляем! Ваш справочник готов к публикации!**