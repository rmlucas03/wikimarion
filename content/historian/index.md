---
title: "From the Historian"
---

<p style="font-style:italic;color:#6b6b63;margin-bottom:2rem;">Recent posts from the Grant County historian's Substack publication, <em>Disappearing Marion</em>. Click any title to read the full post.</p>

<div id="historian-posts" class="historian-posts">
  <p>Loading posts…</p>
</div>

<div id="historian-error" style="display:none">
  <p>Could not load posts automatically. <a href="https://billmunn.substack.com" target="_blank" rel="noopener">Visit Disappearing Marion on Substack directly →</a></p>
</div>

<style>
.historian-post-with-img { display: flex; gap: 1.25rem; align-items: flex-start; }
.historian-post-img { flex-shrink: 0; width: 140px; height: 100px; object-fit: cover; border: 1px solid #d4cfc4; }
.historian-post-text { flex: 1; }
@media (max-width: 640px) {
  .historian-post-with-img { flex-direction: column; }
  .historian-post-img { width: 100%; height: 180px; }
}
</style>

<script>
var proxies = [
  'https://corsproxy.io/?' + encodeURIComponent('https://billmunn.substack.com/feed'),
  'https://api.allorigins.win/raw?url=' + encodeURIComponent('https://billmunn.substack.com/feed')
];

function tryFetch(index) {
  if (index >= proxies.length) {
    document.getElementById('historian-posts').style.display = 'none';
    document.getElementById('historian-error').style.display = 'block';
    return;
  }
  fetch(proxies[index])
    .then(function(r) { return r.text(); })
    .then(function(result) {
      var parser = new DOMParser();
      var xml = parser.parseFromString(result, 'text/xml');
      var entries = xml.querySelectorAll('item');
      if (!entries.length) throw new Error('empty');
      var container = document.getElementById('historian-posts');
      var html = '';
      entries.forEach(function(el) {
        var title = el.querySelector('title') ? el.querySelector('title').textContent : '';
        var link = el.querySelector('link') ? el.querySelector('link').textContent : '';
        var pubDate = el.querySelector('pubDate') ? el.querySelector('pubDate').textContent : '';
        var date = pubDate ? new Date(pubDate).toLocaleDateString('en-US', {
          year: 'numeric', month: 'long', day: 'numeric'
        }) : '';
        var descEl = el.querySelector('description');
        var descHtml = descEl ? descEl.textContent : '';
        var tmp = document.createElement('div');
        tmp.innerHTML = descHtml;
        var imgEl = tmp.querySelector('img');
        var imgSrc = imgEl ? imgEl.getAttribute('src') : '';
        var excerpt = tmp.textContent.slice(0, 220).trim();
        var textBlock =
          '<div class="historian-post-text">' +
          '<a href="' + link + '" target="_blank" rel="noopener" class="historian-post-title">' + title + '</a>' +
          '<span class="historian-post-date">' + date + '</span>' +
          (excerpt ? '<p class="historian-post-excerpt">' + excerpt + '…</p>' : '') +
          '</div>';
        if (imgSrc) {
          html += '<div class="historian-post historian-post-with-img">' +
            '<img class="historian-post-img" src="' + imgSrc + '" alt="" loading="lazy">' +
            textBlock + '</div>';
        } else {
          html += '<div class="historian-post">' + textBlock + '</div>';
        }
      });
      container.innerHTML = html;
    })
    .catch(function() { tryFetch(index + 1); });
}
tryFetch(0);
</script>
