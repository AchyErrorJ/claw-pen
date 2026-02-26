# Lead Hunter Agent

Automated lead finder for And or Design (BCIN permit services).

## Purpose

Scrapes online sources to find clients looking for permit drawings in Northern Ontario:
- Sudbury
- North Bay
- Timmins
- Sault Ste. Marie

## Current Features

- ✅ **Kijiji Scraper** - Searches renovation/permit-related listings
- 🔲 Facebook Groups - Stub (future)
- 🔲 Municipal Permits - Stub (future)

## Installation

```bash
cd /data/claw-pen/agents/lead-hunter
pip install -r requirements.txt
```

## Usage

### Run Once
```bash
python main.py
```

### Dry Run (Test Mode)
```bash
python main.py --dry-run
```

### Scheduled Mode
```bash
python main.py --schedule
```

### With Custom Config
```bash
python main.py --config /path/to/custom-agent.toml
```

## Configuration

Edit `agent.toml` to customize:

```toml
[agent]
name = "lead-hunter"
org = "andor-design"

[search]
locations = ["Sudbury", "North Bay", "Timmins", "Sault Ste Marie"]
keywords = ["permit drawings", "architect", "BCIN", "basement apartment"]

[notifications]
whatsapp = true
channel = "webchat"
recipient = "Jer"

[schedule]
cron = "0 6 * * *"  # 6 AM daily
```

## Output

### Logs
Logs are written to `logs/lead-hunter.log`

### Memory
Seen leads are stored in `memory/leads.json` for deduplication.

### Notifications
Pending notifications are queued in `notifications/` directory.

When running within OpenClaw context, use `openclaw_runner.py` to send notifications via the message tool.

## Notification Format

```
🏠 New Lead: [Title]
📍 Location: [City]
🔗 Link: [URL]
💰 Budget: [if listed]
📝 Description: [snippet]
⏰ Posted: [time]
🔍 Source: kijiji
```

## Directory Structure

```
lead-hunter/
├── agent.toml          # Configuration
├── main.py             # Entry point
├── openclaw_runner.py  # OpenClaw integration
├── notifier.py         # Notification handler
├── requirements.txt    # Python dependencies
├── README.md           # This file
├── sources/
│   ├── __init__.py
│   ├── kijiji.py       # Kijiji scraper
│   ├── facebook.py     # Facebook stub
│   └── municipal.py    # Municipal stub
├── logs/               # Runtime logs
├── memory/             # Lead storage
└── notifications/      # Pending notifications
```

## Development

### Adding New Sources

1. Create a new scraper in `sources/`
2. Implement the `search()` generator that yields lead dicts
3. Import and call in `main.py`
4. Update `sources/__init__.py`

### Lead Data Format

```python
{
    "title": str,
    "url": str,
    "location": str,
    "description": str,
    "budget": Optional[str],
    "posted_time": Optional[str],
    "source": str,
}
```

## Notes

- Be respectful of rate limits when scraping
- Kijiji may block automated access; use appropriate delays
- Check Terms of Service before implementing new scrapers
- Municipal permit data may require FOI requests in some cases
