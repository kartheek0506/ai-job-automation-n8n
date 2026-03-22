# AI Job Automation Workflow (n8n)

## Features
- Fetch jobs from Remotive API
- Filter AI-related roles
- Send email alerts

## Workflow Steps
1. HTTP Request → Fetch jobs
2. Loop Over Items → Iterate jobs
3. IF Node → Filter AI jobs
4. Gmail Node → Send alerts

## Setup
- Import JSON into n8n
- Connect Gmail OAuth
- Update email address

## Future Improvements
- Add fresher filter
- Add multiple APIs
- Add scheduling
