# 🎬 Movie Recommendation System

A content-based movie recommendation system built in Python, trained on the TMDB 5000 Movies dataset, and deployed as an interactive web application using Streamlit.

**Author:** Alankrit Sajwan | **Reg. No:** 25BAI10512

---

## 📌 Overview

This system analyses multiple content features of each movie — including plot overview, genres, keywords, cast, and director — and uses NLP techniques along with cosine similarity to recommend five movies most similar to any given selection.

---

## 🗂️ Project Structure

```
movie-recommender/
│
├── movie-recommender.ipynb   # Training pipeline: preprocessing, vectorization, similarity computation
├── app.py                    # Streamlit web application
├── movie.pkl                 # Serialized movie DataFrame (1,494 movies)
├── similarity.pkl            # Pre-computed 1494×1494 cosine similarity matrix
├── tmdb_5000_movies.csv      # Raw TMDB movie metadata
└── tmdb_5000_credits.csv     # Raw TMDB cast and crew data
```

---

## 🚀 Features

- Content-based filtering using movie metadata (genres, keywords, cast, director, overview)
- NLP pipeline with tokenization, stemming (Porter Stemmer), and CountVectorizer
- Cosine similarity computed across 1,494 movies
- Interactive Streamlit UI with a dropdown selector
- Live movie poster fetching via the TMDB API

---

## 🛠️ Tech Stack

| Library / Tool   | Version   | Purpose                                              |
|------------------|-----------|------------------------------------------------------|
| Python           | 3.12.5    | Core programming language                            |
| pandas           | Latest    | Data loading, merging, cleaning                      |
| NumPy            | Latest    | Numerical computation                                |
| ast              | Built-in  | Parsing stringified Python dict/list columns         |
| NLTK             | Latest    | Porter Stemmer for word stemming                     |
| scikit-learn     | Latest    | CountVectorizer and cosine_similarity                |
| Streamlit        | Latest    | Web application framework                            |
| requests         | Latest    | HTTP GET calls to TMDB API                           |
| pickle           | Built-in  | Serialization of model artifacts                     |
| Jupyter Notebook | Latest    | Interactive training environment                     |
| VS Code          | Latest    | Primary IDE                                          |

---

## ⚙️ Installation & Setup

### 1. Clone the repository
```bash
git clone https://github.com/your-username/movie-recommender.git
cd movie-recommender
```

### 2. Create and activate a virtual environment
```bash
python -m venv venv
source venv/bin/activate        # On Windows: venv\Scripts\activate
```

### 3. Install dependencies
```bash
pip install pandas numpy nltk scikit-learn streamlit requests jupyter
```

### 4. Download NLTK data
```python
import nltk
nltk.download('punkt')
```

### 5. Add your TMDB API Key

In `app.py`, replace `<KEY>` in the `fetch_poster()` function with your TMDB API key:
```python
url = 'https://api.themoviedb.org/3/movie/{}?api_key=YOUR_API_KEY'.format(movie_id)
```
> Get a free API key at [https://www.themoviedb.org/settings/api](https://www.themoviedb.org/settings/api)

---

## 🧠 Training the Model

Open and run all cells in the Jupyter Notebook:
```bash
jupyter notebook movie-recommender.ipynb
```

This will:
1. Load and merge the two TMDB CSV files
2. Parse JSON-encoded columns (genres, keywords, cast, crew)
3. Normalize multi-word tokens and apply Porter stemming
4. Vectorize using `CountVectorizer` (5,000 features, English stop words removed)
5. Compute a 1494×1494 cosine similarity matrix
6. Export `movie.pkl` and `similarity.pkl`

---

## ▶️ Running the App

```bash
streamlit run app.py
```

Then open your browser and navigate to:
```
Local URL:   http://localhost:8501
Network URL: http://192.168.x.x:8501
```

---

## 🔄 Data Preprocessing Pipeline

```
Raw CSVs → Merge on title → Drop nulls → Parse JSON columns
       → Normalize multi-word tokens → Tokenize overview
       → Concatenate into 'tags' → Stem → Vectorize → Cosine Similarity
```

### Dataset Stats

| Property                     | Value                                      |
|------------------------------|--------------------------------------------|
| Raw rows (before cleaning)   | 4,809                                      |
| Rows after dropna()          | 4,806                                      |
| Rows in final model          | 1,494                                      |
| Vocabulary size              | 5,000 features                             |
| Similarity matrix shape      | 1,494 × 1,494                              |
| Features used                | overview, genres, keywords, cast, director |

---

## 📐 How It Works

1. Features (overview, genres, keywords, top-3 cast, director) are merged into a `tags` column per movie.
2. Multi-word tokens like `"Science Fiction"` are collapsed to `"ScienceFiction"` to prevent false co-occurrences.
3. Words are stemmed using Porter Stemmer (`"acting"` → `"act"`).
4. `CountVectorizer` produces a sparse `(1494, 5000)` matrix.
5. Pairwise cosine similarity scores are computed for all movie pairs.
6. For any selected movie, the top 5 most similar movies are returned.

---

## 🧪 Sample Output

```python
recommend('Batman Begins')
# → Kung Fu Panda
# → Kung Fu Panda 2
# → The Peanuts Movie
# → Big Trouble in Little China
# → Khumba
```

---

## ⚠️ Known Limitations

- **Dataset reduction:** Only 1,494 of the original 4,809 movies are retained after the inner merge and null removal.
- **SettingWithCopyWarning:** Some `.apply()` operations raise pandas warnings (non-breaking). Future fix: use `.loc[]` for chained assignments.
- **No personalization:** The system is purely content-based and does not factor in user ratings or viewing history.

---

## 🔮 Future Enhancements

- **Collaborative Filtering** — SVD/ALS using explicit user rating data
- **Hybrid Recommender** — Combine content-based and collaborative filtering
- **TF-IDF / Word Embeddings** — Replace CountVectorizer with TF-IDF, Word2Vec, or BERT
- **Larger Dataset** — Scale to the full TMDB dataset (500,000+ entries)
- **Cloud Deployment** — Streamlit Community Cloud, Heroku, AWS EC2, or Docker
- **REST API** — Expose the engine via FastAPI or Flask
- **Advanced Filters** — Genre, release year, language, and rating filters

---

## 📚 References

1. [TMDB 5000 Movie Dataset — Kaggle](https://www.kaggle.com/datasets/tmdb/tmdb-movie-metadata)
2. [TMDB API Documentation](https://developers.themoviedb.org/3)
3. [Scikit-learn Documentation](https://scikit-learn.org/stable/)
4. [NLTK Documentation — Porter Stemmer](https://www.nltk.org/)
5. [Streamlit Documentation](https://docs.streamlit.io/)
6. [pandas Documentation](https://pandas.pydata.org/docs/)
7. Ricci, F., Rokach, L., & Shapira, B. (2015). *Recommender Systems Handbook*. Springer.
8. Salton, G., & McGill, M. J. (1983). *Introduction to Modern Information Retrieval*. McGraw-Hill.

---

## 📄 License

This project is for academic purposes. See your institution's guidelines for usage and distribution.
