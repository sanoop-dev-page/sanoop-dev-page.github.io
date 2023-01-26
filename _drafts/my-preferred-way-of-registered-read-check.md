---
title: My preferred way of doing register read check.
date: 2023-01-21 
categories: [UVM]
tags: [RAL]     # TAG names should always be lowercase
author: sanoop
---
What's your preferred way of checking a register's read value, in a RAL integrated environment? 
I have often seen engineers doing this - 
```verilog
uvm_status_e   status;
uvm_reg_data_t value;
uvm_reg_data_t chk_value;
chk_value= something_that_sets_chk_value();

my_block.my_reg.read( .status( status ), .value( value ) );

if(value!=chk_value)
    `uvm_error("ERR,"My error message with missing key info")
```

I would rather do it as shown below - 
```verilog
my_block.my_reg.predict(chk_value);
my_block.my_reg.mirror(.status(status),.check(UVM_CHECK);
```

Advantage? UVM does a check against the mirrored value, which is updated by the predict method and the read value, and gives out a nice error message.

> If you would like to use this method, while calling mirror method, make sure the value of check argument is UVM_CHECK. The default arg value of check in mirror method is UVM_NO_CHECK.
{: .prompt-tip }