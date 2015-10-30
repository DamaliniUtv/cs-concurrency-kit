Overview
===========================
This is a stripped version of the Spicy Pixel Concurrency Kit, used to integrate debuggable code in unity projects.
Also alternated to enable support for Unity .Net2.0 Subset compatibility level.

Integrate
----------------
Put SpicyPixel.Threading.dll, SpicyPixel.Threading.Unity.dll and System.Threading.dll in Unity assets

Generate dll
----------------
Checkout System.Threading folder, open solution i Visual Studio and build release version.
Target framework should be .NET Framework 3.5

Modifications
----------------
Following modifications have been done to support Unity .Net 2.0 Subset compatibility level:
	- Call to ThreadPool.UnsafeQueueUserWorkItem have been replaced by ThreadPool.QueueUserWorkItem

Spicy Pixel Concurrency Kit
===========================
[Download it now in the Unity Asset Store](http://u3d.as/content/spicy-pixel/spicy-pixel-concurrency-kit)

[Get the Source Code on GitHub](https://github.com/spicypixel/concurrency-kit)

The Concurrency Kit is a .NET/Mono kit that includes a port of the [Task Parallel Library](http://msdn.microsoft.com/en-us/library/dd460717.aspx) and extends it to support [Fibers](http://en.wikipedia.org/wiki/Fiber_(computer_science)), [Coroutines](http://en.wikipedia.org/wiki/Coroutine), and [Unity](http://unity3d.com/). Fibers allow code paths to execute concurrently using a single thread by leveraging the co-operative yielding behavior of coroutines.

	// Start task 1
	var t1 = Task.Factory.StartNew(() => PatHead());
	 
	// Start task 2
	var t2 = Task.Factory.StartNew(() => RubTummy());
	 
	// This task will complete when t1 and t2 complete and
	// then it will continue by executing a happy dance.
	Task.WhenAll(t1, t2).ContinueWith(t3 => HappyDance());
	
Because code written in this manner is designed with concurrency in mind, tasks can run in parallel across multiple threads or as concurrent fibers on a single thread by changing out the task scheduler. This flexibility makes it easy to write and maintain portable asynchronous code that scales.

Interoperability
----------------
Use the .NET 4+ **asynchronous task model** in your designs – it’s feature rich and the framework standard going forward

	public class HttpClient {
	  public Task<HttpResponseMessage> GetAsync(
	    string requestUri);
	 
	  public Task<HttpResponseMessage> PostAsync(
	    string requestUri,
	    HttpContent content);
	}

Usability
---------
Start a background task using the thread pool and **complete the operation on the main thread**

Declaratively schedule workflows with **chained asynchronous tasks** and **anonymous delegates**

	Task.Factory.StartNew(() => DoSomethingFromThreadPool()).
	  ContinueWith(lastTask => DoSomethingFromMainThread(), 
	  mainThreadScheduler);

**Coordinate** between concurrently executing tasks

	Task.Factory.ContinueWhenAny(tasksToRun, 
	  winner => print("The winner is: " + winner));

Easily **cancel tasks** in progress

	CancellationTokenSource tokenSource = new CancellationTokenSource();
	void Start()
	{
	  Task.Factory.StartNew(() => DoSomething(), tokenSource.Token);
	}
	void OnClick()
	{
	  tokenSource.Cancel();
	}
	
Performance
-----------
* Leverage **multiple CPU cores** for maximum throughput
* Maximize individual thread usage with **co-operative multitasking** and task inlining
* **Control how tasks are scheduled** and the level of concurrency

Productivity
------------
* Write more **maintainable**, more **performant** asynchronous code

See the [API Reference](http://spicypixel.com/developer/concurrency-kit/api-reference/) for what is included or [learn more about the Concurrency Kit](http://spicypixel.com/developer/concurrency-kit/learn/).

---
Copyright (c) 2012-2014 [Spicy Pixel, Inc.](http://spicypixel.com)