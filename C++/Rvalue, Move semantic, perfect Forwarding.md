
## Perfect Forwarding
[[hware_iface#construct_instance()]]

It allows move semantics to be automatically applied, even when the source and the destination of a move are separated by intervening function calls. 
Common examples include constructors and setter functions that forward arguments they receive to the data members of the class they are initializing or setting, as well as standard library functions like make_shared. which “perfect-forwards” its arguments to the class constructor of whatever object the to-be-created shared_ptr is to point to