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
4. Tweak final configurations
5. Edit the index.phtml to add permissions.

## 1. ZF2 Skeleton Application
Follow the instructions on the Github page to install the ZF2 Skeleton Application. Alternately if you use Zend Studio, simply start a new project and the Skeleton Application gets installed. 

A more detailed explanation is available in the [zend framework manual](http://framework.zend.com/manual/current/en/user-guide/skeleton-application.html)

Make sure that you can see the Welcome to Zend Framework 2 message and the 404 error message by following the test procedures at the end.

## 2. Install ScnSocialAuth
Follow the instructions on the Github page to install ScnSocialAuth. The instructions will require you to create a database instance using one of the supported formats. 

Also pay attention to the requirements listed. ScnSocialAuth depends on the ZF2 component Zfcuser among other components. The schemas for several popular database formats are provided. You will need to create database schemas for both Zfcuser (found in its zfc-user/data folder) and for ScnSocialAuth (found in its scn-social-auth/data folder). They should get installed under vendor.

Do not forget to copy the configuration files to your main /config/autoload folder:
* Zfcuser:         
    > zfcuser.global.php
* ScnSocialAuth:   
    > * scn-social-auth.global.php
    > * scn-social-auth.local.php
    
Leave these configurations at their default settings for now.

Again test and see if your skeleton application is still loading properly. In addition append ''/user'' to your skeleton application url and check if you can see the sign-in screen. You should also be able to see a 'sign-up' link. Go ahead and click on it and register yourself. If all goes well, you should see an entry in the users table of the database.

At this point, basic authentication is working.

Now open *scn-social-auth.global.php* and scroll towards the end and uncomment the line:

<pre><code>'enable_social_registration' => true,</code></pre>

You can uncomment one or more of the *XYZSocial Enabled* configurations for example:

<pre><code>'yahoo_enabled' => true,</code></pre>


Please note that you need to have an API account with Yahoo befor this will work but it should enable the Yahoo link when you go to the registration form "sign-up" link.

*The procedure to register for a Client ID and Secret key is outside the scope of this How-To curently.*

## 3. Install BjyAuthorize