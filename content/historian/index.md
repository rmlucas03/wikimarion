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
var feedUrl = 'https://billmunn.substack.com/feed';
// Sources are tried in order until one succeeds. rss2json returns parsed JSON
// (and post thumbnails); the allorigins entry is a raw-XML fallback.
var sources = [
  { url: 'https://api.rss2json.com/v1/api.json?rss_url=' + encodeURIComponent(feedUrl), type: 'json' },
  { url: 'https://api.allorigins.win/raw?url=' + encodeURIComponent(feedUrl), type: 'xml' }
];

function firstImg(html) {
  var tmp = document.createElement('div');
  tmp.innerHTML = html || '';
  var img = tmp.querySelector('img');
  return img ? img.getAttribute('src') : '';
}

function excerptFrom(html) {
  var tmp = document.createElement('div');
  tmp.innerHTML = html || '';
  return tmp.textContent.replace(/\s+/g, ' ').slice(0, 220).trim();
}

function fmtDate(s) {
  if (!s) return '';
  var d = new Date(s.replace(' ', 'T'));
  return isNaN(d.getTime()) ? '' : d.toLocaleDateString('en-US', {
    year: 'numeric', month: 'long', day: 'numeric'
  });
}

function render(items) {
  document.getElementById('historian-posts').innerHTML = items.map(function(item) {
    var text =
      '<div class="historian-post-text">' +
      '<a href="' + item.link + '" target="_blank" rel="noopener" class="historian-post-title">' + item.title + '</a>' +
      '<span class="historian-post-date">' + item.date + '</span>' +
      (item.excerpt ? '<p class="historian-post-excerpt">' + item.excerpt + '…</p>' : '') +
      '</div>';
    if (item.image) {
      return '<div class="historian-post historian-post-with-img">' +
        '<img class="historian-post-img" src="' + item.image + '" alt="" loading="lazy">' +
        text + '</div>';
    }
    return '<div class="historian-post">' + text + '</div>';
  }).join('');
}

function tryFetch(index) {
  if (index >= sources.length) {
    document.getElementById('historian-posts').style.display = 'none';
    document.getElementById('historian-error').style.display = 'block';
    return;
  }
  var src = sources[index];
  fetch(src.url)
    .then(function(r) { return src.type === 'json' ? r.json() : r.text(); })
    .then(function(result) {
      var items;
      if (src.type === 'json') {
        if (!result.items || !result.items.length) throw new Error('empty');
        items = result.items.map(function(it) {
          return {
            title: it.title,
            link: it.link,
            date: fmtDate(it.pubDate),
            image: it.thumbnail || (it.enclosure && it.enclosure.link) || firstImg(it.content || it.description),
            excerpt: excerptFrom(it.description || it.content)
          };
        });
      } else {
        var entries = new DOMParser().parseFromString(result, 'text/xml').querySelectorAll('item');
        if (!entries.length) throw new Error('empty');
        items = [];
        entries.forEach(function(el) {
          var get = function(sel) { var n = el.querySelector(sel); return n ? n.textContent : ''; };
          var desc = get('description');
          items.push({
            title: get('title'),
            link: get('link'),
            date: fmtDate(get('pubDate')),
            image: firstImg(desc),
            excerpt: excerptFrom(desc)
          });
        });
      }
      render(items);
    })
    .catch(function() { tryFetch(index + 1); });
}
tryFetch(0);
</script>
