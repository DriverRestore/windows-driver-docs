---
title: Debugging TAEF Tests
description: Debugging TAEF Tests
ms.assetid: 0239547F-EF29-45e0-BACF-ED0F6C07DB99
---

# Debugging TAEF Tests


## <span id="Debugger_configuration"></span><span id="debugger_configuration"></span><span id="DEBUGGER_CONFIGURATION"></span>Debugger configuration


By default, test cases are executed in a separate process than TE.exe: TE.ProcessHost.exe.

## <span id="debugging_child_processes_of_te.exe__windbg_cdb_only_"></span><span id="DEBUGGING_CHILD_PROCESSES_OF_TE.EXE__WINDBG_CDB_ONLY_"></span>Debugging Child Processes of TE.exe (windbg/cdb only)


If you use a debugger such as cdb or windbg, you can simply pass the '-o' switch to the debugger. This will configure the debugger to automatically debug child processes, all withinin the same debugger instance.

For example:

``` syntax
windbg -o te.exe MyTests.dll
```

Then to switch to the process where your tests run, use the | (pipe) command. The pipe command for switching processes is used exaclty as the ~ (tilda) command for switching threads.

For example:

``` syntax
|1s - sets the current process to the second loaded process.
```

## <span id="Running_Tests__InProc___Visual_Studio_windbg_cdb_"></span><span id="running_tests__inproc___visual_studio_windbg_cdb_"></span><span id="RUNNING_TESTS__INPROC___VISUAL_STUDIO_WINDBG_CDB_"></span>Running Tests "InProc" (Visual Studio/windbg/cdb)


If you prefer to use Visual Studio to do your debugging, the method above will not work for you. In this case, just configure your debugger to run TE.exe, set the appropriate breakpoints in for test case, and pass the /inproc switch to TE.exe. This will ensure that all tests are run within the TE.exe process rather than spawning a new process.

For example:

``` syntax
start devenv /debugexe te.exe MyTests.dll /inproc
```

The command above will launch Visual Studio. Next open the source code for your test cases and set your appropriate breakpoints. Finally, hit F5 to start your test case and it should break on your first breakpoint (if your symbols have loaded correctly).

The steps described above work only with correct symbols set in Visual Studio. At least, you need to set the symbols to the test dll that you are debugging. To set symbols in Visual Studio:

-   Select Tools Menu
-   Select Options...
-   Select Debugging on the left tree-looking menu
-   Select Symbols under Debugging
-   Enter the symbols path under Symbol file (\*.pdb) location: section
-   Save your settings

## <span id="Automatically_Breaking_into_the_Debugger___breakOnCreate__and__breakOnInvoke__"></span><span id="automatically_breaking_into_the_debugger___breakoncreate__and__breakoninvoke__"></span><span id="AUTOMATICALLY_BREAKING_INTO_THE_DEBUGGER___BREAKONCREATE__AND__BREAKONINVOKE__"></span>Automatically Breaking into the Debugger ("breakOnCreate" and "breakOnInvoke")


In order to ease the debugging process, Taef provides the ability to automatically break into the debugger before each test class is instantiated and/or before each test method is invoked.

For example:

``` syntax
cdb -gG te.exe MyTests.dll /inproc /breakOnCreate /breakOnInvoke
```

The command above will launch Te.exe under cdb. Taef will break into the debugger right before each test class is instantiated, and before each test method is invoked.

**Note:** It is recommended that you use this feature while running Te.exe under a debugger, and also specifing the /inProc option.

 

 

[Send comments about this topic to Microsoft](mailto:wsddocfb@microsoft.com?subject=Documentation%20feedback%20[taef\taef]:%20Debugging%20TAEF%20Tests%20%20RELEASE:%20%289/12/2016%29&body=%0A%0APRIVACY%20STATEMENT%0A%0AWe%20use%20your%20feedback%20to%20improve%20the%20documentation.%20We%20don't%20use%20your%20email%20address%20for%20any%20other%20purpose,%20and%20we'll%20remove%20your%20email%20address%20from%20our%20system%20after%20the%20issue%20that%20you're%20reporting%20is%20fixed.%20While%20we're%20working%20to%20fix%20this%20issue,%20we%20might%20send%20you%20an%20email%20message%20to%20ask%20for%20more%20info.%20Later,%20we%20might%20also%20send%20you%20an%20email%20message%20to%20let%20you%20know%20that%20we've%20addressed%20your%20feedback.%0A%0AFor%20more%20info%20about%20Microsoft's%20privacy%20policy,%20see%20http://privacy.microsoft.com/default.aspx. "Send comments about this topic to Microsoft")




