---?image=assets/images/gitpitch-audience.jpg
@title[EDK II Debugging]
<br><br><br><br><br>
## <span class="gold"   >UEFI & EDK II Training</span>

#### EDK II Debugging 

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
 <li>@fa[certificate gp-bullet-yellow]<span style="font-size:0.9em">&nbsp;&nbsp;Use `DEBUG` instead of `Print` function</span> </li>
 <li>@fa[certificate gp-bullet-gold]<span style="font-size:0.9em">&nbsp;&nbsp;Using PCDs to Configure `DebugLib` </span> </li>
 <li>@fa[certificate gp-bullet-ltgreen]<span style="font-size:0.9em">&nbsp;&nbsp;Change Compiler & Linker Flags for debugging</span></li>
 <li>@fa[certificate gp-bullet-yellow]<span style="font-size:0.9em">&nbsp;&nbsp;Change the `DebugLib` instance to modify the debug<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; output</span> </li>
</ul>

---?image=assets/images/binary-strings-black2.jpg
@title[UEFI Driver Lab]
<br><br><br><br><br><br><br>
### <span class="gold"  >&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Debugging Overview </span>
<span style="font-size:0.9em" >&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span>


---?image=/assets/images/slides/Slide4.JPG
@title[Debug Methods]
### <p align="center"><span class="gold" ><b>Debug Methods</b></span></p>
<br>
<br>
<div class="left1">
@ul[no-bullet]
- <span style="font-size:0.9em" ><font color="white">`DEBUG` and `ASSERT` macros in EDK II code </font></span><br>
- <span style="font-size:0.9em" ><font color="cyan">`DEBUG` instead of `Print` functions </font></span><br>
- <span style="font-size:0.9em" ><font color="white">Software/hardware debuggers </font></span><br>
- <span style="font-size:0.9em" ><font color="cyan">Shell commands to test capabilities for simple debugging </font></span>
@ulend
</div>
<div class="right1">
<span style="font-size:0.8em" >&nbsp;  </span>
</div>

Note:
- Ways to Debug…
- Use DEBUG instead of Print functions in code
- Use a software debugger (COM/USB)
- Use a hardware debugger (JTAG/XDB)
- Soft loading driver through UEFI shell
- Use shell commands to test capabilities

- What are some alternatives if I don’t want to use the debug lib? 

- You can use print statements and soft load my driver to the shell to see what is going on. The downside is that you then have print statements in your code.  This doesn’t work really well if you want to make a release to a customer.
- You could use a software debugger, and there is a hardware debugger but if they are not available EDK II debug macro might be a good place to start.

- We believe the debug lib is the simplest and cleanest way to get it all working

---
@title[EDK II DebugLib Library]
<p align="center"><span style="font-size:01.1em" ><font color="#e49436" ><b>EDK II `DebugLib` Library</b></font></span></p>
<br>
@ul[no-bullet]
- <span style="font-size:01.1em" >&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="background-color: #fdb819">
  <b>&nbsp;&nbsp;&nbsp;&nbsp;`Debug` and `Assert` macros in code &nbsp;&nbsp;</b></span></span></p><br><br>
- <span style="font-size:01.1em" >&nbsp;<span style="background-color: #92d050">&nbsp;&nbsp;<b>Enable/disable when compiled (`target.txt`)</b>&nbsp;&nbsp;</span></span><br><br>
- <span style="font-size:01.1em" >&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="background-color: #7030a0">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<b>Connects a Host to capture debug messages &nbsp;</b>&nbsp;&nbsp;</span></span>
@ulend

Note:
- DebugLib library is clean & very portable
- Using DEBUG and ASSERT macros in code
- Enable/disable when compiled (target.txt)
- Can connect a 2nd PC to capture debug messages 

- The main message-- the debug lib library is portable, it’s clean, it’s very easy to use, and we believe it’s the easiest way to do debugging on a UEFI platform.

- The debug lib library has the debug and assert macros. 
- There are library instances that allow you to use a second PC to capture all messages coming out



---?image=assets/images/binary-strings-black2.jpg
@title[Debugging with PCDs]
<br><br><br><br><br><br><br>
### <span class="gold"  >&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Debugging with PCDs </span>
<span style="font-size:0.9em" >&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span>


---?image=/assets/images/slides/Slide9.JPG
@title[Using PCDs to Configure DebugLib]
<p align="right"><span class="gold" >Using PCDs to Configure `DebugLib`</span></p>

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


---?image=/assets/images/slides/Slide11.JPG
@title[PcdDebugPropertyMask Values]
<p align="right"><span class="gold" >`PcdDebugPropertyMask` Values</span></p>

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




---?image=/assets/images/slides/Slide13.JPG
@title[PcdDebugPrintErrorLevel Values]
<p align="right"><span class="gold" >`PcdDebugPrintErrorLevel` Values</span></p>

Note:
- Determines if each print message is displayed
- Use Binary-AND setting to set parameter TRUE
- Note that Aliases EFI_D_INIT == DEBUG_INIT, etc..
- DebugPrintErrorLevel Values

- This has to do with what messages we want to come out

- Let’s say you have a debug print enabled as in the previous slide. 
   - You must state what message type you want to print out

- You can assign your own values to this debug print error level.  
- However, these are the guideline values that these drivers use in terms of what their debug output messaging will be.



---?image=/assets/images/slides/Slide15.JPG
<!-- .slide: data-transition="none" -->
@title[Changing PCD Values ]
<p align="right"><span class="gold" >Changing PCD Values</span></p>


Note:
- Use different PCD values only on the module being debugged
- Minimize message output and minimize size increase

+++?image=/assets/images/slides/Slide16.JPG
<!-- .slide: data-background-transition="none" -->
<!-- .slide: data-transition="none" -->
@title[Changing PCD Values 02 ]
<p align="right"><span class="gold" >Changing PCD Values</span></p>


Note:
- Use different PCD values only on the module being debugged
- Minimize message output and minimize size increase



---?image=/assets/images/slides/Slide18.JPG
@title[Other Debug Related Libraries  ]
<p align="right"><span class="gold" >Other Debug Related Libraries </span></p>

Note:

- ReportStatusCodeLib –Progress codes
	- gEfiMdePkgTokenSpaceGuid.PcdReportStatusCodePropertyMask
- PostCodeLib – Enable Post codes
	- gEfiMdePkgTokenSpaceGuid.PcdPostCodePropertyMask
- PerformanceLib – Enable Measurement
	- gEfiMdePkgTokenSpaceGuid.PcdPerformanceLibraryPropertyMask

---?image=/assets/images/slides/Slide_LabSec.JPG
@title[Lab 1: Adding Debug Statements]
<br>
<br>
<p align="Left"><span class="gold" >Lab 1: Adding Debug Statements</span></p>
<br>
<div class="left1">
<span style="font-size:0.8em" >In this lab, you’ll add debug statements to the previous lab's SampleApp UEFI Shell application</span>
</div>
<div class="right1">
<span style="font-size:0.8em" >&nbsp;  </span>
</div>

Note:
In this lab, you’ll learn how to add debug statements.  
This lab uses code from a previous exercise as a starting point (refer to  Writing Simple UEFI Applications).  Before proceeding, verify that the SampleApp code is present in your workspace and that the code references the OvmfPkgX64.dsc file.

---
@title[Lab 1: Catch Up SampleApp]
<p align="right"><span class="gold" >Lab 1: Catch up from previous lab</span></p>
<span style="font-size:0.8em" >Skip if Lab <a href="https://gitpitch.com/Laurie0131/Writing_UEFI_App_Lab/master#/">Writing UEFI App Lab</a> completed</span>
<ul>
   <li><span style="font-size:0.8em" >Perform <a href="https://gitpitch.com/Laurie0131/Platform_Build_LAB/master#/">Lab Setup</a> from previous Labs  </span></li>
   <li><span style="font-size:0.8em" >Create a Directory under the workspace `~/src/edk2 SampleApp`</span></li>
   <li><span style="font-size:0.8em" >Copy contents of `~/FW/LabSampleCode/SampleAppDebug` to `~/src/edk2 SampleApp`</span></li>
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
@title[Lab 1: Add debug statments SampleApp]
<p align="right"><span class="gold" >Lab 1: Add debug statments to SampleApp</span></p>
<br>
<ul>
  <li><span style="font-size:0.8em" >Open a Terminal Command Prompt (Cnt-Alt-T) and type cd ~/src/edk2<br></span>&nbsp;&nbsp;&nbsp;<span style="font-size:0.6em" ><span style="background-color: #101010">&nbsp;` bash$  .  edksetup `&nbsp;</span> </span></li><br>
  <li><span style="font-size:0.8em" >Open `~/src/edk2/SampleApp/SampleApp.c` </span></li><br>
  <li><span style="font-size:0.8em" >Add the following to the include statements at the top of the file after below the last “include” statement: </span></li>
<pre>
```
 #include <Library/DebugLib.h>
```
</pre>
</ul>

---
@title[Lab 1: Add debug statments SampleApp 02]
<p align="right"><span class="gold" >Lab 1: Add debug statments to SampleApp</span></p>
<span style="font-size:0.7em" >Locate the `UefiMain` function. Then copy and paste the following code after the <span style="background-color: #101010">&nbsp;“`EFI_INPUT_KEY  KEY;`”</span> statement: and before the first <span style="background-color: #101010">&nbsp;`Print()` </span>statement </span>
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

---?image=/assets/images/slides/Slide26.JPG
@title[Lab 1: Update the Qemu Script]
<p align="right"><span class="gold" >Lab 1: Update the Qemu Script</span></p>

Note:

<pre>

 qemu-system-x86_64 -pflash bios.bin -hda fat:rw:hda-contents -net none     -debugcon file:debug.log -global isa-debugcon.iobase=0x402  -serial file:serial.log
</pre>

---
@title[Lab 1 Build and Test Driver]
<p align="right"><span class="gold" >Lab 1: Build and Test Driver</span></p>
<br>
<span style="font-size:0.8em" >Build MyWizardDriver – Cd to ~/src/edk2 dir </span>
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
<p align="right"><span class="gold" >Lab 1: Run the Qemu Script</span></p>
<br>
<div class="left">
<span style="font-size:0.7em" >Test by Invoking Qemu</span>
<pre>
```
  bash$ cd ~/run-ovmf
  bash$ . RunQemu.sh
```
</pre>
<span style="font-size:0.7em" >Run the application from the shell, type </span><br>
<span style="font-size:0.5em" ><span style="background-color: #101010">&nbsp;<font color="yellow">`Shell> `&nbsp;</font>`SampleApp`&nbsp;</span></span><br>
<span style="font-size:0.7em" >Print out the contents of the Debug.log file </span>
<pre>
```
  bash$ cat Debug.log
```
</pre>
<br>
<span style="font-size:0.8em" ><font color="yellow">Exit QEMU</font></span>

</pre>
</div>
<div class="right">
<span style="font-size:0.8em" >&nbsp;  </span>
</div>

---?image=/assets/images/slides/Slide_LabSec.JPG
@title[Lab 2: Changing PCD Value]
<br>
<br>
<p align="Left"><span class="gold" >Lab 2: Changing PCD Value</span></p>
<br>
<div class="left1">
<span style="font-size:0.8em" >In this lab, you’ll you’ll learn how to use PCD values to change debugging capabilities. </span>
</div>
<div class="right1">
<span style="font-size:0.8em" >&nbsp;  </span>
</div>

Note:
In this lab, you’ll learn how to use PCD values to change debugging capabilities.  The previous lab,  Adding Debug Statements, did not display all the DEBUG messages added to SampleApp.c.  This lab shows how to change this behavior.




---  
@title[Summary]
<BR>
### <p align="center"><span class="gold"   >Summary </span></p><br>
<ul style="list-style-type:none">
 <li>@fa[certificate gp-bullet-green]<span style="font-size:0.9em">&nbsp;&nbsp;Define `DebugLib` and its attributes</span> </li>
 <li>@fa[certificate gp-bullet-cyan]<span style="font-size:0.9em">&nbsp;&nbsp;List the ways to debug</span></li>
 <li>@fa[certificate gp-bullet-yellow]<span style="font-size:0.9em">&nbsp;&nbsp;Use `DEBUG` instead of `Print` function</span> </li>
 <li>@fa[certificate gp-bullet-gold]<span style="font-size:0.9em">&nbsp;&nbsp;Using PCDs to Configure `DebugLib` </span> </li>
 <li>@fa[certificate gp-bullet-ltgreen]<span style="font-size:0.9em">&nbsp;&nbsp;Change Compiler & Linker Flags for debugging</span></li>
 <li>@fa[certificate gp-bullet-yellow]<span style="font-size:0.9em">&nbsp;&nbsp;Change the `DebugLib` instance to modify the debug<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; output</span> </li>
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
