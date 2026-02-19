# 🕷️ n8n Crawler Bot

An AI-powered web crawler bot built with n8n that extracts all URLs from any webpage and delivers the results directly to you via Telegram.

## 🚀 How It Works

1. **Send a message** to the Telegram bot containing a URL you want to crawl.
2. An **AI Agent** (powered by OpenRouter) extracts the clean URL from your message.
3. An **HTTP Request** fetches the raw HTML of the target page.
4. If the page is inaccessible (anti-bot protection, restricted domain, etc.), the bot sends you an error message.
5. A **JavaScript code node** parses the HTML and extracts all URLs — links, images, iframes, and meta URLs.
6. An **Information Extractor** (LLM-powered) filters the results, keeping only relevant HTTP links and removing duplicates, scripts, and stylesheets.
7. The cleaned URL data is **saved to a Google Sheet** (url, url_name, type).
8. The sheet data is **converted to a CSV file** and sent back to you on Telegram as a downloadable document.

## 🔧 Workflow Nodes

| Node | Purpose |
|---|---|
| Telegram Trigger | Listens for incoming messages |
| AI Agent | Extracts the URL from the user's message |
| HTTP Request | Fetches the webpage HTML |
| If | Checks if HTML was retrieved successfully |
| Code in JavaScript | Parses HTML and extracts all URLs using regex |
| Information Extractor | LLM filters and structures the URL data |
| Split Out / Split Out1 | Splits the array into individual rows |
| Append or update row in sheet | Saves results to Google Sheets |
| Wait | Waits for all rows to be written |
| Convert to File | Converts sheet data to CSV |
| Download file | Downloads the CSV from Google Drive |
| Send a document | Sends the CSV file back to the user on Telegram |
| Send a text message | Sends an error message if scraping fails |

## 📋 Prerequisites

- [n8n](https://n8n.io/) instance (self-hosted or cloud)
- **Telegram Bot** — create one via [@BotFather](https://t.me/BotFather)
- **OpenRouter API key** — sign up at [openrouter.ai](https://openrouter.ai)
- **Google Account** — for Google Sheets and Google Drive integration

## ⚙️ Setup

1. **Import the workflow** — import `crawler.json` into your n8n instance.

2. **Configure credentials** — add the following credentials in n8n:
   - `Telegram` — your bot token
   - `OpenRouter` — your API key
   - `Google Sheets OAuth2` — your Google account
   - `Google Drive OAuth2` — your Google account

3. **Set up the Google Sheet** — create a Google Sheet with columns `url`, `url_name`, and `type`, then update the Sheet ID in both the **Append or update row** and **Download file** nodes.

4. **Activate the workflow** — toggle the workflow to active and start chatting with your bot.

## 💬 Usage

Simply send a message to your Telegram bot with a URL:

```
Crawl this site: https://example.com
```

or just:

```
https://example.com
```

The bot will reply with a `.csv` file containing all the URLs found on that page.

## ⚠️ Error Handling

If the bot cannot scrape a website (e.g., due to anti-bot protections, CAPTCHA, or legal/access restrictions), it will reply with:

> ⚠️ Error: I am unable to scrape this website. It may have anti-bot protections, or it might be illegal/restricted to crawl this specific domain.

## 📄 License

MIT License — see [LICENSE](./LICENSE) for details.
