# README #

This README covers steps for configuring Ezproxy to use shibboleth for authentication & the Alma User API for authorization decisions.

### What is this repository for? ###

Shibboleth implements the "saml" protocol, and upon authentication returns something called a saml response, which looks like an xml document. The saml response contains the saml attribute statements, which are the attributes that are used by the application (ezproxy) to authorize / entitle the user. In most cases, Shibboleth will release enough attributes for ezproxy to make login authorization decisions. In those cases, there is no need to configure the Alma user API for authorization. However, in cases when shibboleth does not provide enough relevant attributes back, the need arises to take one of the released shibboleth attributes, and query the Alma user API with it, in order to get more information about the patron, and make authorization decisions.

### How do I get set up? ###

These instructions assume that shibboleth has already been configured for authentication. For more information, pleae see:
https://www.oclc.org/support/services/ezproxy/documentation/usr/shibboleth.en.html

This set-up requires the existence/creation of three files:

* user.txt
* shibuser.txt
* alma.txt

In user.txt, make sure to have the Shibboleth declaration, similar to the following (refer to OCLC documentation for more info):

```
::Shibboleth 

IDP20 https://your.institutional.shibboleth.url.here

/Shibboleth 
```

Conclude user.txt with the following directive:

``` ::file=shibuser.txt ```


Your shibboleth administrator will provide you with a set of attributes that shibboleth is releasing to you. You'll want to note what the shib attribute for username is. In our case, that attribute is UID. In shibuser.txt, you only need the following:

``` Set login:user = auth:uid

If ! UserFile("alma.txt"); Deny baduser.txt ```

Basically, you're setting login.user to auth:uid (this value will depend on your shibboleth set-up, but will need to reference the username attribute that shibboleth releases).

You'll also need to obtain an API key for the Users API through the Exlibris Developer Network. More information on how to do so below:
https://developers.exlibrisgroup.com/alma/apis

Finally, alma.txt will contain the directives related to the authorization decisions. Make sure to edit alma.txt to include your API key (Users API) obtained from the Exlibris' Developer Network.

### Contribution guidelines ###

Special thanks to Chris Zagar, original developer of Ezproxy, who provided most of the XML interface directives for interacting with the Alma User API.

### Who do I talk to? ###

For questions related to this repo, please contact Renaldo Gjoshe (rgjoshe@csufresno.edu).