# Data Lake — Online Record Store | Universal Studios Colombia

## Project Overview

This repository contains the NoSQL database implementation for the online record store of Universal Studios Colombia. The project builds a Data Lake using **MongoDB** as the database engine, modeling album collections and song documents for six international and Latin American artists across different music genres.

The database (`disquera_universal`) follows a document-oriented model where each album is a MongoDB collection and each song is a document within its corresponding collection.

---

## Data Model

Each document (song) follows this structure:

```json
{
  "_id": "unique_identifier",
  "titulo": "Song title",
  "anio_salida": 2000,
  "autor": "Artist name",
  "id_imagen_portada": "images/album_cover.jpg",
  "genero": "Music genre",
  "duracion_seg": 240,
  "numero_pista": 1,
  "popularidad": 95
}
```

**Mandatory fields:** `_id`, `titulo`, `anio_salida`, `autor`, `id_imagen_portada`  
**Optional fields:** `genero`, `duracion_seg`, `numero_pista`, `popularidad`

---

## Collections

| Collection | Artist | Album | Year |
|---|---|---|---|
| `lady_gaga_the_fame` | Lady Gaga | The Fame | 2008 |
| `diomedes_diaz_de_nuevo_con_mi_gente` | Diomedes Díaz | De Nuevo con Mi Gente | 1988 |
| `bts_map_of_the_soul_7` | BTS | Map of the Soul: 7 | 2020 |
| `michael_jackson_thriller` | Michael Jackson | Thriller | 1982 |
| `queen_a_night_at_the_opera` | Queen | A Night at the Opera | 1975 |
| `ivy_queen_sentimiento` | Ivy Queen | Sentimiento | 2003 |

---

## Tasks Summary

### Task 1 — Initial Database (branch: `main`)
Created the base database with 7 songs per artist from their most representative album. Each album was modeled as a separate MongoDB collection. Album cover images were collected and stored in the `images/` folder. Full exports were generated in both CSV and JSON formats.

### Task 2 — Album Updates (branch: `actualizaciones`)
Added lesser-known tracks to each album collection, expanding the dataset with B-side and deep cut songs. Data integrity was verified after each insertion and updated exports were generated.

### Task 3 — Record Deletion (branch: `eliminaciones`)
Performed deletion operations: removed at least two songs from each album and dropped the complete collection of one artist. Final exports reflect the resulting state of the database.

---

## Design Decisions

- **One collection per album** rather than one collection per artist — this models the Data Lake structure more granularly and makes album-level queries more efficient.
- **String `_id` fields** with a readable prefix (e.g. `lg_tf_01`) instead of auto-generated ObjectIds — easier to reference and debug during development.
- **Optional fields included** (`genero`, `duracion_seg`, `numero_pista`, `popularidad`) to enrich the dataset and make it more realistic for a production Data Lake scenario.
- **Diomedes Díaz album choice:** "De Nuevo con Mi Gente" (1988) was selected over "La Cañaguatera" as it is a more complete and representative compilation of his classic vallenato work.

---

## Findings & Challenges

- MongoDB's schema-less nature made it easy to add optional fields without breaking existing documents — a key advantage over relational databases for this use case.
- Naming collections with special characters (accents, ñ) can cause issues in some export tools; we kept collection names in ASCII-friendly format.
- Exporting to CSV from Compass requires manually selecting fields — JSON exports are more complete and recommended for full fidelity restores.

---

## How to Replicate

### Prerequisites
- MongoDB Atlas account (free M0 cluster)
- MongoDB Compass installed
- Node.js or mongosh (optional, for shell operations)

### Steps

1. Connect to a MongoDB Atlas cluster via Compass.
2. Create a database named `disquera_universal`.
3. For each file in `scripts/`, create the corresponding collection and import the JSON file via **Add Data → Import JSON or CSV file** in Compass.
4. The `images/` folder contains album covers referenced by the `id_imagen_portada` field in each document.
5. To restore from exports, use the JSON files in `exports/bson/` via Compass import or `mongorestore`.

```bash
# Example restore with mongorestore
mongorestore --uri "mongodb+srv://user:password@cluster.mongodb.net" \
  --db disquera_universal exports/bson/
```
