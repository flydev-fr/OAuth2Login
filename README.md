# Auth2Login for ProcessWire

A Module which give you ability to login **an existing user** using your favorite thrid-party OAuth2 provider (i.e. Facebook, GitHub, Google, LinkedIn, etc.).. 
You can login from the backend to the backend directly or render a form on the frontend and redirect the user to a choosen page.
Built on top of ThePhpLeague OAuth2-Client lib.

*Registration is not handled by this module but planned.*

 
## Howto Install

Install the module [following this procedure](https://modules.processwire.com/install-uninstall/):

 - https://modules.processwire.com/OAuth2Login
 - https://github.com/flydev-fr/OAuth2Login

Next step, in order to use a provider, you need to use [Composer](https://getcomposer.org/doc/00-intro.md) to install each provider.

ie: to install Google, open a terminal, go to your root directory of pw and type the following command-line: `composer require league/oauth2-google`

#### Currently implemented providers/packages :

    Google :  league/oauth2-google
    Facebook: league/oauth2-facebook
    Github: league/oauth2-github
    LinkedIn: league/oauth2-linkedin


More third-party providers are available [there](http://oauth2-client.thephpleague.com/providers/thirdparty/) and can be implemented on demand basis.


## Howto Use It

First (and for testing purpose), you should create a new user in ProcessWire that reflect your real OAuth2 account information. The important informations are, **Last Name**, **First Name** and **Email**. The module will compare existing users by firstname, lastname and email; If the user match the informations, then he is logged in.

> ie, if my Google fullname is *John Wick*, then in ProcessWire, I
> create a new user  *Wick-John*  with email  *johnwick@mydomain.com*

Next step, go to your favorite provider and create an app in order to get the ClientId and ClientSecret keys. *Ask on the forum if you have difficulties getting there.*

Once you got the keys for a provider, just paste it into the module settings and save it. One or more button should appear bellow the standard login form. 


### Frontend Usage
*Small note: At this moment the render method is pretty simple. It output a InputfieldForm with InputfieldSubmit(s) into wrapped in a ul:li tag.* Feedbacks and ideas welcome!

For the following example, I created a `page login` and a `template login` which contain the following code :

```
    <?php namespace ProcessWire;

	if(!$user->isLoggedin()) {

		    $options = array(
		        'buttonClass' => 'my_button_class',
		        'buttonValue' => 'Login with {provider}', // {{provider}} keyword
		        'prependMarkup' => '<div class="wrapper">',
		        'appendMarkup' => '</div>'
		    );

		    $redirectUri = str_lreplace('//', '/', $config->urls->httpRoot . $page->url);

		    $content = $modules->get('Oauth2Login')->config(
	          array(
	              'redirect_uri' => $redirectUri,
	              'success_uri'  => $page->url
	          )
	      )->render($options);
	}
```
  


