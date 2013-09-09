Sometimes, users report that everything works fine on their machine, but doesn't run correctly when they 'git push' to Azure. While there are many possible reasons for this, one common theme is that they have uncommited files on their client that are necessary for things to run. Hence it is important to remember that **only committed files end up on Azure**.

## Test locally on a clean repository

The best way to eliminate that possibility is to test your site using a clean repository on the client. One veru simple way to do this is to do a fresh clone. e.g. if your repo is in the `MySite` folder, try running:

    clone MySite MySiteClean

And then test your scenario from the `MySiteClean` folder. If things don't quite work, then you'll need to go back to your `MySite` folder and figure out what extra files might be in there, which you may need to commit. 