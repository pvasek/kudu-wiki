Here is a short [screencast](http://www.youtube.com/watch?v=kxgTtIeFppk) that demonstrates the use of the Console.

The Kudu Console is an experimental tool that gives you both command line and file browser access to your sites, all from the comfort of a web browser.

If you start at the [[root of the Kudu service|Accessing-the-kudu-service]], you'll see a `Debug console` menu item that leads to the Kudu Console.

Note that there is also a local (non-browser based) client that gives you the same Console functionality. See this [post](http://blog.amitapple.com/post/45675601255/azurewebsiteterminal) for details. The post also has some good information about the Console that also applies to the browser-based version, so please read through.

Here are various things you can do with the Kudu console:

## Use it to run commands (e.g. git operations)

Even though it's not a true remote console, it tries hard to pretend to be one.

You can use it to do most standard console operations: changing folder, copy/rename/delete files, etc.

You can also use it to run arbitrary external commands. One good use case is to run git commands against your repository (if you are using git). e.g. you can do a 'git log', 'git status', and generally run arbitrary git commands, which can be useful to diagnose certain issues.


## Navigate around using the folder UI

e.g. instead of typing 'cd site', you can just click on the site folder in the folder UI. The nice thing is that the console window then automatically navigates to the same folder, so the two halves of the Console are working together.


## Download files and folder

In the UI, Next to each file and folder, there is a Down Arrow icon that lets you download the item.

For files, it directly downloads the file by navigating to it. Depending on the mime type, the browser might navigate to it (in a new Tab), or download it.

For directories, it downloads a zip file containing the full content of the folder, which is very useful to quickly download a bunch of files (either for backup or offline analysis). e.g. if you go to the Site folder, and click the download icon for the wwwroot folder, you will get back the full contents of your live site.


## Upload files and folder using drag and drop

You can drag some files from the Windows shell (or Mac finder), and drop them directly into the Kudu Console's UI, much like you would between two Windows shell folders. You have to try it to believe it! :)


## View and Edit text files

If you click the Edit icon next to a file, the Console will open up a window where you can view and edit the file. It's a super basic editor, so don't expect any syntax coloring or fancy code editor features. But it can still be helpful when investigating issues. e.g. you could use it to quickly tweak a web.config file.