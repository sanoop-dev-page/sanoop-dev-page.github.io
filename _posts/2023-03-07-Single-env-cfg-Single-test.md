---
title: Single env config? then single test!
date: 2023-03-07 
categories: [UVM]
tags: [test]     # TAG names should always be lowercase
author: sanoop
---
This is a continuation to the previous post [Lesser known ways of creating uvm classes.](https://sanoop.dev/posts/lesser-known-ways-of-creating-uvm-classes/). In the previous post we looked at some of the factory methods to create `uvm` objects/classes. In this post lets see a complete example of its application.

Suppose we have a verification item in which all the `test`/`env` conditions remains the same, and you just want to run a different `sequence`. Many a times, we see engineers go ahead and copy paste an existing test and rename the sequence being called to the new one. Let's how this can be avoided and use a single test and control it through `$plus$args$`. This could be an alternate to generating tests using `macros`.

> Disclaimer: These are my lazy way of doing DV and not a recommendation, adapt this at your own risk ;) !
{: .prompt-warning }

```verilog
class my_dut_gen_test extends my_dut_base_test;
   
   //Declare base class handle for the seq;
   uvm_sequence seq;
   //Declare variable that will hold the target sequence's name
   string seq_name;

   // UVM component utility macros
   `uvm_component_utils(my_dut_gen_test)

   // Constructor
   function new(string name = "my_dut_gen_test", uvm_component parent = null);
      super.new(name, parent);
      //Identify the sequence based on the plusarg passed to the command line
      priority case (1)
        $test$plusargs("TEST_SEQ1"): seq_name = "my_dut_test_seq1";
        $test$plusargs("TEST_SEQ2"): seq_name = "my_dut_test_seq2";
        $test$plusargs("TEST_SEQ3"): seq_name = "my_dut_test_seq3";
        $test$plusargs("TEST_SEQ4"): seq_name = "my_dut_test_seq4";
        //When you develop a new sequence, simply add it here with a new plus arg assigned
         default: seq_name= "my_dut_base_vseq";
      endcase
      `uvm_info(get_full_name(), $sformatf("The sequence picked for this run is -%0s", seq_name), UVM_NONE)
   endfunction : new

   // Build phase
   virtual function void build_phase(uvm_phase phase);
      uvm_factory factory;
      super.build_phase(phase);
     factory = uvm_factory::get();
      // Create the sequence
     $cast(seq,factory.create_object_by_name(seq_name, "" ,"seq" ));
   endfunction : build_phase

   // Main phase
   virtual task main_phase(uvm_phase phase);
      phase.raise_objection(this, "Start test main_phase");
      super.main_phase(phase);
      `uvm_info("START_SEQ", $sformatf("Starting %0s", seq_name), UVM_LOW)
      //seq will be holding the handle to the one selected through $plus$arg
      seq.start(env.vsqr);
      phase.drop_objection(this, "End test main_phase");
   endtask : main_phase

endclass : my_dut_gen_test
```

