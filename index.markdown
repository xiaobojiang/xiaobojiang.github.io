---
layout: page
title: Xiao's Notes
permalink: /
---

<style type="text/css" media="screen">
  .post-header {
    display: none;
  }

  .home-notes {
    --ink: #1f2937;
    --muted: #667085;
    --line: #d0d5dd;
    --paper: #ffffff;
    --soft: #f8fafc;
    --accent: #0f766e;
    --accent-strong: #115e59;
    --accent-soft: #ccfbf1;

    color: var(--ink);
  }

  .home-notes a {
    color: inherit;
  }

  .home-notes h2,
  .home-notes h3 {
    letter-spacing: 0;
  }

  .home-notes__section {
    margin: 40px 0 0;
  }

  .home-notes__section:first-child {
    margin-top: 12px;
  }

  .home-notes__section-header {
    display: flex;
    gap: 18px;
    align-items: end;
    justify-content: space-between;
    margin-bottom: 16px;
  }

  .home-notes__section-header h2 {
    margin: 0;
    font-size: clamp(1.5rem, 3vw, 2rem);
  }

  .home-notes__section-header p {
    margin: 6px 0 0;
    max-width: 58ch;
    color: var(--muted);
    line-height: 1.65;
  }

  .home-notes__featured {
    display: grid;
    grid-template-columns: repeat(2, minmax(0, 1fr));
    gap: 16px;
  }

  .home-notes__search {
    padding: clamp(20px, 4vw, 30px);
    border: 1px solid var(--line);
    border-radius: 12px;
    background:
      radial-gradient(circle at top right, rgba(15, 118, 110, 0.12), transparent 30%),
      var(--soft);
  }

  .home-notes__search-input {
    width: 100%;
    min-height: 46px;
    box-sizing: border-box;
    padding: 0 16px;
    border: 1px solid var(--line);
    border-radius: 999px;
    background: var(--paper);
    color: var(--ink);
    font: inherit;
  }

  .home-notes__search-input:focus {
    border-color: var(--accent);
    box-shadow: 0 0 0 3px rgba(15, 118, 110, 0.14);
    outline: none;
  }

  .home-notes__search-status {
    margin: 12px 0 0;
    color: var(--muted);
    font-size: 0.92rem;
  }

  .home-notes__search-results {
    max-height: 360px;
    margin: 18px 0 0;
    padding: 0;
    overflow: auto;
    list-style: none;
  }

  .home-notes__search-results li {
    border-top: 1px solid #eaecf0;
  }

  .home-notes__search-results a {
    display: grid;
    grid-template-columns: 100px minmax(0, 1fr);
    gap: 12px;
    padding: 12px 0;
    text-decoration: none;
  }

  .home-notes__search-results a:hover {
    color: var(--accent-strong);
    text-decoration: none;
  }

  .home-notes__is-hidden {
    display: none;
  }

  .home-notes__post-card,
  .home-notes__topic,
  .home-notes__archive-year {
    border: 1px solid var(--line);
    border-radius: 8px;
    background: var(--paper);
  }

  .home-notes__post-card {
    display: flex;
    min-height: 230px;
    flex-direction: column;
    justify-content: space-between;
    padding: 24px;
    text-decoration: none;
    transition: transform 160ms ease, border-color 160ms ease, box-shadow 160ms ease;
  }

  .home-notes__post-card:hover,
  .home-notes__post-card:focus {
    border-color: var(--accent);
    box-shadow: 0 18px 42px rgba(15, 118, 110, 0.12);
    text-decoration: none;
    transform: translateY(-2px);
  }

  .home-notes__date {
    color: var(--muted);
    font-size: 0.88rem;
    font-weight: 700;
  }

  .home-notes__post-card h3,
  .home-notes__topic h3 {
    margin: 12px 0 10px;
    font-size: 1.18rem;
    line-height: 1.35;
  }

  .home-notes__excerpt {
    margin: 0;
    color: var(--muted);
    line-height: 1.65;
  }

  .home-notes__chips {
    display: flex;
    flex-wrap: wrap;
    gap: 8px;
    margin-top: 18px;
  }

  .home-notes__chip {
    display: inline-flex;
    align-items: center;
    min-height: 28px;
    padding: 0 10px;
    border-radius: 999px;
    background: var(--accent-soft);
    color: var(--accent-strong);
    font-size: 0.78rem;
    font-weight: 700;
  }

  .home-notes__topic-nav {
    display: grid;
    grid-template-columns: repeat(4, minmax(0, 1fr));
    gap: 10px;
  }

  .home-notes__topic-link {
    display: flex;
    justify-content: space-between;
    gap: 12px;
    padding: 14px 16px;
    border: 1px solid var(--line);
    border-radius: 8px;
    background: var(--soft);
    font-weight: 800;
    text-decoration: none;
  }

  .home-notes__topic-link span {
    color: var(--muted);
    font-weight: 700;
  }

  .home-notes__topics {
    display: grid;
    grid-template-columns: repeat(2, minmax(0, 1fr));
    gap: 16px;
  }

  .home-notes__topic {
    padding: 22px;
  }

  .home-notes__topic ol,
  .home-notes__archive-year ol {
    margin: 0;
    padding: 0;
    list-style: none;
  }

  .home-notes__topic li + li,
  .home-notes__archive-year li + li {
    border-top: 1px solid #eaecf0;
  }

  .home-notes__topic a,
  .home-notes__archive-year a {
    display: grid;
    grid-template-columns: 86px minmax(0, 1fr);
    gap: 12px;
    padding: 12px 0;
    text-decoration: none;
  }

  .home-notes__topic a:hover,
  .home-notes__archive-year a:hover {
    color: var(--accent-strong);
    text-decoration: none;
  }

  .home-notes__archive {
    display: grid;
    gap: 12px;
  }

  .home-notes__archive-year {
    padding: 0 18px;
  }

  .home-notes__archive-year summary {
    cursor: pointer;
    padding: 16px 0;
    font-size: 1.1rem;
    font-weight: 900;
  }

  @media (max-width: 820px) {
    .home-notes__featured,
    .home-notes__topics {
      grid-template-columns: 1fr;
    }

    .home-notes__topic-nav {
      grid-template-columns: repeat(2, minmax(0, 1fr));
    }

    .home-notes__section-header {
      display: block;
    }
  }

  @media (max-width: 520px) {
    .home-notes__topic-nav,
    .home-notes__search-results a,
    .home-notes__topic a,
    .home-notes__archive-year a {
      grid-template-columns: 1fr;
    }
  }
</style>

<div class="home-notes">
  {% assign topics = "AI|Linux|Python|Devops|Kubernetes|Cloud|Web|One Line Tip" | split: "|" %}
  <section class="home-notes__section" aria-labelledby="latest-notes-title">
        <div class="home-notes__section-header">
          <div>
            <h2 id="latest-notes-title">Latest Notes</h2>
            <p>Fresh write-ups from the notebook, shown first so the homepage stays useful without becoming a wall of links.</p>
          </div>
        </div>
        <div class="home-notes__featured">
          {% for post in site.posts limit:4 %}
            <a class="home-notes__post-card" href="{{ post.url | relative_url }}">
              <div>
                <span class="home-notes__date">{{ post.date | date: "%Y-%m-%d" }}</span>
                <h3>{{ post.title }}</h3>
                <p class="home-notes__excerpt">{{ post.excerpt | strip_html | normalize_whitespace | truncate: 150 }}</p>
              </div>
              <div class="home-notes__chips" aria-label="Categories">
                {% for category in post.categories %}
                  {% unless category == "Technology" %}
                    <span class="home-notes__chip">{{ category }}</span>
                  {% endunless %}
                {% endfor %}
              </div>
            </a>
          {% endfor %}
        </div>
  </section>

  <section class="home-notes__section home-notes__search" aria-labelledby="post-search-title">
        <div class="home-notes__section-header">
          <div>
            <h2 id="post-search-title">Find a Note</h2>
            <p>Search {{ site.posts | size }} posts by title.</p>
          </div>
        </div>
        <input class="home-notes__search-input" id="post-title-search" type="search" aria-label="Search post titles" placeholder="Search titles, for example: podman, ansible, python" autocomplete="off">
        <p class="home-notes__search-status" id="post-search-status" aria-live="polite">Type a few words to search post titles.</p>
        <ol class="home-notes__search-results home-notes__is-hidden" id="post-search-results">
          {% for post in site.posts %}
            <li class="home-notes__is-hidden" data-post-search-item data-title="{{ post.title | downcase | escape }}">
              <a href="{{ post.url | relative_url }}">
                <span class="home-notes__date">{{ post.date | date: "%Y-%m-%d" }}</span>
                <span>{{ post.title }}</span>
              </a>
            </li>
          {% endfor %}
        </ol>
  </section>

  <section class="home-notes__section" aria-labelledby="topics-title">
    <div class="home-notes__section-header">
      <div>
        <h2 id="topics-title">Browse by Topic</h2>
        <p>Jump into the recurring themes across the archive.</p>
      </div>
    </div>
    <nav class="home-notes__topic-nav" aria-label="Topic shortcuts">
      {% for topic in topics %}
        {% assign topic_posts = site.categories[topic] %}
        <a class="home-notes__topic-link" href="#topic-{{ topic | slugify }}">{{ topic }} <span>{{ topic_posts | size }}</span></a>
      {% endfor %}
    </nav>
  </section>

  <section class="home-notes__section" aria-labelledby="topic-shelves-title">
    <div class="home-notes__section-header">
      <div>
        <h2 id="topic-shelves-title">Topic Shelves</h2>
        <p>Each shelf keeps the newest notes close at hand while the full archive remains available below.</p>
      </div>
    </div>
    <div class="home-notes__topics">
      {% for topic in topics %}
        {% assign topic_posts = site.categories[topic] %}
        <section class="home-notes__topic" id="topic-{{ topic | slugify }}" aria-labelledby="topic-title-{{ topic | slugify }}">
          <h3 id="topic-title-{{ topic | slugify }}">{{ topic }}</h3>
          <ol>
            {% for post in topic_posts limit:5 %}
              <li>
                <a href="{{ post.url | relative_url }}">
                  <span class="home-notes__date">{{ post.date | date: "%Y-%m-%d" }}</span>
                  <span>{{ post.title }}</span>
                </a>
              </li>
            {% endfor %}
          </ol>
        </section>
      {% endfor %}
    </div>
      </section>

  {% assign posts_by_year = site.posts | group_by_exp: "post", "post.date | date: '%Y'" %}
      <section class="home-notes__section" aria-labelledby="archive-title">
    <div class="home-notes__section-header">
      <div>
        <h2 id="archive-title">Full Archive</h2>
        <p>The complete article list is still here, grouped by year so it is easier to scan.</p>
      </div>
    </div>
    <div class="home-notes__archive">
      {% for year in posts_by_year %}
        <details class="home-notes__archive-year" {% if forloop.first %}open{% endif %}>
          <summary>{{ year.name }} <span class="home-notes__date">{{ year.items | size }} notes</span></summary>
          <ol>
            {% for post in year.items %}
              <li>
                <a href="{{ post.url | relative_url }}">
                  <span class="home-notes__date">{{ post.date | date: "%b %d" }}</span>
                  <span>{{ post.title }}</span>
                </a>
              </li>
            {% endfor %}
          </ol>
        </details>
      {% endfor %}
    </div>
  </section>
</div>

<script>
  (function () {
    var input = document.getElementById("post-title-search");
    var results = document.getElementById("post-search-results");
    var status = document.getElementById("post-search-status");
    var items = Array.prototype.slice.call(document.querySelectorAll("[data-post-search-item]"));

    if (!input || !results || !status || !items.length) {
      return;
    }

    input.addEventListener("input", function () {
      var query = input.value.trim().toLowerCase();
      var matches = 0;

      if (!query) {
        results.classList.add("home-notes__is-hidden");
        items.forEach(function (item) {
          item.classList.add("home-notes__is-hidden");
        });
        status.textContent = "Type a few words to search post titles.";
        return;
      }

      items.forEach(function (item) {
        var isMatch = item.getAttribute("data-title").indexOf(query) !== -1;
        item.classList.toggle("home-notes__is-hidden", !isMatch);
        if (isMatch) {
          matches += 1;
        }
      });

      results.classList.toggle("home-notes__is-hidden", matches === 0);
      status.textContent = matches === 1 ? "1 matching title found." : matches + " matching titles found.";
    });
  }());
</script>
