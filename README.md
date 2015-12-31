# SwiftRefresher

`UIRefreshControl` on UIViewController's UITableView works strange. SwiftRefresher is on of the alternatives of UIRefreshControl.

![refresher.gif](refresher.gif)

##Features

- Simple and easy to use.
- Customize loading view whatever you want.

## Usage

### Basic

Add codes below and add it to UITableView with `srf_addRefresher`. The closure will be called when refresh starts.

```Swift
let refresher = RefresherView { [weak self] () -> Void in
    self?.updateItems()
}
tableView.srf_addRefresher(refresher)
```

And call `srf_endRefreshing()` whenever/wherever your refreshing task finished.

```Swift
s.tableView.srf_endRefreshing()
```

### Use with a little Customize

The view of SwiftRefresher is independent from its main system. The only requirement is to conform to `SwfitRefresherEventReceivable` protocol. Default view is `SimpleRefreshView`. You can use it with a little customization like this below:

```Swift
let refresher = Refresher { [weak self] () -> Void in
    self?.updateItems()
}
refresher.createCustomRefreshView { () -> SwfitRefresherEventReceivable in
    return SimpleRefreshView(activityIndicatorViewStyle: .White)
}
tableView.srf_addRefresher(refresher)
```

In this example, I changed SimpleRefreshView's activityIndicatorViewStyle to .White. But you can customize more with create your own refreh view!

### Customize!!!

Just create a view with conforming to `SwfitRefresherEventReceivable`. `SwfitRefresherEventReceivable` has one required function `func didReceiveEvent(event: SwiftRefresherEvent)`. Through this function, refresher send the view the events. The events are below:

```Swift
public enum SwiftRefresherEvent {
    case Pull(offset: CGPoint, threshold: CGFloat)
    case StartRefreshing
    case EndRefreshing
    case RecoveredToInitialState
}
```

For example, preset SimpleRefreshView has this easy codes.

```Swift
public func didReceiveEvent(event: SwiftRefresherEvent) {
    switch event {
    case .Pull:
        pullingImageView.hidden = false
    case .StartRefreshing:
        pullingImageView.hidden = true
        activityIndicatorView.startAnimating()
    case .EndRefreshing:
        activityIndicatorView.stopAnimating()
    case .RecoveredToInitialState:
        break
    }
}
```

`RecoveredToInitialState` means that after EndRefreshing, the view will go back to initial state. And then the state becamed to the initial, this event will be called.

##Runtime Requirements

- iOS8.1 or later
- Xcode 7.0

## Installation and Setup

### Installing with Carthage

Just add to your Cartfile:

```ogdl
github "morizotter/SwiftRefresher"
```

### Manual Installation

To install SwiftyDrop without a dependency manager, please add all of the files in `/SwiftRefresher` to your Xcode Project.

## Contribution

Please file issues or submit pull requests for anything you’d like to see! We're waiting! :)

## License
SwiftRefresher is released under the MIT license. Go read the LICENSE file for more information.
