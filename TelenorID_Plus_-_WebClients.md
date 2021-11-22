## Web Clients

### TelenorID\+ OIDC Authorization Code Flow With ClientID and Secret

![TelenorID Pluss OIDC Authorization Code FLow Client Secret](https://www.websequencediagrams.com/files/render?link=iuABiysNv1svNezK5AJXxSw18XNwkC05MB7EK8dx2PMoN7W33tqbOkg1MejqYkOB)

[Standard OIDC Authorization Code Flow](TelenorID_Plus_-_standard_oidc_flows.md#standard-oidc-authorization-code-flow) for reference.

### Authentication Using Client ID and Secret

|                           |
| ------------------------- |
| **Do NOT store the client id and secret unencrypted in git. Never pass the client secret as a URL parameter.** |

[Creating a Shared Secret](https://docs.identityserver.io/en/release/topics/secrets.html)

You can either send the client id/secret combination as part of the POST body:

| POST Body                           |
| ----------------------------------- |
| POST /connect/token <br/> client\_id=client1& <br/> client\_secret=secret& <br/> ... |

..or as a basic authentication header:

| Basic Authentication Header         | 
| ----------------------------------- | 
| POST /connect/token<br/> Authorization: Basic xxxxx<br/> ... |

You can manually create a basic authentication header using the following C# code:

| C# Code                             | 
| ----------------------------------- | 
| var credentials = string.Format("{0}:{1}", clientId, clientSecret);<br/>var headerValue = Convert.ToBase64String(Encoding.UTF8.GetBytes(credentials));<br/>var client = new HttpClient();<br/>client.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Basic", headerValue); | 

The [IdentityModel](https://urldefense.proofpoint.com/v2/url?u=https-3A__github.com_IdentityModel_IdentityModel&d=DwMGaQ&c=eIGjsITfXP_y-DLLX0uEHXJvU8nOHrUK8IrwNKOtkVU&r=PmUJdtih5o42k0kXOdnC3jHg07jxhDU5Y1DQXG7amQQIL8kqZzyCGCvUnwsv5IzY&m=sp-JS3m0rgBIy_zWGHRgIoOeUv4dBIgtPeosEPdS9js&s=SUp79QWcBy2CIMSl8O5qhqEm94wbJSgh0oRmFbeXnZo&e=) library has helper classes called **TokenClient** and **IntrospectionClient** that encapsulate both authentication and protocol messages. 

|                              | 
| ---------------------------- | 
| We **strongly encourage** the use of libraries for the integration. <br/> This will simplify the integration, help avoid common pitfalls/bugs, and ease upgrading to newer versions of the protocol.| 
| You can find libraries for Java, .NET, JavaScript, TypeScript, PHP, Python, and even Erlang here: [https://openid.net/developers/certified/](https://openid.net/developers/certified/) |

### JavaScript

#### Preferred Library

[https://github.com/IdentityModel/oidc-client-js](https://github.com/IdentityModel/oidc-client-js)

#### Simple Code Example

Using the client library
*   Browser client: [https://github.com/IdentityModel/oidc-client-js](https://github.com/IdentityModel/oidc-client-js)

or  

*   node.js backend: [https://github.com/panva/node-openid-client](https://github.com/panva/node-openid-client)

Showing a simple client login and callback:

**[DCSEC / telenor-idpluss-integration-examples / JavaScript / node-openid-client.js](https://prima.corp.telenor.no/bitbucket/projects/DCSEC/repos/telenor-idpluss-integration-examples/browse/JavaScript/node-openid-client.js?at=refs/heads/master)**

```javascript
const { Issuer } = require('openid-client');
const express = require('express');
const session = require('express-session')
const configloader = require('tn-dchub-encryptedconfig');

const config = configloader.loadJsonConfigSync('config/config.json', '/secrets/master-key');

const app = new express();

app.set('trust proxy', true);

app.use(session({
    secret: config.sessionSecret,
    resave: false,
    saveUninitialized: true,
    cookie: { secure: config.secureCookie }
}));

var client = null;

Issuer.discover('https://idp.telenorid-staging.com') // => Promise
    .then(issuer => {
        client = new issuer.Client({
            client_id: config.client_id,
            client_secret: config.client_secret,
            redirect_uris: [`${config.host}${config.callbackUrl}`],
            response_types: ['code']
        });
        app.listen(config.port, () => {
            console.log("It's configured. Listening on " + config.port);
        });
    });

app.get('/authorize', (req, res) => {
    let url = client.authorizationUrl({
        scope: config.scopes.join(' '),
        redirect_uri: `${config.host}${config.callbackUrl}`
    });
    res.setHeader('Location', url);
    res.status(302)
    res.end();
});

app.get(config.callbackUrl, (req, res) => {
    const params = client.callbackParams(req);
    client.callback(`${config.host}${config.callbackUrl}`, params) // => Promise
        .then(function (tokenSet) {
            req.session["access_token"] = tokenSet.access_token;
            req.session.save(() => {
                res.status(302);
                res.setHeader('Location', '/');
                res.end();
            });
        });
});

app.get('/userinfo', (req, res) => {
    let access_token = req.session["access_token"];
    if (!access_token) {
        res.status(401)
        return res.end("You are not logged in.");
    }

    client.userinfo(access_token) // => Promise
        .then(function (userinfo) {
            console.log(userinfo);
            res.end(JSON.stringify(userinfo));
        });
});

app.get('/', (req, res) => {
    res.setHeader('Content-Type', 'text/html');
    res.end(`   
        <p>Hello world <a href="/authorize">login</a></p>
        <div id="hola"></div>
        <script>
        fetch('/userinfo')
            .then(d => d.json())
            .then(d => document.getElementById('hola').innerText = JSON.stringify(d))
        </script>
    `);
});
```

### Java

#### Preferred Library

We will develop our own library. If you cannot wait, use one of the Java-libraries here: [https://openid.net/developers/certified/](https://openid.net/developers/certified/)

#### Simple Code Example

Showing a simple client login and callback:

**[DCSEC / telenor-idpluss-integration-examples / Java / SpringBoot.java](https://prima.corp.telenor.no/bitbucket/projects/DCSEC/repos/telenor-idpluss-integration-examples/browse/Java/SpringBoot.java?at=refs/heads/master)**

```java
import dchub.feature.encryptedproperties.EncryptedPropertiesInitializer;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;
import org.springframework.security.oauth2.client.registration.ClientRegistrations;
import org.springframework.security.oauth2.client.registration.InMemoryClientRegistrationRepository;
import org.springframework.security.oauth2.core.AuthorizationGrantType;

@SpringBootApplication
public class Application extends WebSecurityConfigurerAdapter {

    @Value("${oidc.issuer}")
    private String issuer;
    @Value("${oidc.clientId}")
    private String clientId;
    @Value("${oidc.clientSecret}")
    private String clientSecret;
    @Value("${oidc.scopes}")
    private String scopes;
    @Value("${oidc.redirectPath}")
    private String redirectPath;
    @Value("${oidc.authorizationPath}")
    private String authorizationPath;
    @Value("${oidc.loginPath}")
    private String loginPath;



    public static void main(String[] args) {

        SpringApplication application = new SpringApplication(Application.class);

        application.addInitializers(new EncryptedPropertiesInitializer());
        application.run(args);
    }

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        var oidcScopes = scopes.split(",");

        var reg = new InMemoryClientRegistrationRepository(
                ClientRegistrations
                        .fromOidcIssuerLocation(issuer)
                        .clientId(clientId)
                        .clientSecret(clientSecret)
                        .scope(oidcScopes)
                        .redirectUriTemplate("{baseUrl}" + redirectPath)
                        .authorizationGrantType(AuthorizationGrantType.AUTHORIZATION_CODE)
                        .build()
        );

        http
                .oauth2Login(oauthclient -> {
                       oauthclient.clientRegistrationRepository(reg);
                       oauthclient.authorizationEndpoint(c -> c.baseUri(authorizationPath));
                       oauthclient.redirectionEndpoint(r ->
                          r.baseUri(redirectPath)
                       );
                       //oauthclient.loginPage(loginPath);
                })
                .authorizeRequests(a -> a
                    .antMatchers("/", "/error").permitAll()
                    .anyRequest().authenticated()
                )
                .logout(l -> l
                        .logoutSuccessUrl("/").permitAll()
                );
    }

}
```

### .NET

#### Preferred Library

[https://identitymodel.readthedocs.io/en/latest/](https://identitymodel.readthedocs.io/en/latest/)

#### Simple Code Example

Coming...

### Example 1 - Step-by-step: Login

The following example will walk through the common use case for logging in with IBIS. The examples show raw HTTP requests generated using the public debugger (available here: [https://oidc-test.telenor.no/](https://oidc-test.telenor.no/)) and a public demo-client.  
**Thus this example uses a OpenID Connect Flow with PKCE even though most web clients will be confidential and should use the OpenID Connect Authorization Flow with client id and secret handled in a backend.**

The debugger utilises the **[oidc-client-js](https://github.com/IdentityModel/oidc-client-js)** client. As can be seen by the parameters in the example there are several parameters which must be securely generated and later validated; we once again stress that you should not try to "hand craft" oauth / oidc requests.

[End User Login](TelenorID_Plus_-_user_login_-_integration_example_step_by_step.md)