# Setting up email service for a client

If requested by a client, we can setup an email for them as `<clientname>.penguin.tools`. This domain will be used to send transactional emails from our system.

This is great to simplify the process when you need to send e.g. forgot password emails. If you have a non-tech client, it can be confusing for them to setup and maintain an account.

In no situation this setup can be handed out to the client. It is supposed to always stay a service that is only used for internally required functionality.

**Requirements: You need someone with a Penguin Lab Admin Level to complete this task.**

## Using sendgrid

### Login
Login to two services:
1) Sendgrid (sendgrid.com) - for setting up the email address
2) Cloudflare (cloudflare.com) - for setting up the customer subdomain

### Authenticate Domain

1) In the left sidebar goto: Settings > Sender Authentication (or directly access https://app.sendgrid.com/settings/sender_auth).
2) Press the button "Authenticate Your Domain".
3) Choose Cloudflare as a DNS host and select "YES" to brand the tracking links.
4) Enter the client domain that looks like this: `<clientname>.penguin.tools` and hit "next".

Kepp Sendgrid open and head over to Cloudflare!

5) In cloudflare: Select the "penguin.tools" domain and make sure you are in the DNS settings.
6) Create each of the entries shown in sendgrid in cloudflare. Disable the proxy for those entries.
7) Once done, confirm the setup in sendgrid. It could take a few moments to verify - but normally it shold happen instantly.

### Add a single sender

1) Make sure you are in the Sender Authentication settings: Settings > Sender Authentication (or directly access https://app.sendgrid.com/settings/sender_auth).
2) Press the button "Verify a single sender"
3) Enter the data of our client. As an email (both send and reply), you should configure: `noreply@<clientname>.penguin.tools`. And use `<clientname>` as nickname.
4) You should see a success message that your email was "automatically verified because it matches an authenticated domain".

### Receive the API Key

We are using a set of two API Key. One for the Server - a production API Key - and one for the developer - a dev API Key.

#### Production API Key

Please create exactly one Production API key per client. Do not share API keys between clients. Double check on the API Key Page that no key exists yet. Goto Settings > API Keys (https://app.sendgrid.com/settings/api_keys)

*In case there is already an API Key and you don't have it, you are in trouble. You should go and check who has it or where it is (probably it's a good idea to look in the server env variables - eg. heroku ) because you can't get it from Sendgrid anymore. Only continue to create an additional key if you really can't find the existing one.*

1) In the sidebar goto: EMAIL API > Integration Guide (or directly access https://app.sendgrid.com/guide/integrate).
2) Select your setup method and your programming language if required.
3) Once you see the instructions, under "Create your API key", enter `<clientname>` as API Key Name and press "Create Key". This will create a key with restricted access to only allow to send emails. (that is probably what you want!)

Add this API Key to the production Server (and any other servers like staging environments). Per default you should use the default environment vairable name `SENDGRID_API_KEY` - share the environment variable name with the developers.
**DO NOT share** this API key with anyone.

#### Developer API Key

Please create one Developer API Key *per lab request*! Ensure to delete the API key after the request is completed. You are responsible for this.

Follow the steps above to create a second API key. But enter `<clientname>-#<request-id>` as API Key Name.
Share the API Key with the developers who need it using the lab sharing functionality. You probably also want to share the instructions.

Keep in mind to delete the API Key once the request is completed.
