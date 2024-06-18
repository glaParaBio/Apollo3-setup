<!-- vim-markdown-toc GFM -->

* [Configuring logins](#configuring-logins)
    * [Set up Google login:](#set-up-google-login)
    * [Set up Microsoft login (incomplete)](#set-up-microsoft-login-incomplete)

<!-- vim-markdown-toc -->

## Configuring logins

We'll need to do some more setup to get regular Apollo logins working. As a
note, this is why a domain name is necessary. You can access Apollo using just
the public IP address of your server, but Google and Microsoft will not allow
you to configure a login for the server without a domain name.

The first thing you'll need to do is

### Set up Google login:

We'll start by configuring Google. You'll need to use a Google account to create
a "client ID" and "client secret" for your Apollo instance. Here is how to set
up authentication with Google and get those values.

- Go to https://console.developers.google.com/
- Log in
- To the left of the search, click on the project selector dropdown
- Click "New project", enter a "Project name" and "Location"
- Once in the project, click the top left hamburger menu -> "APIs & Services" ->
  "Credentials"
- Click "+ Create Credentials" at the top, select "OAuth client ID"
- Select application type "Web application"
- Give it a name (e.g. MyOrg's Apollo)
- Enter the URL of your app as an authorized JavaScript origin, e.g.
  `http://example.com`
- Enter the following as an authorized redirect URI, replacing the `example.com`
  with the correct value for your URL:
  `http://example.com:3999/auth/google`
- Click "Create"
- Take note of Client ID and Client secret listed

Now that you have the client ID and secret, you'll need to add them to your
`.env` file. There are placeholders in the file for these in the file, so
you'll need to uncomment those lines (remove the `# ` from the beginning) and
fill in the correct values for `GOOGLE_CLIENT_ID` and `GOOGLE_CLIENT_SECRET`.

Now when you refresh the page (you may need to clear your cache), you should be
able to log in with a Google account. The first user to log in automatically
becomes an admin user, and all following users get the role specified by
`DEFAULT_NEW_USER_ROLE` in `.env`. By default, new users do not have any
role (not even read-only access) and must be assigned one by an admin.

At this point you can also make other changes to your `.env` if you would
like, making sure to run `docker compose restart` to apply them. For example,
you can disable guest access, or change the role of guest users.

Congratulations! As an admin user, you can now start adding assemblies to Apollo
and start annotating!

### Set up Microsoft login (incomplete)

The guide for Microsoft logins is still in development and is incomplete. A
rough sketch for creating the necessary tokens is below. Microsoft logins,
however, require HTTPS access, and this section requires more work to lay out
the requirements and options of working with SSL certificates.

- Go to
  https://portal.azure.com/#view/Microsoft_AAD_RegisteredApps/ApplicationsListBlade
- Log in
- Click "New registration"
- Give the app a name
- Select supported account types (suggest "Accounts in any organizational
  directory (Any Azure AD directory - Multitenant) and personal Microsoft
  accounts (e.g. Skype, Xbox)")
  - Could be either or depending on use case
- Click "Register"
- Note Application (client) ID
- Go to new app's details
- Under "Client credentials" click "Add a certificate or secret"
- Click "New client secret"
- Enter a description and an expiration date
  - Note the expiration date so you can rotate keys before then
- Note newly registered client secret (Value, not Secret ID)
