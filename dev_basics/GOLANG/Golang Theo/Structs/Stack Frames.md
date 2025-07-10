Think back to the warnings I gave you about working with `void*` data types. It's easy to push the wrong kinds of data onto our `stack_t` because, like Starbucks Wi-Fi, it will let _anything_ in.

So, to prevent us from shooting ourselves in the foot, we will create some functions that are more type safe and make it much more difficult to do the wrong thing when interacting with our `vm_t`. Wrong things like:

```c
// The C compiler won't stop us :'(
stack_push(vm->frames, (void *)7);
stack_push(vm->frames, (void *)"uh oh");
```

But we want to make it easier on ourselves to only push `frame_t *` types onto `vm->frames`, so we'll write some wrapper functions to help us out.