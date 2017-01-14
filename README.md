### Swift snippets

These are current as of Swift 3.0.1.

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

### Reading and writing values to UserDefaults
Writing a value:
```
let value = 5
UserDefaults.standard.set(value, forKey: "key")
```

Reading a value:
```
let value = UserDefaults.standard.integer(forKey: "key")
```

### File access (create, write, read)
Get a URL to a file in the Documents directory:
```
func getFileURL(filename: String) -> URL? {
    let dir = FileManager.default.urls(for: .documentDirectory, in: .userDomainMask).first!
    let pathStr = "\(dir)/\(filename)"
    return URL.init(string: pathStr)
}
```

Create file if it doesn't exist:
```
guard let fileURL = getFileURL(filename: "text.txt") else { return }

if !FileManager.default.fileExists(atPath: fileURL.path) {
    FileManager.default.createFile(atPath: fileURL.path, contents: nil, attributes: nil)
}
```

Write to a file:
```
let content = "content goes here"
let data = Data(base64Encoded: content)!
try? content.write(to: fileURL, atomically: true, encoding: .utf8)
try? data.write(to: fileURL)
```

Read from a file:
```
let content = try? String(contentsOf: fileURL)
let data = try? Data(contentsOf: fileURL)
```

#### Loading files from the app bundle
This will return an array of Strings (file paths) in the bundle directory:
```
let bundlePath = Bundle.main.resourcePath!
let files = try! FileManager.default.contentsOfDirectory(atPath: bundlePath)
```

Read contents of a text file in the bundle (example.txt) to String:
```
let filePath = Bundle.main.path(forResource: "example", ofType: "txt")
let url = URL(fileURLWithPath: filePath)
let text = try! String(contentsOf: url)
```

### NSKeyedArchiver and NSSKeyedUnarchiver

#### NSKeyedArchiver
let person = Person(name: "Incognito", height: 187)
NSKeyedArchiver.archiveRootObject(person, toFile: "filename")
```

#### NSKeyedUnarchiver
```
let restoredPerson = NSKeyedUnarchiver.unarchiveObject(withFile: "filename")
```

#### NSCoding protocol

The NSCoding protocol requires that conforming classes implement the encode() method and a required initializer, as follows:
```
class Person: NSObject, NSCoding {  // <- NSObject!

    private let name: String
    private let height: Double

    private static let keyName = "name"
    private static let keyHeight = "height"

    func encode(with aCoder: NSCoder) {
        aCoder.encode(name, forKey: Person.keyName)
        aCoder.encode(height, forKey: Person.keyHeight)
    }

    required init?(coder aDecoder: NSCoder) {
        guard let name = aDecoder.decodeObject(forKey: Person.keyName) as? String else { return nil }
        let height = aDecoder.decodeDouble(forKey: Person.keyHeight)

        self.name = name
        self.height = height
    }

}
```

Note that you need to derive from NSObject in addition to implementing the NSCoding protocol. Or you could just use a struct instead.

### Setting a timer
By using a Timer object you can call a selector after a specified delay, and optionally repeat the call. The following will call timerFired() after 10 seconds, and every two seconds after that:
```
func setTimer() {
    let fiveSecondsFromNow = Date().addingTimeInterval(10)
    let timer = Timer(fireAt: fiveSecondsFromNow, interval: 2, target: self, selector: #selector(timerFired), userInfo: nil, repeats: false)
    RunLoop.main.add(timer, forMode: RunLoopMode.commonModes)
}

func timerFired() {
    print("timerFired()")
}
```

### Hide the status bar
In a UIViewController-derived class, override the prefersStatusBarHidden property:
```
override var prefersStatusBarHidden: Bool {
    return true
}
```

### Grand Central Dispatch (GCD)
```
// Default QoS:
DispatchQueue.global().async { [unowned self] in
    // Code to execute
}

// User Initiated QoS:
DispatchQueue.global(qos: .userInitiated).async { [unowned self] in
    // Code to execute
}
```

#### Quality of Service classes
* User Interactive
* User Initiated
* Default
* Utility
* Background

#### performSelector()
Creates a new thread. The `with` parameter indicates an argument passed to the method.
```
performSelector(inBackground: #selector(doStuff), with: nil)

performSelector(onMainThread: #selector(doStuff), with: nil, waitUntilDone: false)
```

### UITableView

#### Deselect a view after selection

```
override func tableView(_ tableView: UITableView, didSelectRowAt indexPath: IndexPath) {
    tableView.deselectRow(at: indexPath, animated: true)
}
```
