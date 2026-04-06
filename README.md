# ⚽ World Cup Database

A data engineering project that automates the creation, schema definition, and population of a relational PostgreSQL database using FIFA World Cup match data.

## 📊 Overview
This project demonstrates the transition from raw CSV data into a fully normalized relational database. It features:
* **Automated Ingestion:** A Bash script that processes data with duplicate prevention.
* **Relational Mapping:** Clean separation between teams and match statistics.
* **Data Analysis:** A suite of SQL queries for tournament insights.

## 🛠️ Tech Stack
* **Database:** PostgreSQL
* **Scripting:** Bash (Shell)
* **Data Source:** `games.csv` (2014 & 2018 Tournament Data)

---

## 📁 Project Structure
* **`worldcup.sql`**: Database dump containing the schema definition and constraints.
* **`insert_data.sh`**: Script to read the CSV and populate the database using `ON CONFLICT` logic.
* **`queries.sh`**: SQL scripts for data extraction and statistics.
* **`games.csv`**: The raw dataset.

---

## ⚙️ Database Schema

### `teams` Table
| Column | Type | Constraints |
| :--- | :--- | :--- |
| `team_id` | SERIAL | PRIMARY KEY |
| `name` | VARCHAR(20) | UNIQUE, NOT NULL |

### `games` Table
| Column | Type | Constraints |
| :--- | :--- | :--- |
| `game_id` | SERIAL | PRIMARY KEY |
| `year` | INTEGER | NOT NULL |
| `round` | VARCHAR(20) | NOT NULL |
| `winner_id` | INTEGER | FOREIGN KEY (teams.team_id) |
| `opponent_id` | INTEGER | FOREIGN KEY (teams.team_id) |
| `winner_goals` | INTEGER | NOT NULL |
| `opponent_goals` | INTEGER | NOT NULL |

---

## 🚀 Getting Started

### Prerequisites
* PostgreSQL installed and running.
* Bash terminal (Linux/macOS/Git Bash).

### Installation & Setup

1.  **Rebuild the Database:**
    ```bash
    psql -U postgres < worldcup.sql
    ```

2.  **Run the Ingestion Script:**
    ```bash
    chmod +x insert_data.sh
    ./insert_data.sh
    ```

3.  **Execute Analysis Queries:**
    ```bash
    chmod +x queries.sh
    ./queries.sh
    ```

---

## 🔍 Key SQL Implementations
The `queries.sh` script utilizes advanced SQL techniques:
* **Aggregations:** `SUM()` and `AVG()` for goal statistics.
* **Joins:** `INNER JOIN` to map `team_id` to readable names.
* **Set Theory:** `UNION` to find unique teams across winner and opponent columns.
* **Filtering:** Pattern matching with `LIKE`.

---

### Technical Challenge: Data Integrity
To ensure a clean relational mapping, the ingestion script uses:
`INSERT INTO teams(name) VALUES('$TEAM') ON CONFLICT(name) DO NOTHING`
This prevents duplicate team entries and ensures foreign keys in the `games` table always point to valid, unique IDs.
