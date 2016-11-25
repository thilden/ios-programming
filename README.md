### Attaching a view controller to window programmatically

When done using StoryBoards, the root view controller is attached to your window automatically. If you are not using StoryBoards, however, you can do the same as follows:

```
func application(_ application: UIApplication,
                 didFinishLaunchingWithOptions launchOptions:
                         [UIApplicationLaunchOptionsKey: Any]?) -> Bool {

        window = UIWindow(frame: UIScreen.main.bounds)
        window?.rootViewController = ViewController()
        window?.makeKeyAndVisible()

        return true
    }
```

### Adding a UIGestureRecognizer to UILabel

To make UILabel respond to gesture recognisers, remember to set its isUserInteractionEnabled property to true:

```
let label = UILabel()
label.isUserInteractionEnabled = true
```

Here's the Swift 3 syntax for setting a gesture recognizer, and definition of the action method (assuming <code>self</code> target):

```
let recognizer = UITapGestureRecognizer(
        target: self, action: #selector(tapped(_:)))
label.addGestureRecognizer(recognizer)

func tapped(_ sender: UITapGestureRecognizer) {
    print("tapped")
}
```
