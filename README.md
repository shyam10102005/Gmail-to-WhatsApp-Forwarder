# Gmail to WhatsApp Forwarder (n8n Workflow)

A lightweight n8n workflow that monitors your Gmail inbox for unread messages and automatically forwards a formatted summary (Sender, Subject, and Snippet) to a specific WhatsApp number using the WhatsApp Business Cloud API.

🚀 **Features**
- **Real-time Polling**: Checks for new unread emails every minute.
- **Formatted Alerts**: Sends clean, bolded WhatsApp messages for easy reading.
- **Automatic Maintenance**: Marks the email as "Read" in Gmail after a successful forward to prevent duplicate notifications.
- **Variables Support**: Uses environment variables for sensitive data like phone numbers and IDs.

🛠️ **Prerequisites**
To use this workflow, you will need:
- **n8n Instance**: Self-hosted or Cloud version.
- **Google Cloud Project**: With the Gmail API enabled and OAuth2 credentials configured.
- **Meta Developer Account**: A configured WhatsApp Business App to get your Phone Number ID and System User Access Token.

📦 **Installation**

1. **Download the JSON**: 
   Download the `Gmail_to_Whatsapp_Forwarder.json` file from this repo.

2. **Import to n8n**:
   - Open your n8n canvas.
   - Click on the Menu (three lines) -> **Import from File...**
   - Select the downloaded JSON.

3. **Configure Credentials**:
   - **Gmail Trigger/Node**: Connect your Google OAuth2 credentials.
   - **Send WhatsApp (HTTP Request)**: Set up a Header Auth credential named `Header Auth` (or similar) with:
     - **Name**: `Authorization`
     - **Value**: `Bearer YOUR_ACCESS_TOKEN`

⚙️ **Configuration**

This workflow uses expressions that look for variables. You should define these in your n8n environment or replace them directly in the nodes:

| Variable | Description |
| :--- | :--- |
| `RECIPIENT_PHONE` | The phone number (including country code) to receive alerts. |
| `WHATSAPP_PHONE_ID` | Your unique Phone Number ID from the Meta Developer Portal. |

**Node Breakdown**
1. **Gmail Trigger**: Watches for unread messages. Filters out Spam/Trash.
2. **Format Message**: A 'Set' node that constructs the string:
   ```text
   📧 New Email
   From: {sender}
   Subject: {subject}
   ...
   ```
3. **Send WhatsApp**: Makes a POST request to the Graph API.
4. **Mark as Read**: Removes the `UNREAD` label from the processed email.

⚠️ **Important Notes**
- **API Limits**: If you receive a high volume of emails, ensure your WhatsApp Business account is out of "Sandbox" mode to avoid rate limits.
- **Security**: Never commit your JSON file to a public repo if you have hardcoded your API tokens or phone numbers into the nodes. Use n8n credentials or environment variables.

📄 **License**
This project is open-source and available under the MIT License.
