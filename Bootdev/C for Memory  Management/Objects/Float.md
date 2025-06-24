Sneklang needs floats (obviously). How else will all the crypto bros write weird floating-point bugs into their smart contracts?

So how do we store both floats and ints into the same type?

We're going to use the `union` and `enum` features in C to be able to do this, just like we discussed in previous chapters. You will need to extend the existing `snek_object_kind_t`and `snek_object_data_t` types to be able to include both `int`s and `float`s (and we will continue to add more types in the following chapters to exercise your newfound mastery of memory management).