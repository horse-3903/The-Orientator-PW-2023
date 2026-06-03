<div align="center">

# The Orientator PW 2023

A DialoGPT-powered Discord bot that helps Hwa Chong Institution freshmen navigate school life — the original 2023 project work version

[![Python](https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=python&logoColor=white)](https://www.python.org/)
[![Discord](https://img.shields.io/badge/Discord-5865F2?style=flat-square&logo=discord&logoColor=white)](https://discord.com/)
[![HuggingFace](https://img.shields.io/badge/HuggingFace-FFD21E?style=flat-square&logo=huggingface&logoColor=black)](https://huggingface.co/)
[![Last Commit](https://img.shields.io/github/last-commit/horse-3903/The-Orientator-PW-2023?style=flat-square)](../../commits)

</div>

---

## Overview

The Orientator PW 2023 is the original version of The Orientator, developed as a Project Work (PW) assignment in 2023. It is a Discord bot powered by a locally fine-tuned **DialoGPT** model, trained on HCI-specific Q&A data to assist freshmen with questions about school culture, traditions, and important dates.

This project is the direct predecessor to [The Orientator 2.0](https://github.com/horse-3903/The-Orientator-2.0), which replaced the Discord interface with a web app and upgraded the AI backend from DialoGPT to Google Gemini.

## Features

- **Fine-tuned DialoGPT model** — locally trained causal language model on HCI-specific question-and-answer data
- **Private ticket channels** — users click a button to create a private Discord channel for one-on-one interaction with the bot
- **ISP event lookup** — `?date` command retrieves school event details from a scraped ISP-HS calendar
- **Conversation history** — per-user chat history stored in SQLite for multi-turn dialogue
- **Data augmentation pipeline** — scripts to expand the base dataset from ~81 rows to ~6.7M rows for model training

## Tech Stack

[![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://www.python.org/)
[![Discord](https://img.shields.io/badge/Discord%20(nextcord)-5865F2?style=for-the-badge&logo=discord&logoColor=white)](https://docs.nextcord.dev/)
[![HuggingFace](https://img.shields.io/badge/HuggingFace%20Transformers-FFD21E?style=for-the-badge&logo=huggingface&logoColor=black)](https://huggingface.co/docs/transformers)
[![SQLite](https://img.shields.io/badge/SQLite-003B57?style=for-the-badge&logo=sqlite&logoColor=white)](https://www.sqlite.org/)

## Getting Started

### Prerequisites

- Python 3.9+
- A Discord bot token (from the [Discord Developer Portal](https://discord.com/developers/applications))
- A trained DialoGPT model checkpoint stored locally
- HCI ISP-HS account (for the `?date` event scraper)

### Installation

1. Clone the repository:
   ```bash
   git clone https://github.com/horse-3903/The-Orientator-PW-2023.git
   cd The-Orientator-PW-2023
   ```

2. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```

3. Create the environment file at `src/.env`:
   ```env
   PARENT_DIR=<full path to The-Orientator-PW-2023/ folder>
   EXECUTABLE_PATH=<path to chromedriver.exe>
   ```

4. Create `src/bot.json` with your Discord bot token:
   ```json
   { "token": "YOUR_DISCORD_BOT_TOKEN" }
   ```

5. Set up ISP cookies for the event scraper:
   - Install the [Cookie Editor](https://chrome.google.com/webstore/detail/cookie-editor/) Chrome extension
   - Log into ISP-HS in your browser
   - Click the Cookie Editor extension and export cookies as JSON
   - Save the exported JSON to `src/isp_cookies.json`

### Training the Model

1. Ensure `src/data/base_data.csv` exists with HCI Q&A pairs
2. Run the augmentation script: `python src/data-collection/augment_data.py`
3. Open `src/model/train_model.ipynb` and run all cells to fine-tune the model

### Running the Bot

```bash
python src/bot/main.py
```

Invite your bot to a Discord server and send the intro message to a channel. Users can then click **Create Ticket** to open a private conversation.

### Bot Commands

| Command | Description |
|---------|-------------|
| `?date <event name>` | Look up the date and duration of a school event |
| *(any message in ticket channel)* | Receive an AI-generated answer about HCI |

## Project Structure

```
The-Orientator-PW-2023/
├── src/
│   ├── bot/
│   │   ├── main.py             # Discord bot entry point
│   │   ├── query_response.py   # DialoGPT inference and conversation history
│   │   └── get_isp_events.py   # ISP-HS event lookup
│   ├── data/
│   │   ├── base_data.csv       # Original HCI Q&A dataset (~81 rows)
│   │   ├── augmented_data.csv  # Augmented dataset (~6.7M rows)
│   │   ├── processed_data.csv  # Final training dataset (~2000 rows)
│   │   └── isp_events.json     # Scraped ISP event data
│   ├── data-collection/
│   │   ├── augment_data.py     # Data augmentation script
│   │   └── scrape_isp.py       # ISP-HS web scraper
│   └── model/
│       ├── train_model.ipynb   # Model fine-tuning notebook
│       └── test_model.py       # Model inference testing
```

## Differences from The Orientator 2.0

| Feature | PW 2023 (this repo) | 2.0 |
|---------|---------------------|-----|
| Interface | Discord bot | Web app (Flask) |
| AI model | Fine-tuned DialoGPT | Fine-tuned Google Gemini |
| School calendar | ISP-HS scraper + `?date` command | N/A |
| Deployment | Local / server process | Flask server |

## License

This project does not include a license file. All rights reserved unless otherwise stated.
