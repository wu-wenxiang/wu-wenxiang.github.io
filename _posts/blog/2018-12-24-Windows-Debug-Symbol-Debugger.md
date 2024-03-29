---
layout:         post
title:          Windows User Mode Native Debugging的常见问题
category:       blog
description:    概念（Exported Function、Symbol、Debugging API）和常见场景的处理
---

## 问题整理

- Installation, Configuration & QuickStart
    - Precheck Installation: Python / VisualStudio / Windbg / Sysinternal Kit
    - How to launch a process as debug target with Windbg?
    - How to attach/detach a process with Windbg?
- About Symbol
    - How did assemblies export their external functions?
    - What's the symbols?
    - How to config the symbol path in debugger settings?
    - How did assemblies map to their symbol files?
    - How did assemblies map to their source codes?
    - What're the differences between public & private symbol?
    - How to generate a public symbol?
    - What does the symbol path syntax mean?
    - How to build a symbol server?
    - The best practices about symbol configurations
    - What's the order when windbg search the symbols?
    - How to call a function with its symbol name from an assembly?
- About Debugger
    - What's the differents between c call & standard call?
    - What's the basic principle of the debugger?
    - List the typical debug events
    - What's the differences between soft, hard and memory breakpoints?
    - How to implement Onlaunch debugging?
    - How to implement Attach debugging?
    - How to implement Breakpoint?
    - How to implement Hook & Injection?
- Windbg Usage
    - How to examine the Process Information?
    - How to viewing and edit Memory?
    - How to set a Breakpoint at function entries?
    - How to set a Breakpoint target writing memory?
    - How to use debugger extensions?
    - How to use !analyze in hang / crash scenarios?
    - Why my source code could not display with !analyze -v?
    - How to find error code of a win32 api that returns false
    - How to set a Conditional Data Breakpoint?
    - How to write a Simple Debugger Command Program?
    - How to build the SimpleLabExts Debugger Extension
    - How to write scripts by pykd, or javascript?
- About Sysinternal tools
    - How to find which process locked the files?
    - How to release the file from the process?
    - How to check the owner about a popup window?
    - How to monitor memory/cpu/io by procexp?
    - How to check the callstack for a high cpu process?
    - How to check the strings from heap?
    - How to check the PATH environment about the process?
    - How to monitor file/registry/network/process/thread?
    - How to capture a full memory dump?
    - How to capture dumps based on conditions?
    - How to analyze dumps automatically?
- About Performance Tunning
    - Performance Counters
    - ETLTrace Collecting
    - Demo: Xperf
    - Demo: GpuView
- About Crash
    - How to debug Access Violations?
    - How to debug Heap Corruption?
    - How to debug Stack Corruption?
- About Hang
    - Common hang scenarios
        - Wait for Lock
        - Wait for Web Services
        - Wait for DB
    - How to debug a Spinning Thread?
- About Leak
    - GFlags
    - How to debug a Native Memory Leak?
    - Finding COM Leaks Using Extensions
    - VMMap
    - RAMMap
    - LeakTrack extension
    - UMDH
- About HighCPU
    - Live Debug with HighCPU
    - Dump Analysis with HighCPU
    - Common HighCPU scenarios
        - Regex
        - Parallel race
        - Blank loop
        - GC
- About Mex
    - Demo: Mex Usage
- About Windbg Preview
    - Time Travel Debugging Overview
- About X64 Debug
    - Parameter Passing and Stack
    - Home Space and Optimized Code
- About Real Case (Optional, real case from cisco also OK)
    - Demo: Crash when export certificate
    - Demo: Crash when call a function from an assembly
    - Demo: Hang with Lock / WebService / DB
    - Demo: High CPU since blank loop
    - Demo: High CPU since parallel race
    - Demo: High CPU since regex

## 答案

- 参考: [Github](https://github.com/wu-wenxiang/Training-Debug-Windows-Public)


