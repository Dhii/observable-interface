# Dhii - Observable Interface
A standard for things that raise events.

## Why
The [`SplSubject`][] is meant to represent an observable object. Unfortunately, there are inherent architectural problems with it, and it is almost undocumented. Dhii imagine this to be the reason for it not being widely used. Nevertheless, the pattern is very advantageous in certain cases. Dhii believe there should be an interop standard for observables which allows easy implementation and consumption.

## What's The Problem with `SplSubject`?
In simple terms, it breaks [SRP][] and [ISP][]. In more depth:

  1. The [`notify()`][`SplSubject#notify()`] method is extra. If observers want to react to events from a subject, then that subject
    is the one to decide when to raise those events. Exposing a `notify()` method allows subject
    _consumers_ to decide when observers get notified. This breaks encapsulation.

  2. The [`detach()`][`SplSubject#detach()`] method is not always applicable. Many consumers would only need to attach
    observers, and never need to detach anything. Furthermore, having to `detach()` a nameless object forces implementations
    use [`SplObjectStorage `][] or similar, which adds complexity. All this puts a lot of unnecessary burden
    on the implementors, even if this is to support functionality that will never be used.
    
### More Detail
For a few years, the authors of this document made a few attempts to understand the architecture behind the Subject/Observer pair. Recently we tried again. We just could not imagine why someone would design this in the way it is documented.  "What were they even thinking?!", we thought.

However, there is one scenario where this would be valid. That is, if the Subject represents a Hook, like the [`WP_Hook`][] class in WordPress. For example, in an event system where events are invoked with a name or code, a Hook could represent a named event in the system, and by calling `notify()` from outside, the dispatcher could notify observers. If this is the intended scenario, then the problems are mostly with naming: the name "Subject" implies that it is what is being observed, and the Observer pattern is different.
    

[`SplSubject`]: https://www.php.net/manual/en/class.splsubject.php
[`SplSubject#notify()`]: https://www.php.net/manual/en/splsubject.notify.php
[`SplSubject#detach()`]: https://www.php.net/manual/en/splsubject.detach.php
[`SplObjectStorage `]: http://ir2.php.net/manual/en/class.splobjectstorage.php
[`WP_Hook`]: https://github.com/WordPress/WordPress/blob/master/wp-includes/class-wp-hook.php
[SRP]: https://en.wikipedia.org/wiki/Single_responsibility_principle
[ISP]: https://en.wikipedia.org/wiki/Interface_segregation_principle
