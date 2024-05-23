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

- [Two Factor Authentication](https://plugins.craftcms.com/two-factor-authentication?craft4) - required for anyone with admin access
- [Password Policy](https://plugins.craftcms.com/password-policy?craft4=) - to enforce strong passwords for all users

Larger sites with projected long life-spans are required to implement [Audit](https://plugins.craftcms.com/audit?craft4) to enable the tracking of changes within the admin. 

The requirement for the Audit plugin can be skipped for shorter project-related websites with a projected lifespan of less than 24 months.

Two Factor Authentication **must** be enabled / enforced for all admin users.

The password policy settings can be changes via `config/password-policy.php` but the default configuration complies with the minimum standards for Renewal SA's security standards.

### Craft Settings for Enhanced Security
Craft contains a number of settings which can enhance the security of the application, removing the need for additional plugins as might be the case in a WordPress based site. By default these are usually off or set to very forgiving defaults to not slow down development, but these should be set for the production environment to secure the application properly.

#### Change the admin path

[Craft Docs - cpTrigger](https://craftcms.com/docs/5.x/reference/config/general.html#cptrigger)

The `cptrigger` setting controls the url path by which the login and admin sections of the site can be accessed. Default is `admin` which means you can log in by visiting `https://yoursite.com/admin/`. It is recommended to change this from the default, to prevent common scanning or brute force attacks against the login page. This is an example of security through obscurity - which will not stop a determined attacker, but does reduce the load on the server from automated attacks, botnets etc which will be searching for the defaults.

#### Prevent brute force password attacks

[Craft Docs - maxInvalidLogins](https://craftcms.com/docs/5.x/reference/config/general.html#maxinvalidlogins)

The `maxInvalidLogins` setting controls how many login attempts a user can fail before they are temporarily blocked by the system. The default value is `5`. It is recommended to set this to `3` on production sites. The period of time that these attempts can occur within is controlled by `invalidLoginWindowDuration`, which is set to `3600` (1 hour in seconds) by default. The time that a user is locked out of the system for is set by `cooldownDuration` which has a default value of `300`, which is 5 minutes in seconds.

For production apps the following settings are recommended

```
'maxInvalidLogins' => 3,
'invalidLoginWindowDuration' => 21600 //6 hours in seconds
'cooldownDuration' => 7200 //2 hours in seconds
```

#### Prevent user enumeration attacks

[Craft Docs - preventUserEnumeration](https://craftcms.com/docs/5.x/reference/config/general.html#preventuserenumeration)

The `preventUserEnumeration` setting will ensure that the failed login messages that are returned to the user are vague, to not reveal whether it is the username or password that is incorrect. This setting is `false` by default, and should be set to `true` for production applications.

Setting this to true can cause confusion for end users, who will not be told if it is their username or password that is incorrect - but prevents attackers from brute-force enumerating common usernames to find the valid users of the site.

### Change management and version control
The use of version control is mandatory for the development of websites, and our partners are encourage to pick approaches that promote a separation of concerns between structure and content, with structure being managed and tracked via code, and content is kept in the database.

Some practical examples of this include

 - Minimising storage of code within the database (ie: not storing too much raw HTML)
 - No fields for `script` or `style` tags that get output into the page (GTM is the correct tool for tagging and tracking)

Thankfully the modern nature of Craft CMS means that many of the standard practices are good practices, and you should not need to drastically change your development style to suit the requirements.

### Credentials, API Keys and Sensitive Information
It is important NOT to commit sensitive information such as API keys etc to the github repository.

When developing locally things such as Google API Keys, AWS Credentials, Campaign Monitor API Keys etc should be defined in `.env` which is not committed to the repo.

When deploying to Platform.sh it will be necessary to set these as environment variables.