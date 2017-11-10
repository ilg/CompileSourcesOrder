# CompileSourcesOrder

A situation in which the order of the (Swift) files in the "Compile Sources" build phase affects whether the project will or will not build from a clean state.

The repo is in the won't-build state.  Drag `Foo+Bar.swift` above `Foo.swift` in the "Compile Sources" build phase to make the project build.  Dragging `Foo+Bar.swift` back below `Foo.swift`, the project will still build, but after cleaning, it no longer will build.

## Not just Xcode...

```
$ swiftc main.swift Foo.swift Foo+Bar.swift 
Foo.swift:14:15: error: 'Bar' is not a member type of 'Foo'
extension Foo.Bar {
          ~~~ ^
$ swiftc main.swift Foo+Bar.swift Foo.swift 
$ swiftc main.swift Foo.swift Foo+Bar.swift 
Foo.swift:14:15: error: 'Bar' is not a member type of 'Foo'
extension Foo.Bar {
          ~~~ ^
```

## Reporting

This appears to be the relevant Swift issue:
- https://bugs.swift.org/browse/SR-631
