# alx-project-0x14

A simple TypeScript/Node.js project demonstrating consumption of the TMDb (The Movie Database) API: sending requests, parsing responses, and defining type-safe data models.

## API Overview

The TMDb API provides a comprehensive RESTful interface for retrieving movie, TV show, and celebrity data, including:

- Search and discovery of movies, TV shows, and people  
- Detailed resource queries (e.g., movie details, cast & crew, images, videos)  
- Trending, popular, now-playing, and upcoming content  
- Support for filters like language, region, dates, and pagination :contentReference[oaicite:1]{index=1}

## Version

The API's current stable version is **v3** (with v4 in progress) :contentReference[oaicite:2]{index=2}.

## Available Endpoints

Main endpoints include:

- `GET /search/movie` – Search movies by keyword  
- `GET /movie/{movie_id}` – Retrieve detailed information about a movie  
- `GET /movie/{movie_id}/credits` – Fetch cast and crew for a movie  
- `GET /movie/{movie_id}/videos` – Fetch trailers, clips, etc.  
- `GET /movie/{movie_id}/similar`, `.../recommendations` – Fetch related movies  
- `GET /movie/popular`, `/now_playing`, `/upcoming`, `/top_rated`, `/latest` – Curated lists for dynamic content :contentReference[oaicite:3]{index=3}  
- Additional endpoints exist for TV, people, genres, collections, keywords, and more :contentReference[oaicite:4]{index=4}

## Request and Response Format

### Example Request:

```http
GET https://api.themoviedb.org/3/movie/550
  ?api_key=YOUR_API_KEY
  &language=en-US
Example Response:
json
Copy code
{
  "id": 550,
  "title": "Fight Club",
  "overview": "...",
  "release_date": "1999-10-15",
  "genres": [{ "id": 18, "name": "Drama" }],
  "poster_path": "/a26cQPRhJPX6GbWfQbvZdrrp9j9.jpg",
  "runtime": 139,
  "credits": { /* cast & crew JSON nested if appended */ }
}
TypeScript Interfaces:
ts
Copy code
interface Genre { id: number; name: string; }
interface MovieDetail {
  id: number;
  title: string;
  overview: string;
  release_date: string;
  genres: Genre[];
  poster_path: string;
  runtime: number;
}

## Authentication

All requests require your API key, provided via query parameter:

ruby
Copy code
?api_key=YOUR_API_KEY
Optionally, you can authenticate via bearer token for v4 API requests 
zuplo.com
+8
developer.themoviedb.org
+8
omdbapi.com
+8
.

## Error Handling

Common HTTP errors include:

401 Unauthorized – Invalid or missing API key

404 Not Found – Invalid resource identifier

429 Too Many Requests – Rate limit exceeded

500 Series – Server-side errors

Typical API error responses look like:

json
Copy code
{ "status_code": 34, "status_message": "The resource you requested could not be found." }
Check response status codes and handle each case appropriately, especially retrying on 429.

## Usage Limits and Best Practices

Rate Limit: ~40 requests every 10 seconds 

Batching & Pagination: Use paginated endpoints and append_to_response to reduce calls

Caching: Store frequently accessed data (e.g., movie details) to curb API usage

Backoff Strategy: Implement exponential backoff when hitting rate limits

Security: Securely manage API keys via environment variables

Type Safety: Define TypeScript types for API contracts and utilize libraries that support TMDb