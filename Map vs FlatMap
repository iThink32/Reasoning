# Why do we use flatMap for asynchronous operations and not map in RxSwift

I have seen many answers on the internet but i find the main point missing.There are two ways to get the answer first is
direct way - "flatMap auto-subscribes to its internal observables so as observable making an async call will go out of 
scope in case of a map as it does not auto-subscribe whereas will remain subscribed in flatMap due to which we can process
the result".
Now if your like me who likes to go in depth of this read on !


```
        let outerObservable = Observable<Int>
            .interval(1, scheduler: MainScheduler.instance)
            .take(3)
        
        let combinedObservable = outerObservable.map { value  in
            return Observable<Int>
                .interval(1, scheduler: MainScheduler.instance)
                .take(3)
                .map { innerValue in
                    print("Outer Value \(value) Inner Value \(innerValue)") // will never be called
                }
               // .subscribe({ (event) in
                 //   print(event)
                //})
            
        }
        
        combinedObservable
            .subscribe({ (event) in
                print(event)
            })
            .disposed(by: disposeBag)
 ```

Consider the example above , both the outer and combined observables will emit events at the same time but the catch is the 
print statement in the combinedObservable will never be called.Only the print in the outer subscription will be called.That 
too only once as map does not auto-subscribe to the observables created within it.But once you manually subscibe by 
uncommenting the above code , print is called normally as in flatMap the only difference being in the case of a flatMap 
you will get a combined Observable that is formed by merging the internal three whereas in a map you will just get
a Observable containing the inner three observables.

If you uncomment the subscribe in the combined observable , both the print statements will be called.This time giving you a 
total of 4 `completed` events , 3 for the inner observables (one for each outer observable) and one for the combined one.

So in short an async call has to be subscribed (to be kept in memory) to receive events in a map whereas it is auto-subscibed
in case of a flatMap due to which it is preferred for async operations.
