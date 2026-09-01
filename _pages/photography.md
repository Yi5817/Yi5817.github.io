---
layout: page
permalink: /photography/
title: Photography
nav: true
nav_order: 4
description: Snapshots from my travels. Click any frame to enlarge.
_styles: >
  .album-nav {
    display: flex;
    flex-wrap: wrap;
    gap: 0.5rem;
    margin-bottom: 2.5rem;
  }

  .album-nav a {
    padding: 0.25rem 0.75rem;
    border: 1px solid var(--global-divider-color);
    border-radius: 999px;
    font-size: 0.8rem;
    color: var(--global-text-color-light);
    white-space: nowrap;
    transition: all 0.2s ease-in-out;
  }

  .album-nav a:hover {
    color: var(--global-hover-text-color);
    background-color: var(--global-hover-color);
    border-color: var(--global-hover-color);
    text-decoration: none;
  }

  .album {
    margin-bottom: 3.5rem;
    scroll-margin-top: 5rem;
  }

  .album-header {
    display: flex;
    flex-wrap: wrap;
    align-items: baseline;
    justify-content: space-between;
    gap: 0.25rem 1rem;
    padding-bottom: 0.5rem;
    margin-bottom: 1.25rem;
    border-bottom: 1px solid var(--global-divider-color);
  }

  .album-header h2 {
    margin: 0;
    font-size: 1.5rem;
    font-weight: 500;
  }

  .album-header .album-meta {
    font-size: 0.85rem;
    color: var(--global-text-color-light);
    letter-spacing: 0.02em;
  }

  /* Justified rows: flex-grow and flex-basis both scale with the aspect ratio,
     so every frame in a row lands on the same height and the row fills the width. */
  .album-grid {
    --row-height: 12rem;
    display: flex;
    flex-wrap: wrap;
    align-items: flex-start;
    gap: 0.75rem;
  }

  .album-grid figure {
    flex-grow: var(--ar);
    flex-basis: calc(var(--ar) * var(--row-height));
    min-width: 0;
    margin: 0;
  }

  /* Soaks up the slack in the final row so its frames keep their natural size. */
  .album-grid::after {
    content: "";
    flex-grow: 999999;
    min-width: 0;
  }

  /* Deterrents: no drag-to-save, no long-press save sheet on iOS, no selection.
     Applied to the zoomed clone (.medium-zoom-image) as well. */
  .album-grid img,
  .medium-zoom-image {
    -webkit-user-drag: none;
    user-select: none;
    -webkit-user-select: none;
    -webkit-touch-callout: none;
  }

  .album-grid img {
    display: block;
    width: 100%;
    height: auto;
    border-radius: 4px;
    cursor: zoom-in;
    background-color: var(--global-divider-color);
    box-shadow: 0 1px 4px rgba(0, 0, 0, 0.12);
    transition:
      transform 0.25s ease-in-out,
      box-shadow 0.25s ease-in-out;
  }

  .album-grid figure:hover img {
    transform: translateY(-2px);
    box-shadow: 0 6px 18px rgba(0, 0, 0, 0.22);
  }

  @media (max-width: 768px) {
    .album-grid {
      --row-height: 9rem;
    }
  }

  @media (max-width: 576px) {
    .album-grid figure {
      flex: 0 0 100%;
    }
    .album-grid::after {
      display: none;
    }
  }

  @media (prefers-reduced-motion: reduce) {
    .album-grid img,
    .album-nav a {
      transition: none;
    }
    .album-grid figure:hover img {
      transform: none;
    }
  }
---

<nav class="album-nav">
  {% for album in site.data.photography %}
    <a href="#{{ album.slug }}">{{ album.title }}</a>
  {% endfor %}
</nav>

{% for album in site.data.photography %}

  <section class="album" id="{{ album.slug }}">
    <div class="album-header">
      <h2>{{ album.title }}</h2>
      <span class="album-meta">{{ album.place }} &middot; {{ album.date }}</span>
    </div>
    <div class="album-grid">
      {% for photo in album.photos %}
        {% assign base = '/assets/photos/' | append: album.slug | append: '/' | append: photo.file %}
        {% assign ar = photo.w | times: 1.0 | divided_by: photo.h | round: 3 %}
        <figure style="--ar: {{ ar }}">
          <img
            src="{{ base | append: '-thumb.jpg' | relative_url }}"
            data-zoom-src="{{ base | append: '.jpg' | relative_url }}"
            width="{{ photo.w }}"
            height="{{ photo.h }}"
            loading="lazy"
            decoding="async"
            alt="{% if photo.caption %}{{ photo.caption }} — {% endif %}{{ album.title }}, {{ album.place }}"
            data-zoomable
          >
        </figure>
      {% endfor %}
    </div>
  </section>
{% endfor %}

<p class="photo-copyright" style="margin-top: 3rem; font-size: 0.8rem; color: var(--global-text-color-light); text-align: center;">
  &copy; {{ site.time | date: "%Y" }} Yi Yang. All photographs on this page are protected by copyright and may not be downloaded, reproduced, or used without permission.
</p>

<script>
  // Deterrent only: blocks right-click and drag-to-save on the photos
  // (including the medium-zoom clone). Does not stop a determined visitor.
  document.addEventListener("contextmenu", (e) => {
    if (e.target.closest(".album-grid img, .medium-zoom-image")) e.preventDefault();
  });
  document.addEventListener("dragstart", (e) => {
    if (e.target.closest(".album-grid img, .medium-zoom-image")) e.preventDefault();
  });
</script>
