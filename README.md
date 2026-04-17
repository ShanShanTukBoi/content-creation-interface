# Content Creation Interface (GitHub Issues → Blog Content)

## Overview

This repository demonstrates a content creation workflow for a simple travel blog powered entirely by GitHub Issues and Actions.

Content is authored through structured GitHub Issue templates. When an issue is created, GitHub Actions process the input and transform it into structured JSON files stored in the repository. These files are then used as the data source for the blog on deployment.

The system supports:
- Managing travel locations
- Creating posts associated with those locations
- Automatically generating a consolidated dataset for the frontend

---

## Core Concepts

### 1. Locations (`locations.json`)

The `locations.json` file stores all available locations for the trip.

Each location contains:
- `name`
- `latitude`
- `longitude`

#### Adding a Location

Locations are added via a GitHub Issue using the `location.yml` template.

When a location issue is created:
1. A GitHub Action validates and extracts the submitted data
2. `locations.json` is updated with the new location
3. `trip.json` is regenerated to reflect the updated data structure

---

### 2. Posts (`/posts/*.json`)

Posts represent individual pieces of content tied to a specific location.

Each post includes:
- `title`
- `text` (text body)
- `photos` (array of image URLs)
- `location` (selected from available locations)

#### Creating a Post

Posts are created via a GitHub Issue using the `post.yml` template.

Key details:
- The location field is dynamically populated based on `locations.json`
- Each issue submission represents a single post

When a post issue is created:
1. A GitHub Action converts the issue into a JSON file under `/posts/`
2. `trip.json` is regenerated to include the new post

---

### 3. Aggregated Data (`trip.json`)

`trip.json` is the final, consolidated data source used by the blog frontend.

Structure:
- A list of locations
- Each location contains a `posts` array
- Posts are grouped under their associated location

Example structure:

```json
[
  {
    "name": "Paris",
    "latitude": 48.8566,
    "longitude": 2.3522,
    "posts": [
      {"id": "...", "title": "...", "text": "...", "photos": [...], "date": "..." }
    ]
  }
]
```

### 4. Utterances

Note that the issue ID of each post made is stored within the `trips.json` file, making it trivial to integrate Utterances for providing user comment sections.