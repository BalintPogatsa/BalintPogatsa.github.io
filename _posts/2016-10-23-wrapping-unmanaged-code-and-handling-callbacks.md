---
layout: post
title: Wrapping unmanaged C++ code with C++ CLI and handling callbacks
date: 2016-10-23
---

One way of using an unmanaged C++ library from managed code is to create a wrapper layer in C++ CLI.
In that situation it also happens that you have to implement an interface of the unmanaged library to provide some kind of functionality.
For example there is a log interface which you have to implement. 

![alt text](https://github.com/BalintPogatsa/BalintPogatsa.github.io/raw/master/img/CrossAppDomainManagedLogger_components.png "Unmanaged callback components")

Because you use the unmanaged library from managed code, you also want to implement the logging functionality in C#.
But this implementation cannot be passed directly to the unmanaged code, so another indirection is needed.
The solution is to implement in C++ CLI with mixed code (uses both unmanaged and managed code) a bridge object (LoggerBridge),
which can implement the interface of the unmanaged library, and can also call your C# log implementation.

![alt text](https://github.com/BalintPogatsa/BalintPogatsa.github.io/raw/master/img/CrossAppDomainManagedLogger_sequence_simple.png "Unmanaged callback sequence")

This solution works fine until you create the bridge object in another app domain (not in the default) and the native call comes from a different thread.
Then you get the System.ArgumentException: Cannot pass a GCHandle across AppDomains. The source of this problem is that the native to managed transition happens in this case in the default app domain,
where the created gcroot reference cannot be called.

It's not very straightforward to resolve this problem, but there is already one solution which uses delegates, 
[described here](http://www.lenholgate.com/blog/2009/07/error-cannot-pass-a-gchandle-across-appdomains.html "GCHandle accross app domains")

Another solution, which I explain here is to implement the managed logger as a [CrossAppDomainSingleton](http://www.ingebrigtsen.info/2007/05/18/cross-appdomain-singleton/ "CrossAppDomainSingleton"), which will direct the callback to the correct app domain.
One way to reproduce the problem with this kind of unmanaged-managed callback is to run some tests with the [NUnit Gui](http://www.nunit.org/index.php?p=nunit-gui&r=2.2.10 "NUnit Gui"), 
because the NUnit Gui has it's own default app domain, and loads the tests in a new one.

![alt text](https://github.com/BalintPogatsa/BalintPogatsa.github.io/raw/master/img/CrossAppDomainManagedLogger_sequence.png "Unmanaged callback with singleton")

You can find my test project in [this git repository](https://github.com/BalintPogatsa/CrossAppDomainManagedLogger "CrossAppDomainManagedLogger repository")

If there are different kind of implementations of the callback interface, the CrossAppDomainManagedLogger can be extended to keep multiple instances with some kind of dictionary to decide which one to call in which moment.