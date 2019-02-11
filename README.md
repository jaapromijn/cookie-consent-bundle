[![Scrutinizer Code Quality](https://scrutinizer-ci.com/g/ConnectHolland/cookie-consent-bundle/badges/quality-score.png?b=master&s=15b793ae2474fa313d343c43f30ce4f9aa594f00)](https://scrutinizer-ci.com/g/ConnectHolland/cookie-consent-bundle/?branch=master)
[![Code Coverage](https://scrutinizer-ci.com/g/ConnectHolland/cookie-consent-bundle/badges/coverage.png?b=master&s=d8e84bcf2e3e5bed47d4c6aa4702f246de74dbdf)](https://scrutinizer-ci.com/g/ConnectHolland/cookie-consent-bundle/?branch=master)
[![Build Status](https://scrutinizer-ci.com/g/ConnectHolland/cookie-consent-bundle/badges/build.png?b=master&s=bcccde957df75df8622fa346ba348dee002efebb)](https://scrutinizer-ci.com/g/ConnectHolland/cookie-consent-bundle/build-status/master)


# Cookie Consent bundle for Symfony
Symfony bundle to append Cookie Consent to your website to comply to AVG/GDPR for cookies.

## Installation
In a Symfony application run this command to install and integrate Cookie Consent bundle in your application:
```bash
composer require connectholland/cookie-consent-bundle
```

Configure your Cookie Consent with the following possible settings
```yaml
ch_cookie_consent:
    theme: 'light' # light, dark
    categories: # Below are the default supported categories
        - 'analytics'
        - 'tracking'
        - 'marketing'
        - 'social_media'
    use_logger: true # Logs user actions to database
```
Dark theme:
![alt text](https://raw.githubusercontent.com/ConnectHolland/cookie-consent-bundle/master/doc/dark_theme.png)

Light theme:
![alt text](https://raw.githubusercontent.com/ConnectHolland/cookie-consent-bundle/master/doc/light_theme.png)

## Usage
Load the cookie consent in Twig via render_esi ( to prevent caching ) at any place you like:
```twig
{{ render_esi(path('ch_cookie_consent.show')) }}
{{ render_esi(path('ch_cookie_consent.show_if_cookie_consent_not_set')) }}
```
When a user submits the form the preferences are saved as cookies and logged into the database. The cookies have a lifetime of 1 year. The cookie **Cookie_Consent** is saved with the date of submit and for each configured category a cookie saved e.g. **Cookie_Category_analytics** with it's value set to either *true* or *false*

# Logging
AVG/GDPR requires all given cookie preferences of users to be explainable by the webmasters. For this we log all cookie preferences to the database. This option can be disabled in the config.
![alt text](https://raw.githubusercontent.com/ConnectHolland/cookie-consent-bundle/master/doc/log.png)

# TwigExtension
You can use the TwigExtension to check if user has given it's permission for certain cookie categories
```twig
{% if chcookieconsent_isAllowed('analytics') == true %}
    ...
{% endif %}
```

## Customization
### Categories
You can add or remove any category by changing the config and making sure there are translations available for these categories.

### Translations
All texts can be altered via Symfony translations by overwriting the CHCookieConsentBundle translation files.

### Styling
CHCookieConsentBundle comes with a default styling. A sass file is available in Resources/assets/css/cookie_consent.scss and a build css file is available in Resoureces/public/css/cookie_consent.css. Colors can easily be adjusted by setting the variables available in the sass file.

### Javascript
By loading Resources/public/js/cookie_consent.js the cookie consent will be shown on top of your website while pushing down the rest of the website.

### Template Themes
You can override the templates by placing templates inside your poject:

```twig
# app/Resources/CHCookieConsentBundle/views/cookie_consent.html.twig
{% extends '@!CHCookieConsent/cookie_consent.html.twig' %}

{% block title %}
    Your custom title
{% endblock %}
```
