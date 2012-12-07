### Prerequisites
* Visual Studio 2012
* IIS >= 7 (Configured to run ASP.NET 4.0)
* Use [WebPI](http://go.microsoft.com/fwlink/?LinkID=145505) and install IIS and ASP.NET 4.0
* Install Git (http://git-scm.com/downloads) to the default location (we expect it in ```C:\Program Files (x86)\Git\bin```).


If you manually install IIS, here is what needs to be enabled:

![Windows Features](http://i.imgur.com/ZdFpz.png)


### Deploying your first application
1. First clone the repository.

        git clone git://github.com/projectkudu/kudu.git

3. Open Kudu.sln as **Administrator** and build it.

4. Set **Kudu.Web** as the startup project and run!

After running Kudu.Web, you should be able to use the dashboard to manage your application. Below is a walk through on how to create and deploy your first application using git.

### Step 1
Create an application.

![Step 1](http://i.imgur.com/pScf7.png)

### Step 2
Give it a name.

![Step 2](http://i.imgur.com/xebWn.png)

### Step 3
Copy the git url and push.

![Step 3](http://i.imgur.com/vsWqb.png)

### Step 4
You're live!

![Step 4](http://i.imgur.com/zN5lf.png)