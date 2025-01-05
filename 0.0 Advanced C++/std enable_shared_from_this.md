from Dr Dobbs article on weak pointers, I think this example is easier to understand (source: [http://drdobbs.com/cpp/184402026](http://drdobbs.com/cpp/184402026)):

...code like this won't work correctly:

```
int *ip = new int;
shared_ptr<int> sp1(ip);
shared_ptr<int> sp2(ip);
```

Neither of the two `shared_ptr` objects knows about the other, so both will try to release the resource when they are destroyed. That usually leads to problems.

Similarly, if a member function needs a `shared_ptr` object that owns the object that it's being called on, it can't just create an object on the fly:

```
struct S
{
  shared_ptr<S> dangerous()
  {
     return shared_ptr<S>(this);   // don't do this!
  }
};

int main()
{
   shared_ptr<S> sp1(new S);
   shared_ptr<S> sp2 = sp1->dangerous();
   return 0;
}
```

This code has the same problem as the earlier example, although in a more subtle form. When it is constructed, the `shared_pt`r object `sp1` owns the newly allocated resource. The code inside the member function `S::dangerous` doesn't know about that `shared_ptr` object, so the `shared_ptr` object that it returns is distinct from `sp1`. Copying the new `shared_ptr` object to `sp2` doesn't help; when `sp2` goes out of scope, it will release the resource, and when `sp1` goes out of scope, it will release the resource again.

# Solution

The way to avoid this problem is to use the class template `enable_shared_from_this`. The template takes one template type argument, which is the name of the class that defines the managed resource. That class must, in turn, be derived publicly from the template; like this:

```
struct S : enable_shared_from_this<S>
{
  shared_ptr<S> not_dangerous()
  {
    return shared_from_this();
  }
};

int main()
{
   shared_ptr<S> sp1(new S);
   shared_ptr<S> sp2 = sp1->not_dangerous();
   return 0;
}
```

When you do this, keep in mind that the object on which you call `shared_from_this` must be owned by a `shared_ptr` object. This won't work:

```
int main()
{
   S *p = new S;
   shared_ptr<S> sp2 = p->not_dangerous();     // don't do this
}
```