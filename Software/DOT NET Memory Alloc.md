# Chapter 1

## Garbage collection
All the GC does is look for allocated objects on the heap that aren't being referenced by anything

![[Pasted image 20221215103943.png]]

GC lists all the root references, move along its ref tree, marks every objects as *living* then cleans up any object that is not found.

## Small Object Heap (SOH)
Here the marked (or live) objects are copied over the space taken by the unmarked objects, overwriting them. This keeps the heap unfragmanted, diasadvantage is copying large chunks of memory around which uses CPU.

## Large Object Heap (LOH)
The LOH is not compacted, it would be too costly to copy large objects, instead, the LOH keeps track of freed and unused memory space and attempts to allocate the the new objects into these spaces. Therefore, the LOH is fragmanted and the gaps between can only be filled with same or smaller sized objects than the gaps.

>"It's worth pointing out that the actual algorithms used to carry out garbage collection are known only to the .NET GC team, but one thing I do know is that they will have used every optimization possible."

## Static Objects
Static methods, properties, variables or events, the runtime creates a global instance of them right after the code referencing them is loaded. These are accessible by all threads, except if you give the the `ThreadStatic` attribute, and are never garbage collected because these are root references in themselves. 

The static object and initialization can be seen. Once loaded the static contructor will execute, creating a static instance of the `Customer` class for the duration of the application domain.
```csharp
public class MyData 
{ 
	public static Customer Client; 
	public static event EventType OnOrdersAdded; 
	static MyData() 
	{ 
		// Initialize Client=new Customer(); 
	} 
}
```

> "Static collections can also be a problem, as the collection itself will act as a root reference, holding all added objects in memory for the lifetime of the app domain."

# Chapter 2
