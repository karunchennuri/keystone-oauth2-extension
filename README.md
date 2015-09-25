# keystone-oauth2-extension
OpenStack Keystone extension to enable OAuth 2.0.

## How to Install
To install this extension in Keystone, you have to do the following:

1. Place the `oauth2` folder inside the `keystone/contrib` folder in your Keystone project.

2. Place the files in `tests/` inside the `keystone/tests` folder in your Keystone project.

3. This extension implements an auth plugin. You need to add the `plugins/oauth2.py` module to the `keystone/auth/plugins` folder in your Keystone project.

   > You can use the files inside the `config` folder to override your `etc/keystone.conf.sample` and `etc/keystone-paste.ini` files. They are a modified version of the default Keystone files that make this extension work. If you do so, **you can skip steps 4-6**. Should you prefer to set up everything your own, please read on.

4. Since this extension is augmenting a pipeline (see [Keystone docs](http://docs.openstack.org/developer/keystone/extension_development.html#modifying-the-keystone-paste-ini-file) for more info), a corresponding `filter:` section is necessary to be introduced in your `etc/keystone-paste.ini` file. Just place the following:
   ```
   [filter:oauth2_extension]
   paste.filter_factory = keystone.contrib.oauth2.routers:OAuth2Extension.factory
   ``` 
5. In order for the extension to work, it must be placed in the `pipeline`. This extension is recommended to be placed right instead of the default `oauth1_extension`.

6. Finally, edit the `[auth]` section in your `keystone.conf` file (the one placed in the `etc` folder in your Keystone project), to include OAuth 2.0 auth method, just like this:
   <pre>
   # Default auth methods. (list value)
   methods=external,password,token,<b>oauth2</b>
   </pre>

   At the end of the section you have to add this:
   ```
   # The oauth2 plugin module (string value)
   oauth2=keystone.auth.plugins.oauth2.OAuth2
   ```

> The files inside the `config` folder are an example of the settings 4-6.
