Technical Showcase: Intelligent Data Pipeline via n8n & Python

👤 Project Purpose

I built this workflow to solve a common bottleneck in data engineering: handling messy, unformatted CSV exports that require logic too complex for standard drag-and-drop tools. This project demonstrates how I bridge the gap between low-code automation and custom scripts to create a scalable ETL (Extract, Transform, Load) pipeline.

🏗️ The Architecture
The workflow follows a logical four-stage progression:

Ingestion: I configured the pipeline to consume raw country-level demographic data.
Binary-to-JSON Parsing: Here, I handle the transition from a physical file into an actionable data object, ensuring encoding and headers are correctly mapped.
Custom Python Logic (The "Brain"): This is the core of the project. I chose to use a Python Code Node specifically because the source data (World Statistics) often uses non-standard decimal formats and inconsistent string padding. Python allows for more granular control over data types than native n8n nodes, ensuring the output is perfectly sanitized.
Final Formatting: The data is restructured into a clean JSON schema, ready to be injected into a database, a dashboard, or a CRM.

🛠️ Tech I Used

n8n: Used as the primary orchestrator for the workflow logic.
Python 3: Implemented for advanced data manipulation and normalization.
Docker: Used to host the environment and manage the Python Task Runner.

🧠 Technical Challenges I Overcame

Environment Configuration: I encountered an issue where the Python runner was missing from the local system. I resolved this by modifying my Docker environment to include a dedicated Task Runner sidecar, ensuring the workflow remains portable.
Data Type Sanitization: Many fields in the "Countries of the World" dataset are stored as strings with commas. I wrote logic to transform these into floats/integers to ensure they are mathematically useful for whoever consumes this data next.

🚀 How to Run This

Clone this repo.
Import the .json file into your n8n instance.
Configure your Python environment (ensure n8n is set to N8N_RUNNERS_ENABLED=true).
Upload the included CSV to the Read Binary node and execute.
