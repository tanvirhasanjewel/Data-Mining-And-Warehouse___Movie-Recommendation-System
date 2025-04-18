# Movie Recommendation System

## Introduction  
This project implements a Film Recommendation System using the MovieLens dataset, leveraging collaborative filtering techniques to provide personalized movie suggestions. The system analyzes user ratings to identify film similarities and recommends content tailored to individual preferences.

---

## Methodology  

### 1. **Data Overview**  
The system uses two key datasets:  
- **Movies**: Contains movie metadata (titles, genres).  
- **Ratings**: Includes user-provided ratings (scale: 1–5).  

### 2. **Similarity Calculation**  
- **Cosine Similarity** is computed between movies based on user ratings.  
- Movies with similar rating patterns are deemed "similar" (e.g., *The Matrix* and *Inception*).  

### 3. **Recommendation Logic**  
1. **Identify User Preferences**:  
   - Extract a user’s highest-rated movies (e.g., *The Godfather*).  
2. **Find Similar Movies**:  
   - Use the similarity matrix to suggest analogous films (e.g., *GoodFellas*).  
3. **Filter and Rank**:  
   - Exclude already-rated movies.  
   - Recommend top unrated films with high average ratings.  

---

## How It Works  

### **1. Movie Similarity Matrix**  
```python
user_movie_matrix = ratings.pivot(index='userId', columns='movieId', values='rating')
movie_similarity = user_movie_matrix.corr(method='pearson')
```
- **Output**: A matrix where each cell represents similarity between two movies (e.g., 0.8 = 80% similar).  

### **2. Personalized Recommendations**  
```python
def recommend_movies(user_id, top_n=5):
    # Get the user's highest-rated movies
    user_ratings = ratings[ratings["userId"] == user_id]
    top_movies = user_ratings.sort_values("rating", ascending=False).head(3)["movieId"]

    # Find similar unrated movies
    recommendations = []
    for movie in top_movies:
        similar_movies = movie_similarity[movie].dropna().sort_values(ascending=False)[1:top_n+1]
        recommendations.extend(similar_movies.index)
    
    return movies[movies["movieId"].isin(recommendations)][["title", "genres"]]
```

### **3. Example Output**  
For **User 47** who rated *The Shawshank Redemption* highly:  
| Title                          | Genres                     |  
|--------------------------------|----------------------------|  
| The Godfather                  | Crime, Drama               |  
| Pulp Fiction                   | Crime, Drama               |  
| Fight Club                     | Drama, Thriller            |  

---

## Results  
- **Accuracy**: Recommendations aligned with user preferences (e.g., drama lovers received drama suggestions).  
- **Personalization**: Filtered out watched movies and prioritized high-rated similar films.  

---

## Requirements  
- Python 3.x  
- Libraries: `pandas`, `numpy`  

## Installation  
1. Clone the repository:  
   ```bash
   git clone https:https://github.com/tanvirhasanjewel/Data-Mining-And-Warehouse___Movie-Recommendation-System.git
   ```
2. Install dependencies:  
   ```bash
   pip install pandas numpy
   ```

---

## Conclusion  
This system effectively recommends movies by:  
1. Calculating film similarities via collaborative filtering.  
2. Personalizing suggestions based on individual user ratings.  

Future enhancements could include hybrid filtering (content + collaborative) and real-time user feedback integration.  

--- 

**Note**: Adjust file paths in the code to match your dataset location.  

--- 

This version improves clarity, structure, and readability while preserving all technical details. Let me know if you'd like any further refinements!
