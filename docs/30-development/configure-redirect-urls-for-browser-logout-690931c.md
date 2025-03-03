<!-- loio690931c0098a4aefbe2bdc8c52d2c27b -->

# Configure Redirect URLs for Browser Logout

To avoid open redirect attacks, direct users to a safe and valid URL when they log out.



<a name="loio690931c0098a4aefbe2bdc8c52d2c27b__prereq_v35_tnr_yfb"/>

## Prerequisites

-   You've an application that is using the application router.

-   Use application router 5.7.0 or higher. For more information, see the related link.




<a name="loio690931c0098a4aefbe2bdc8c52d2c27b__context_whn_2bc_h4b"/>

## Context

You’ve deployed an application in the Cloud Foundry environment of SAP BTP. Business users log in and out of the application. As an application developer, you want to set a redirect URI, which directs users to a specific page once they've logged out. This page can be a logout page or your organization’s employee portal. For security reasons, there’s a check of this redirect URI.

-   In the application descriptor \(`xs-app.json`\), you define a public logout page.

-   To avoid redirects, for example to malicious web sites, `xsuaa` compares this public URL with a list of allowed redirect URLs. You define them in the OAuth 2.0 configuration property of the application security descriptor \(`xs-security.json`\).




<a name="loio690931c0098a4aefbe2bdc8c52d2c27b__steps_xhn_2bc_h4b"/>

## Procedure

1.  Define `xs-app.json` in the application router folder of your application. Include a logout endpoint and define a logout page \(here ***logout.html***\) which can be accessed without authentication.

    > ### Sample Code:  
    > ```
    > {
    > 	"welcomeFile": "index.html",
    > 	"authenticationMethod": "route",
    > 	"logout": {
    > 		"logoutEndpoint": "/my/logout",
    > 		"logoutPage": "logout.html"
    > 	},
    > 	"routes": [{
    > 		"source": "^/hw/",
    > 		"target": "/",
    > 		"destination": "hw-dest",
    > 		"scope": {
    > 			"GET": [
    > 				"$XSAPPNAME.Display",
    > 				"$XSAPPNAME.Update"
    > 			],
    > 			"default": "$XSAPPNAME.Update"
    > 		}
    > 	},
    >     {
    > 		"source": "/index.html",
    > 		"localDir": "resources",
    > 		"cacheControl": "no-cache, no-store, must-revalidate"
    >     },
    >     {
    > 		"source": "/logout.html",
    > 		"localDir": "resources",
    > 		"authenticationType": "none"
    > 	},
    >     {
    > 		"source": "^/(.*)",
    > 		"localDir": "resources"
    >     }]
    > }
    > ```

2.  Open a command prompt.

3.  Log in to your Cloud Foundry environment using the Cloud Foundry command-line interface \(cf CLI\).

4.  Choose your subaccount.

    > ### Note:  
    > The cf CLI prompts you to choose an org. To find the org of your subaccount, use the cockpit to go to your subaccount. You can find the org in the Cloud Foundry tile under *Organization* \(see [Navigate to Orgs and Spaces](../50-administration-and-ops/navigate-to-orgs-and-spaces-5bf8735.md)\).

5.  Choose the space where your application is located.

6.  Deploy the application to your space.

7.  To define the redirect URIs, go to the folder where `xs-security.json` is stored.

8.  Define the redirect URIs in the `oauth2-configuration` property.

9.  Add a URL that contains the hostname of your application.

    > ### Sample Code:  
    > ```
    > {
    >  "xsappname": "<application_name>",
    >  "tenant-mode": "dedicated",
    >  "description": "My Sample Application with Application Router",
    >  "oauth2-configuration": {
    >                                            "token-validity": 900, 
    >                                            "redirect-uris": ["https://[/pandoc/div/div/horizontalrule/orderedlist/li/note/codeblock/strong/varname
    >      {"varname"}) <application_hostname> (varname].[/pandoc/div/div/horizontalrule/orderedlist/li/note/codeblock/strong/varname
    >      {"varname"}) <landscape_domain> (varname]/**"
    >                                                             ],
    >                                            "autoapprove": "true"
    > }
    > }
    > ```

    > ### Note:  
    > The application domain is the first part of the application URL: For more information, see the related link.
    > 
    > > ### Example:  
    > > <code><b>myapplication</b>.cfapps.hana.ondemand.com/**</code>
    > 
    > > ### Example:  
    > > For China \(Shanghai\) region:
    > > 
    > > <code><b>myapplication</b>.mycustomdomain.cn/**</code>

10. Update the `xsuaa` service instance that is bound to your application.

    ***cf update-service *<service\_instance\>* -c xs-security.json***

    > ### Note:  
    > If the service instance isn’t available, create it, bind your application to the service instance \(see the related links\), and restage the application.

    You’ve implemented a valid redirect after logout, and `xsuaa` checks the redirect URL against the list of allowed URIs in the OAuth 2.0 configuration.


**Related Information**  


[Application Router](application-router-01c5f9b.md "The application router is the single point-of-entry for an application running in the Cloud Foundry environment on SAP BTP. The application router is used to serve static content, authenticate users, rewrite URLs, and forward or proxy requests to other micro services while propagating user information.")

[Configuring Application URLs](https://help.sap.com/viewer/ea72206b834e4ace9cd834feed6c0e09/Cloud/en-US/7ceeaa5e528140c48ae53b68433293ba.html "By default, all applications running on SAP BTP are accessed on the hana.ondemand.com domain.") :arrow_upper_right:

[Update a Service Instance](update-a-service-instance-7f926eb.md "You can update a service instance from the xsuaa service using the service broker.")

[Cloud Foundry Command Line Interface](https://docs.cloudfoundry.org/cf-cli/cf-help.html)

[Application Security Descriptor Configuration Syntax](application-security-descriptor-configuration-syntax-517895a.md "The syntax required to set the properties and values defined in the xs-security.json application security descriptor file.")

[Security Considerations for the SAP Authorization and Trust Management Service](../60-security/security-considerations-for-the-sap-authorization-and-trust-management-service-f117cab.md#loiof117cab6b92d438cb2a0b5204713994b "Decisions you make when using or administrating the SAP Authorization and Trust Management service (XSUAA) can have an impact on the security of your applications. The information provided is meant to help you decide.")

