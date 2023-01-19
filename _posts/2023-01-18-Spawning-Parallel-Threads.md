---
title: Spawning Parallel Threads
date: 2023-01-18 
categories: [SYSTEMVERILOG]
tags: [fork]     # TAG names should always be lowercase
author: sanoop
---

Scenario: Join once all processes are completed


```verilog
fork 
  begin : outer_thread //this holds the parent process that spawns the child processes 
    for(int loop_count=0;loop_count<10;loop_count++)begin : inner_loop //the actual for loop
      fork
        automatic int iter=loop_count; 
        begin
          `uvm_info("", $sformatf("my thread id is %0d", iter), UVM_NONE);
        end
      join_none;
    end : inner_loop
  wait fork;
  end : outer_thread
join
```

>The `automatic` keyword holds the key in this implementation. Without it, at the time of executing the inner block, the iter variable will hold the final value the -loop_count variable 
{: .prompt-info }
