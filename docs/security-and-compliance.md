---
title: Security and compliance
layout: default
nav_order: 5
---
# Security and Compliance

## Compliance
This documentation offers some helpful practical tips for complying with the standards, but first and foremost the point of truth for all compliance and standards are the SA Government's own documentation. 

At the time of writing these are summarised in the [SACSF-S4-16-Secure-Web-Service-Standard.pdf](/assets/SACSF-S4-16-Secure-Web-Service-Standard.pdf) document and supporting documents.

Government websites are required to be accessible - with sites complying with either [WCAG 2.0](https://www.w3.org/TR/WCAG20/) at Level A or Level AA.

Performance is also a form of accessibility - slow sites with huge bandwidth requirements discriminate against people with older devices or slower internet connections. All sites should score 85 or higher in [Google Lighthouse](https://developer.chrome.com/docs/lighthouse/overview) for both Mobile or Desktop.

### Security 
To help meet the security requirements this project comes with two plugins installed out the box

- Two Factor Authentication (Which is required for anyone with admin access)
- Password Policy (to enforce strong passwords for all users)

Larger sites with projected long life-spans are required to implement [Audit](https://plugins.craftcms.com/audit?craft4) to enable the tracking of changes within the admin. 

The requirement for the Audit plugin can be skipped for shorter project-related websites with a projected lifespan of less than 24 months.

Two Factor Authentication must be enabled / enforced for all admin users.

The password policy settings can be changes via `config/password-policy.php` but the default configuration complies with the minimum standards for Renewal SA's security standards.


### Change management and version control
The use of version control is mandatory for the development of websites, and our partners are encourage to pick approaches that promote a separation of concerns between structure and content, with structure being managed and tracked via code, and content is kept in the database.

Some practical examples of this include

 - Minimising storage of code within the database (ie: not storing too much raw HTML)
 - No fields for `script` or `style` tags that get output into the page (GTM is the correct tool for tagging and tracking)

Thankfully the modern nature of Craft CMS means that many of the standard practices are good practices, and you should not need to drastically change your development style to suit the requirements.

### Credentials, API Keys and Sensitive Information
It is important NOT to commit sensitive information such as API keys etc to the github repository.

When developing locally things such as Google API Keys, AWS Credentials, Campaign Monitor API Keys etc should be defined in `.env` which is not committed to the repo.

When deploying to Platform.sh it will be necessary to set these as enviornment variables.