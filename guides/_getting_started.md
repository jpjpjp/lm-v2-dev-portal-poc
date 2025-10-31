# Getting Started with the Lunch Money API

Welcome to the Lunch Money developer API! This API has enabled the Lunch Money user community to build a broad set of tools and plug-ins to complement their Lunch Money experience and help other users. This guide will help you get started using the Lunch Money API.

Most users interact with their Lunch Money data using the Web or Mobile applications.  Making API calls is like using these apps. You can make API calls that get your data for you, allowing you to do other things with it, like generating your own charts or simply archiving your data. You can also make API calls that change or delete your data. Any changes you make are permanent, just like they would be if you made changes using the Mobile or Web apps, so it's important to be careful when you start using the API.

## Create a Test Budget

Since changes to your data made by the API are permanent, the best way to begin interacting with the API is to [create a new test budget](https://support.lunchmoney.app/miscellaneous/unlimited-budgeting-accounts) in your existing Lunch Money account.  As you start using the API you can interact with this test budget and ensure that your real data is not modified.

During the process of creating a new budget you may choose to copy your existing categories and tags to make your data more familiar:

<scalar-image
  src="../static/CopyCategoriesAndTags.png"
  alt="Copy Categories and Tags">
</scalar-image>
http://localhost:7970/lunch-money-developer-docs/static/CopyCategoriesAndTags.png
Alternatively, you may ask Lunch Money to populate your new budget with demo data which will include approximately three months of transaction data for you to test with:

![Demo Data](../static/DemoData.png)

## Getting an access token
Lunch Money API requests are authenticated using the Bearer Token authentication method. In plain english this means that you will pass a token associated with your budget to Lunch Money, so we know that which budget to operate on, and know that the request came from someone who can access that budget.

You can create an access token by going to [the developers page in the Lunch Money web app](https://my.lunchmoney.app/developers). Look at the green circle in the upper right hand corner and make sure that it is showing the test budget that you just created. If you aren't sure which budget is active you can click on the green circle to bring up the "Switch Accounts" dialog.  This will show you the full names of all the budgets associated with your account. Select the test budget you created. 

In the upper right hand corner of the page, you can give your access token a label and specify what you are using it for.  Then hit the button "Request Access Token":

![Request Access Token](../static/RequestAccessToken.png)

After you do this the web page will show you a long alphanumeric string. This is your access token. Hit the copy button and save this token somewhere safe. It is only shown to you when you create it.

Treat your access tokens like passwords and don't share them with anyone that you don't trust. If you ever worry that your access token has been compromised you can always come back to this page and hit the Revoke button. (This is a good reason to always give your access token a name.)

Later, when you are more comfortable with the API, you can repeat this process to create and access token for your real budget.

## Using your access token

When you start to write code that makes API calls you will need to include this token in each request using an Authorization header that includes your token. Here is an example curl request:

```bash
curl --location 'http://localhost:3002/me' \    # TODO fix URL                                                                                
--header 'Authorization: Bearer <YOUR_ACCESS_TOKEN'
```

If this doesn't make sense yet, that's OK, there is an easier way to use your access token to make requests. Head over to the [API Reference Documentation](../lunch-money-api-v2-reference#description/overview). In the upper right hand corner you'll see a box that says "Server", and below that a box which says "Authentication".  In the Authentication box you'll see a row that says "Bearer Token:".  Simply paste your access token into that box. Once that is set up you make API requests directly from the documentation.

Try this with the `/me` endpoint by scrolling down to the section called "Get Current User" and hitting the "Test Request" button.  This will pop up a dialog box that you can use to send an API request. This is a simple endpoint, so just hit the "Send" button.  You should see a response that includes your name, and the name of your test budget.

From here you can explore the rest of the Lunch Money API and start thinking about what you might do with it.

## What should I build?
Once you have a basic understanding of how the API works, there are few things to think about next.  The first think to consider is what you want to build.

Many users have built Transaction Import tools. These are especially useful in cases where you bank isn't supported by Plaid. But does your bank offer an API? You can build a bridge between Lunch Money and your bank to import transactions automatically.

Other user like to export their transactions into their own tools like a custom spreadsheet or other interface. If you have something like this you can use the API to sync data for personalized viewing and analytics.

This is just the tip of the iceberg. If you are looking for more inspiration or SDKs to help you build in your favorite programming language check out the [Awesome Lunch Money Projects Page](https://lunchmoney.dev/#awesome-projects) on github.  You might also get inspired by reading about some of the community developers we have featured in our [Community Newsletter](https://lunchmoney.app/blog?filter=community-news)

## I still need help!

If you still aren't sure how to get started, we have other resources to help. 

A great way to find get unstuck is to [join the Lunch Money community on Discord](https://lunchmoney.app/discord) and send a question in the [Developer API Channel](https://discord.com/channels/842337014556262411/1134594318414389258).

You can also [email our developer advocate](mailto:jp@lunchmoney.app) to get help as well.

These are also great channels to provide feedback! Our v2 API was built based on feedback from our developer community. Your ideas might help us make them even better, so please don't hesitate to reach out.

