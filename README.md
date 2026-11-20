CampusConnect

CampusConnect is a student complaint tracking app built as an Azure AI-901 learning project. Students can submit a complaint, receive a reference number, and track updates. Staff can review complaints and update their status.

Current features

Complaint categories: Academics, Facilities, IT, Administration, Safety, and Other

Student complaint submission and reference numbers

Complaint status tracking and staff updates

Staff review dashboard

Optional AI suggestions for category, urgency, summary, and explanation

Local demo mode with sample complaints

AI note: When Microsoft Foundry is not configured, the app uses simple keyword rules. These are a demo fallback, not an AI model. AI suggestions are only recommendations; students review them before submitting.

Technology

HTML, CSS, and JavaScript frontend

Python Azure Functions API

Local JSON storage for the demo, with optional Azure Cosmos DB integration

Optional Microsoft Foundry model integration using the Azure AI Projects SDK

Static Web Apps authentication configuration for a future Azure deployment

Run locally on macOS

Install Python, Azure Functions Core Tools v4, Node.js/npm, and Azurite.

From the project folder, create the local settings file and install Python packages:

cp local.settings.sample.json local.settings.json
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt

Keep local.settings.json private; do not commit it.

Open three terminals in the project folder:

Start Azurite:

npx --yes azurite --silent

Start the API:

source .venv/bin/activate
func start

Serve the frontend:

python3 -m http.server 5500

Open http://localhost:5500. The local demo settings should include "LOCAL_DEMO": "1". Leave all three terminals running while using the app.

Enable Microsoft Foundry suggestions

To use a real model instead of the keyword fallback:

Create a Microsoft Foundry project and deploy a text model.

Set FOUNDRY_PROJECT_ENDPOINT and FOUNDRY_MODEL_NAME in your private local.settings.json.

Sign in locally with Azure CLI using az login. Your account needs permission to use the model.

Restart func start.

Model use may incur charges. To keep this project at $0, leave both Foundry settings blank and use the local demo fallback.

API routes

Method

Route

Purpose

GET

/api/me

Check the current demo or signed-in user

POST

/api/suggest

Get AI or demo-rule suggestions

POST

/api/complaints

Submit a complaint

GET

/api/complaints

List complaints for staff

GET

/api/complaints/{ref}

View a complaint by reference

PATCH

/api/complaints/{ref}

Staff updates status or adds a note

Azure integration status

The code includes optional Cosmos DB and Microsoft Foundry connections, plus a Static Web Apps authentication configuration. Azure resources and deployment are not included in this starter project. Blob Storage attachments and Application Insights setup are not implemented yet. Do not describe those as completed features.

Responsible AI and privacy

AI output is a suggestion, not a decision.

Students can edit the suggested category and urgency or choose them without AI.

Avoid entering passwords, health information, or other sensitive personal details.

Do not use local demo authentication for a public deployment.

Troubleshooting

If the UI buttons don’t respond, confirm the page is http://localhost:5500, the API is running with func start, and local.settings.json has "LOCAL_DEMO": "1".

If the app says it can’t reach the API, check that the Functions terminal lists routes on port 7071.

If Foundry is not configured, suggestions should identify themselves as demo keyword rules.

Project summary

CampusConnect demonstrates a complaint workflow using a Python serverless API and an optional generative AI service. It explores prompt-based text analysis, human review of AI suggestions, cloud identity, and persistent storage.
