---
layout: page
permalink: /publications/
title: publications
description: Preprints, journal articles, conference papers, and patents, newest first.
nav: true
nav_order: 2
---

<!-- _pages/publications.md -->

Also on [Google Scholar](https://scholar.google.com/citations?user=Gk4nFPEAAAAJ&sortby=pubdate).

<div class="pub-filter mt-3 mb-2" role="group" aria-label="Filter publications by type">
  <button type="button" class="btn btn-sm z-depth-0 pub-filter-btn active" data-filter="all">All</button>
  <button type="button" class="btn btn-sm z-depth-0 pub-filter-btn" data-filter="preprints">Preprints</button>
  <button type="button" class="btn btn-sm z-depth-0 pub-filter-btn" data-filter="journals">Journals</button>
  <button type="button" class="btn btn-sm z-depth-0 pub-filter-btn" data-filter="conferences">Conferences</button>
  <button type="button" class="btn btn-sm z-depth-0 pub-filter-btn" data-filter="patents">Patents</button>
</div>

<!-- Bibsearch Feature -->

{% include bib_search.liquid %}

<div class="publications">

<div class="pub-section" data-section="preprints">
<h2 class="bibliography">preprints</h2>
{% bibliography --query @unpublished --group_by none %}
</div>

<div class="pub-section" data-section="journals">
<h2 class="bibliography">journal articles</h2>
{% bibliography --query @article --group_by none %}
</div>

<div class="pub-section" data-section="conferences">
<h2 class="bibliography">conference papers</h2>
{% bibliography --query @inproceedings --group_by none %}
</div>

<div class="pub-section" data-section="patents">
<h2 class="bibliography">patents</h2>
<p style="font-size: 0.9rem; opacity: 0.8;">A granted U.S. patent and published patent applications, assigned to Cisco Technology, Inc. One entry per patent family.</p>
{% bibliography --query @misc --group_by none %}
</div>

</div>

<style>
  .pub-filter { display: flex; flex-wrap: wrap; gap: 0.5rem; }
  .pub-filter-btn { border: 1px solid var(--global-theme-color); color: var(--global-theme-color); background: transparent; border-radius: 999px; padding: 0.3rem 1rem; font-size: 0.85rem; }
  .pub-filter-btn.active, .pub-filter-btn:hover { background: var(--global-theme-color); color: var(--global-bg-color); }
</style>

<script>
  (function () {
    var buttons = document.querySelectorAll(".pub-filter-btn");
    var sections = document.querySelectorAll(".pub-section");
    function apply(filter) {
      if (!document.querySelector('.pub-filter-btn[data-filter="' + filter + '"]')) filter = "all";
      sections.forEach(function (s) {
        s.style.display = filter === "all" || s.dataset.section === filter ? "" : "none";
      });
      buttons.forEach(function (b) {
        b.classList.toggle("active", b.dataset.filter === filter);
        b.setAttribute("aria-pressed", b.dataset.filter === filter ? "true" : "false");
      });
    }
    buttons.forEach(function (b) {
      b.addEventListener("click", function () {
        apply(b.dataset.filter);
        history.replaceState(null, "", b.dataset.filter === "all" ? location.pathname : "#" + b.dataset.filter);
      });
    });
    apply((location.hash || "#all").slice(1));
  })();
</script>
