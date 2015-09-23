### New Roslyn based compiler takes more memory

Recently, we updated the .NET build process to use the new VS 2015 toolset. The primary reason is to give access to new C# 6 compiler features like string interpolation.

One downside of this change that has been affecting some users is that the new compiler appears to use quite a bit more memory than the old one, at least in some scenarios. When running on VMs that are already tight on memory, that can cause it to go over the edge and start thrashing, leading to extremely slow performance.

Luckily, there is a simple workaround to go back to the previous compiler. Add the following App Settings in the Azure portal:

    MSBUILD_PATH=D:\Windows\Microsoft.NET\Framework\v4.0.30319\MSBuild.exe

We're still investigating the issue, but in the meantime, this should completely eliminate it. The obvious drawback is that you won't be able to use new compiler features. If that is a deal killer for you, the alternative workaround is to avoid running out of memory in the VM, using one or more of the following techniques:

- Scale up to a larger VM size.
- Reduce the number of sites you run in your App Service Plan (remember that slots are distinct sites).
- Avoid workflows that use a lot of memory. e.g. in some user's case, there was a WebJob that was at time taking 2GB of memory, which combine with the compiler memory usage to trigger the issue.
   

### You may get occasional "Couldn't reserve space for cygwin's heap" errors when git pushing

This is tracked by https://github.com/projectkudu/kudu/issues/878.

The full error looks like this:

```
remote:       0 [main] us 0 init_cheap: VirtualAlloc pointer is null, Win32 error 487
remote: AllocationBase 0x0, BaseAddress 0x68570000, RegionSize 0x4A0000, State 0x10000
remote: D:\Program Files (x86)\Git\bin\sh.exe: *** Couldn't reserve space for cygwin's heap, Win32 error 0
To https://xxx.scm.azurewebsites.net:443/xxx.git
 * [new branch]      master -> master
```

It seems to be a general msysgit issue that people run into occasionally (not related to Azure Web Apps). e.g. see various [references](https://www.google.com/search?q=VirtualAlloc+pointer+is+null&rlz=1C1CHFX_enUS498US498&oq=VirtualAlloc+pointer+is+null&aqs=chrome..69i57&sourceid=chrome&espv=210&es_sm=93&ie=UTF-8) to it. From our experience, this happens at most once on a given server, so I suspect you won't hit this continually (though you may get moved to a different server later).

The workaround is to make a dummy commit (e.g. using `--allow-empty` so you don't have to change any files) and push again. If you want to keep your repo clean, you can then reset that commit and force push.

Our hope is that this issue is fixed in the newest msysgit (version 2.5.x). Right now, Kudu is still using an older build (1.8.4). Unless we find some issues with using the newer build, we're planning to move to it before the end of 2015, and hopefully this will take care of this.

### You get a 404 during 'git push'

That can happen when you have a large site, and you have the git http.postBuffer setting set to a high value.

The workaround is to unset it, using:

    git config --global --unset http.postBuffer
