# Task 2: Smart Student Certificate & Report Router

An n8n course assignment that receives student result PDFs through Telegram, reads them using OCR, extracts structured results with AI, and routes email notifications through Gmail.

## Workflow

Telegram Trigger -> Get a file -> OCR.space HTTP Request -> Basic LLM Chain (Groq + Structured Output Parser) -> JavaScript -> IF

- Passed: download a sample certificate from Google Drive and send a congratulations email with the attachment.
- Other results: send the retake and academic-support email.

The extracted fields are student_name, course_name, final_grade, and result_status.

## Files

- task2-workflow.json: sanitized n8n workflow export, inactive by default.
- task2-README.md: setup instructions and limitations.

## Setup

1. Import task2-workflow.json into a compatible n8n installation.
2. Configure Telegram credentials on the trigger and Get a file nodes; ensure file download is enabled.
3. Replace YOUR_OCR_SPACE_API_KEY in the HTTP Request header with your own key. Store secrets in credentials rather than publishing them.
4. Configure a Groq credential and an available chat model.
5. Configure Gmail and Google Drive OAuth credentials. Enable Gmail API and Google Drive API in your Google Cloud project.
6. Replace student@example.com in both Gmail nodes with your own test inbox.
7. Upload a clearly labeled sample certificate to your Drive and replace YOUR_CERTIFICATE_FILE_ID. The demo uses a static certificate image, not automatic personalized certificate generation.
8. In the Passed Gmail node, set Options -> Attachments -> Attachment Field Name to data. Confirm that the Drive download outputs binary data.
9. Run the workflow manually and send a PDF to the Telegram bot. Check extraction, routing, email content, and attachment delivery before publishing the workflow.

## Expected test cases

| Report | Expected result | Expected email |
| --- | --- | --- |
| Sara | Passed, with honors | Congratulations and certificate |
| Omar | Passed | Congratulations and certificate |
| Mohamed | Failed | Grade, retake requirement, and support information |

A successful Omar Passed-email send was observed during development. Full end-to-end validation for all three cases has not been independently verified.

## Demo limitations

- Student email addresses are not present in the reports; the demo uses a fixed test inbox. A real deployment should look up recipients in a student directory, preferably by student ID.
- The certificate is a sample image with a fictional name and is for attachment testing only. Replace it with the matching certificate for each passed student.
- The current IF checks only whether the status equals Passed; all other values enter the retake branch. Before real use, add an explicit Failed check and send Unknown or incomplete results for review.
- Honors is not currently extracted as a separate field or included as a special message.
- PDF validation, OCR-error handling, and duplicate-email prevention should be added before real deployment.
- The disabled Extract from File node remains in the export as a record of the initial text-extraction approach. OCR was needed because some visible PDF fields were missing from ordinary text extraction.

## Publishing hygiene

This export removes credential references, the hard-coded OCR API key, personal recipient address, private Drive file URL, pinned data, and instance metadata. Credentials must be configured after import. No student PDFs or execution data are included.
