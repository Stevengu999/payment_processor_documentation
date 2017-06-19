# Introduction

The API is designed to be both simple and technically correct, following RESTful principles. All endpoints are named for the logical models they interact with and provide CRUD style actions. 

For testing purposes, we highly recommend [Postman](https://www.getpostman.com/apps). It enables you to easily play with the API.

This API documentation was created with [Slate](https://github.com/tripit/slate). Please don't hestitate to reach out with feedback and suggestions.

# Overview

The API has 2 primary models - Projects and Accounts. A Project is the base model of the system, representing one, unique use case of the API. You will have one project per application that you create. Each project will come to have many accounts. These accounts represent hot wallets that can be used to receive payments and to uniquely associated them with the users of your application. 

Projects are used as to authenticate requests and to set default configuration for the accounts you will create. But accounts can also be customized at time of creation. 

This documentation also refers to payments, deposits and withdrawals. Payments and deposits are interchangeable. They represent money being transferred by your users into a hot wallet that you have created. Withdrawals are created by our system and represent the transfer of money from the hot wallet to your designated recipients.

We suggest reading the entire set of documentation before beginning your implementation. At a minimum you should understand Project Defaults, Account creation and Callback handling. If you want to learn more about how we handle deposit tracking and withdrawal creation, you should read the Payment Handling Procedure Section.