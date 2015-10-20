# Creating a Directive

# Contents

* Introduction
* Prerequisites
* Template
* Javascript code
* Enabling Directive
* MHub Screencast

# Introduction

Directives are a powerfull part of the Angular js package. It allows for the creation of reusable functionality in a website. Many websites consist of repetitive elements that are included on multiple pages, for instance a navigation bar. Pasting the same html for each page would srart to become unmanagable, especially when implementing big changes.  

Directives allow you to place html and javascript code in a single location and call it using a single tag. 

# Prerequisites

Before continuing, the following prerequisits must be met -

1. fip_banking_node project cloned with repository permissions.
2. HMTL, Javascript and Angular js knowlege.

# Template

The HTML code sits in the client > templates > views > mobile folder. 

![alt text](/Images/template.png)

# Javascript code

All javascript code for directives sits in  client > js > directives

![alt text](/Images/directive.png)

# Enabling Directives

In order to make the Directive active, a variable needs to be assigned for the directive in app.js. The location of this file is client > js

![alt text](/Images/app.png) 

Then the parameters need to be assigned to the Directive.

![alt text](/Images/appdirective.png) 

One this is done, simply add a tag with the directive name into an existing page. 

![alt text](/Images/directivetag.png) 

# Mhub Screencast

[View screencast 1 on Mhub](http://link.mhub.tv/?t=RjYbla) to see a walkthrough of a directive being created from scratch.
