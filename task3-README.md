# Task 3: Customer Feedback Analysis

An n8n course project that collects feedback through a form, uses Groq to classify sentiment and urgency, logs submissions in Google Sheets, and sends personalized HTML email responses.

## Flow
Form -> Basic LLM Chain -> JavaScript parser -> Merge with form data -> Sheet1 -> IF
- Positive: thank-you email.
- Negative: apology email and a parallel escalation entry in Sheet2.

## Setup
1. Import task3-feedback-workflow.json into n8n.
2. Select your own Groq, Google Sheets and Gmail credentials in the relevant nodes.
3. Select an available Groq model in your account (the export uses openai/gpt-oss-20b).
4. Replace REPLACE_WITH_YOUR_SPREADSHEET_ID in both Sheets nodes with your spreadsheet ID and select the correct tabs.
5. Create Sheet1 headers: name, email, feedback, action, priority.
6. Create Sheet2 headers: name, phone, action.
7. Replace the n8n demo favicon in the email templates with your company logo URL.
8. Use the form test URL and your own email address for testing. Publish/activate only after checking both branches.

## Analysis
The AI returns a JSON array containing one object with feedback, action, status and priority. The Code node validates its shape and allowed values. Status is positive or negative; priority is high, neutral or low. The prompt asks for recommendations in the feedback language and preservation of the original text. Sheet1 records the original form feedback.

## Validation
Screenshots supplied during development show Sheet1 logging, negative-case logging in Sheet2, and successful Gmail delivery. An urgent Arabic payment/reservation test and final logo rendering have not been independently verified in this export review.

Suggested tests:
- Praise: positive / low; Sheet1 and thank-you email.
- Minor complaint: negative / neutral; both sheets and apology email.
- Payment deducted but reservation failed: negative / high; action in the feedback language, both sheets and apology email.

## Notes
This is an inactive sharing copy of the current workflow, not a newly executed deployment. Credential references, private spreadsheet identifiers, webhook identifiers, and pinned test data were removed. Reconnect accounts after import. AI classifications require review; this is a course demo. The email templates are Arabic, and the n8n image is a temporary demo logo. Phone is a numeric form field as used in the assignment; it can lose leading zeros. Form fields are not currently marked required in the exported workflow.
