# Ripe's starter email workflow

Our "Starter Email" is based on the Gulp Email Creator by [Daryll Dole](https://github.com/darylldoyle/Gulp-Email-Creator), which was inspired by [Lee Munroe's](https://github.com/leemunroe) [grunt-email-design](https://github.com/leemunroe/grunt-email-design)  workflow.

It aims to help speed up email template production by allowing you to use SCSS and inlining the generated CSS for you.  We've added support for Jade to compile the markup

#Workflow Overview

The gulpfile is the engine of the workflow.  It does the following:

* Compiles templates from Jade to HTML
* Processes SCSS into CSS
* Inlines CSS into HTML via MailChimp Inliner
* Outputs HTML with inlined CSS to an `./output/` directory
* Send test emails to any inbox via [Mailgun](http://mailgun.com)
* Delivers email to directly to a [Litmus](http://litmus.com) test

#Design Resources

* MailChimp's [Email Design Reference](http://templates.mailchimp.com/) is a very handy resource 
* Read through [this](https://www.campaignmonitor.com/resources/will-it-work/guidelines/) and [this](https://litmus.com/blog/html-email-coding-101-infographic) before you start any coding. 
* Handy code snippet for buttons [here](https://gist.github.com/elidickinson/9424116#file-html_email_buttons_1-html)
* More reading on coding buttons. [Litmus's bulletproof buttons](https://litmus.com/blog/a-guide-to-bulletproof-buttons-in-email-design) 
* MailChimp [Merge tags cheatsheet](http://kb.mailchimp.com/merge-tags/all-the-merge-tags-cheatsheet?_ga=1.198326381.313160508.1440767401) 
* See [MailChimp's Email Blueprints](https://github.com/mailchimp/Email-Blueprints) or [Responsive Email Patterns](http://responsiveemailpatterns.com/) for design patterns.  

For more information on web fonts, image blocking, width, CSS support, etc check out Campaign Monitor's [Design Guidelines](https://www.campaignmonitor.com/dev-resources/will-it-work/), which features a handy table on [CSS support](https://www.campaignmonitor.com/css/)

# Installation

You need to have both [Node.js](http://nodejs.org/) and [Gulp.js](http://gulpjs.com/) installed . 

You can install it by cloning this repository to a local folder and running the following from inside the directory.

```javascript
    npm install
```
This will install all dependencies.

If the install fails try running the install as root. For some reason browsersync seemed to fail for me unless installed as root.

```javascript
    sudo npm install
```

# Configuration

You need configure a few basic settings before use.  This tool relies on a `config.json` file

* Rename `config.default.json` to `config.json`
* Add MailChimp API key.  This workflow uses their free CSS inliner.  [API Key Instructions Here](http://kb.mailchimp.com/accounts/management/about-api-keys)
* Add to/from email addresses.  This will be used by Mailgun to send tests. 
* 'To' can have multiple email addresses.  They should be comma separated.
* Subject line should be '`{{Client}}` `{{Project Name}}` (test `{{test no}}`)'
* Add Mailgun credentials for testing.  These are found by click on "[Domains](https://mailgun.com/app/domains)" in your Mailgun dashboard. Login usually begins with postmaster@sandbox....
* Add [Litmus static email address](https://litmus.com/help/testing/static-email-addresses/) to automatically send the design to a Litmus test

# Using the package

You can use this email creator workflow by creating your HTML in the `./src/html/` directory and your SCSS in the `./src/scss/` directory.

## Compile Templates
Running `gulp` from the terminal will build the new HTML email template into the `./output/` directory and the proceed to watch `./src/html/` and `./src/scss/` for any updates.

You can write your templates in either Jade or HTML.  The default gulp task will look for Jade templates in `./src/jade/` and compile them to `./src/html`.  Any changes to HTML will build and reload via BrowserSync.  The worflow doesn't care if those changes come via compiling Jade or if you write the HTML manually.

The gulpfile has browsersync built in and will reload on any HTML or CSS edit, allowing you to focus on your code.

## Sending the Template to yourself
Running `gulp send` will allow you to send the compiled template to yourself through [Mailgun](https://mailgun.com) (you'll need to sign up for a free account). The settings for this can be found in the `config.json` file.

To run this, you can use the following command

```javascript
gulp send --template="compiled-template-name.html"
```

##Testing

Thanks to the awesome guys over [Litmus](http://litmus.com/), we can now throw your emails straight into their tests. You'll need an active [Litmus](http://litmus.com/) account (well worth the money!) and to get your [static email](https://litmus.com/static-email). Plug this into the `config.json` file and then run the following command.

```javascript
gulp litmus --template="compiled-template-name.html"
```
In a few seconds/minutes, you'll see the test appear in Limus for you!

#Contributing#

Any contributions will be happily recieved. Just open an issue or make a pull request.