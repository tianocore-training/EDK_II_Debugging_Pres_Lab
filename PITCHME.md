---?image=assets/images/gitpitch-audience.jpg
@title[EDK II Debugging]
<br><br><br><br><br>
## <span class="gold"   >UEFI & EDK II Training</span>

#### EDK II Debugging w/ Linux Lab

<br>
<span style="font-size:0.75em" ><a href='http://www.tianocore.org'>tianocore.org</a></span>
Note:
  PITCHME.md for UEFI / EDK II Training  EDK II Debugging Pres-lab

  Copyright (c) 2018, Intel Corporation. All rights reserved.<BR>

  Redistribution and use in source (original document form) and 'compiled'
  forms (converted to PDF, epub, HTML and other formats) with or without
  modification, are permitted provided that the following conditions are met:

  1) Redistributions of source code (original document form) must retain the
     above copyright notice, this list of conditions and the following
     disclaimer as the first lines of this file unmodified.

  2) Redistributions in compiled form (transformed to other DTDs, converted to
     PDF, epub, HTML and other formats) must reproduce the above copyright
     notice, this list of conditions and the following disclaimer in the
     documentation and/or other materials provided with the distribution.

  THIS DOCUMENTATION IS PROVIDED BY TIANOCORE PROJECT "AS IS" AND ANY EXPRESS OR
  IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF
  MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO
  EVENT SHALL TIANOCORE PROJECT  BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL,
  SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO,
  PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR PROFITS;
  OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY,
  WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR
  OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS DOCUMENTATION, EVEN IF
  ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.



---  
@title[Lesson Objective]
<BR>
### <p align="center"<span class="gold"   >Lesson Objective </span></p><br>

<!---  Add bullets using https://fontawesome.com/cheatsheet certificate
-->
<ul style="list-style-type:none">
 <li>@fa[certificate gp-bullet-green]<span style="font-size:0.9em">&nbsp;&nbsp;Define `DebugLib` and its attributes</span> </li>
 <li>@fa[certificate gp-bullet-cyan]<span style="font-size:0.9em">&nbsp;&nbsp;List the ways to debug</span></li>
 <li>@fa[certificate gp-bullet-gold]<span style="font-size:0.9em">&nbsp;&nbsp;Using PCDs to Configure `DebugLib` -LAB </span> </li>
 <li>@fa[certificate gp-bullet-ltgreen]<span style="font-size:0.9em">&nbsp;&nbsp;Change Compiler & Linker Flags for debugging</span></li>
 <li>@fa[certificate gp-bullet-yellow]<span style="font-size:0.9em">&nbsp;&nbsp;Change the `DebugLib` instance to modify the debug<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; output - LAB</span> </li>
 <li>@fa[certificate gp-bullet-magenta]<span style="font-size:0.9em">&nbsp;&nbsp;Debug EDK II using GDB - LAB</span> </li>
</ul>

---?image=assets/images/binary-strings-black2.jpg
@title[Debugging Overview]
<br><br><br><br><br><br><br>
### <span class="gold"  >&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Debugging Overview </span>
<span style="font-size:0.9em" >&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span>

Note:


---?image=/assets/images/slides/Slide4.JPG
@title[Debug Methods]
### <p align="center"><span class="gold" ><b>Debug Methods</b></span></p>
<br>

<div class="left1">
@ul[no-bullet]
- <span style="font-size:0.9em" ><font color="white">`DEBUG` and `ASSERT` macros in EDK II code </font></span><br><br>
- <span style="font-size:0.9em" ><font color="cyan">`DEBUG` instead of `Print` functions </font></span><br><br>
- <span style="font-size:0.9em" ><font color="white">Software/hardware debuggers </font></span><br><br>
- <span style="font-size:0.9em" ><font color="cyan">Shell commands to test capabilities for simple debugging </font></span>
@ulend
</div>
<div class="right1">
<span style="font-size:0.8em" >&nbsp;  </span>
</div>

Note:
- Ways to Debug . . .
- Use DEBUG instead of Print functions in code
- Use a software debugger (COM/USB)
- Use a hardware debugger (JTAG/XDB)
- Soft loading driver through UEFI shell
- Use shell commands to test capabilities

- What are some alternatives if I don't want to use the debug lib? 

- You can use print statements and soft load my driver to the shell to see what is going on. The downside is that you then have print statements in your code.  This doesn't work really well if you want to make a release to a customer.
- You could use a software debugger, and there is a hardware debugger but if they are not available EDK II debug macro might be a good place to start.

- We believe the debug lib is the simplest and cleanest way to get it all working

---
@title[EDK II DebugLib Library]
<p align="center"><span style="font-size:01.1em" ><font color="#e49436" ><b>EDK II `DebugLib` Library</b></font></span></p>
@snap[north-west span-60 fragment]
<br>
<br>
<br>
@box[bg-gold2 text-white rounded my-box-pad2  ](<p style="line-height:70%" ><span style="font-size:0.9em; font-weight: bold;" >Debug and Assert macros in code<br>&nbsp;</span></p>)
<br>
@snapend


@snap[north-east span-80 fragment]
<br>
<br>
<br>
<p style="line-height:50%" ><br><br><br><br>&nbsp;</p>
@box[bg-green-pp text-white rounded my-box-pad2  ](<p style="line-height:70%" ><span style="font-size:0.9em; font-weight: bold;" >Enable/disable when compiled &lpar;"target.txt"&rpar;<br>&nbsp;</span></p>)
<br>
@snapend


@snap[north-west span-80 fragment]
<br>
<br>
<br>
<br>
<br>
<br>
<p style="line-height:50%" ><br><br><br><br><br>&nbsp;</p>
@box[bg-purple-pp text-white rounded my-box-pad2  ](<p style="line-height:70%" ><span style="font-size:0.9em; font-weight: bold;" > Connects a Host to capture debug messages <br>&nbsp;</span></p>)
<br>
@snapend


Note:
- DebugLib library is clean & very portable
- Using DEBUG and ASSERT macros in code
- Enable/disable when compiled (target.txt)
- Can connect a 2nd PC to capture debug messages 

- The main message-- the debug lib library is portable, it's clean, it's very easy to use, and we believe it's the easiest way to do debugging on a UEFI platform.

- The debug lib library has the debug and assert macros. 
- There are library instances that allow you to use a second PC to capture all messages coming out



---?image=assets/images/binary-strings-black2.jpg
@title[Debugging with PCDs]
<br><br><br><br><br><br><br>
### <span class="gold"  >&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Debugging with PCDs </span>
<span style="font-size:0.9em" >&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span>

Note:


---
@title[Using PCDs to Configure DebugLib]
<p align="right"><span class="gold" ><b>Using PCDs to Configure `DebugLib`</b></span></p>

@snap[west span-100 ]
<br>
@box[bg-grey-05 text-white my-box-pad2  ](<p style="line-height:40%" align="left"><span style="font-size:0.450em; font-family:Consolas; " ><br><br><br><br><br><br>&nbsp;&nbsp;</span></p>)
@snapend

@snap[north-west span-100 ]
<br>
<br>
<br>
<br>
<p style="line-height:70%" ><span style="font-size:01.0em; font-weight: bold;" ><font color="#87E2A9">MdePkg Debug Library Class </font><br></span></p>
<br>
<p style="line-height:60%" align="left"><span style="font-size:0.60em; font-family:Consolas; " >
&nbsp;&nbsp;
[PcdsFixedAtBuild. PcdsPatchableInModule]<br>
&nbsp;&nbsp;&nbsp;&nbsp;  gEfiMdePkgTokenSpaceGuid.@color[red](PcdDebugPropertyMask)|0x1f<br>
&nbsp;&nbsp;&nbsp;&nbsp;  gEfiMdePkgTokenSpaceGuid.@color[red](PcdDebugPrintErrorLevel)|0x80000040<br>
</span></p>

@snapend



@snap[south span-90 fragment]
@box[bg-purple-pp text-white rounded my-box-pad2  ](<p style="line-height:70%" ><span style="font-size:0.9em; font-weight: bold;" > PCDs set which drivers report errors and change what messages get printed<br>&nbsp;</span></p>)
<br>
@snapend


Note:

- MdePkg Debug Library Class
   - PcdDebugPropertyMask  
- Bit mask to determine which features are on/off
   - PcdDebugPrintErrorLevel 
    - Types of messages produced
	
- PCDs set which drivers report errors and change what messages get printed
- Example from Nt32Pkg.dsc:
  - [PcdsFixedAtBuild.IA32]
    - EfiMdePkgTokenSpaceGuid.PcdDebugPropertyMask|0x1f
    - gEfiMdePkgTokenSpaceGuid.PcdDebugPrintErrorLevel|0x80000040

- How do you configure the debug lib?

- It is configured through PCD entries and some platform configuration database entries.
- This is part of the MDE package, where is the debug library class is defined.
 
- Remember, the library instance can be anywhere, but the library class is defined in the MDE package. This is where the PCDs are defined. 

- It is required for the debug Lib instance to use these PCDs for control.

- PcdDebugPropertyMask and PcdDebugPrintErrorLevel can change sets of driver report errors, and they can also change the error messages that print out.
- Per the example at the bottom of the slide, the assignment for PcdDebugPropertyMask is 0x1f and PcdDebugPrintErrorLevel is 0x80000040 . These are the values for these two PCDs. 


---
@title[PcdDebugPropertyMask Values]
<p align="right"><span class="gold" ><b>@color[white](`PcdDebugPropertyMask`) Values</b></span></p>


@snap[north-west span-100 ]
<br>
<br>
<p style="line-height:30%" align="left"><br>&nbsp;</p>
@box[bg-grey-05 text-white my-box-pad2  ](<p style="line-height:40%" align="left"><span style="font-size:0.450em; font-family:Consolas; " ><br><b><b><b><br><br><br><br><br><br>&nbsp;&nbsp;</span></p>)
@snapend

@snap[north-west span-100 ]
<br>
<br>
<p style="line-height:70%" ><span style="font-size:01.0em;" ><font color="#87E2A9"><b>Debugging Features Enabled </b></font><br></span></p>
<p style="line-height:60%" align="left"><span style="font-size:0.550em; font-family:Consolas; " >
&nbsp;&nbsp;
&nbsp;&nbsp;&num;define DEBUG_PROPERTY_DEBUG_ASSERT_ENABLED &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;            0x01<br>&nbsp;&nbsp;
&nbsp;&nbsp;&num;define DEBUG_PROPERTY_DEBUG_PRINT_ENABLED &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; &nbsp;       0x02<br>&nbsp;&nbsp;
&nbsp;&nbsp;&num;define DEBUG_PROPERTY_DEBUG_CODE_ENABLED &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;       0x04<br>&nbsp;&nbsp;
&nbsp;&nbsp;&num;define DEBUG_PROPERTY_CLEAR_MEMORY_ENABLED &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;            0x08<br>&nbsp;&nbsp;
&nbsp;&nbsp;&num;define DEBUG_PROPERTY_ASSERT_BREAKPOINT_ENABLED &nbsp;                   0x10<br>&nbsp;&nbsp;
&nbsp;&nbsp;&num;define DEBUG_PROPERTY_ASSERT_DEADLOOP_ENABLED &nbsp;&nbsp;&nbsp;              0x20
</span></p>

<p style="line-height:60%" align="left"><span style="font-size:0.50em;"> <font color="yellow">Default value in `OvmfPkg` is `0x2f`<br>Default value in `Nt32Pkg` is `0x1f`</font></span></p>
@snapend




@snap[south span-90 fragment]

@box[bg-purple-pp text-white rounded my-box-pad2  ](<p style="line-height:70%" ><span style="font-size:0.9em; font-weight: bold;" > Determines which debugging features are enabled.<br>&nbsp;</span></p>)
<br>
@snapend




Note:
- Enables debugging features
- What kinds of outputs are produced?
- What kind of debugging is being done?

<pre>
 #define DEBUG_PROPERTY_DEBUG_ASSERT_ENABLED       0x01
 #define DEBUG_PROPERTY_DEBUG_PRINT_ENABLED        0x02
 #define DEBUG_PROPERTY_DEBUG_CODE_ENABLED         0x04
 #define DEBUG_PROPERTY_CLEAR_MEMORY_ENABLED       0x08
 #define DEBUG_PROPERTY_ASSERT_BREAKPOINT_ENABLED  0x10
 #define DEBUG_PROPERTY_ASSERT_DEADLOOP_ENABLED    0x20
</pre>
- another Note: Default value in Nt32 is 0x1f

- Determines which debugging features are enabled.

- What kinds of output are produced.
- What kind of debugging is being done.

- Default in Ntr32 is 0x1f

- So what does that mean?
 
- for DebugPropertyMask, 1F turns on:
 	- debug assert, 
	- debug print, 
	- debug code enabled, 
	- clear memory, 
	- assert breakpoint
- It does not turn on assert dead loop.

- This turns on the debug features for the property mask one at a time. The property mask tells the debug command what we really want to have happen.



---
@title[PcdDebugPrintErrorLevel Values]
<p align="right"><span class="gold" ><b>@color[white](`PcdDebugPrintErrorLevel`) Values</b></span></p>
<p style="line-height:70%" ><span style="font-size:01.0em;" ><font color="#87E2A9"><b>Debugging Messages Displayed</b></font><br></span></p>

```
 #define DEBUG_INIT      0x00000001  // Initialization
 #define DEBUG_WARN      0x00000002  // Warnings
 #define DEBUG_LOAD      0x00000004  // Load events
 #define DEBUG_FS        0x00000008  // EFI File system
 #define DEBUG_POOL      0x00000010  // Alloc & Free's  Pool
 #define DEBUG_PAGE      0x00000020  // Alloc & Free's  Page
 #define DEBUG_INFO      0x00000040  // Verbose
 #define DEBUG_DISPATCH  0x00000080  // PEI/DXE Dispatchers
 #define DEBUG_VARIABLE  0x00000100  // Variable
 #define DEBUG_BM        0x00000400  // Boot Manager
 #define DEBUG_BLKIO     0x00001000  // BlkIo Driver
 #define DEBUG_NET       0x00004000  // SNI Driver
 #define DEBUG_UNDI      0x00010000  // UNDI Driver
 #define DEBUG_LOADFILE  0x00020000  // Load File 
 #define DEBUG_EVENT     0x00080000  // Event messages
 #define DEBUG_ERROR     0x80000000  // Error

    Aliases EFI_D_INIT == DEBUG_INIT, etc...
 
```

<p style="line-height:60%" align="left"><span style="font-size:0.50em;"> <font color="yellow">
Default value in `OvmfPkg` is `0x8000004f`  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Default value in `Nt32Pkg` is `0x80000040`</font></span></p>




@snap[south span-90 fragment]

@box[bg-purple-pp text-white rounded my-box-pad2  ](<p style="line-height:70%" ><span style="font-size:0.9em; font-weight: bold;" > Determines which messages we want to print<br>&nbsp;</span></p>)

@snapend







Note:
- Determines if each print message is displayed
- Use Binary-AND setting to set parameter TRUE
- Note that Aliases EFI_D_INIT == DEBUG_INIT, etc..
- DebugPrintErrorLevel Values

- This has to do with what messages we want to come out

- Let's say you have a debug print enabled as in the previous slide. 
   - You must state what message type you want to print out

- You can assign your own values to this debug print error level.  
- However, these are the guideline values that these drivers use in terms of what their debug output messaging will be.

---
@title[Changing PCD Values ]
<p align="right"><span class="gold" ><b>Changing PCD Values</b></span></p>
@snap[north-west span-100 fragment]
<br>
<br>
<p align="left" style="line-height:80%"><span style="font-size:0.9em; ">@color[yellow](Change all instances of a PCD in platform DSC)</span></p>
@box[bg-black text-white rounded my-box-pad2  ](<p style="line-height:40%" align="left"><span style="font-size:0.450em; font-family:Consolas; " >&nbsp;&nbsp;[PcdFixedAtBuild]<br>&nbsp;&nbsp;gEfiMdePkgTokenSpaceGuid.PcdDebugPrintErrorLevel|0x00000000<br>&nbsp;&nbsp;</span></p>)
<br>
@snapend

@snap[north-west span-90 rounded fragment]
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<p align="left" style="line-height:80%"><span style="font-size:0.9em; ">@color[yellow](Change a single module's PCD values in the DSC)</span></p>
@box[bg-black text-white rounded my-box-pad2  ](<p style="line-height:40%" align="left"><span style="font-size:0.450em; font-family:Consolas; " >&nbsp;&nbsp;MyPath/MyModule.inf&nbsp;{<br>&nbsp;&nbsp;&lt;PcdFixedAtBuild&gt;<br>&nbsp;&nbsp;gEfiMdePkgTokenSpaceGuid.PcdDebugPrintErrorLevel|0x00000000<br>&nbsp;&nbsp;}<br>&nbsp;&nbsp;</span></p>)
<br>
@snapend



@snap[south span-100 fragment]

@box[bg-purple-pp text-white rounded my-box-pad2  ](<p style="line-height:70%" ><span style="font-size:0.9em; font-weight: bold;" > Minimize message output and minimize size increase<br>&nbsp;</span></p>)

@snapend

Note:
- Use different PCD values only on the module being debugged
- Minimize message output and minimize size increase



---
@title[Other Debug Related Libraries  ]
<p align="right"><span class="gold" ><b>Other Debug Related Libraries </b></span></p>

@snap[north-west span-90 fragment]
<br>
<br>
@box[bg-green-pp text-white rounded my-box-pad2  ](<p style="line-height:70%" ><span style="font-size:0.9em; font-weight: bold;" >`ReportStatusCodeLib` - Progress codes<br>&nbsp;</span></p>)
<pre>
```
  gEfiMdePkgTokenSpaceGuid.PcdReportStatusCodePropertyMask
```
</pre>
<br>
@snapend


@snap[north-west span-90 fragment]
<br>
<br>
<br>
<p style="line-height:50%" ><br><br><br><br>&nbsp;</p>
@box[bg-royal text-white rounded my-box-pad2  ](<p style="line-height:70%" ><span style="font-size:0.9em; font-weight: bold;" >`PostCodeLib` - Enable Post codes<br>&nbsp;</span></p>)
<pre>
```
  gEfiMdePkgTokenSpaceGuid.PcdPostCodePropertyMask

```
</pre>
<br>
@snapend


@snap[north-west span-90 fragment]
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<p style="line-height:50%" ><br><br><br><br><br>&nbsp;</p>
@box[bg-gold2 text-white rounded my-box-pad2  ](<p style="line-height:70%" ><span style="font-size:0.9em; font-weight: bold;" >`PerformanceLib` - Enable Measurement <br>&nbsp;</span></p>)
<pre>
```
  gEfiMdePkgTokenSpaceGuid.PcdPerformanceLibraryPropertyMask

```
</pre>
<br>
@snapend




Note:

- ReportStatusCodeLib - Progress codes
	- gEfiMdePkgTokenSpaceGuid.PcdReportStatusCodePropertyMask
- PostCodeLib -  Enable Post codes
	- gEfiMdePkgTokenSpaceGuid.PcdPostCodePropertyMask
- PerformanceLib -  Enable Measurement
	- gEfiMdePkgTokenSpaceGuid.PcdPerformanceLibraryPropertyMask


---?image=/assets/images/slides/Slide_LabSec.JPG
@title[Lab 1: Adding Debug Statements]
<br>
<br>
<p align="Left"><span class="gold" ><b>Lab 1: Adding Debug Statements</b></span></p>
<br>
<div class="left1">
<span style="font-size:0.8em" >In this lab, you'll add debug statements to the previous lab's SampleApp UEFI Shell application</span>
</div>
<div class="right1">
<span style="font-size:0.8em" >&nbsp;  </span>
</div>

Note:
In this lab, you'll learn how to add debug statements.  
This lab uses code from a previous exercise as a starting point (refer to  Writing Simple UEFI Applications).  Before proceeding, verify that the SampleApp code is present in your workspace and that the code references the OvmfPkgX64.dsc file.

---
@title[Lab 1: Catch Up SampleApp]
<p align="right"><span class="gold" ><b>Lab 1: Catch up from previous lab</b></span></p>
<span style="font-size:0.8em" >Skip if Lab <a href="https://gitpitch.com/tianocore-training/Writing_UEFI_App_Lab/master#/">Writing UEFI App Lab</a> completed</span>
<ul>
   <li><span style="font-size:0.8em" >Perform <a href="https://gitpitch.com/tianocore-training/Platform_Build_LAB/master#/">Lab Setup</a> from previous Labs  </span></li>
   <li><span style="font-size:0.8em" >Create a Directory under the workspace `~/src/edk2` : "`SampleApp`"</span></li>
   <li><span style="font-size:0.8em" >Copy contents of `~/FW/LabSampleCode/SampleAppDebug` to `~/src/edk2/SampleApp`</span></li>
   <li><span style="font-size:0.8em" >Open `~src/edk2/OvmfPkg/OvmfPkgX64.dsc`</span></li>
   <li><span style="font-size:0.8em" >Add the following to the `[Components]` section: </span></li>
<pre lang="php">
```
    # Add new modules here
    SampleApp/SampleApp.inf
```
</pre>
   <li><span style="font-size:0.8em" >Save and close the file `~src/edk2/OvmfPkg/OvmfPkgX64.dsc`  </span></li>
</ul>

Note:

---
@title[Lab 1: Add debug statements SampleApp]
<p align="right"><span class="gold" ><b>Lab 1: Add debug statements to SampleApp</b></span></p>
<br>
<ul>
  <li><span style="font-size:0.8em" >Open a Terminal Command Prompt (Cnt-Alt-T) and type cd ~/src/edk2<br></span>&nbsp;&nbsp;&nbsp;<span style="font-size:0.6em" ><span style="background-color: #101010">&nbsp;` bash$  .  edksetup.sh `&nbsp;</span> </span></li><br>
  <li><span style="font-size:0.8em" >Open `~/src/edk2/SampleApp/SampleApp.c` </span></li><br>
  <li><span style="font-size:0.8em" >Add the following to the include statements at the top of the file after below the last "`#include`" statement: </span></li>
<pre>
```
 #include <Library/DebugLib.h>
```
</pre>
</ul>

Note:

---
@title[Lab 1: Add debug statements SampleApp 02]
<p align="right"><span class="gold" ><b>Lab 1: Add debug statements to SampleApp</b></span></p>
<p style="line-height:90%"><span style="font-size:0.7em" >Locate the `UefiMain` function. Then copy and paste the following code after the <span style="background-color: #101010">&nbsp;"`EFI_INPUT_KEY  KEY;`"</span> statement: and before the first <span style="background-color: #101010">&nbsp;`Print()` </span>statement </span></p>
```c
DEBUG ((0xffffffff, "\n\nUEFI Base Training DEBUG DEMO\n") );
DEBUG ((0xffffffff, "0xffffffff USING DEBUG ALL Mask Bits Set\n") );

DEBUG ((EFI_D_INIT,     " 0x%08x USING DEBUG EFI_D_INIT\n" , (UINTN)(EFI_D_INIT))  );
DEBUG ((EFI_D_WARN,     " 0x%08x USING DEBUG EFI_D_WARN\n", (UINTN)(EFI_D_WARN))  );
DEBUG ((EFI_D_LOAD,     " 0x%08x USING DEBUG EFI_D_LOAD\n", (UINTN)(EFI_D_LOAD))  );
DEBUG ((EFI_D_FS,       " 0x%08x USING DEBUG EFI_D_FS\n", (UINTN)(EFI_D_FS))  );
DEBUG ((EFI_D_POOL,     " 0x%08x USING DEBUG EFI_D_POOL\n", (UINTN)(EFI_D_POOL))  );
DEBUG ((EFI_D_PAGE,     " 0x%08x USING DEBUG EFI_D_PAGE\n", (UINTN)(EFI_D_PAGE))  );
DEBUG ((EFI_D_INFO,     " 0x%08x USING DEBUG EFI_D_INFO\n", (UINTN)(EFI_D_INFO))  );
DEBUG ((EFI_D_DISPATCH, " 0x%08x USING DEBUG EFI_D_DISPATCH\n",(UINTN)(EFI_D_DISPATCH)));
DEBUG ((EFI_D_VARIABLE, " 0x%08x USING DEBUG EFI_D_VARIABLE\n",(UINTN)(EFI_D_VARIABLE)));
DEBUG ((EFI_D_BM,       " 0x%08x USING DEBUG EFI_D_BM\n", (UINTN)(EFI_D_BM))  );
DEBUG ((EFI_D_BLKIO,    " 0x%08x USING DEBUG EFI_D_BLKIO\n", (UINTN)(EFI_D_BLKIO))  );
DEBUG ((EFI_D_NET,      " 0x%08x USING DEBUG EFI_D_NET\n", (UINTN)(EFI_D_NET))  );
DEBUG ((EFI_D_UNDI,     " 0x%08x USING DEBUG EFI_D_UNDI\n", (UINTN)(EFI_D_UNDI))  );
DEBUG ((EFI_D_LOADFILE, " 0x%08x USING DEBUG EFI_D_LOADFILE\n",(UINTN)(EFI_D_LOADFILE)));
DEBUG ((EFI_D_EVENT,    " 0x%08x USING DEBUG EFI_D_EVENT\n", (UINTN)(EFI_D_EVENT))  );
DEBUG ((EFI_D_ERROR,    " 0x%08x USING DEBUG EFI_D_ERROR\n", (UINTN)(EFI_D_ERROR))  );

```

Note:


---?image=/assets/images/slides/Slide26.JPG
@title[Lab 1: Update the Qemu Script]
<p align="right"><span class="gold" ><b>Lab 1: Update the Qemu Script</b></span></p>

Note:

<pre>

 qemu-system-x86_64 -pflash bios.bin -hda fat:rw:hda-contents -net none     -debugcon file:debug.log -global isa-debugcon.iobase=0x402  -serial file:serial.log
</pre>



---
@title[Lab 1 Build and Test Application]
<p align="right"><span class="gold" ><b>Lab 1: Build and Test Application</b></span></p>
<br>
<span style="font-size:0.8em" >Build SampleApp - Cd to ~/src/edk2 dir </span>
```shell
   bash$ build
```
<span style="font-size:0.8em" >Copy  the OVMF.fd to the run-ovmf directory naming it bios.bin</span>
```shell
 bash$ cd ~/run-ovmf
 bash$ cp cp ~/src/edk2/Build/OvmfX64/DEBUG_GCC5/FV/OVMF.fd bios.bin
```

<span style="font-size:0.8em" >Copy  SampleApp.efi to hda-contents</span>
```shell
 bash$ cd ~/run-ovmf/hda-contents
 bash$ cp ~/src/edk2/Build/OvmfX64/DEBUG_GCC5/X64/SampleApp.efi .
```

Note:


---?image=/assets/images/slides/Slide28.JPG
@title[Lab 1: Run Qemu Script]
<p align="right"><span class="gold" ><b>Lab 1: Run the Qemu Script</b></span></p>
<br>
<div class="left">
<span style="font-size:0.7em" >Test by Invoking Qemu</span>
<pre>
```
  bash$ cd ~/run-ovmf
  bash$ . RunQemu.sh
```
</pre>
<span style="font-size:0.7em" >Run the application from the shell</span><br>
<span style="font-size:0.5em" ><span style="background-color: #101010">&nbsp;<font color="yellow">`Shell> `&nbsp;</font>`SampleApp`&nbsp;</span></span><br>
<p style="line-height:90%"><span style="font-size:0.7em" >Check the contents of the debug.log file </span></p>
<pre>
```
  bash$ cat debug.log
```
</pre>
<span style="font-size:0.8em" ><font color="yellow">Exit QEMU</font></span>

</pre>
</div>
<div class="right">
<span style="font-size:0.8em" >&nbsp;  </span>
</div>

Note:

---?image=/assets/images/slides/Slide_LabSec.JPG
@title[Lab 2: Changing PCD Value]
<br>
<br>
<p align="Left"><span class="gold" ><b>Lab 2: Changing PCD Value</b></span></p>
<br>
<div class="left1">
<span style="font-size:0.8em" >In this lab, you'll  learn how to use PCD values to change debugging capabilities. </span>
</div>
<div class="right1">
<span style="font-size:0.8em" >&nbsp;  </span>
</div>

Note:
In this lab, you'll learn how to use PCD values to change debugging capabilities.  The previous lab,  Adding Debug Statements, did not display all the DEBUG messages added to SampleApp.c.  This lab shows how to change this behavior.




---
@title[Lab 2: Change PCDs for SampleApp]
<p align="right"><span class="gold" ><b>Lab 2: Change PCDs for SampleApp</b></span></p>
<br>
<span style="font-size:0.7em" >Open `~src/edk2/OvmfPkg/OvmfPkgX64.dsc` </span><br>
<span style="font-size:0.7em" >Replace `SampleApp/SampleApp.inf` with the following:</span><br>
```c
  SampleApp/SampleApp.inf {
    <PcdsFixedAtBuild>
      gEfiMdePkgTokenSpaceGuid.PcdDebugPropertyMask|0xff
      gEfiMdePkgTokenSpaceGuid.PcdDebugPrintErrorLevel|0xffffffff
 }
```
<span style="font-size:0.7em" >Save and close `~src/edk2/OvmfPkg/OvmfPkgX64.dsc` </span><br>
<span style="font-size:0.7em" >Build SampleApp : </span><span style="font-size:0.5em" ><span style="background-color: #000000">&nbsp;&nbsp;`bash$ build`&nbsp;&nbsp;</span></span><br>
<span style="font-size:0.7em" >Copy  SampleApp.efi to hda-contents</span>
```shell
 bash$ cd ~/run-ovmf/hda-contents
 bash$ cp ~/src/edk2/Build/OvmfX64/DEBUG_GCC5/X64/SampleApp.efi .
```

Note:


---?image=/assets/images/slides/Slide28.JPG
@title[Lab 2: Run Qemu Script]
<p align="right"><span class="gold" ><b>Lab 2: Run the Qemu Script</b></span></p>
<br>
<div class="left">
<span style="font-size:0.7em" >Test by Invoking Qemu</span>
<pre>
```
  bash$ cd ~/run-ovmf
  bash$ . RunQemu.sh
```
</pre>
<span style="font-size:0.7em" >Run the application from the shell</span><br>
<span style="font-size:0.5em" ><span style="background-color: #101010">&nbsp;<font color="yellow">`Shell> `&nbsp;</font>`SampleApp`&nbsp;</span></span><br>
<span style="font-size:0.7em" >Check the contents of the debug.log file </span>
<pre>
```
  bash$ cat debug.log
```
</pre>
<span style="font-size:0.8em" ><font color="yellow">Exit QEMU</font></span>

</pre>
</div>
<div class="right">
<span style="font-size:0.8em" >&nbsp;  </span>
</div>

Note:

---?image=assets/images/binary-strings-black2.jpg
@title[Changing Compiler & Linker Flags Section]
<br><br><br><br><br><br><br>
### <span class="gold"  >&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Changing Flags</span>
<span style="font-size:0.9em" >&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Changing Compiler & Linker Flags</span>

Note:


---
@title[Precedence for Debug Flags Hierarchy]
<p align="right"><span class="gold" ><b>Precedence for Debug Flags Hierarchy</b></span></p>

@snap[north span-85 fragment]
<br>
<br>
@box[bg-green-pp text-white waved   ](<p style="line-height:90%" ><span style="font-size:01.0125em; font-weight: bold;" >Tools_def.txt<br><br>&nbsp;</span></p>)
@snapend

@snap[north span-70 fragment]
<br>
<br>
<br>
<br>
@box[bg-navy text-white waved   ](<p style="line-height:90%" ><span style="font-size:01.0em; font-weight: bold;" >DSC `[BuildOptions]` section &lpar;platform scope&rpar;<br>&nbsp;</span></p>)
@snapend

@snap[north-west span-50 fragment]
<br>
<br>
<br>
<br>
<br>
<br>
@box[bg-purple-pp text-white waved  ](<p style="line-height:90%" ><span style="font-size:01.0em; font-weight: bold;" >INF `[BuildOptions]` section <br>&nbsp;</span></p>)
@snapend


@snap[north-east span-50 fragment]
<br>
<br>
<br>
<br>
<br>
<br>
@box[bg-lt-blue-pp text-white waved  ](<p style="line-height:90%" ><span style="font-size:01.0em; font-weight: bold;" >DSC  &lt;`BuildOptions`&gt; under a specific module <br>&nbsp;</span></p>)
@snapend


@snap[south span-70 fragment]
@box[bg-grey-05 text-white rounded my-box-pad2](<p style="line-height:60%" align="left" ><span style="font-size:0.7em; " >&nbsp;&nbsp; 1. Tools_def.txt<br>&nbsp;&nbsp; 2. DSC [BuildOptions] section  &lpar;platform scope&rpar;<br>&nbsp;&nbsp; 3. INF [BuildOptions] section &lpar;module scope&rpar;<br>&nbsp;&nbsp; 4. DSC &lt;BuildOptions&gt; under a specific module<br>&nbsp;&nbsp;</span></p>)
<br>
<br>
@snapend

Note:
- think of the rules for compiler switches and options as a pyramid
- Pyramid top overrides middle, middle overrides the bottom



- Tools_def.txt
  - Baseline set of command line options for compiler and linker
- INF [BuildOptions] section
  - Append onto existing command line with "="
  - Replace entire existing command line with "=="

- DSC [BuildOptions] section (platform scope)
  - Same usage

- DSC <BuildOptions> under a specific module
  - Same usage 


---
@title[Compiler / Linker Flags]
<p align="right"><span class="gold" ><b>Compiler / Linker Flags</b></span></p>

@snap[north-west span-100]
<br>
<br>
<p align="left" style="line-height:80%"><span style="font-size:0.9em; ">@color[#87E2A9](Example from Microsoft* compiler to turn off optimization)</span></p>
<p style="line-height:40%" align="left"><span style="font-size:0.650em; font-family:Consolas; " >&nbsp;&nbsp;"`/02` " to "`/01`" requires "`/0d /01`"</span></p>
<br>
@snapend

@snap[north-west span-90 rounded fragment]
<br>
<br>
<br>
<br>
<br>
<p align="left" style="line-height:80%"><span style="font-size:0.9em; ">@color[yellow](Change common flags in platform DSC)</span></p>
@box[bg-grey-05 text-white rounded my-box-pad2  ](<p style="line-height:40%" align="left"><span style="font-size:0.450em; font-family:Consolas; " >&nbsp;&nbsp;[BuildOptions]&nbsp;<br>&nbsp;&nbsp;&nbsp;&nbsp;DEBUG_*_IA32_CC_FLAGS = /Od /Oy-<br>&nbsp;&nbsp;</span></p>)
<br>
@snapend


@snap[north-west span-90 rounded fragment]
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<p align="left" style="line-height:80%"><span style="font-size:0.9em; ">@color[yellow](Change a single module's flags in the DSC)</span></p>
@box[bg-grey-05 text-white rounded my-box-pad2  ](<p style="line-height:40%" align="left"><span style="font-size:0.450em; font-family:Consolas; " >&nbsp;&nbsp;MyPath/MyModule.inf&nbsp;{<br>&nbsp;&nbsp;&lt;BuildOptions&gt;<br>&nbsp;&nbsp;&nbsp;&nbsp;DEBUG_*_IA32_CC_FLAGS = /Od /Oy-<br>&nbsp;&nbsp;}<br>&nbsp;&nbsp;</span></p>)
<br>
@snapend




Note:
- Change common flags in platform DSC
 - [BuildOptions]
   - DEBUG_*_IA32_CC_FLAGS = /Od

- Change a single module's flags in DSC
 - MyPath/MyModule.inf {
  - <BuildOptions>
     - DEBUG_*_IA32_CC_FLAGS = /Od 
 - }

- Change optimizations, etc. . .


---?image=assets/images/binary-strings-black2.jpg
@title[DebugLib Usage Section]
<br><br><br><br><br><br><br>
### <span class="gold"  >&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`DebugLib` Usage</span>
<span style="font-size:0.9em" >&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span>

Note:


---?image=/assets/images/slides/Slide25_2.JPG
@title[DebugLib Class]
<p align="center"><span class="gold" ><b>The `DebugLib` Class</b></span></p>

@snap[north-west span-85]
<p style="line-height:40%" align="left"><span style="font-size:0.650em; font-family:Consolas; " ><br><br>&nbsp;</span></p>
@box[bg-ubuntu text-white my-box-pad2  ](<p style="line-height:40%" align="left"><span style="font-size:0.70em; font-family:Consolas; " >&nbsp;&nbsp;MdePkg/Include/Library/DebugLib.h<br>&nbsp;&nbsp;</span></p>)
<br>
@snapend


@snap[north-west span-80 rounded fragment]
<br>
<br>
<br>
<br>
<p align="left" style="line-height:60%"><span style="font-size:0.9em; "><font color="#87E2A9"><b>Macros</b><br>@size[0.65em](&lpar;where PCDs ard checked&rpar;)</font></span></p>
@box[bg-grey-05 text-white rounded my-box-pad2  ](<p style="line-height:40%" align="left"><span style="font-size:0.450em; font-family:Consolas; " >&nbsp;&nbsp;ASSERT &lpar;Expression&rpar;<br>&nbsp;&nbsp;DEBUG &lpar;Expression&rpar;<br>&nbsp;&nbsp;ASSERT_EFI_ERROR &lpar;StatusParameter&rpar;<br>&nbsp;&nbsp;ASSERT_PROTOCOL_ALREADY_INSTALLED&lpar;. . .&rpar;<br>&nbsp;&nbsp;</span></p>)
<br>
@snapend



@snap[north-west span-80 rounded fragment]
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<p align="left" style="line-height:80%"><span style="font-size:0.9em; "><font color="#87E2A9"><b>Advanced Macros</b></font></span></p>
@box[bg-grey-05 text-white rounded my-box-pad2  ](<p style="line-height:40%" align="left"><span style="font-size:0.450em; font-family:Consolas; " >&nbsp;&nbsp;DEBUG_CODE &lpar;Expression&rpar;<br>&nbsp;&nbsp;DEBUG_CODE_BEGIN&lpar;&rpar; & DEBUG_CODE_END&lpar;&rpar;<br>&nbsp;&nbsp;DEBUG_CLEAR_MEMORY&lpar;...&rpar;<br>&nbsp;&nbsp;</span></p>)
<br>
@snapend



Note:

- This is the interface so it will be describing the title of the function and / or Macro not the implementation
- The interface will describe the parameters needed 

- MdePkg\Include\Library\DebugLib.h

- Do not call internal worker functions directly
- Macros are where the PCDs are checked
  - ASSERT (Expression)
  - DEBUG (Expression)
  - ASSERT_EFI_ERROR (StatusParameter)
  - ASSERT_PROTOCOL_ALREADY_INSTALLED(...)

  - Advanced Macros:
  - DEBUG_CODE (Expression)
  - DEBUG_CODE_BEGIN() & DEBUG_CODE_END()
  - DEBUG_CLEAR_MEMORY(...)



---?image=/assets/images/slides/Slide26_1.JPG
@title[DebugLib Instances (1)]
<br>
<p align="left"><span class="gold" ><b>`DebugLib` Instances (1)</b></span></p>

@snap[north-west span-75]
<p style="line-height:40%" align="left"><span style="font-size:0.650em; font-family:Consolas; " ><br><br><br><br><br>&nbsp;</span></p>
@box[bg-ubuntu text-white my-box-pad2  ](<p style="line-height:40%" align="left"><span style="font-size:0.70em; font-family:Consolas; " >&nbsp;&nbsp;BaseDebugLibSerialPort<br>&nbsp;&nbsp;</span></p>)
<br>
@snapend

@snap[north-west span-100]
<br>
<br>
<br>
<br>
<p style="line-height:40%" align="left"><span style="font-size:0.650em; font-family:Consolas; " ><br><br>&nbsp;</span></p>
@box[bg-blue-pp text-white my-box-pad2  ](<p style="line-height:40%" align="left"><span style="font-size:0.70em; font-family:Consolas; " >&nbsp;&nbsp;<br><br><br><br><br><br><br><br><br><br><br><br>&nbsp;&nbsp;</span></p>)
<br>
@snapend

@snap[north-west span-90]
<br>
<br>
<br>
<br>
<p style="line-height:40%" align="left"><span style="font-size:0.650em; font-family:Consolas; " ><br><br><br>&nbsp;</span></p>
<ul>
  <li><span style="font-size:0.8em">Instance of `DebugLib`   </span></li>
  <li><span style="font-size:0.8em">Uses `SerialPortLib` class to send debug output to serial port</span></li>
  <li><span style="font-size:0.8em">Default for many platforms:  `BaseDebugLibNull`  </span></li>
  <li><span style="font-size:0.8em">OVMF uses it with Switch `DEBUG_ON_SERIAL_PORT`  </span></li>
</ul>
@snapend

@snap[south-east span-20]
![implementation](/assets/images/Implementation.png)
@snapend


@snap[south-east span-20]
<p style="line-height:40%" align="left">@color[black](&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1)&nbsp;&nbsp;&nbsp;<br><br>&nbsp;</p>
<br>
@snapend

Note:
- debugLib library instances

- The first debug Lib library instance is the BaseDebugLibSerialPort 
 this is a debug Lib that is good for PEI and DXE.
 It uses the serial Port Lib class and sends all the debug information out the serial port.
 
- Every time that you type DEBUG, it prints the information to the serial port such that if there is another PC capturing that information out the serial port, it allows easy viewing of the debug information. 
- This is good because it works early on in the platform. You can run very early and get a lot of debug information. 
- At this point the only serial port library instance that is in the public domain or open source is the DUET version 



---?image=/assets/images/slides/Slide26_1.JPG
@title[DebugLib Instances (2)]
<br>
<p align="left"><span class="gold" ><b>`DebugLib` Instances (2)</b></span></p>



@snap[north-west span-80]
<p style="line-height:40%" align="left"><span style="font-size:0.650em; font-family:Consolas; " ><br><br><br><br><br>&nbsp;</span></p>
@box[bg-ubuntu text-white my-box-pad2  ](<p style="line-height:40%" align="left"><span style="font-size:0.70em; font-family:Consolas; " >&nbsp;&nbsp;UefiDebugLibConOut&nbsp;&nbsp;  UefiDebugLibStdErr<br>&nbsp;&nbsp;</span></p>)
<br>
@snapend

@snap[north-west span-100]
<br>
<br>
<br>
<br>
<p style="line-height:40%" align="left"><span style="font-size:0.650em; font-family:Consolas; " ><br><br>&nbsp;</span></p>
@box[bg-navy text-white my-box-pad2  ](<p style="line-height:40%" align="left"><span style="font-size:0.70em; font-family:Consolas; " >&nbsp;&nbsp;<br><br><br><br><br><br><br><br><br><br><br><br>&nbsp;&nbsp;</span></p>)
<br>
@snapend

@snap[north-west span-90]
<br>
<br>
<br>
<br>
<p style="line-height:40%" align="left"><span style="font-size:0.650em; font-family:Consolas; " ><br><br><br>&nbsp;</span></p>
<ul>
  <li><span style="font-size:0.8em">Instances of `DebugLib` (for apps and drivers)</span></li><br>
  <li><span style="font-size:0.8em">Send all debug output to console/debug console</span></li>
</ul>
@snapend

@snap[south-east span-20]
![implementation](/assets/images/Implementation.png)
@snapend



@snap[south-east span-20]
<p style="line-height:40%" align="left">@color[black](&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2)&nbsp;&nbsp;&nbsp;<br><br>&nbsp;</p>
<br>
@snapend

Note:
- UefiDebugLibConOut   UefiDebugLibStdErr
- Instances of DebugLib (for Apps and Drivers)
- Send all debug output out to console/debug console
- This allows for viewing of debug information
- Make sure that the console is visible


---?image=/assets/images/slides/Slide26_1.JPG
@title[DebugLib Instances (3)]
<br>
<p align="left"><span class="gold" ><b>`DebugLib` Instances (3)</b></span></p>

@snap[north-west span-75]
<p style="line-height:40%" align="left"><span style="font-size:0.650em; font-family:Consolas; " ><br><br><br><br><br>&nbsp;</span></p>
@box[bg-ubuntu text-white my-box-pad2  ](<p style="line-height:40%" align="left"><span style="font-size:0.70em; font-family:Consolas; " >&nbsp;&nbsp;PeiDxeDebugLibReportStatusCode<br>&nbsp;&nbsp;</span></p>)
<br>
@snapend

@snap[north-west span-100]
<br>
<br>
<br>
<br>
<p style="line-height:40%" align="left"><span style="font-size:0.650em; font-family:Consolas; " ><br><br>&nbsp;</span></p>
@box[bg-green-pp text-white my-box-pad2  ](<p style="line-height:40%" align="left"><span style="font-size:0.70em; font-family:Consolas; " >&nbsp;&nbsp;<br><br><br><br><br><br><br><br><br><br><br><br>&nbsp;&nbsp;</span></p>)
<br>
@snapend

@snap[north-west span-90]
<br>
<br>
<br>
<br>
<p style="line-height:40%" align="left"><span style="font-size:0.650em; font-family:Consolas; " ><br><br><br>&nbsp;</span></p>
<ul>
  <li><span style="font-size:0.8em">Sends ASCII String  specified by  Description Value to the `ReportStatusCode()`  </span></li>
  <li><span style="font-size:0.8em">May also use the `SerialPortLib` class to send debug output to serial port</span></li>
  <li><span style="font-size:0.8em">`BaseDebugLibNull`  - Resolves references </span></li>
</ul>
<br>
<br>
<p align="center"><span style="font-size:0.8em"><font color="yellow">Default for most platforms</font></span></p>
@snapend

@snap[south-east span-20]
![implementation](/assets/images/Implementation.png)
@snapend



@snap[south-east span-20]
<p style="line-height:40%" align="left">@color[black](&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;3)&nbsp;&nbsp;&nbsp;<br><br>&nbsp;</p>
<br>
@snapend


Note:
- So there are a total of 5 open source debug lib instances
- The ones we did not cover are "DebugLibNull" - does nothing and 

- "PeiDxeDebugLibReportStatusCode "  is a form of  'DebugLibReportStatusCode"  that wraps into the report status code library the same way that the serial port one does and may send ASCII String  specified by Description Value that is sent to ReportStatusCode() function



- So there may be other instances in your workspace. It is easy to develop a new  library instance. There is no requirement that someone tell us that they've done it. 
- So what you want to do is search for the library name equals in the INF file.
- Example search the INF files in your workspace for the string "LIBRARY_CLASS                  = DebugLib"

- the ASCII string specified by Description is 
  also passed to the handler that displays the POST card value.  Some 
  implementations of this library function may perform I/O operations directly 
  to a POST card device.  Other implementations may send Value to ReportStatusCode(), 



---
@title[Changing Library Instances ]
<p align="right"><span class="gold" ><b>Changing Library Instances </b></span></p>


@snap[north-west span-90 rounded fragment]
<br>
<br>
<p align="left" style="line-height:80%"><span style="font-size:0.9em; ">@color[yellow](Change common library instances in the platform DSC by Module type)</span></p>
@box[bg-grey-05 text-white rounded my-box-pad2  ](<p style="line-height:40%" align="left"><span style="font-size:0.450em; font-family:Consolas; " >&nbsp;&nbsp;[LibraryClasses.common.IA32]&nbsp;<br>&nbsp;&nbsp;&nbsp;&nbsp;DebugLib|MdePkg/Library/BaseDebugLibNull/BaseDebugLibNull.inf<br>&nbsp;&nbsp;</span></p>)
<br>
@snapend


@snap[north-west span-90 rounded fragment]
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<p align="left" style="line-height:80%"><span style="font-size:0.9em; ">@color[yellow](Change a single module's library instance in the platform DSC)</span></p>
@box[bg-grey-05 text-white rounded my-box-pad2  ](<p style="line-height:40%" align="left"><span style="font-size:0.450em; font-family:Consolas; " >&nbsp;&nbsp;MyPath/MyModule.inf&nbsp;{<br>&nbsp;&nbsp;&lt;LibraryClasses&gt;<br>&nbsp;&nbsp;&nbsp;&nbsp;DebugLib|MdePkg/Library/BaseDebugLibSerialPort.inf<br>&nbsp;&nbsp;}<br>&nbsp;&nbsp;</span></p>)
<br>
@snapend






Note:
- Change common library instances in the platform DSC by module type
<pre>
  [LibraryClasses.common.IA32]
    DebugLib|MdePkg/Library/BaseDebugLibNull/BaseDebugLibNull.inf
</pre>
- Change a single module's library instance in the platform DSC
<pre>
  MyPath/MyModule.inf {
    <LibraryClasses>
     DebugLib|MdePkg/Library/BaseDebugLibSerialPort.inf
  }
</pre>
- another Note: Use a different debugging library instance only on the module in question (managing size changes)


---?image=/assets/images/slides/Slide_LabSec.JPG
@title[Lab 3: Library Instances for Debugging]
<br>
<br>
<p align="Left"><span class="gold" ><b>Lab 3: Library Instances for Debugging</b></span></p>
<br>
<div class="left1">
<span style="font-size:0.8em" >In this lab,  you'll learn how to add specific debug library instances. </span><br>
</div>
<div class="right1">
<span style="font-size:0.8em" >&nbsp;  </span>
</div>

Note:


---
@title[Lab 3: Using Library Instances for Debugging]
<p align="right"><span class="gold" ><b>Lab 3: Using Library Instances for Debugging</b></span></p>
<br>
<span style="font-size:0.7em" >Open `~src/edk2/OvmfPkg/OvmfPkgX64.dsc` </span><br>
<span style="font-size:0.7em" >Replace `SampleApp/SampleApp.inf { . . .}` with the following:</span><br>
```c
  SampleApp/SampleApp.inf {
   <LibraryClasses>
    DebugLib|MdePkg/Library/UefiDebugLibConOut/UefiDebugLibConOut.inf
 }
```
<span style="font-size:0.7em" >Save and close `~src/edk2/OvmfPkg/OvmfPkgX64.dsc` </span><br>
<span style="font-size:0.7em" >Build SampleApp : </span><span style="font-size:0.5em" ><span style="background-color: #000000">&nbsp;&nbsp;`bash$ build`&nbsp;&nbsp;</span></span><br>
<span style="font-size:0.7em" >Copy  SampleApp.efi to hda-contents</span>
```shell
 bash$ cd ~/run-ovmf/hda-contents
 bash$ cp ~/src/edk2/Build/OvmfX64/DEBUG_GCC5/X64/SampleApp.efi .
```

Note:

---?image=/assets/images/slides/Slide62.JPG
@title[Lab 3: Run Qemu Script]
<p align="right"><span class="gold" ><b>Lab 3: Run the Qemu Script</b></span></p>
<br>
<div class="left">
<span style="font-size:0.7em" >Test by Invoking Qemu</span>
<pre>
```
  bash$ cd ~/run-ovmf
  bash$ . RunQemu.sh
```
</pre>
<span style="font-size:0.7em" >Run the application from the shell</span><br>
<span style="font-size:0.5em" ><span style="background-color: #101010">&nbsp;<font color="yellow">`Shell> `&nbsp;</font>`SampleApp`&nbsp;</span></span><br>
<p style="line-height:90%"><span style="font-size:0.7em" >See that the output from the Debug statements now goes to the QEMU console </span><br></p>
<br>
<span style="font-size:0.8em" ><font color="yellow">Exit QEMU</font></span>
</div>
<div class="right">
<span style="font-size:0.8em" >&nbsp;  </span>
</div>

Note:

---?image=/assets/images/slides/Slide_LabSec.JPG
@title[Lab 4: Serial port Instance of DebugLib]
<br>
<br>
<p align="Left"><span class="gold" ><b>Lab 4: Serial port Instance of `DebugLib`</b></span></p>
<br>
<div class="left1">
<span style="font-size:0.8em" >In this lab,  you'll change the `DebugLib` to the Serial port instance. </span>
</div>
<div class="right1">
<span style="font-size:0.8em" >&nbsp;  </span>
</div>

Note:

The DEBUG output for SampleApp is redirected to the serial.log file. This debug library instance only applies to SampleApp and does not alter the general debug behavior of other modules.
To change the entire debug to Serial.log the switch "-D DEBUG_ON_SERIAL_PORT" can be used with the Build command. 


---
@title[Lab 4: Using Serial port Library Instances]
<p align="right"><span class="gold" ><b>Lab 4: Using Serial port Library Instances</b></span></p>
<br>
<span style="font-size:0.7em" >Open `~src/edk2/OvmfPkg/OvmfPkgX64.dsc` </span><br>
<span style="font-size:0.7em" >Replace `SampleApp/SampleApp.inf { . . .}` with the following:</span><br>
```c
  SampleApp/SampleApp.inf {
   <LibraryClasses>      
      DebugLib|MdePkg/Library/BaseDebugLibSerialPort/BaseDebugLibSerialPort.inf
 }
```
<span style="font-size:0.7em" >Save and close `~src/edk2/OvmfPkg/OvmfPkgX64.dsc` </span><br>
<span style="font-size:0.7em" >Build SampleApp : </span><span style="font-size:0.5em" ><span style="background-color: #000000">&nbsp;&nbsp;`bash$ build`&nbsp;&nbsp;</span></span><br>
<span style="font-size:0.7em" >Copy  SampleApp.efi to hda-contents</span>
```shell
 bash$ cd ~/run-ovmf/hda-contents
 bash$ cp ~/src/edk2/Build/OvmfX64/DEBUG_GCC5/X64/SampleApp.efi .
```

Note:


---?image=/assets/images/slides/Slide67.JPG
@title[Lab 4: Run Qemu Script]
<p align="right"><span class="gold" ><b>Lab 4: Run the Qemu Script</b></span></p>
<br>
<div class="left">
<span style="font-size:0.7em" >Test by Invoking Qemu</span>
<pre>
```
  bash$ cd ~/run-ovmf
  bash$ . RunQemu.sh
```
</pre>
<span style="font-size:0.7em" >Run the application from the shell</span><br>
<span style="font-size:0.5em" ><span style="background-color: #101010">&nbsp;<font color="yellow">`Shell> `&nbsp;</font>`SampleApp`&nbsp;</span></span><br>
<p style="line-height:90%"><span style="font-size:0.7em" >Check the contents of the serial.log file </span></p>
<pre>
```
  bash$ cat serial.log
```
</pre>
<span style="font-size:0.8em" ><font color="yellow">Exit QEMU</font></span>

</pre>
</div>
<div class="right">
<span style="font-size:0.8em" >&nbsp;  </span>
</div>

Note:

---?image=/assets/images/slides/Slide_LabSec.JPG
@title[Lab 5: Debugging EDK II with GDB]
<br>
<br>
<p align="Left"><span class="gold" ><b>Lab 5: Debugging EDK II with GDB</b></span></p>
<br>
<div class="left1">
<span style="font-size:0.8em" >In this lab,  you'll learn how setup the Linux GDB to use with EDK II and Qemu </span><br>
<span style="font-size:0.7em" >See also the tianocore.org wiki page:<br> <a href="https://github.com/tianocore/tianocore.github.io/wiki/How-to-debug-OVMF-with-QEMU-using-GDB "> How to use GDB with QEMU</a>
</div>
<div class="right1">
<span style="font-size:0.8em" >&nbsp;  </span>
</div>

Note:

---
@title[Lab 5.1: Update the Qemu Script]
<p align="right"><span class="gold" ><b>Lab 5.1: Update the Qemu Script</b></span></p>
<br>
<p style="line-height:90%"><span style="font-size:0.8em" >Edit  the Linux shell script to run the QEMU from the run-ovmf directory and add  the option for GDB â€œ-sâ€ to 
generate a symbol file and also use IA32 instead of x86_64</span></p><br>
```
 bash$ cd ~/run-ovmf
 bash$ gedit RunQemu.sh
```
<span style="font-size:0.8em" > add the following to RunQemu.sh</span></span>
```
  qemu-system-i386 -s  -pflash bios.bin -hda fat:rw:hda-contents -net none -debugcon file:debug.log -global isa-debugcon.iobase=0x402 
```
<span style="font-size:0.8em" >Save and Exit</span>

Note:
- Lab 5.1
- add the following to the script 
<pre>
  qemu-system-i386 -s -pflash bios.bin -hda fat:rw:hda-contents -net none     -debugcon file:debug.log -global isa-debugcon.iobase=0x402  -serial file:serial.log
</pre>


---
@title[Lab 5.2: Build Ovmf for IA32]
<p align="right"><span class="gold" ><b>Lab 5.2: Build Ovmf for IA32</b></span></p>
<br>
<p style="line-height:90%"><span style="font-size:0.8em">Open `~/src/edk2/OvmfPkg/OvmfPkgIa32.dsc` and add the application to the  (using IA32 )  at the end of the 
   `[Components]` section.</span></p>
```php
  [Components]
   #  add at the end of the components section OvmfPkgIa32.dsc
    SampleApp/SampleApp.inf
```
<span style="font-size:0.8em">Build OVMF for IA32 :  </span>
```
bash$ build -a IA32 -p OvmfPkg/OvmfPkgIa32.dsc
```

<span style="font-size:0.8em">Copy the the OVMF.fd to the run-ovmf directory renaming it bios.bin: </span>
```
bash$ cd ~/run-ovmf/
bash$ cp ~/src/edk2/Build/OvmfIa32/DEBUG_GCC5/FV/OVMF.fd  bios.bin
```


Note:
- lab 5.2

---
@title[Lab 5.3: Build Ovmf for IA32 03]
<p align="right"><span class="gold" ><b>Lab 5.3: Build Ovmf for IA32</b></span></p>
<br>
<span style="font-size:0.8em">Copy the output of SampleApp to the `hda-contents` directory: </span>
```
bash$ cd ~/run-ovmf/hda-contents
bash$ cp ~/src/edk2/Build/OvmfIa32/DEBUG_GCC5/IA32/SampleApp  .
```
<span style="font-size:0.8em">The following will be in the `~/run-ovmf/hda-contents/`</span>
```
   SampleApp.efi
   SampleApp.debug
   SampleApp (Directory)
```
<span style="font-size:0.7em" >Open a Terminal(1) Prompt and Invoke Qemu</span>
```
bash$ cd ~/run-ovmf
bash$ . RunQemu.sh
```
<span style="font-size:0.7em" >Run the application from the shell</span><br>
<span style="font-size:0.5em" ><span style="background-color: #101010">&nbsp;<font color="yellow">`Shell> `&nbsp;</font>`SampleApp`&nbsp;</span></span><br>


Note:
- lab 5.3


---
@title[Lab 5.4: Check debug.log ]
<p align="right"><span class="gold" ><b>Lab 5.4: Check debug.log</b></span></p>
<br>
<p style="line-height:90%"><span style="font-size:0.7em" >Open <font color="yellow"><b>another</b></font> Terminal(2) Prompt in the `run-ovmf` directory and check the `debug.log` file.</span></p>
```
bash$ cd ~/run-ovmf
bash$ cat debug.log
```
<p style="line-height:90%"><span style="font-size:0.7em" >See the line: `Loading driver at 0x00006AEE000` is the memory location where your UEFI Application is loaded. </span></p>
```
 InstallProtocolInterface: 5B1B31A1-9562-11D2-8E3F-00A0C969723B 6F0F028
 Loading driver at 0x00006AEE000 EntryPoint=0x00006AEE756 SampleApp.efi
 InstallProtocolInterface: BC62157E-3E33-4FEC-9920-2D3B36D750DF 6F0FF10
```

Note:
- lab 5.4

---
@title[Lab 5.5: Add a Debug Print]
<p align="right"><span class="gold" ><b>Lab 5.5: Add a Debug Print</b></span></p>
<p style="line-height:90%"><span style="font-size:0.7em" >Add a DEBUG statement to your SampleApp.c application to get the entry point of your code. </span><br>
<span style="font-size:0.5em" >Add the following DEBUG line just before the DEBUG statements from the previous lab:</span></p>
```C
UefiMain (
// . . .
	EFI_INPUT_KEY	   Key;
	// ADD the following line
    DEBUG ((EFI_D_INFO, "My Entry point: 0x%08x\r\n", (CHAR16*)UefiMain )  );
```
<p style="line-height:90%"><span style="font-size:0.7em" >When you print out the debug.log again, the exact entry point for your code will show.</span><br>
<span style="font-size:0.5em" >This is useful to double check symbols are fixed up to the correct line numbers in the source file.</span></p>
```
  Loading driver at 0x00006AEE000 EntryPoint=0x00006AEE756 SampleApp.efi
  InstallProtocolInterface: BC62157E-3E33-4FEC-9920-2D3B36D750DF 6F0FF10
  ProtectUefiImageCommon - 0x6F0F028
    - 0x0000000006AEE000 - 0x0000000000002B00
  InstallProtocolInterface: 752F3136-4E16-4FDC-A22A-E5F46812F4CA 7EA4B00
  My Entry point: 0x06AEE496 
```

Note:
- lab 5.5

---
@title[Lab 5.6: Invoking GDB]
<p align="right"><span class="gold" ><b>Lab 5.6: Invoking GDB</b></span></p>
<span style="font-size:0.7em" >In the terminal(2) prompt Invoke GDB</span><span style="font-size:0.5em" >(note - at first there will be nothing in the source window)</span>
```
bash$ cd ~/run-ovmf/hda-contents
bash$ gdb --tui 
```
<span style="font-size:0.7em" >Load your UEFI Application SampleApp.efi with the "`file`" command.</span>
```
 (gdb) file SampleApp.efi
 Reading symbols from SampleApp.efi...(no debugging symbols found)...done. 
```
<p style="line-height:90%"><span style="font-size:0.7em" >Check where GDB has for ".text" and ".data" offsets with "`info files`" command.</span></p>
```
 (gdb) info files
 Symbols from "/home/u-mypc/run-ovmf/hda-contents/SampleApp.efi".
 Local exec file:
        `/home/u-mypc/run-ovmf/hda-contents/SampleApp.efi',
        file type pei-i386.
        Entry point: 0x756
        0x00000240 - 0x000028c0 is .text
        0x000028c0 - 0x00002980 is .data
        0x00002980 - 0x00002b00 is .reloc
```

Note:
- Lab 5.6
- For the GDB commands, only type what is after the "(gdb)" prompt


---
@title[Lab 5.7: Calculate Addresses]
<p align="right"><span class="gold" ><b>Lab 5.7: Calculate Addresses</b></span></p>
<br>
<p style="line-height:80%"><span style="font-size:0.7em" >We need to calculate our addresses for ".text" and ".data" section.</span><br>
<span style="font-size:0.5em" > The application is loaded under `0x00006AEE000` (loading driver point - <b>NOT Entrypoint</b>) and we know text and data offsets.</span></p>
```
 text = 0x00006AEE000  +  0x00000240 = 0x06AEE240
 data = 0x00006AEE000  +  0x00000240 + 0x000028c0 = 0x06AF0B00 
```
<span style="font-size:0.7em" >Unload the .efi file</span>
```
(gdb) file
No executable file now.
No symbol file now.
```

Note:
- Lab 5.7
- For the GDB commands, only type what is after the "(gdb)" prompt


---
@title[Lab 5.8: Load the Symbols for SampleApp]
<p align="right"><span class="gold" ><b>Lab 5.8: Load the Symbols for SampleApp</b></span></p>
<br>
<p style="line-height:90%"><span style="font-size:0.7em" >Load the symbols with the fixed up address using SampleApp output .debug file using the "`add-symbol-file`" command:</span></p>
```
(gdb) add-symbol-file SampleApp.debug 0x06AEE240 -s .data 0x06AF0B00 
add symbol table from file "SampleApp.debug" at

        .text_addr = 0x6aee240
        .data_addr = 0x6af0b00
(y or n) y
Reading symbols from SampleApp.debug...done.
```
<span style="font-size:0.7em" >Set a break point at UefiMain</span>
```
(gdb) break UefiMain
Breakpoint 1 at 0x6aee496: file /home/u-uefi/src/edk2/SampleApp/SampleApp.c, line 40.
```

Note:
- Lab 5.8
- For the GDB commands, only type what is after the "(gdb)" prompt


---
@title[Lab 5.9: Attach GDB to QEMU]
<p align="right"><span class="gold" ><b>Lab 5.9: Attach GDB to QEMU</b></span></p>

<span style="font-size:0.7em" >Attach the GDB debugger to QEMU</span>


```
(gdb) target remote localhost:1234
Remote debugging using localhost:1234
0x07df6ba4 in ?? ()
```
<span style="font-size:0.7em" >Continue in GDB</span>
```
(gdb) c
Continuing.
```
<span style="font-size:0.7em" >In the QEMU Window Invoke your application again</span>
```
Fs0:\> SampleApp.efi
```
<p style="line-height:80%"><span style="font-size:0.7em" >The GDB will hit your break point in your UEFI application's entry point and you can begin to debug with source code debugging.</span></p>


Note:
- Lab 5.9
- For the GDB commands, only type what is after the "(gdb)" prompt


---?image=/assets/images/slides/Slide71.JPG
@title[Lab 5: GBD and QEMU Windows]
<p align="right"><span class="gold" ><b>Lab 5: GBD and QEMU Windows</b></span></p>
<span style="font-size:0.7em" >The GDB window will look similar to this</span>

Note:
- Lab 5.10



---  
@title[Summary]
<BR>
### <p align="center"><span class="gold"   >Summary </span></p><br>
<ul style="list-style-type:none">
 <li>@fa[certificate gp-bullet-green]<span style="font-size:0.9em">&nbsp;&nbsp;Define `DebugLib` and its attributes</span> </li>
 <li>@fa[certificate gp-bullet-cyan]<span style="font-size:0.9em">&nbsp;&nbsp;List the ways to debug</span></li>
 <li>@fa[certificate gp-bullet-gold]<span style="font-size:0.9em">&nbsp;&nbsp;Using PCDs to Configure `DebugLib` - LAB </span> </li>
 <li>@fa[certificate gp-bullet-ltgreen]<span style="font-size:0.9em">&nbsp;&nbsp;Change Compiler & Linker Flags for debugging</span></li>
 <li>@fa[certificate gp-bullet-yellow]<span style="font-size:0.9em">&nbsp;&nbsp;Change the `DebugLib` instance to modify the debug<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; output - LAB</span> </li>
 <li>@fa[certificate gp-bullet-magenta]<span style="font-size:0.9em">&nbsp;&nbsp;Debug EDK II using GDB - LAB</span> </li>
</ul>

---?image=assets/images/gitpitch-audience.jpg
@title[Questions]
<br>
![Questions](/assets/images/questions.JPG) 


---?image=assets/images/gitpitch-audience.jpg
@title[Logo Slide]
<br><br><br>
![Logo Slide](/assets/images/TianocoreLogo.png =10x)


---
@title[Acknowledgements]
#### <p align="center"><span class="gold"   >Acknowledgements</span></p>

```c++
/**
Redistribution and use in source (original document form) and 'compiled' forms (converted
to PDF, epub, HTML and other formats) with or without modification, are permitted provided
that the following conditions are met:

Redistributions of source code (original document form) must retain the above copyright 
notice, this list of conditions and the following disclaimer as the first lines of this 
file unmodified.

Redistributions in compiled form (transformed to other DTDs, converted to PDF, epub, HTML
and other formats) must reproduce the above copyright notice, this list of conditions and 
the following disclaimer in the documentation and/or other materials provided with the 
distribution.

THIS DOCUMENTATION IS PROVIDED BY TIANOCORE PROJECT "AS IS" AND ANY EXPRESS OR IMPLIED 
WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND 
FITNESS FOR A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO EVENT SHALL TIANOCORE PROJECT BE 
LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES 
(INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, 
DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY, 
WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) 
ARISING IN ANY WAY OUT OF THE USE OF THIS DOCUMENTATION, EVEN IF ADVISED OF THE POSSIBILITY 
OF SUCH DAMAGE.

Copyright (c) 2018, Intel Corporation. All rights reserved.
**/

```
