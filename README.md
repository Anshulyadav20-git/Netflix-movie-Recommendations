 #Public app(frontend logic+fetch). 

async function fetchJSON(url) {
  const res = await fetch(url);
  if (!res.ok) throw new Error('Network error: ' + res.status);
  return res.json();
}

function createCard(movie, onClick) {
  const div = document.createElement('div');
  div.className = 'card';
  div.innerHTML = `
    <img src="${movie.poster}" alt="${escapeHtml(movie.title)} poster" />
    <div class="meta">
      <div class="title">${escapeHtml(movie.title)} <span style="float:right; font-weight:600;">${movie.rating ?? ''}★</span></div>
      <div class="small">${movie.genres.join(', ')} • ${movie.year}</div>
      <div style="margin-top:6px"><button class="btn">Recommend Similar</button></div>
    </div>
  `;
  div.querySelector('.btn').addEventListener('click', () => onClick(movie));
  return div;
}

function escapeHtml(s) {
  return String(s).replace(/[&<>"']/g, function (m) {
    return ({ '&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":'&#39;' }[m]);
  });
}

async function loadMovies() {
  try {
    const movies = await fetchJSON('/api/movies');
    const grid = document.getElementById('moviesGrid');
    grid.innerHTML = '';
    movies.forEach(m => {
      grid.appendChild(createCard(m, showRecommendations));
    });
  } catch (e) {
    console.error(e);
    document.getElementById('moviesGrid').innerHTML = `<div class="empty">Couldn't load movies.</div>`;
  }
}

async function showRecommendations(movie) {
  const recDiv = document.getElementById('recommendations');
  recDiv.innerHTML = `<div class="empty">Loading recommendations for <strong>${escapeHtml(movie.title)}</strong>…</div>`;
  try {
    const recs = await fetchJSON(`/api/recommendations/${movie.id}`);
    if (!recs || recs.length === 0) {
      recDiv.innerHTML = `<div class="empty">No recommendations found.</div>`;
      return;
    }
    recDiv.innerHTML = '';
    recs.forEach(r => {
      recDiv.appendChild(createCard(r, showRecommendations));
    });
    // scroll to recommendations
    recDiv.scrollIntoView({ behavior: 'smooth' });
  } catch (err) {
    console.error(err);
    recDiv.innerHTML = `<div class="empty">Error loading recommendations.</div>`;
  }
}

// initial load
loadMovies();