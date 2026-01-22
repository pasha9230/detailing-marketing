# Blog Redesign Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** Превратить минималистичный блог в полноценный с обложками и рубриками.

**Architecture:** Hugo taxonomies для рубрик, partials для переиспользуемых компонентов, CSS в baseof.html.

**Tech Stack:** Hugo, HTML/CSS, Go templates

---

## Task 1: Настройка taxonomies в Hugo

**Files:**
- Modify: `hugo.toml`

**Step 1: Добавить taxonomies и параметры рубрик**

В конец файла `hugo.toml` добавить:

```toml
[taxonomies]
  category = "categories"
  tag = "tags"

[params.categories]
  [params.categories.privlechenie-klientov]
    name = "Привлечение клиентов"
  [params.categories.razvitie-studii]
    name = "Развитие детейлинг центра"
```

**Step 2: Проверить конфиг**

Run: `cd /Users/pavelmalysev/claude/seo_ris_detailing/detailing-marketing && hugo config`
Expected: Конфиг выводится без ошибок

**Step 3: Commit**

```bash
git add hugo.toml
git commit -m "feat: add taxonomies for categories"
```

---

## Task 2: Обновить стили в baseof.html

**Files:**
- Modify: `layouts/_default/baseof.html`

**Step 1: Заменить стили**

Заменить весь блок `<style>...</style>` на:

```css
<style>
  * { box-sizing: border-box; }

  body {
    font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
    max-width: 600px;
    margin: 0 auto;
    padding: 20px;
    line-height: 1.7;
    background: #fff;
    color: #1a1a1a;
    font-size: 17px;
  }

  a { color: #0066cc; text-decoration: none; }
  a:hover { text-decoration: underline; }

  /* Header */
  .site-header { margin-bottom: 32px; }
  .site-header h1 { font-size: 24px; margin: 0 0 4px 0; }
  .site-header h1 a { color: #1a1a1a; }
  .site-header .intro { color: #666; font-size: 15px; margin: 0 0 4px 0; }
  .site-header .author { color: #666; font-size: 13px; margin: 0; }

  /* Category filter */
  .category-filter {
    display: flex;
    gap: 8px;
    margin-bottom: 24px;
    flex-wrap: wrap;
  }
  .category-filter a {
    padding: 6px 12px;
    border: 1px solid #ddd;
    border-radius: 4px;
    font-size: 14px;
    color: #666;
  }
  .category-filter a:hover,
  .category-filter a.active {
    border-color: #0066cc;
    color: #0066cc;
    text-decoration: none;
  }

  /* Article card */
  .article-card {
    margin-bottom: 32px;
    border-bottom: 1px solid #eee;
    padding-bottom: 24px;
  }
  .article-card:last-child { border-bottom: none; }
  .article-card img {
    width: 100%;
    height: auto;
    aspect-ratio: 16/9;
    object-fit: cover;
    border-radius: 4px;
    margin-bottom: 12px;
  }
  .article-card .category {
    font-size: 12px;
    text-transform: uppercase;
    letter-spacing: 0.5px;
    color: #0066cc;
    margin-bottom: 4px;
  }
  .article-card h2 {
    font-size: 20px;
    margin: 0 0 8px 0;
    line-height: 1.3;
  }
  .article-card h2 a { color: #1a1a1a; }
  .article-card .meta {
    font-size: 13px;
    color: #666;
    margin-bottom: 8px;
  }
  .article-card .excerpt {
    color: #444;
    font-size: 15px;
    line-height: 1.5;
  }

  /* Single article */
  .breadcrumbs {
    font-size: 13px;
    color: #666;
    margin-bottom: 16px;
  }
  .breadcrumbs a { color: #666; }

  .article-header { margin-bottom: 24px; }
  .article-header .category {
    font-size: 12px;
    text-transform: uppercase;
    letter-spacing: 0.5px;
    color: #0066cc;
    margin-bottom: 8px;
  }
  .article-header h1 {
    font-size: 28px;
    margin: 0 0 12px 0;
    line-height: 1.3;
  }
  .article-header .meta {
    font-size: 14px;
    color: #666;
  }
  .article-cover {
    width: 100%;
    height: auto;
    aspect-ratio: 16/9;
    object-fit: cover;
    border-radius: 4px;
    margin-bottom: 24px;
  }

  /* Content */
  h2 { font-size: 20px; margin-top: 32px; margin-bottom: 12px; }
  h3 { font-size: 17px; margin-top: 24px; margin-bottom: 8px; }
  p { margin-bottom: 16px; }

  ul, ol { margin: 16px 0; padding: 0 0 0 1.5em; }
  li { margin-bottom: 6px; }

  blockquote {
    border-left: 3px solid #0066cc;
    margin: 20px 0;
    padding: 12px 16px;
    background: #f8f9fa;
    color: #444;
  }
  blockquote p { margin: 0; }

  pre {
    background: #f8f9fa;
    border: 1px solid #e9ecef;
    padding: 16px;
    overflow-x: auto;
    font-size: 14px;
    border-radius: 4px;
    margin: 20px 0;
  }
  code {
    font-family: 'SF Mono', Monaco, monospace;
    background: #f1f3f5;
    padding: 2px 6px;
    border-radius: 3px;
    font-size: 0.9em;
  }
  pre code { background: none; padding: 0; }

  table { width: 100%; border-collapse: collapse; margin: 20px 0; }
  th, td { border: 1px solid #e9ecef; padding: 10px; text-align: left; }
  th { background: #f8f9fa; font-weight: 600; }

  /* CTA */
  .cta {
    background: #f8f9fa;
    padding: 20px;
    border-radius: 4px;
    margin-top: 32px;
    text-align: center;
  }
  .cta p { margin: 0 0 12px 0; }
  .cta a {
    display: inline-block;
    background: #0066cc;
    color: #fff;
    padding: 10px 20px;
    border-radius: 4px;
  }
  .cta a:hover { background: #0052a3; text-decoration: none; }

  /* Footer */
  footer {
    margin-top: 40px;
    padding-top: 20px;
    border-top: 1px solid #eee;
    text-align: center;
    font-size: 13px;
    color: #666;
  }
</style>
```

**Step 2: Проверить сборку**

Run: `cd /Users/pavelmalysev/claude/seo_ris_detailing/detailing-marketing && hugo`
Expected: Сборка без ошибок

**Step 3: Commit**

```bash
git add layouts/_default/baseof.html
git commit -m "style: update to sans-serif and modern design"
```

---

## Task 3: Создать partial для карточки статьи

**Files:**
- Create: `layouts/partials/card.html`

**Step 1: Создать файл карточки**

```html
<article class="article-card">
  {{ with .Params.cover }}
  <a href="{{ $.Permalink }}">
    <img src="{{ . }}" alt="{{ $.Title }}">
  </a>
  {{ end }}

  {{ with .Params.category }}
  {{ $catName := . }}
  {{ with index $.Site.Params.categories . }}
  <div class="category">{{ .name }}</div>
  {{ else }}
  <div class="category">{{ $catName }}</div>
  {{ end }}
  {{ end }}

  <h2><a href="{{ .Permalink }}">{{ .Title }}</a></h2>

  <div class="meta">{{ .Date.Format "2 января 2006" }}</div>

  {{ with .Description }}
  <p class="excerpt">{{ . | truncate 120 }}</p>
  {{ end }}
</article>
```

**Step 2: Проверить сборку**

Run: `cd /Users/pavelmalysev/claude/seo_ris_detailing/detailing-marketing && hugo`
Expected: Сборка без ошибок

**Step 3: Commit**

```bash
git add layouts/partials/card.html
git commit -m "feat: add article card partial"
```

---

## Task 4: Обновить список статей с фильтром

**Files:**
- Modify: `layouts/blog/list.html`

**Step 1: Заменить содержимое list.html**

```html
{{ define "main" }}
<header class="site-header">
  <h1><a href="/">Detailing Marketing</a></h1>
  <p class="intro">{{ .Site.Params.description }}</p>
  <p class="author">Автор: {{ .Site.Params.author }} · <a href="{{ (index .Site.Params.social 0).url }}">Telegram</a></p>
</header>

<nav class="category-filter">
  <a href="/blog/" class="active">Все</a>
  <a href="/categories/privlechenie-klientov/">Привлечение клиентов</a>
  <a href="/categories/razvitie-studii/">Развитие студии</a>
</nav>

<div class="articles">
  {{ range .Pages.ByDate.Reverse }}
    {{ partial "card.html" . }}
  {{ end }}
</div>
{{ end }}
```

**Step 2: Проверить сборку**

Run: `cd /Users/pavelmalysev/claude/seo_ris_detailing/detailing-marketing && hugo`
Expected: Сборка без ошибок

**Step 3: Commit**

```bash
git add layouts/blog/list.html
git commit -m "feat: update blog list with cards and filter"
```

---

## Task 5: Создать шаблон для страницы рубрики

**Files:**
- Create: `layouts/categories/list.html`

**Step 1: Создать директорию и файл**

Run: `mkdir -p /Users/pavelmalysev/claude/seo_ris_detailing/detailing-marketing/layouts/categories`

**Step 2: Создать шаблон рубрики**

```html
{{ define "main" }}
<header class="site-header">
  <h1><a href="/">Detailing Marketing</a></h1>
  <p class="intro">{{ .Site.Params.description }}</p>
  <p class="author">Автор: {{ .Site.Params.author }} · <a href="{{ (index .Site.Params.social 0).url }}">Telegram</a></p>
</header>

<nav class="category-filter">
  <a href="/blog/">Все</a>
  {{ $current := .Title }}
  {{ range $key, $val := .Site.Params.categories }}
  <a href="/categories/{{ $key }}/"{{ if eq $val.name $current }} class="active"{{ end }}>{{ $val.name }}</a>
  {{ end }}
</nav>

<div class="articles">
  {{ range .Pages.ByDate.Reverse }}
    {{ partial "card.html" . }}
  {{ end }}
</div>
{{ end }}
```

**Step 3: Commit**

```bash
git add layouts/categories/
git commit -m "feat: add category list template"
```

---

## Task 6: Обновить страницу статьи

**Files:**
- Modify: `layouts/blog/single.html`

**Step 1: Заменить содержимое single.html**

```html
{{ define "main" }}
<nav class="breadcrumbs">
  <a href="/blog/">Блог</a>
  {{ with .Params.category }}
  {{ with index $.Site.Params.categories . }}
   → <a href="/categories/{{ $.Params.category }}/">{{ .name }}</a>
  {{ end }}
  {{ end }}
</nav>

<article>
  <header class="article-header">
    {{ with .Params.category }}
    {{ with index $.Site.Params.categories . }}
    <div class="category">{{ .name }}</div>
    {{ end }}
    {{ end }}

    <h1>{{ .Title }}</h1>

    <div class="meta">
      {{ .Site.Params.author }} · {{ .Date.Format "2 января 2006" }}
    </div>
  </header>

  {{ with .Params.cover }}
  <img class="article-cover" src="{{ . }}" alt="{{ $.Title }}">
  {{ end }}

  <div class="content">
    {{ .Content }}
  </div>

  <div class="cta">
    <p>Вопросы? Обсудим вашу ситуацию</p>
    <a href="{{ (index .Site.Params.social 0).url }}">Написать в Telegram</a>
  </div>

  <footer>
    {{ .Date.Format "Январь 2006" }}
  </footer>
</article>
{{ end }}
```

**Step 2: Проверить сборку**

Run: `cd /Users/pavelmalysev/claude/seo_ris_detailing/detailing-marketing && hugo`
Expected: Сборка без ошибок

**Step 3: Commit**

```bash
git add layouts/blog/single.html
git commit -m "feat: update single article with cover and category"
```

---

## Task 7: Обновить существующую статью

**Files:**
- Modify: `content/blog/yandex-karty-dlya-detailinga.md`

**Step 1: Обновить front matter**

Заменить front matter на:

```yaml
---
title: "Яндекс.Карты для детейлинга в 2025: что реально работает, а что — миф"
date: 2025-01-21
description: "Разбираем актуальные факторы ранжирования в Яндекс.Картах для детейлинг-студий. Без воды — только то, что влияет на позиции."
cover: ""
category: "privlechenie-klientov"
tags: ["яндекс карты", "локальное seo", "маркетинг"]
---
```

Примечание: `cover` пока пустой — обложку сгенерируешь через Gemini позже.

**Step 2: Commit**

```bash
git add content/blog/yandex-karty-dlya-detailinga.md
git commit -m "feat: add category to existing article"
```

---

## Task 8: Финальная проверка

**Step 1: Запустить dev-сервер**

Run: `cd /Users/pavelmalysev/claude/seo_ris_detailing/detailing-marketing && hugo server -D`

**Step 2: Проверить в браузере**

- Открыть http://localhost:1313/blog/
- Проверить: шапка, фильтр рубрик, карточки статей
- Открыть http://localhost:1313/categories/privlechenie-klientov/
- Проверить: фильтр активен, статья отображается
- Открыть статью, проверить: хлебные крошки, рубрика, CTA

**Step 3: Финальный коммит (если были правки)**

```bash
git add -A
git commit -m "fix: minor adjustments after testing"
```
