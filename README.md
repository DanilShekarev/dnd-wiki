# База кампании D&D — сайт

Статический сайт для D&D-вики, собранный из Obsidian-vault движком
[Quartz v5](https://quartz.jzhao.xyz/) и публикуемый на GitHub Pages.

**Адрес сайта:** https://danilshekarev.github.io/dnd-wiki/

## Как это устроено

- **Источник заметок** — Obsidian-vault в `D:\Agent\DnD-Vault` (его правишь в Obsidian).
- **Этот репозиторий** — движок Quartz + копия заметок в папке `content/`.
- При каждом `push` в ветку `main` GitHub Action собирает сайт и публикует его.

Vault и сайт — два отдельных репозитория. Заметки переносятся в `content/`
скриптом синхронизации; вручную в `content/` ничего менять не нужно.

## Обновление сайта после правок в Obsidian

```powershell
# 1. Перенести свежие заметки из vault в content/
.\sync-content.ps1

# 2. Закоммитить и запушить — деплой запустится сам
git add -A
git commit -m "Обновление заметок"
git push
```

Скрипт `sync-content.ps1` зеркалит `DnD-Vault` → `content/` (исключая `.git`
и `.obsidian`) и делает дашборд `00 Старт.md` главной страницей (`index.md`).

## Локальный предпросмотр

```powershell
npm ci                                            # один раз — зависимости
node ./quartz/bootstrap-cli.mjs plugin install    # один раз — плагины Quartz
node ./quartz/bootstrap-cli.mjs build --serve     # http://localhost:8080
```

> На Windows вызывай CLI через `node ./quartz/bootstrap-cli.mjs ...`
> (`npx quartz ...` иногда сбоит с кэшем npx).

## Настройка

- `quartz.config.yaml` — заголовок, `baseUrl`, локаль (`ru-RU`), тема, плагины.
- `.github/workflows/deploy.yml` — сборка и деплой на GitHub Pages.

## Первичная настройка GitHub Pages

1. Создать на GitHub репозиторий `dnd-wiki` (пустой, без README).
2. `git remote add origin https://github.com/DanilShekarev/dnd-wiki.git`
3. `git push -u origin main`
4. На GitHub: **Settings → Pages → Source → GitHub Actions**.
