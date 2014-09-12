METEOR IP ADDRESS BLOCKER
=========

cybit:ip-blocker is a meteor package that allows meteor developers to set up IP address based whitelists and blacklists for their app.

**Version 1.0**

About
------------
We recently launched a public beta app and found that some users were using it maliciously. Since we were behind a PaaS that doesn't allow for user customizable IDS (Intrusion Detection Systems) or IPS (Intrustion Prevention Systems) we found that we needed to block requests from malicious IPs at the server level. There was no way to do this. **There is NOW!**

Features
------------
  - Supports Meteor versions 0.9.0 and above.**We haven't tested it for prior releases**
  - Install it with a single command
  - Support for both REDIS and in-memory hashmaps of IP addresses
  - Expiry - Block or allow IPs for limited time periods or forever
  - Switch between blacklisting (allowing all except some) or whitelisting (denying all except some) seamlessly
  - Configure, based on performance the interval at which expired keys are deleted.
  - Use simple GET requests to add or remove IP addresses from the hashmap (blacklist or whitelist)
  - Block or Ignore unauthorized access when adding or removing IP addresses from the hashmap
  - Define your own redirect URL - **must be a URL other than your app.**

Setup
------------
Just run `meteor add cybit:ip-blocker` in your meteor app directory.

Configuration
------------
**This is the default configuration. It's already part of the package.**

```
IP_BLOCKER={
    redis_enabled:true,//By default IP blocker tries to use REDIS. If you don't wish to use REDIS you can switch to an in memory hashmap by setting this to false.
    
    redis_host:"127.0.0.1",//Your redis hostname. To be changed when using production environments like HEROKU
    
    redis_port:"6379",//Your redis port. To be changed when using production environments like HEROKU
    
    default_rule:"blacklist",//The default rule is to allow all except blacklisted. Change this to whitelist if you wish to allow some and block all.
    
    secret_key:"SJ4tk6nv35WS8b6PT3K799K86d549m535Sfm8gHY6y6zAm97928896283S568n5S",//The secret key. Change this to an A-Za-z0-9 string
    
    default_expiry:86400000,//0 to never expire or in milliseconds the amount of time to block a user for.
    
    redirect_url:'http://nyanit.com/nyanit.com',//The redirect URL
    
    on_error:'allow',//If there's an error allow the request through or block it
    
    push_ip_route:'push_ip',//The iron-router route through which you can push IP addresses to your whitelist or blacklist
    
    pull_ip_route:'pull_ip',//The iron-router route through which you can pull IP addresses to your whitelist or blacklist
    
    on_auth_error:'block',//If a user tries to push or pull an IP and the authorization fails block the user and redirect.
    
    on_auth_limit:3,//The max number of times an auth request can fail.
    
    interval_duration:7200000, //In milliseconds the interval at which the server checks for expired keys and deletes them.

    redis_password:null //No password for localhost. Put your password here if you're using a production setup.

};
```


If you wish to override only some properties you can simply do `IP_BLOCKER."property"="value"` in your main server block.

Alternatively you can override the whole object in your main server block.
**To Push an IP Address to the Hashmap**

To add an IP address to the hashmap simply call `YOUR_APP_URL/"PUSH_IP_ROUTE"?secret_key="YOUR_SECRET_KEY"&ip_address="THE_IP_ADDRESS"` as a **GET** request. It will return **1** if successful or **0** for failure.

e.g. `http://localhost:3000/push_ip?secret_key=SJ4tk6nv35WS8b6PT3K799K86d549m535Sfm8gHY6y6zAm97928896283S568n5S&ip_address=192.168.0.100`

**To Pull an IP Address from the Hashmap**

To remove an IP address to the hashmap simply call `YOUR_APP_URL/"PULL_IP_ROUTE"?secret_key="YOUR_SECRET_KEY"&ip_address="THE_IP_ADDRESS"` as a **GET** request. It will return **1** if successful or **0** for failure.

e.g. `http://localhost:3000/pull_ip?secret_key=SJ4tk6nv35WS8b6PT3K799K86d549m535Sfm8gHY6y6zAm97928896283S568n5S&ip_address=192.168.0.100`



Contributions
------------
Contributions are always welcome. If you have any issue that you can resolve then send us a pull request. 


License
------------

MIT

**Free Software, Hell Yeah!**

Contributors
------------
Abhijeet Sutar: [ajduke]

Shrenik Kenia (**Me**): [shrenik] 
[ajduke]:http://github.com/ajduke
[shrenik]:http://github.com/scorpwarp23
