---
title: Lesser known ways of creating uvm classes.
date: 2023-03-06 
categories: [UVM]
tags: [factory]     # TAG names should always be lowercase
author: sanoop
---

Some digging into uvm `factory` reveals that the components and objects can also be created using the below functions of uvm `factory`.

- `create_object_by_type`	
- `create_component_by_type`	
- `create_object_by_name`	
- `create_component_by_name`	

All of these functions creates and returns a component or object.


Where can you use this? -
I can think of one application for `create_*_by_name`. If you have different child classes and you would like to create it based on the name of the child class (which can be passed as a `string`), you can pass it as an argument to the simulator and build it by calling these functions.
This can be achieved using only few lines of code, for eg-
```verilog
uvm_factory factory;
factory = uvm_factory::get();
$cast(base_class_h,factory.create_object_by_name(var_that_holds_child_class_name, "" ,"child_class_type_name" ));
```
More on this in another post.

> This will work only if the components/objects are registered with the uvm factory while declaring.
{: .prompt-warning }