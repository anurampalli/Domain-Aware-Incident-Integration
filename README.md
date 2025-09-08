# Domain Aware Incident Integration (ServiceNow Scoped App)

## Overview

This repository contains the **ServiceNow Scoped Application** for domain-aware incident integration.

The app provides a **Scripted REST API** that accepts incidents from external systems and automatically assigns them to the correct **domain** based on the authenticated user.

This is particularly useful in **multi-tenant ServiceNow implementations** where each customer must have isolated data using **Domain Separation**.

---

## Features

- Scoped application (`x_yourname_domain_integration`)  
- **Scripted REST API** to create incidents  
- Auto-assigns incidents to correct `sys_domain` based on the user  
- Fully compatible with **Domain Separation plugin**  
- Packaged entirely as a **Studio scoped app** for easy import/export  

---

## Architecture
External System → ServiceNow Scripted REST API → User Lookup → Incident (sys_domain assigned)


---

## Getting Started

Follow these steps to set up and start using the app:

### 1. Import Scoped App

1. Clone this repository or download the app XML files.  
2. Open **ServiceNow Studio** in your instance.  
3. Click **File > Import > Scoped Application** and select the XML files.

### 2. Activate Required Plugins

- Ensure **Domain Support – Domain Extensions** plugin is active.

### 3. Configure Domain Mapping

- Add companies in the `core_company` table.  
- Map each company to a domain in the `domain` table.

### 4. Use the REST API

- Endpoint format:

POST https://<your-instance>.service-now.com/api/<your application>/remote_incidents/create

- Headers:

```http
Content-Type: application/json
Accept: application/json
```
- Authentication: OAuth 2.0
**Example Request**
```json
{
  "short_description": "Server CPU usage high"
}
```
**Example Response**
```json
{
"result": {
    "status": "success",
    "incident_number": "INC0012271",
    "company": "CompanyA",
    "domain": "TOP/DomainA"
  }
}
```
### 5. Learning Outcomes

- Integrating ServiceNow with external systems using REST APIs

- Enforcing domain separation in integrations

- Packaging a ServiceNow Scoped App for reuse

## Next Steps

- Extend the app to support Problem or Change records

- Implement Automated Test Framework (ATF) tests for regression

## Studio Source Control Tips

- To avoid errors when syncing with Git:
  - Track only XML app files
    - Studio expects XML files for application records.
    - Do not add screenshots, Word documents, PDFs, or binaries to the repo.
    - Use .gitignore for non-app files (*.png, *.jpg, *.docx, *.pdf, *.zip)
- This ensures Studio only sees supported files.

- Clean Git history before linking

- Remove any commits with non-XML files that Studio might attempt to read.

- Only commit valid app XML records.

- Unlink and relink carefully

- If you encounter errors, unlink the repo from Studio, clean it, then re-link.

- Manual Import as fallback

  - If the Git sync fails, use Studio > Import Scoped Application to load the app manually.

  - Following these practices keeps Studio sync stable and prevents NullPointerException or other Git diff errors.

