---
title: "From the Historian"
---

<p style="font-style:italic;color:#6b6b63;margin-bottom:2rem;">Recent posts from the Grant County historian's Substack publication, <em>Disappearing Marion</em>. Click any title to read the full post.</p>

<div id="historian-posts" class="historian-posts">
  <p>Loading posts…</p>
</div>

<div id="historian-error" style="display:none">
  <p>Could not load posts. <a href="https://billmunn.substack.com" target="_blank" rel="noopener">Visit Disappearing Marion on Substack directly →</a></p>
</div>

<script>
fetch('https://api.allorigins.win/raw?url=' + encodeURIComponent('https://billmunn.substack.com/feed'))
  .then(function(r) { return r.text(); })
  .then(function(text) {
    var parser = new DOMParser();
    var xml = parser.parseFromString(text, 'text/xml');
    var items = xml.querySelectorAll('item');
    var container = document.getElementById('historian-posts');
    if (!items.length) throw new Error('No items');
    var html = '';
    items.forEach(function(item) {
      var title = item.querySelector('title').textContent;
      var link = item.querySelector('link').textContent;
      var pubDate = item.querySelector('pubDate');
      var date = pubDate ? new Date(pubDate.textContent).toLocaleDateString('en-US', {
        year: 'numeric', month: 'long', day: 'numeric'
      }) : '';
      var desc = item.querySelector('description');
      var tmp = document.createElement('div');
      tmp.innerHTML = desc ? desc.textContent : '';
      var excerpt = tmp.textContent.slice(0, 220).trim();
      html += '<div class="historian-post">' +
        '<a href="' + link + '" target="_blank" rel="noopener" class="historian-post-title">' + title + '</a>' +
        '<span class="historian-post-date">' + date + '</span>' +
        (excerpt ? '<p class="historian-post-excerpt">' + excerpt + '…</p>' : '') +
        '</div>';
    });
    container.innerHTML = html;
  })
  .catch(function(err) {
    document.getElementById('historian-posts').style.display = 'none';
    document.getElementById('historian-error').style.display = 'block';
  });
</script>
