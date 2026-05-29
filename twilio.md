# Integrate Twilio SMS with CleverTap

This integration enables brands to send personalized SMS messages through Twilio's robust messaging infrastructure while leveraging CleverTap's segmentation, campaign orchestration, and analytics capabilities. Marketers benefit from CleverTap's user insights and journey mapping combined with Twilio's reliable global message delivery network.

## Prerequisites

- Active CleverTap account with SMS channel enabled
- Twilio account with verified phone number or Messaging Service
- Twilio Account SID and Auth Token (found in Twilio Console dashboard)
- Admin or campaign manager permissions in CleverTap dashboard
- Phone numbers formatted with country code (E.164 format) in CleverTap user profiles

## Architecture overview

This is a **CleverTap to Twilio** unidirectional integration. When users trigger campaigns or enter journey flows in CleverTap, the platform sends SMS requests to Twilio's API. Twilio processes the message delivery and returns status updates. CleverTap stores delivery metrics for campaign analytics while Twilio maintains detailed message logs in its console.

## Integration steps

### Step 1: Retrieve Twilio credentials

- Log in to your Twilio Console at https://console.twilio.com
- Locate your **Account SID** on the dashboard homepage and copy it
- Click **Show** next to Auth Token, then copy the **Auth Token** value
- Navigate to Phone Numbers → Manage → Active Numbers and note your Twilio phone number, or go to Messaging → Services and copy your **Messaging Service SID**

### Step 2: Configure Twilio as SMS provider in CleverTap

- Log in to CleverTap dashboard and navigate to Settings → Channels → SMS
- Click **Add provider** or **Configure provider** button
- Select **Twilio** from the provider dropdown (if available) or select **Custom webhook**
- Enter your Twilio **Account SID** in the Account SID field
- Enter your Twilio **Auth Token** in the Auth Token field
- Enter your Twilio phone number with country code (e.g., +12025551234) or Messaging Service SID in the **From** field
- Click **Save** or **Test connection** to validate credentials

### Step 3: Create and send an SMS campaign

- Navigate to Campaigns → Create campaign → SMS
- Select your target segment under **Who** section
- Choose **Twilio** as the SMS provider in the delivery settings
- Compose your message content with personalization using {{user_property}} variables
- Configure send time (immediate or scheduled) and click **Launch campaign**

### Step 4: Monitor delivery and performance

- View real-time delivery metrics in the campaign dashboard (sent, delivered, failed, clicked)
- Check detailed message logs in Twilio Console → Monitor → Logs → Messaging for delivery status and error codes
- Export campaign results from CleverTap for deeper analysis if needed

## Troubleshooting

**SMS not delivered - Invalid phone number format**
- Ensure phone numbers in CleverTap user profiles include country code in E.164 format (+1234567890)
- Navigate to Settings → Data → User properties and verify the phone number field format
- Re-upload or update user profiles with correctly formatted numbers

**Authentication error - Invalid Account SID or Auth Token**
- Verify you copied the complete Account SID starting with "AC" and full Auth Token from Twilio Console
- Check for extra spaces or characters when pasting credentials into CleverTap
- Regenerate Auth Token in Twilio Console → Settings → API credentials if necessary and update in CleverTap

**Messages queued but not sending - Insufficient Twilio balance**
- Log in to Twilio Console and check account balance under Billing
- Add funds to your Twilio account if balance is low or depleted
- Review Twilio message logs for specific error codes (Error 21608 indicates insufficient balance)

## Verification

Your CleverTap and Twilio integration is working correctly when:

- Test SMS campaigns deliver successfully to verified phone numbers within 30-60 seconds
- Campaign analytics in CleverTap show "Delivered" status for sent messages
- Twilio message logs display "delivered" status with matching message SIDs
- Personalization variables ({{first_name}}, {{product_name}}) render correctly in received messages
- Delivery rates align between CleverTap campaign metrics and Twilio console reports