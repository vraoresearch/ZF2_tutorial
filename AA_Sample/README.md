Authentication & Authorization
==============================

Introduction
------------
This tutorial example is based on the [Zend framework 2 Skeleton Application] (https://github.com/zendframework/ZendSkeletonApplication). It shows how to integrate Social Authentication and Authorization into the Skeleton Application to demonstrates how two community component modules can be used to extend existing ZF2 components and form the basis of a practical working application.

Components Used
---------------
### ScnSocialAuth

[Github link](https://github.com/SocalNick/ScnSocialAuth).

Uses the HybridAuth PHP library to Enable authentication via Google, Facebook, Twitter, Yahoo!, etc for the ZfcUser ZF2 module.

### BjyAuthorize

[Github link](https://github.com/bjyoungblood/BjyAuthorize).

This module is designed to provide a facade for Zend\Permissions\Acl that will ease its usage with modules and applications. The default simple setup via config files or by using Zend\Db is used.



Installation
------------
For tutorial purposes, it is best to start by installing the Zend Framework 2 Skeleton Application and following the steps in the How-To below. However, you can also clone this working Application.


How-To: Add Authentication and Authorization to your Skeleton Application
-------------------------------------------------------------------------

### Outline
1. Install the Zend Framework 2 Skeleton Application.
2. Install ScnSocialAuth
3. Install BjyAuthorize
4. Tweak configurations
5. Edit the index.phtml to add permissions.



