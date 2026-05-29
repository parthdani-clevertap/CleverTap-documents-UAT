# Integrate CleverTap with Unifonic for WhatsApp messaging

This integration enables brands to send personalized WhatsApp messages to customers using CleverTap's segmentation and campaign orchestration combined with Unifonic's WhatsApp Business API. Marketers gain the ability to trigger contextual WhatsApp communications based on user behavior while leveraging Unifonic's reliable message delivery infrastructure.

## Prerequisites

- Active CleverTap account with webhook channel enabled
- Unifonic account with WhatsApp Business API access provisioned
- Unifonic API credentials (Bearer token) from your Unifonic dashboard
- Approved WhatsApp message templates created in Unifonic platform
- WhatsApp Business phone number configured as sender in Unifonic
- User phone numbers stored in CleverTap user profiles in E.164 format (+country code + number)

## Architecture overview

This is a **CleverTap to Unifonic** unidirectional integration. CleverTap sends outbound webhook requests to Unifonic's WhatsApp API when campaign conditions are met (user segment membership, event triggers, or journey steps). User properties and event data from CleverTap are mapped to template parameters in the webhook payload, which Unifonic uses to personalize and deliver WhatsApp messages to end users.

## Integration steps

### Step 1: Gather Unifonic credentials and template details

- Log into your Unifonic dashboard and navigate to Settings → API Credentials
- Copy your **API Key** or **Bearer Token** and store it securely
- Navigate to WhatsApp → Templates to view your approved message templates
- Note the exact **template name**, **language code**, and **parameter count** for the template you want to use
- Confirm your **WhatsApp Business phone number** (sender ID) from WhatsApp → Configuration

### Step 2: Configure Unifonic webhook in CleverTap

- Log into CleverTap dashboard and navigate to Settings → Channels → Webhooks
- Click **Add Webhook** and enter webhook name as **Unifonic WhatsApp**
- Set **Webhook URL** to `https://el.cequens.com/api/v1/messages`
- Set **Method** to **POST**
- Add the following headers:
  - **Content-Type**: `application/json`
  - **Authorization**: `Bearer YOUR_UNIFONIC_API_TOKEN` (replace with your actual token)
- Click **Save**

### Step 3: Configure webhook payload structure

- In the webhook configuration screen, locate the **Request Body** section
- Enter the following JSON payload template, replacing placeholder values with your actual Unifonic configuration:

```json
{
  "recipient": "{{phone}}",
  "sender": "YOUR_WHATSAPP_NUMBER",
  "type": "template",
  "template": {
    "name": "your_template_name",
    "language": "en_US",
    "parameters": [
      "{{parameter1}}",
      "{{parameter2}}"
    ]
  }
}
```

- Replace **YOUR_WHATSAPP_NUMBER** with your Unifonic WhatsApp Business number (e.g., `+14155552671`)
- Replace **your_template_name** with your exact Unifonic template name
- Replace **language** code if using non-English template (e.g., `ar_SA` for Arabic)
- Map **parameters** array to match your template variables (add or remove as needed)
- Click **Save Payload**

### Step 4: Create user segment or define campaign trigger

- Navigate to Segments → Create Segment
- Define your target audience using CleverTap's segmentation criteria (e.g., "Users who completed Order Placed event in last 1 hour")
- Name your segment descriptively (e.g., "Recent purchasers for WhatsApp confirmation")
- Save the segment

### Step 5: Create webhook campaign with property mapping

- Navigate to Campaigns → Create Campaign → Webhook
- Select **Unifonic WhatsApp** from the webhook dropdown
- Choose your target segment or define event-based trigger
- Map CleverTap user properties and event properties to webhook variables:
  - Map **phone** to user property **Phone** (ensure E.164 format)
  - Map **parameter1** to relevant user property (e.g., **Name**)
  - Map **parameter2** to event property (e.g., **order_id** from Order Placed event)
- Set campaign name, schedule, and frequency controls
- Click **Launch Campaign**

### Step 6: Test and verify message delivery

- Create a test user profile in CleverTap with a valid phone number you can access
- Trigger the campaign condition (enter segment or fire qualifying event)
- Navigate to Campaigns → Your Campaign → Analytics to view webhook delivery status
- Check your test phone number for the WhatsApp message
- Verify personalized content appears correctly in the message
- Review webhook response logs in CleverTap for any errors

## Troubleshooting

**Error: 401 Unauthorized response from Unifonic**
- Verify your Bearer token is correct and not expired in Settings → Channels → Webhooks → Unifonic WhatsApp → Headers
- Regenerate API credentials in Unifonic dashboard if needed and update in CleverTap
- Ensure the Authorization header format is exactly `Bearer YOUR_TOKEN` with a space after "Bearer"

**Error: Template not found or invalid template parameters**
- Confirm the template name in your webhook payload exactly matches the template name in Unifonic (case-sensitive)
- Verify the parameters array length matches the number of placeholders in your Unifonic template
- Check that the language code matches your template's approved language in Unifonic dashboard

**Error: Invalid recipient phone number format**
- Ensure all phone numbers in CleverTap user profiles are stored in E.164 format: +[country code][number] with no spaces or special characters
- Update existing user profiles to include the + prefix and country code
- Implement data validation on user profile uploads to enforce E.164 format

## Verification

Your integration is working correctly when:
- Campaign analytics show successful webhook deliveries with 200 OK response codes
- WhatsApp messages are received on test phone numbers within 1-2 minutes of trigger
- Personalized content from CleverTap user properties appears correctly in messages
- Webhook logs in Settings → Channels → Webhooks → Unifonic WhatsApp show successful requests
- You can see message delivery confirmation in your Unifonic dashboard analytics