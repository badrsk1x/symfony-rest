Symfony Rest Angular 
========================

This application is a combination of:
  * REST API backend written in Symfony2 with [FOSRestBundle](https://github.com/FriendsOfSymfony/FOSRestBundle)
  * [Custom authentication provider](http://symfony.com/doc/current/cookbook/security/custom_authentication_provider.html) for the REST API via WSSE
  * MVC frontend client written in AngularJS consuming a RESTful Web Service

Dependencies
------------

  * [Symfony 2.7](http://symfony.com/) + Doctrine + Assetic + Twig
  *	[FOSRestBundle](https://github.com/FriendsOfSymfony/FOSRestBundle)
  	* This bundle has various tools to quickly create RESTful Web Service
  *	[FOSUserBundle](https://github.com/FriendsOfSymfony/FOSUserBundle)
  	* This bundle takes over user management
  * [JMSSerializerBundle](https://github.com/schmittjoh/JMSSerializerBundle)
  	* This bundle works together with FOSRestBundle in order to serialize objects requested via REST API
  * [Hateoas](https://github.com/willdurand/Hateoas)
  	* This bundle is used to add self links to all objects requested via RESTful Web Service
  * [SpBowerBundle](https://github.com/Spea/SpBowerBundle/)
  	* This bundle is used to manage bower assets inside ApiClientBundle
  * [AngularJS](https://angularjs.org/) + ngRoute + ngCookies
  * [Restangular](https://github.com/mgonto/restangular)
  	* This library replaces standard ngResource library to handle REST API requests
  * [Angular UI Bootstrap](https://github.com/angular-ui/bootstrap)
  	* This library contains majority of Bootstrap components written in "Angular Way"
  * [Bootstrap 3](http://getbootstrap.com/)
  	* Frontend theme

Composer and SpBower will automatically install all dependencies (see Installation below)
Dependencies are defined in the "composer.json" and "src/Acme/ApiClientBundle/Resources/config/bower/bower.json"

Installation
------------

Download the project using Git and then run Composer

```bash
$ git clone https://github.com/rsobon/Symfony-Rest-Angular-Demo.git
$ cd Symfony-Rest-Angular-Demo/
$ composer install
```

Composer will automatically download all dependencies listed above.
It will also automatically run "assets:install --symlink" and "assetic:dump" commands, which will make Assetic install all public resources (JavaScript files and HTML partials) required for MVC frontend client.

I suggest to install MySQL on the server and adjust parameters.yml accordingly. Then we can run following commands to install database and sample data:

```bash
$ cd Symfony-Rest-Angular-Demo/
$ app/console doctrine:database:create
$ app/console doctrine:schema:update
$ app/console doctrine:fixtures:load
```

Then we have to add the first user (adjust login, password and email to your liking) and grant him ROLE_API role:

```bash
$ app/console fos:user:create admin admin@email password
$ app/console fos:user:promote admin ROLE_API
```

User needs the ROLE_API role to ignore standard CSRF protection in Symfony2 forms. You can adjust this setting in the FOSRestBundle configuration.

Usage
-----

You can configure your own virtual host in your web server or use built-in web server:

```bash
$ app/console server:run
```

After that you can access the REST API!
Now you can navigate to login page of the MVC frontend client application:

```
http://127.0.0.1:8000/ApiClient#/login
```

Run following command to show all available routes:

```bash
$ app/console debug:router
```

Firewall in the backend application will block all requests starting with the prefix "api/*". Navigating to following URL will result in 403 (Forbidden) Error:
```
http://127.0.0.1:8000/api/pages
```
