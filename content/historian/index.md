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
fetch('https://api.rss2json.com/v1/api.json?rss_url=https%3A%2F%2Fbillmunn.substack.com%2Ffeed&count=20')
  .then(function(r) { return r.json(); })
  .then(function(data) {
    var container = document.getElementById('historian-posts');
    if (!data.items || data.items.length === 0) {
      throw new Error('No items');
    }
    container.innerHTML = data.items.map(function(item) {
      var date = new Date(item.pubDate).toLocaleDateString('en-US', {
        year: 'numeric', month: 'long', day: 'numeric'
      });
      var tmp = document.createElement('div');
      tmp.innerHTML = item.description || '';
      var excerpt = tmp.textContent.slice(0, 220).trim();
      return '<div class="historian-post">' +
        '<a href="' + item.link + '" target="_blank" rel="noopener" class="historian-post-title">' + item.title + '</a>' +
        '<span class="historian-post-date">' + date + '</span>' +
        (excerpt ? '<p class="historian-post-excerpt">' + excerpt + '…</p>' : '') +
        '</div>';
    }).join('');
  })
  .catch(function(err) {
    document.getElementById('historian-posts').style.display = 'none';
    document.getElementById('historian-error').style.display = 'block';
  });
</script>
