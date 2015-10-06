Authentication & Authorization
==============================

Introduction
------------
This tutorial example is based on the [Zend framework 2 Skeleton Application] (https://github.com/zendframework/ZendSkeletonApplication). It shows how to integrate Social Authentication and Authorization into the Skeleton Application to demonstrates how community component modules can be used to extend existing ZF2 components and form the basis of a practical working application.

Main Components Used
---------------
### [ScnSocialAuth](https://github.com/SocalNick/ScnSocialAuth).

Uses the [HybridAuth](http://hybridauth.sourceforge.net) PHP library to Enable authentication via [Google](https://console.developers.google.com), [Facebook](https://developers.facebook.com/products/login), [Twitter](https://apps.twitter.com), [Yahoo!](https://developer.yahoo.com), etc for the ZfcUser ZF-Commons module.

### [BjyAuthorize](https://github.com/bjyoungblood/BjyAuthorize).

This module is designed to provide a facade for Zend\Permissions\Acl that will ease its usage with modules and applications. The default simple setup via config files or by using Zend\Db is used.

Installation
------------
For tutorial purposes, it is best to start by installing the Zend Framework 2 Skeleton Application and following the steps in the How-To below to create your own version of this application. However, you can also clone this working Application directly.


How-To: Add Authentication and Authorization to your Skeleton Application
-------------------------------------------------------------------------

### Outline
1. Install the Zend Framework 2 Skeleton Application.
2. Install ScnSocialAuth.
3. Install BjyAuthorize.
4. Edit the index.phtml to add permissions.
5. Tweak final configurations.

## 1. ZF2 Skeleton Application
Follow the instructions on the Github page to install the ZF2 Skeleton Application. Alternately if you use Zend Studio, simply start a new project and the Skeleton Application gets installed. 

A more detailed explanation is available in the [zend framework manual](http://framework.zend.com/manual/current/en/user-guide/skeleton-application.html)

Make sure that you can see the Welcome to Zend Framework 2 message and the 404 error message by following the test procedures at the end.

## 2. Install ScnSocialAuth
Follow the instructions on the Github page to install ScnSocialAuth. The instructions will require you to create a database instance using one of the supported formats. 

Also pay attention to the requirements listed. ScnSocialAuth depends on the ZF-Commons component Zfcuser among other components. The schemas for several popular database formats are provided. You will need to create database tables for both Zfcuser (found in its zfc-user/data folder) and for ScnSocialAuth (found in its scn-social-auth/data folder).

Do not forget to copy the configuration files to your main /config/autoload folder:
* Zfcuser:         
    * zfcuser.global.php
* ScnSocialAuth:   
    * scn-social-auth.global.php
    * scn-social-auth.local.php
    
Leave these configurations at their default settings for now.

Again test and see if your skeleton application is still loading properly. In addition append ''/user'' to your skeleton application url and check if you can see the sign-in screen. You should also be able to see a 'sign-up' link. Go ahead and click on it and register yourself. If all goes well, you should see an entry in the users table of the database.

At this point, basic authentication should be working.

Now open *scn-social-auth.global.php* and scroll towards the end and uncomment the line:

<pre><code>'enable_social_registration' => true,</code></pre>

You can uncomment one or more of the *XYZSocial Enabled* configurations for example:

<pre><code>'yahoo_enabled' => true,</code></pre>


Please note that you need to have an API account with Yahoo befor this will work but it should enable the Yahoo link when you go to the registration form "sign-up" link.

*The procedure to register for a Client ID and Secret key is outside the scope of this How-To currently. Please follow the links embedded in the description of Scn-Social-Auth to get to the developer pages for the respective social-media service to obtain the necessary ID's and keys*

## 3. Install BjyAuthorize
Follow the instructions on the Github page to install BjyAuthorize. The instructions will require you to create two new database tables.

Create a new file in /config/autoload folder called *bjyauthorize.global.php* This will hold the configuration for BjyAuthorize that will help us to create Access Control Lists or ACL's. In this example, we will keep it simple and simply check to see if a user is authenticated and logged in or is a guest and provide access acordingly.

Copy the following code into *bjyauthorize.global.php*.

<pre><code>
<?php
return array(
    'bjyauthorize' => array(
        'default_role'          => 'guest',
        'authenticated_role'    => 'user',
        'identity_provider' => 'BjyAuthorize\Provider\Identity\AuthenticationIdentityProvider',
        'role_providers'        => array(
            'BjyAuthorize\Provider\Role\Config' => array(
                'guest' => [],
                'user'  => ['children' => [
                    'admin' => [],
                ]],
            ),                         
        ),
        'resource_providers'    => array(
            'BjyAuthorize\Provider\Resource\Config' => array(
                'yadayada' => array(),
            ),
        ),
        'rule_providers' => array(
            'BjyAuthorize\Provider\Rule\Config' => array(
                'allow' => array(
                    array(array('guest'), 'yadayada', 'err_msg'),
                    
                    array(array('user'), 'yadayada', 'all'),
                ),
            ),
        ),
    ), 
);   
          
</code></pre>

Please note that more sophisticated authorization using a more complex ACL that can guard Routes or Controllers is possible. However, this is a basic and very simple example.

## 4. Edit the index.phtml to add permissions.
This file contains the content below the navbar displayed as the Welcome to Zend Framework 2 message. For the purpose of this example, we will prevent un-authenticated users, aka 'guests' from seeing this message unless they login. Guests will see a prompt to login and authenticated users will see the normal skeleton application page.

Open the file /module/Application/view/application/index/index.phtml in an editor and add the following changes. At the top of the page before the existing code, insert the following code.

```php
<?php if ($this->isAllowed('yadayada', 'err_msg')) { ?>
```    
Now add the error message as follows:
 
```html

<div class="jumbotron">
    <h1><?php echo sprintf($this->translate('Welcome to %sZend Framework 2%s'), '<span class="zf-green">', '</span>') ?></h1>
    <p><?php echo sprintf($this->translate('Please Login to Continue ... '), '<a href="https://github.com/zendframework/ZendSkeletonApplication" target="_blank">', '</a>', \Zend\Version\Version::VERSION) ?></p>
    <p><a class="btn btn-success btn-lg" href="<?php echo $this->url('zfcuser') ?>" target="_blank"><?php echo $this->translate('Login') ?> &raquo;</a></p>
</div>
```
Close the PHP tag.

```php

    <?php } ?>
```

Now enclose the existing HTML div with the following code at the top.\
   
```php
    <?php if ($this->isAllowed('yadayada', 'all')) { ?>
```

Now scroll to the end and close the PHP tag at the end.

```php
    <?php } ?>
```
Save the file and check the skeleton application. You should now see a 'Login to continue' message. Even clicking on home will not display the regular welcome message. If you login you should be able to see a the user greeting screen with logout link and clicking on home shows the regular Skeleton Application page.

## 5. Tweak final configurations.
If all went well up to this point, we can make the final tweaks to the config files to add more features such as redirection after login and a captcha for the registration form.

These settings are part of the ZF2 component Zfcuser. More information about this component can be found in the Zend Framework Manual.

<h4>Post-Login Redirect</h4> 

To redirect the user to the main welcome page as soon as they login instead of the user greeting, change the following line in *zfcuser.global.php*.

<pre><code>
'login_redirect_route' => 'home',
</code></pre>


<h4>Add Captch to Registration Page</h4> 

To enable Captch which is turned off by default, change the value of 'use_registration_form_captcha' to true. Also uncomment the Form captcha options below it.

<pre><code>
'use_registration_form_captcha' => true,
</code></pre>


```php
'form_captcha_options' => array(
        'class'   => 'figlet',
        'options' => array(
            'wordLen'    => 5,
            'expiration' => 300,
            'timeout'    => 300,
        ),
    ),
    
```
TBD
---
1. Logout Button
Currently user needs to navigate to {url}/user/logout


