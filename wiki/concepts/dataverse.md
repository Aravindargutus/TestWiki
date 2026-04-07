---
title: SmartBrowz Dataverse
type: concept
created: 2026-04-07
updated: 2026-04-07
sources: [catalyst-nodejs-sdk-zia-smartbrowz-jobs.md]
tags: [smartbrowz, dataverse, lead-enrichment, web-scraping]
---

# SmartBrowz Dataverse

## Definition

Dataverse is a [[smartbrowz|Catalyst SmartBrowz]] component that performs intelligent data extraction from the web. Provides three categories of enrichment: Lead Enrichment, Tech Stack Finder, and Similar Companies.

## Key Aspects

### Lead Enrichment
Fetches comprehensive organization details from the web given a name, email, or website URL. Returns: employee_count, website, address, social links, description, CEO, revenue, founding_year, industries, organization_type, business_model, contact info, logo.

### Tech Stack Finder
Identifies technologies and frameworks used by a website. Returns: website, technographic_data (categorized), organization_name.

### Similar Companies
Returns a list of organizations providing similar services. Input: organization name and/or website URL.

## Sources

- [[catalyst-nodejs-sdk-zia-smartbrowz-jobs]]

## Related Concepts

- [[smartbrowz]] — Parent service
- [[browser-grid]] — Headless browser infrastructure
- [[pdf-screenshot]] — Visual document generation

[Source: catalyst-nodejs-sdk-zia-smartbrowz-jobs.md]
