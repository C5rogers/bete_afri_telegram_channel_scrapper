# Telegram Channel Scraper

This project uses `Telethon` to scrape rental-style posts from a Telegram channel, extract structured fields, download post images, and optionally repost the result to another Telegram channel.

The default source channel is:

`https://t.me/Bet_afri`

## What It Does

- Scrapes Telegram post history from a channel
- Filters and structures rental-related posts
- Extracts post text and images
- Exports structured JSON
- Generates Telegram-ready HTML assets
- Posts the extracted listings to a destination Telegram channel

## Project Files

- [`main.py`](./main.py): scraper entrypoint
- [`post_to_telegram.py`](./post_to_telegram.py): repost exported listings
- [`telegram_scrapper_service/scrape_channel.py`](./telegram_scrapper_service/scrape_channel.py): channel scraping logic
- [`telegram_scrapper_service/post_to_telegram.py`](./telegram_scrapper_service/post_to_telegram.py): Telegram posting logic
- [`telegram_scrapper_service/telegram_assets.py`](./telegram_scrapper_service/telegram_assets.py): post formatting and asset export

## Environment

Create or update `.env` with:

```env
TELEGRAM_CHANNEL=https://t.me/Bet_afri
TELEGRAM_TARGET_CHANNEL=@your_destination_channel
TELEGRAM_API_ID=123456
TELEGRAM_API_HASH=your_api_hash
TELEGRAM_BOT_TOKEN=your_bot_token
TELEGRAM_SOURCE_HANDLE=@yenekiray_ethio
```

Optional values:

- `TELEGRAM_SESSION_NAME`
- `TELEGRAM_OUTPUT_JSON`
- `TELEGRAM_OUTPUT_DIR`
- `TELEGRAM_POSTED_STATE_FILE`
- `DOWNLOAD_TELEGRAM_IMAGES`
- `TELEGRAM_SOURCE_HANDLE`

## Install

Activate your virtual environment, then install dependencies:

```bash
pip install -r requirements.txt
```

If you already have a project environment, use the one you normally run this repo with.

## Scrape Commands

Scrape the default channel:

```bash
python main.py
```

Scrape a specific channel:

```bash
python main.py --channel https://t.me/Bet_afri
```

Start from a specific message id:

```bash
python main.py --start-id 12345
```

Limit how many rental posts are exported:

```bash
python main.py --limit 20
```

Skip image downloads:

```bash
python main.py --no-download-images
```

Custom output paths:

```bash
python main.py --output telegram_rental_listings.json --assets-dir telegram_exports
```

## Post Commands

Post exported listings to a destination Telegram channel:

```bash
python post_to_telegram.py --channel @your_destination_channel
```

Post only a few items:

```bash
python post_to_telegram.py --channel @your_destination_channel --limit 3
```

Preview what would be posted without sending:

```bash
python post_to_telegram.py --channel @your_destination_channel --dry-run
```

Repost everything from the input file:

```bash
python post_to_telegram.py --channel @your_destination_channel --force
```

If you want to post through the bot account configured in `.env`:

```bash
python post_to_telegram.py --channel @your_destination_channel --use-bot
```

## Output

The scraper writes:

- Structured JSON listing data
- Per-post HTML files for Telegram formatting
- Downloaded post images

The exported records are stored under:

`telegram_exports/<listing_family>/<listing_folder>/<listing_id>/`

## Notes

- `--start-id` is useful when you want to resume history scraping from a known message id.
- `--limit` is useful when testing a small portion of the channel history.
- The Telegram bot must be an admin in the destination channel if you post with `--use-bot`.
- The first run may create a Telethon session file for authentication.
