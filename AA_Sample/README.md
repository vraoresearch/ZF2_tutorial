Authentication & Authorization
==============================

Introduction
------------
This tutorial example is based on the [Zend framework 2 Skeleton Application] (https://github.com/zendframework/ZendSkeletonApplication). It shows how to integrate Social Authentication and Authorization into the Skeleton Application to demonstrates how two community component modules can be used to extend existing ZF2 components and form the basis of a practical working application.

Components Used
---------------
### [ScnSocialAuth](https://github.com/SocalNick/ScnSocialAuth).

Uses the HybridAuth PHP library to Enable authentication via Google, Facebook, Twitter, Yahoo!, etc for the ZfcUser ZF2 module.

### [BjyAuthorize](https://github.com/bjyoungblood/BjyAuthorize).

This module is designed to provide a facade for Zend\Permissions\Acl that will ease its usage with modules and applications. The default simple setup via config files or by using Zend\Db is used.



Installation
------------
For tutorial purposes, it is best to start by installing the Zend Framework 2 Skeleton Application and following the steps in the How-To below to create your own version of this application. However, you can also clone this working Application directly.


How-To: Add Authentication and Authorization to your Skeleton Application
-------------------------------------------------------------------------

### Outline
1. Install the Zend Framework 2 Skeleton Application.
2. Install ScnSocialAuth
3. Install BjyAuthorize
4. Tweak configurations
5. Edit the index.phtml to add permissions.

## 1. ZF2 Skeleton Application
Follow the instructions on the Github page to install the ZF2 Skeleton Application. Alternately if you use Zend Studio, simply start a new project and the Skeleton Application gets installed.

## 2. Install ScnSocialAuth
Follow the instructions on the Github page to install ScnSocialAuth. The instructions will require you to create a database instance using one of the supported formats. 

Also pay attention to the requirements listed. ScnSocialAuth depends on the ZF2 component Zfcuser among other components. The schemas for several popular database formats are provided. You will need to create database schemas for both Zfcuser (found in its zfc-user/data folder) and for ScnSocialAuth (found in its scn-social-auth/data folder). They should get installed under vendor.

Do not forget to add the configuration files for:
1. Zfcuser:         >zfcuser.global.php
2. ScnSocialAuth:   >scn-social-auth.global.php
                    >scn-social-auth.local.php



