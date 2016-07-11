The Azure Portal supports setting up continuous deployment from a VSTS account. One tricky part is that it requires your VSTS account to be linked with one of your Azure subscription.

## Linking your VSTS account to your Azure subscription

The last step is that you need to link your VSTS account to your Azure subscription (see also [this post](https://www.visualstudio.com/en-us/get-started/setup/set-up-billing-for-your-account-vs) on this topic).

To do this, go to the Azure Portal, click Browse and search for 'Team':

![image](https://cloud.githubusercontent.com/assets/556238/13531726/d8c9608e-e1dc-11e5-83a0-d35df99cc62b.png)

Now select the relevant Team Services account, and click Link:

![image](https://cloud.githubusercontent.com/assets/556238/13531647/7fb69b24-e1dc-11e5-9bf1-c313cfe04cb6.png)

And you're done! You will now be able to set up continuous deploying to your git repos hosted in VSTS.