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

<script>
var proxies = [
  'https://corsproxy.io/?' + encodeURIComponent('https://billmunn.substack.com/feed'),
  'https://api.allorigins.win/raw?url=' + encodeURIComponent('https://billmunn.substack.com/feed'),
  'https://api.rss2json.com/v1/api.json?rss_url=' + encodeURIComponent('https://billmunn.substack.com/feed')
];

function tryFetch(index) {
  if (index >= proxies.length) {
    document.getElementById('historian-posts').style.display = 'none';
    document.getElementById('historian-error').style.display = 'block';
    return;
  }
  var isRss2json = proxies[index].indexOf('rss2json') !== -1;
  fetch(proxies[index])
    .then(function(r) { return isRss2json ? r.json() : r.text(); })
    .then(function(result) {
      var items;
      if (isRss2json) {
        if (!result.items || !result.items.length) throw new Error('empty');
        items = result.items.map(function(i) {
          return { title: i.title, link: i.link, date: i.pubDate, desc: i.description };
        });
      } else {
        var parser = new DOMParser();
        var xml = parser.parseFromString(result, 'text/xml');
        var entries = xml.querySelectorAll('item');
        if (!entries.length) throw new Error('empty');
        items = [];
        entries.forEach(function(el) {
          items.push({
            title: el.querySelector('title').textContent,
            link: el.querySelector('link').textContent,
            date: el.querySelector('pubDate') ? el.querySelector('pubDate').textContent : '',
            desc: el.querySelector('description') ? el.querySelector('description').textContent : ''
          });
        });
      }
      var container = document.getElementById('historian-posts');
      container.innerHTML = items.map(function(item) {
        var date = item.date ? new Date(item.date).toLocaleDateString('en-US', {
          year: 'numeric', month: 'long', day: 'numeric'
        }) : '';
        var tmp = document.createElement('div');
        tmp.innerHTML = item.desc || '';
        var excerpt = tmp.textContent.slice(0, 220).trim();
        return '<div class="historian-post">' +
          '<a href="' + item.link + '" target="_blank" rel="noopener" class="historian-post-title">' + item.title + '</a>' +
          '<span class="historian-post-date">' + date + '</span>' +
          (excerpt ? '<p class="historian-post-excerpt">' + excerpt + '…</p>' : '') +
          '</div>';
      }).join('');
    })
    .catch(function() { tryFetch(index + 1); });
}
tryFetch(0);
</script>
