# n8n Resume Generator

An n8n workflow that automatically generates tailored resumes from job listings using Claude AI.

## Overview

This workflow:
1. Fetches the first unprocessed job from the "Relevant Roles" Google Sheet
2. Reads your master resume and prompt instructions from Google Docs
3. Copies a Google Docs resume template with placeholders
4. Uses Claude Sonnet 4.5 to generate tailored content for each placeholder
5. Replaces all placeholders in the copied document
6. Sends a Google Chat notification for human review with Approve/Reject buttons
7. On approval: moves job to "Processed Jobs" sheet with resume link
8. On rejection: deletes the draft and moves job to "Skipped Jobs" sheet

## Setup Requirements

### Google Workspace Credentials
Configure the following OAuth credentials in n8n:
- **Google Sheets** - for reading/writing job data
- **Google Docs** - for reading master resume/instructions and updating documents
- **Google Drive** - for copying template and deleting rejected drafts
- **Google Chat** - for sending review notifications

### Required Google Documents

| Document | Description |
|----------|-------------|
| Master Resume | Your full resume with all experience (source for Claude) |
| Prompt Instructions | Instructions for Claude on how to generate resume content |
| Resume Template | Google Doc with `{{PLACEHOLDER}}` markers |
| Jobs Sheet | Google Sheet with "Relevant Roles", "Processed Jobs", "Skipped Jobs" tabs |

### Template Placeholders

The resume template should contain these 34 placeholders:

**Taglines (3)**
- `{{TAGLINE_1}}`, `{{TAGLINE_2}}`, `{{TAGLINE_3}}`

**About Me (1)**
- `{{ABOUT_ME}}`

**Experience Bullets**
- Shopify: `{{SHOPIFY_BULLET_1}}` through `{{SHOPIFY_BULLET_2}}`
- Neo4j: `{{NEO4J_BULLET_1}}` through `{{NEO4J_BULLET_5}}`
- Fivetran: `{{FIVETRAN_BULLET_1}}` through `{{FIVETRAN_BULLET_5}}`
- Dentsu: `{{DENTSU_BULLET_1}}` through `{{DENTSU_BULLET_3}}`
- Text100: `{{TEXT100_BULLET_1}}` through `{{TEXT100_BULLET_3}}`
- Republic: `{{REPUBLIC_BULLET_1}}` through `{{REPUBLIC_BULLET_2}}`

**Competencies (10)**
- `{{COMPETENCY_1}}` through `{{COMPETENCY_10}}`

### Sheet Structure

**Relevant Roles** (input)
- Date Added, Title, Company, URL, Description

**Processed Jobs** (approved)
- Date Added, Date Processed, Title, Company, URL, Resume URL

**Skipped Jobs** (rejected)
- Date Added, Date Skipped, Title, Company, URL

## Usage

1. Import the workflow JSON into n8n
2. Configure all Google Workspace credentials
3. Select your Google Chat space in the "Send Review Request" node
4. Create the required sheet tabs if they don't exist
5. Run manually or set up a schedule trigger

## Notes

- The workflow processes one job at a time to allow for review
- Rejected drafts are permanently deleted from Google Drive
- The "Update Doc" node may need adjustment depending on n8n version
