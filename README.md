### Video Demo Here: https://www.youtube.com/watch?v=dTmbmiftRnA

# What is GitBot?

GitBot is a Discord bot which aims to provide basic integration between Discord servers and GitHub repositories.

Currently, GitBot allows users to subscribe a Discord server's channel to a GitHub repository that they own.
Then, GitBot will send messages to that channel providing notification of newly pushed commits and pull request interactions.

# How does it work?

GitBot makes extensive use of OAuth and GitHub's public API to perform it's tasks.

When adding GitBot to your Discord server, Discord's OAuth service is used to authorize GitBot to join your server.

When subscribing a Discord server's channel to a GitHub repository, GitBot will instruct you to visit a link on our web server
which redirects to GitHub's OAuth portal. Once you sign in and give GitBot the authorization it needs, it creates a webhook
in the specified repository using an access token obtained from GitHub's API once you provide authorization.

This webhook instructs GitHub to send us a POST request with an event payload any time someone: 
* pushes to the repository
* opens a pull request
* closes a pull request
* re-opens a pull request.

Upon receiving this payload, we use its contents to determine the proper message to send to Discord.
Once this is determined and the message is constructed, GitBot sends it to the channel.

# Security

When GitBot creates a Webhook, a secret hash is generated and stored in GitBot's database. Then, anytime a payload is received
from a webhook, we pull up the stored secret and make sure it matches the one provided by GitHub.
This ensures that webhook payloads are not tampered with, and provides an additional layer of security to the end user.

# Setup

GitBot requires a `keys.ts` file to be present in the `src/config` folder. This contains config options and keys that GitBot needs to run.

An example `keys.ts` file is below:

```javascript
export default {
	SERVER_PORT: 8080,
	GITHUB_CALLBACK_HOST: "123.123.123.123",
	DISCORD_CLIENT_ID: "DISCORD_CLIENT_ID_HERE",
	BOT_TOKEN: "BOT_TOKEN_HERE",
	GITHUB_CLIENT_ID: "GITHUB_CLIENT_ID_HERE",
	GITHUB_CLIENT_SECRET: "GITHUB_CLIENT_SECRET_HERE",
	MYSQL: {
		HOST: "localhost",
		USER: "USER_HERE",
		PASSWORD: "PASSWORD_HERE",
		DATABASE: "GitBot"
	}
};

```
