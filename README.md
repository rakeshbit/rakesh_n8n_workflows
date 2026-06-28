# Rakesh_n8n_workflows
# 🏃‍♂️ Trail Recommendation Agent

## 📝 Description
An intelligent, weather-aware AI agent built in n8n that acts as your personal running assistant. Triggered automatically every day at 5:00 AM, the agent checks your Google Calendar for a scheduled "Trail Run". If an event is found, it evaluates the local Air Quality Index (AQI) and current weather conditions. Using a predefined list of trails from Google Sheets, it intelligently selects the most suitable trail (e.g., favoring shaded trails on hot days) and sends a beautifully formatted HTML email via Gmail with the recommendation. If conditions are unsafe (e.g., poor AQI), it proactively emails a cancellation notice.

## ⚡ Trigger
* **Schedule Trigger:** Runs daily at 5:00 AM. *(Note: You can adjust the exact hour inside the Schedule Trigger node).*

## 🛤️ Main Steps
1. **Schedule Trigger:** Initiates the workflow daily.
2. **AI Agent (OpenAI gpt-4o-mini):** Processes the logic and orchestrates the tools based on a master system prompt.
3. **checkCalendar (Google Calendar Tool):** Looks for a "Trail Run" event today.
4. **getAirQuality (HTTP Request Tool):** Fetches current PM2.5 AQI data from the AirNow API.
5. **getWeather (OpenWeatherMap Tool):** Fetches the current temperature and conditions.
6. **getHikeList (Google Sheets Tool):** Retrieves a list of available trails, including distance, elevation, time, and shade level.
7. **sendMessage (Gmail Tool):** Dispatches the final formatted HTML email with the day's recommendation.

## 🔌 Apps & APIs Used
* **OpenAI** (Language Model - `gpt-4o-mini`)
* **Google Calendar**
* **Google Sheets**
* **Gmail**
* **OpenWeatherMap**
* **AirNow API** (via standard HTTP Request)

## 🔑 Required Credentials
To successfully run this workflow, you will need to authenticate the following within your n8n instance:
* `OpenAI API`: An active API key.
* `Google Calendar OAuth2 API`: Connected to the account holding your running schedule.
* `Google Sheets OAuth2 API`: Connected to the account hosting your Trail Database spreadsheet.
* `Gmail OAuth2 API`: Connected to the email account you want to send *from*.
* `OpenWeatherMap API`: A free OpenWeatherMap API key.
* `AirNow API Key`: Added directly into the HTTP Request tool URL parameters.

## ⚙️ Setup Instructions
1. Download the `Trail Recommendation Agent.json` file from this folder.
2. Import the JSON file into your n8n instance.
3. Open the workflow and configure the credentials for **OpenAI**, **Google Calendar**, **OpenWeatherMap**, **Google Sheets**, and **Gmail** nodes.
4. **Configure AirNow API:** Open the `getAirQuality` HTTP Request node and replace the `API_KEY` placeholder in the URL with your actual AirNow API key. Also, update the `zipCode` parameter to your target location.
5. **Update Google Sheets:** Open the `getHikeList` node and ensure it points to your specific Google Sheet Document ID and Sheet Name containing your trails. The sheet must have columns for *Name, Distance (miles), Elevation gain (feet), Estimated time (minutes), and Shade Level*.
6. Review the system prompt inside the **AI Agent** node to tweak the tone or formatting if desired.
7. Toggle the workflow to **Active**.

## 💡 Example Use Case
* **Input:** It's 5:00 AM on a Tuesday. You have "Trail Run" on your calendar. The weather is 85°F and Sunny, and AQI is "Good".
* **Output:** The agent searches your Google Sheet, skips the "Exposed" trails due to the heat, and selects a "Shady" trail. It immediately sends an email formatted as an elegant HTML card saying: *"Hey there! 🏃‍♂️ It's quite hot today, so I picked a shaded route to keep you cool. You're tackling the Corner Canyon Pine Loop (4.2 miles, 800 ft gain, Shady)."*

## ⚠️ Troubleshooting Notes
* **Workflow stops abruptly:** The AI agent is instructed to stop execution if no "Trail Run" event is found on the calendar today. Ensure your calendar event precisely matches the spelling.
* **Email looks unformatted:** Ensure your email client supports inline HTML styles, as the agent is prompted to output pure HTML.
