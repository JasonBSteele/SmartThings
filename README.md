# SmartThings

A repository for my SmartThings groovy scripts.

## To Install a SmartApp
1. Browse to [https://ide.smartthings.com](https://ide.smartthings.com)
2. Click the **My SmartApps** link.
3. Click on the **+ New SmartApp** button.
4. Click the **From Code** tab.
5. Paste in the script code and click the **Create** button.
6. Click the **Save** button.
7. Click the **Publish** button then select **For Me**.
8. Run the SmartThings app on your phone and select the new SmartApp from the **My Apps** category.

## To Enable OAuth
1. Browse to [https://ide.smartthings.com](https://ide.smartthings.com)
2. Click the **My SmartApps** link.
3. Click the **App Settings** button.
4. Scroll down the page and click the **OAuth** link.
5. Click the **Enable OAuth in SmartApp** button.
6. Copy the revealed **Client Id** and **Client Secret** for later use.
6. Click the **Update** button to save the changes.

## To Authenticate
> Thanks to Joshua Lyon I now know that it is necessary to open an Incognito Chrome Tab for the browser mentioned in the steps below due to a session cookie biendg sent and preventing the authentication from working properly.
>See https://community.smartthings.com/t/oauth-flow-changed/19077/2
1. Navigate to this URL into your browser, substituting in the **Client Id**: `https://graph.api.smartthings.com/oauth/authorize?response_type=code&client_id=<Client Id>&scope=app&redirect_uri=https%3A%2F%2Fgraph.api.smartthings.com%2Foauth%2Fcallback`
2. If you are prompted to login to SmartThings, go ahead.
3. Select you location from the drop down list and the devices you want to have access to through the REST API
4. Click the **Authorize** button.
5. You'll be redirected to a URL that looks like this: `https://graph.api.smartthings.com/oauth/callback?code=<Code>`
6. Copy the **Code** from the URL for later use.
7. Navigate to this URL in your browser, substituting in the **Client Id**, **Client Secret** and **Code**: `https://graph.api.smartthings.com/oauth/token?grant_type=authorization_code&client_id=<Client Id>&client_secret=<Client Secret>&code=<Code>&scope=app&redirect_uri=https%3A%2F%2Fgraph.api.smartthings.com%2Foauth%2Fcallback` 
8. You should get a response that looks like:
	````JSON
	{
		"access_token": "00000000-1111-2222-3333-444444444444",
		"expires_in": 1576799999,
		"scope": "app",
		"token_type": "bearer"
	}
	````

9. Copy the **Access Token** for later use.
10. Navigate to this URL, substituting in the **Access Token**: `https://graph.api.smartthings.com/api/smartapps/endpoints?access_token=<Access Token>`
11. You should get a response that looks like:
	````JSON
	[
		{
			"oauthClient":
			{
				"clientId":"00000000-1111-2222-3333-444444444444",
				"authorizedGrantTypes":"authorization_code"
			},
			"url": "/api/smartapps/installations/00000000-1111-2222-3333-444444444444"
		}
	]
	````

12. Copy the **Installation URL** for later use.

## To Call the SmartApp Installation Endpoint
1. Append one of the patterns that the SmartApp handles to the end of the **Installation URL**. For example to get a list of all the `temperature` devices append `/temperatures` to the **Installation URL**.
2. Now navigate to this URL, substituting in the **Installation URL** and **Access Token**: `https://graph.api.smartthings.com<Installation URL>/temperatures?access_token=<Access Token>`
3. You should get a response that looks like:
	````JSON	
	[
		{
			"id":"00000000-1111-2222-3333-444444444444",
			"name":"Living Room",
			"unitTime":1431785594107,
			"temperature":"20"
		},
		{
			"id":"00000000-1111-2222-3333-444444444444",
			"name":"Hall",
			"unitTime":1431773896858,
			"temperature":"17"
		}
	]
	````

4. Passing the **Access Token** on the URL isn't secure, so ideally you would pass it in the HTTP header where it would get encrypted (as we are using HTTPS). To pass it in the HTTP header add the following item with the **Access Token** substituted in: `Authorization: Bearer  <Access Token>` and remove `access_token` from the URL.





