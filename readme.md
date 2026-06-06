Here is the complete, end-to-end roadmap to get your Telegram smart inbox fully operational. Follow these phases in order.

Phase 1: Gather Your API Tokens
Before touching code, you need the keys to the castle.

Telegram Bot Token: Open Telegram, search for @BotFather, send /newbot, follow the prompts, and copy the HTTP API Token.

AI API Key: Log into your OpenAI developer dashboard (or Anthropic/Gemini) and generate a new secret API key.

Phase 2: Set Up Your VS Code Workspace
Ensure your project folder contains the exact files we fixed earlier.

Create a folder named telegram-automation and open it in VS Code.

Create your .env file and paste your tokens:

Code snippet
TELEGRAM_BOT_TOKEN=your_telegram_bot_token_here
OPENAI_API_KEY=your_openai_api_key_here
3. Create your `docker-compose.yml` file (using the updated `n8nio/n8n:latest` image).
4. Create your `telegram-workflow.json` blueprint file.

---

## Phase 3: Launch n8n Infrastructure
1. Open the integrated terminal in VS Code (`Ctrl+`` or `Cmd+``).
2. Run the command:
   ```bash
docker compose up -d
Open your browser and go to http://localhost:5678.

Create your owner account (Choose an admin email and password—keep note of these!).

Phase 4: Import and Authenticate the Workflow
In the n8n dashboard, go to Workflows on the left menu.

Click the three dots (...) in the top right corner and select Import from File.

Choose the telegram-workflow.json file from your project folder.

Configure Telegram: Double-click the Telegram Trigger node. Under "Credential for Telegram API", click Create New Credential, paste your token from .env, and save.

Configure AI: Double-click the AI Parser node. Look for the connected OpenAI Chat Model sub-node, click Create New Credential, paste your OpenAI API key, and save.

Phase 5: Wire the Destinations (Calendar & Notes)
Now, finish the open-ended connections coming out of the Switch Node.

For the Calendar Path (Output 0):
Hover over the top output dot (0) of the Switch node and add a Google Calendar node.

Set the Operation to Create an Event.

Authenticate your Google Account via OAuth when prompted.

Set the fields using expressions (click the cloud/gear icon next to the field -> Add Expression):

Summary: {{ $json.title }}

Start Time: {{ $json.start }}

End Time: {{ $json.end }}

For the Notes Path (Output 1):
Hover over the bottom output dot (1) of the Switch node and add your preferred notes node (e.g., Notion).

Set the Operation to Create a Page or Append to Page.

Connect your Notion account and map the content field to the expression: {{ $json.note_content }}.

Add the Success Text-Back:
Connect a final Telegram Node to the end of both your Calendar and Notes nodes.

Set the Operation to Send Message.

For Chat ID, use the expression: {{ $hero.['Telegram Trigger'].body.message.chat.id }} (this targets your specific chat thread).

For the Text field, type: ✅ Successfully processed: {{ $json.title || $json.note_content }}.

Phase 6: Activate and Test
Click the Listen for test event button at the bottom of n8n.

Open Telegram, open the chat with your new bot, and press Start.

Send a test message like: "Remind me to call the landlord tomorrow at 3 PM"

Watch the data move through the green lines in n8n.

If everything lights up green, flip the toggle in the top-right corner of n8n from Inactive to Active.

Your smart inbox is now running 24/7 on your machine!