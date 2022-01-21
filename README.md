### Timer Basic

```swift
import UIKit
import Combine

let runLoop = RunLoop.main

/// tolerance:  tolerance simply means that if the timer is late within a certeain amount of time, it impacts energy usage
let subscription = runLoop.schedule(after: runLoop.now,
                                    interval: .seconds(2),
                                    tolerance: .microseconds(100)) {
    print("Timer fired")
}

runLoop.schedule(after: .init(Date(timeIntervalSince1970: 3.0))) {
    subscription.cancel()
}
```

### Interactive Timer

```swift
import UIKit
import Combine

let subscription =  Timer.publish(every: 1.0, on: .main, in: .common)
    .autoconnect()
    .scan(0, { counter, _ in
        counter + 1
    })
    .sink {
        print($0)
    }
```

### dispatchqueue

```swift
import UIKit
import Combine

let queue = DispatchQueue.main

let source = PassthroughSubject<Int, Never>()

var counter = 0

let cancellable = queue.schedule(after: queue.now, interval: .seconds(1)) {
    source.send(counter)
    counter += 1
}

let subscription = source.sink {
    print($0)
}
```
