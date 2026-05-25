# Step 0: Integration Approach Analysis

## Data Flow Direction
**Bidirectional**
- CleverTap → Twilio: Send SMS/WhatsApp campaigns via Twilio's messaging channels
- Twilio → CleverTap: Receive message delivery status, user responses, and engagement data back to CleverTap for analytics and journey orchestration

## Integration Method
**Direct API call (CleverTap → Twilio) + Webhook (Twilio → CleverTap)**

**Rationale:**
- Twilio provides REST APIs for sending messages (SMS, WhatsApp, etc.) that CleverTap can call directly
- Twilio supports webhooks (StatusCallback URLs) to send delivery receipts and message status updates back to CleverTap
- This is the standard approach for CRM/marketing platforms integrating with Twilio
- No middleware required - both platforms support direct HTTP/REST integration

---

# CleverTap + Twilio Integration

## Overview

This integration enables brands to send personalized SMS and WhatsApp messages through Twilio's messaging infrastructure directly from CleverTap campaigns and journeys, while receiving real-time delivery status and user engagement data back into CleverTap. Marketers gain access to Twilio's reliable global messaging network while maintaining unified customer engagement analytics in CleverTap, and Twilio expands its reach to CleverTap's customer base seeking enterprise-grade omnichannel orchestration.

## Prerequisites

- Active CleverTap account with messaging permissions enabled
- Active Twilio account with verified phone numbers or WhatsApp Business API access
- Twilio Account SID from the Twilio Console dashboard
- Twilio Auth Token from the Twilio Console dashboard
- Configured Twilio phone number or WhatsApp sender for your messaging channel
- CleverTap webhook URL access for receiving Twilio status callbacks

## Integration Steps

### Step 1: Configure Twilio credentials in CleverTap

- Navigate to Settings → Channels → SMS or Settings → Channels → WhatsApp in your CleverTap dashboard
- Click Add provider or Configure new provider
- Select Twilio from the provider dropdown list
- Enter your {{twilio_account_sid}} in the Account SID field
- Enter your {{twilio_auth_token}} in the Auth Token field

### Step 2: Add Twilio messaging phone numbers

- Enter your Twilio phone number in the From phone number field using E.164 format (e.g., +1234567890)
- For WhatsApp, enter your WhatsApp-enabled Twilio number in the WhatsApp sender field with the whatsapp: prefix (e.g., whatsapp:+1234567890)
- Select the default sender number for your account
- Click Save or Save configuration

### Step 3: Configure status callback webhook in Twilio

- Copy the webhook URL provided in CleverTap under Settings → Integrations → Twilio webhook endpoint
- Log in to Twilio Console and navigate to Phone Numbers → Manage → Active numbers
- Select the phone number you configured in Step 2
- Scroll to Messaging Configuration section and paste the CleverTap webhook URL in the A message comes in webhook field
- Set the HTTP method to POST
- Click Save to enable status callbacks

### Step 4: Test the integration

- Create a test campaign in CleverTap by navigating to Campaigns → Create campaign
- Select SMS or WhatsApp as the messaging channel
- Choose Twilio as the provider in the channel configuration
- Send a test message to a verified phone number
- Verify message delivery in CleverTap analytics dashboard and confirm delivery status is received from Twilio