# MovieRecommenderApp
This project is a content-based filtering movie recommendation system. Based on the selected movie, the system suggests the five most similar movies. The similarity between movies is determined using TF-IDF (Term Frequency - Inverse Document Frequency) and Cosine Similarity.

Project Features

Processes movie data: Merges tmdb_5000_movies.csv and tmdb_5000_credits.csv datasets, cleans the data, and extracts relevant information.
Recommendation mechanism: Uses overview, genres, keywords, cast, and crew columns to generate a tags column for each movie.
Text processing: Performs stemming and tokenization on movie tags.
Machine learning algorithms: Utilizes CountVectorizer to extract the top 5000 words from the dataset.
Similarity calculation: Computes movie similarity using cosine_similarity.
Movie poster retrieval: Fetches movie posters using The Movie Database API (TMDb).
Web interface: Implements a Streamlit interface for user interaction.
Technologies Used

Python
Pandas
NumPy
Scikit-learn
NLTK
Streamlit
Requests (TMDb API)
Pickle (for model saving/loading)
Project Structure

├── app.py                 # Main Streamlit application
├── movies_dict.pkl         # Movie data in dictionary format
├── movies.pkl              # Processed movie dataset
├── similarity.pkl          # Cosine similarity matrix
├── tmdb_5000_movies.csv    # Raw movie dataset
├── tmdb_5000_credits.csv   # Raw credits dataset
├── requirements.txt        # Required libraries
Installation and Execution

1. Install Dependencies
Run the following command to install the required libraries:

pip install -r requirements.txt
2. Run the Streamlit Application
Execute the following command to start the Streamlit application:

streamlit run app.py
This will launch the application in a web browser at http://localhost:8501.

Working Mechanism

1. Data Preprocessing
The tmdb_5000_movies.csv and tmdb_5000_credits.csv datasets are merged, keeping only relevant columns such as movie_id, title, overview, genres, keywords, cast, crew.
Missing values are removed, and unnecessary columns are dropped.
Genres, keywords, cast, and crew columns are converted into lists.
Text processing techniques are applied:
Converts text to lowercase.
Performs stemming to reduce words to their root form.
Uses CountVectorizer to extract top 5000 frequent words.
2. Similarity Calculation
Cosine Similarity is used to measure the similarity between movies.
When a user selects a movie, the system retrieves the top 5 most similar movies.
3. Recommendation Display
The system fetches movie posters using the TMDb API.
The Streamlit interface displays the recommended movies along with their posters.
API Usage

This project utilizes The Movie Database (TMDb) API to fetch movie posters. To use the API, an API key is required. Follow these steps to obtain an API key:

Visit TMDb Developer.
Create a free account and generate an API key.
Update the api_key value in the fetch_poster(movie_id) function in the app.py file.
Development Stages

✔ Data Cleaning and Processing
✔ Text Vectorization (TF-IDF)
✔ Cosine Similarity for Movie Similarity Calculation
✔ Streamlit Web Interface Development
✔ API Integration for Movie Posters
✔ Testing and Debugging

Code Examples

Function to Fetch Movie Posters
import requests

def fetch_poster(movie_id):
    response = requests.get(
        f"https://api.themoviedb.org/3/movie/{movie_id}?api_key=YOUR_API_KEY&language=en-US"
    )
    data = response.json()
    return "https://image.tmdb.org/t/p/w500/" + data["poster_path"]
Movie Recommendation Function
def recommend(movie):
    movie_index = movies[movies["title"] == movie].index[0]
    distances = similarity[movie_index]
    movies_list = sorted(list(enumerate(distances)), reverse=True, key=lambda x: x[1])[1:6]
    
    recommended_movies = []
    recommended_movies_posters = []
    for i in movies_list:
        movie_id = movies.iloc[i[0]].movie_id
        recommended_movies.append(movies.iloc[i[0]].title)
        recommended_movies_posters.append(fetch_poster(movie_id))
    
    return recommended_movies, recommended_movies_posters
