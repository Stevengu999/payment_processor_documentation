---
title: Zerion's Ethereum Payment Processor

language_tabs:
  - python

includes:
  - project
  - account
  - callbacks
  - withdrawal_procedure
  - errors

search: true
---

# Introduction

The API is designed to be both simple and correct, following RESTful principles. All endpoints are named for the models they interact with and provide CRUD style actions. 

For testing purposes and speeding up development we highly recommend [Postman](https://www.getpostman.com/apps). It enables you to easily play with the API.

This API documentation was created with [Slate](https://github.com/tripit/slate). 

# Overview

The API has 2 primary models - Projects and Accounts. A Project is the base model, representing one, unique use case of the API. Each project will have many accounts, representing hot wallets that can be used to receive payments that will be uniquely associated with your users. 

You can set default payment behaviours for your Project and the use these defaults when creating Accounts. But, Accounts can also be customized on a per-account basis at creation time.

# API Authentication

**All endpoints** will require authentication with a Project Key. This key will be associated with your project and will be given to you by a member of the Zerion team. 

In order to authenticate, you must pass your key in the headers of any request to the API under the key `PROJECT_KEY`. 