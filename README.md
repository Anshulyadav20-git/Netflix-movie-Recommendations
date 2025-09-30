 netflix-recommendations/
│── index.html
│── style.css
│── script.js

<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Netflix Movie Recommendations</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>
  <!-- Navbar -->
  <header class="navbar">
    <div class="logo">NETFLIX</div>
    <nav>
      <a href="#">Home</a>
      <a href="#">Trending</a>
      <a href="#">Genres</a>
      <a href="#">My List</a>
    </nav>
  </header>

  <!-- Search -->
  <section class="search-container">
    <input type="text" id="searchInput" placeholder="Enter a movie name...">
    <button onclick="getRecommendations()">Get Recommendations</button>
  </section>

  <!-- Recommendations -->
  <section>
    <h2 style="padding-left:20px;">Recommended Movies</h2>
    <div id="recommendations" class="movie-list"></div>
  </section>

  <footer>
    <p>Netflix Movie Recommendations © 2025</p>
  </footer>

  <script src="script.js"></script>
</body>
</html>

body {
  margin: 0;
  font-family: Arial, sans-serif;
  background-color: #141414;
  color: #fff;
}

/* Navbar */
.navbar {
  display: flex;
  justify-content: space-between;
  align-items: center;
  background-color: #000;
  padding: 15px 30px;
}

.logo {
  font-size: 28px;
  font-weight: bold;
  color: #e50914;
}

.navbar nav a {
  color: #fff;
  text-decoration: none;
  margin-left: 20px;
}

.navbar nav a:hover {
  color: #e50914;
}

/* Search */
.search-container {
  text-align: center;
  margin: 20px;
}

.search-container input {
  padding: 10px;
  width: 300px;
  border: none;
  border-radius: 5px;
}

.search-container button {
  padding: 10px 15px;
  margin-left: 10px;
  background: #e50914;
  border: none;
  color: #fff;
  border-radius: 5px;
  cursor: pointer;
}

.search-container button:hover {
  background: #b20710;
}

/* Movie grid */
.movie-list {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(180px, 1fr));
  gap: 15px;
  padding: 20px;
}

.movie-card {
  background: #222;
  border-radius: 10px;
  overflow: hidden;
  transition: transform 0.3s;
  cursor: pointer;
}

.movie-card:hover {
  transform: scale(1.05);
}

.movie-card img {
  width: 100%;
  height: 270px;
  object-fit: cover;
}

.movie-card h3 {
  text-align: center;
  padding: 10px;
  font-size: 16px;
}

/* Footer */
footer {
  text-align: center;
  background: #000;
  padding: 15px;
  margin-top: 20px;
  font-size: 14px;
}

// Dummy dataset: movie → recommended movies
const recommendations = {
  "inception": [
    { title: "Interstellar", img: "https://m.media-amazon.com/images/I/81kTYPwS0-L._AC_SL1500_.jpg" },
    { title: "The Matrix", img: "https://m.media-amazon.com/images/I/51EG732BV3L._AC_.jpg" },
    { title: "Tenet", img: "https://m.media-amazon.com/images/I/81S2g1+6L5L._AC_SL1500_.jpg" }
  ],
  "avengers": [
    { title: "Avengers: Endgame", img: "https://m.media-amazon.com/images/I/81ExhpBEbHL._AC_SY679_.jpg" },
    { title: "Iron Man", img: "https://m.media-amazon.com/images/I/81Cq4VDXj+L._AC_SL1500_.jpg" },
    { title: "Thor: Ragnarok", img: "https://m.media-amazon.com/images/I/91w1xQKQ8eL._AC_SL1500_.jpg" }
  ],
  "joker": [
    { title: "The Dark Knight", img: "https://m.media-amazon.com/images/I/51l5XzLrC4L._AC_SY445_.jpg" },
    { title: "V for Vendetta", img: "https://m.media-amazon.com/images/I/81tGp2bG0jL._AC_SY679_.jpg" },
    { title: "Logan", img: "https://m.media-amazon.com/images/I/81oXW4VYxzL._AC_SL1500_.jpg" }
  ]
};

// Function to show recommendations
function getRecommendations() {
  const query = document.getElementById("searchInput").value.toLowerCase();
  const movieList = document.getElementById("recommendations");
  movieList.innerHTML = "";

  if (recommendations[query]) {
    recommendations[query].forEach(movie => {
      const card = document.createElement("div");
      card.classList.add("movie-card");

      card.innerHTML = `
        <img src="${movie.img}" alt="${movie.title}">
        <h3>${movie.title}</h3>
      `;

      movieList.appendChild(card);
    });
  } else {
    movieList.innerHTML = "<p style='padding:20px;'>No recommendations found. Try 'Inception', 'Avengers', or 'Joker'.</p>";
  }
}
