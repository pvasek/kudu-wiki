The Kudu Console is a tool that gives you both command line and file browser access to your sites, all from the comfort of a web browser.

**To access it, simply go to `{yoursite}.scm.azurewebsites.net`, and click on Debug Console at the top.**

Here is a short [screencast](http://www.youtube.com/watch?v=kxgTtIeFppk) that demonstrates the use of the Console (though it's a bit outdated).

Note that there is also a local (non-browser based) client that gives you the same Console functionality. See this [post](http://blog.amitapple.com/post/45675601255/azurewebsiteterminal) for details. The post also has some good information about the Console that also applies to the browser-based version, so please read through.

Here are various things you can do with the Kudu console:

## Use it to run commands (e.g. git operations)

You can use it to do most standard console operations: changing folder, copy/rename/delete files, etc.

You can also use it to run arbitrary external commands. One good use case is to run git commands against your repository (if you are using git). e.g. you can do a 'git log', 'git status', and generally run arbitrary git commands, which can be useful to diagnose certain issues.


## Navigate around using the folder UI

e.g. instead of typing 'cd site', you can just click on the site folder in the folder UI. The nice thing is that the console window then automatically navigates to the same folder, so the two halves of the Console are working together.


## Download files and folder

In the UI, Next to each file and folder, there is a Down Arrow icon that lets you download the item.

For files, it directly downloads the file by navigating to it. Depending on the mime type, the browser might navigate to it (in a new Tab), or download it.

For directories, it downloads a zip file containing the full content of the folder, which is very useful to quickly download a bunch of files (either for backup or offline analysis). e.g. if you go to the Site folder, and click the download icon for the wwwroot folder, you will get back the full contents of your live site.


## Upload files and folder using drag and drop

You can drag some files from the Windows shell (or Mac finder), and drop them directly into the Kudu Console's File Explorer UI, much like you would between two Windows shell folders. You have to try it to believe it! :)


## Upload and expand zip file

If you try to drop a zip file into the File Explorer UI (per previews section), you'll see a special Zip file drop zone appearing. If you drop it there, it will copy the content of the expanded zip file, instead of the zip file itself. It's a much more efficient way to upload a large number of small files.

This demonstrates the UI:

![kudu_drop](https://cloud.githubusercontent.com/assets/6472374/8329527/b0a2734c-1a82-11e5-97aa-949fbde67a8f.gif)

## View and Edit text files

If you click the Edit icon next to a file, the Console will open up a window where you can view and edit the file. It's a super basic editor, so don't expect any syntax coloring or fancy code editor features. But it can still be helpful when investigating issues. e.g. you could use it to quickly tweak a web.config file.