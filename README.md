spring-social-appengine
=======================

## Adds Google AppEngine-based ConnectionRepository implementation to Spring Social

## Apply dirty workarounds on these issues:

[ProviderSignInAttempt - NotSerializableException: ConnectionFactoryRegistry](https://jira.springsource.org/browse/SOCIAL-203)

The author of Spring Social, Craig Walls, states that this is a bug of Google App Engine dynamic proxies.
Maybe, but, think again, what is the purpose of ProviderSignInAttempt?
It is just a log indicating that there was a failed attempt to sing-in, it is just for reference.
So, why save a full Connection object, and in turn, the ConnectionFactoryLocator and the UsersConnectionRepository object?
Remember, these two are interfaces, anyone can provide their own implementation, god know what else will be serialized!
Why not just save string information that human can read and can safely be serialized?

[Persistent session managment - NotSerializableException for OAuth2Connection and OAuth1Connection](https://jira.springsource.org/browse/SOCIAL-355)

This is actually the same type of bug as above, I think.

[SocialAuthenticationProvider should support multiple connections](https://jira.springsource.org/browse/SOCIAL-402)

The default behavior is some kind of odd, as I stated in the jira issue tracker.

#### Notes on my dirty workarounds:

Changes the references to non-serializable objects transient:

* org.springframework.social.connect.web.ProviderSignInAttempt
* org.springframework.social.security.SocialAuthenticationToken

Fix the if condition directly:

* org.springframework.social.security.SocialAuthenticationProvider

#### Maven Configuration for Google App Engine

* Add Maven dependencies and plugins

  According to this guide: [Using Apache Maven](https://developers.google.com/appengine/docs/java/tools/maven), add Google App Engine maven library and plugin dependencies to pom.xml.

* Enable session

  [Java Application Configuration with appengine-web.xml - Enabling sessions](https://developers.google.com/appengine/docs/java/config/appconfig#Enabling_Sessions)
