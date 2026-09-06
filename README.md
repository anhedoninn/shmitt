# SHMITT — ERKINDIK

Одностраничный сайт бренда SHMITT (Бишкек). Чистый HTML/CSS/JS, без сборки и зависимостей.

## Структура

```
index.html              вся вёрстка, стили и скрипты в одном файле
assets/
  hero.mp4              видео первого экрана (обрезано с 2-й секунды)
  hero_poster.jpg       постер для hero-видео
  detail.mp4            видео производства (внутри букв ERKINDIK)
  detail_poster.jpg     постер
  logo.png              вордмарк shmitt, чёрный на прозрачном
  {cream,grey,black,white}_front.webp   лицевая сторона футболки
  {cream,grey,black,white}_back.webp    спина футболки
```

## Как запустить локально

```bash
python3 -m http.server 8000
# открыть http://localhost:8000
```

Открывать `index.html` двойным кликом не стоит: `file://` мешает автоплею видео.

## Публикация на GitHub Pages

```bash
git init
git add .
git commit -m "shmitt site"
git branch -M main
git remote add origin git@github.com:USERNAME/REPO.git
git push -u origin main
```

Дальше в репозитории: **Settings → Pages → Source: Deploy from a branch → main / (root)**.
Через минуту сайт будет на `https://USERNAME.github.io/REPO/`.

Для своего домена: положить рядом файл `CNAME` с одной строкой — доменом (например `shmitt.kg`), и прописать DNS по инструкции GitHub Pages.

## Что где править

| Что | Где в `index.html` |
|---|---|
| Цвета, шрифты, отступы | блок `:root` в начале `<style>` |
| Товары, цены, размеры | массивы `PRICE` и `PRODUCTS` в `<script>` внизу |
| Бегущая строка | массив `items` в `<script>` |
| Меню, футер, контакты | разметка `<div class="menu">` и `<footer class="foot">` |
| Спек-лист (печать, размеры, цена, тираж) | `<div class="stat util">` в секции `#process` |

Добавить товар — дописать объект в `PRODUCTS` и положить в `assets/` пару картинок
`<id>_front.webp` / `<id>_back.webp`, затем добавить `<id>` в объект `IMG`.

## Шрифты

Unbounded, Manrope, IBM Plex Mono подключаются с Google Fonts одной строкой в `<head>`.
Если нужен офлайн-режим — скачать `.woff2` в `assets/fonts/` и заменить `<link>` на `@font-face`.

## Замена видео

```bash
# обрезать первые 2 секунды и сжать под веб
ffmpeg -ss 2 -i original.mp4 -an -vf "scale=1000:-2,fps=25" \
  -c:v libx264 -profile:v main -pix_fmt yuv420p -crf 28 -preset slow \
  -movflags +faststart assets/hero.mp4

# постер из первого кадра
ffmpeg -i assets/hero.mp4 -vframes 1 -vf scale=640:-2 assets/hero_poster.jpg
```

## Лицензия и права

Видео, фотографии товаров и логотип принадлежат бренду SHMITT.
Код вёрстки — свободно используйте и меняйте.
