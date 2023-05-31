# CoreData
## CoreData stack
### Things to know
1. Every CoreData application has a CoreData stack
2. A CoreData application is useless without a CoreData stack
3. A CoreData stack consists of three objects:
**- a managed object model**
**- a managed object context**
**- a persistant store coordinator**



### 1. Create CoreData model
- create new file (CoreData model, Data Model)
- name it like the app (AppName)
- add Entity (uppercased, singular, e.g. Event)
- add Attributes with data types (camelCased, e.g. startLocation)
- mark what is not optional

### 2. Create Data Controller
- create new file (Swift)
- add import CoreData on top
- create class DataController, conforming to ObservableObject
- make container as NSPersistantContainer(name: "Name of CoreData-Model")
- create an initializer, providing an error message if anything goes wrong

```Swift
import CoreData
import Foundation

class DataController: ObservableObject {
  let container = NSPersistentContainer(name: "AppName")

  init() {
    container.loadPersistentStores { description, error in
      if let error = error {
          print("Core Data failed to load: \(error.localizedDescription)")
      }
    }
  }
}
```

### 3. Create an instance of the DataController and make it available in SwiftUI
- go to AppNameApp.swift
- add property

```Swift
@StateObject private var dataController = DataController()
```

- add an environment-modifier to the Content-View Window Group

```Swift
import SwiftUI

@main
struct AppNameApp: App {
    @State private var dataController = DataController()
    
    var body: some Scene {
        WindowGroup {
            ContentView()
                .environment(\.managedObjectContext, dataController.container.viewContext)
        }
    }
}
```

Tip: If you’re using Xcode’s SwiftUI previews, you should also inject a managed object context into your preview struct for ContentView.

4. Sending and recieving data from CoreData
- receiving data from CoreData is done using a fetch request
