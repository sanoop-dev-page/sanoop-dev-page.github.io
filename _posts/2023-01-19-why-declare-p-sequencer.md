---
title: Why we need to declare p_sequencer? Isn't m_sequencer enough?
date: 2023-01-19 
categories: [UVM]
tags: [sequencer]     # TAG names should always be lowercase
author: sanoop
---

The `m_sequencer` handle, belonging to a sequence is of base class type of the customized sequencer. So If we would like to access the properties of the customized sequencer, we need to cast the `m_sequencer` to the `p_sequencer` by using
```verilog
`uvm_declare_p_sequencer(<custom_seqr's_name>)
```

> If you have nothing else to do, be brave, try using a different variable name instead of `p_sequencer` by casting the m_sequencer handle to your new variable. Cheers!
{: .prompt-tip }