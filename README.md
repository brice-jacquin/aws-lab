# aws-lab

## Overview

This lab is made to apprehend AWS basic concepts and learn to use the AWS console.

We will see a simple example of how to evolve a unique server architecture to a 3 tier web app.


## General guidelines

## Region

In this lab, we will only use the ireland region:

**eu-west-1**

## Ressources parameters

For all non specified parameters when creating ressources with console / cli / sdk, please leave default ones.

## Security groups

When working ith security groups on AWS, we usually have the following politic:

* Strict restriction on inbound rules
* No restriction on outbound rules

That means that we always apply a full authorisation on the outbounds rules of every security group.
Please keep that in mind for the lab.

## When AFK for a long time

Always think about stopping your ressources if you don't use them for a long time (lunch break for example).

If you have finished the lab and won't use the ressources anymore, think about all cleaning/deleting.

Very important for not paying what you do not use !

### Ressource tagging

Tags is a mandatory feature in cloud computing, especially using AWS.
It is used for :
* Recognizing ressources
* Finops 

In this lab, the mandatory tagging policy is :
* **Name** : the name of the ressource
* **Owner** : the name of the owner
* **Project** : the name of the project

Always try to give a comprehensive name for ressources, so you can easily find them when needed.

## How to

## At the end

Please think about deleting all your ressources when done.
This is important to not exceed the allocated budget.

Go reverse on the module, and delete one by one all ressources created before leaving.