The Azure Portal supports setting up continuous deployment from a VSTS account. One tricky part is that this is only supported if the account is backed by an Azure Active Directory.

## How you can tell if a given account is backed by AAD

- go to https://{youraccount}.visualstudio.com/ and sign in
- Click on the settings wheel in the top right
- Click on the Settings tab
- Go to the bottom

If it's **not** backed by AAD, it'll look like this, and the account will not be usable for deployment:

![image](https://cloud.githubusercontent.com/assets/556238/13366727/13e4026e-dc92-11e5-80ba-6a48fd8c9782.png)

If it **is** backed by AAD, it'll look like this (with whatever the directory name is):

![image](https://cloud.githubusercontent.com/assets/556238/13366748/2f4252fe-dc92-11e5-8307-e8fc91c9b151.png)


## Creating an AAD backed VSTS account

After the account is created, if it's not AAD backed, you cannot change that. So it's important to do it right from the start!

- From a clean Ingognito browser, go to https://www.visualstudio.com/
- Click on Sign in in the top right. You'll see a page that looks like this:

![image](https://cloud.githubusercontent.com/assets/556238/13366851/c168d8a6-dc92-11e5-9a22-c445667a449e.png)

- Do **not** sign in from this page! Instead, click the 'Sign in with your work or school account' button
- In the next page, type in your Microsoft ID (aka Live ID) and press Continue. You'll be redirected to a new page that looks like this:

![image](https://cloud.githubusercontent.com/assets/556238/13366914/2b8559b2-dc93-11e5-80f2-6b71eff66e56.png)

- It looks very similar to the first one you saw, but it doesn't have the 'Sign in with your work or school account' link. Go ahead and Sign in from this page.
- If your account belongs to multiple directories, you'll see a page asking you to pick one. Otherwise, skip this step.
- You are now logged in to VSTS. Click 'Create a free account now'. The account that will be created will be correctly backed by AAD, and will work in Azure.


## Linking your VSTS account to your Azure subscription

The last step is that you need to link your VSTS account to your Azure subscription (see also [this post](https://www.visualstudio.com/en-us/get-started/setup/set-up-billing-for-your-account-vs) on this topic).

To do this, go to the Azure Portal, click Browse and search for 'Team':

![image](https://cloud.githubusercontent.com/assets/556238/13531726/d8c9608e-e1dc-11e5-83a0-d35df99cc62b.png)

Now select the relevant Team Services account, and click Link:

![image](https://cloud.githubusercontent.com/assets/556238/13531647/7fb69b24-e1dc-11e5-9bf1-c313cfe04cb6.png)

And you're done! You will now be able to set up continuous deploying to your git repos hosted in VSTS.