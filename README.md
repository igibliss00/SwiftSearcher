# SwiftSearcher

An iOS app that demonstrates the use case of Dynamic Type, SafariServices, and Core Spotlight.

<img src="https://github.com/igibliss00/SwiftSearcher/blob/master/README_assets/2.png" width="400">

## Features

### Dynamic Type

Dynamic Type is when you allow the user to set the font size of your app according to their preference.  While having a fixed font size for your app ensures that the visual layout of the app stays as you intended, it might affect the readability for the users who prefer the font size to be larger so that it’s easier to read or smaller to fit more content within their screen.  More importantly, Dynamic Type provides more consistent user experience of your app in the context of the rest of your phone experience.  This is because the font size set by Dynamic Type is in accordance with the settings of the operating system, not standalone settings just for your app.

To be more precise, Dynamic Type isn’t just about the size of the font. It’s the weight of the font, the font style, as well as the leading points.

<img src="https://github.com/igibliss00/SwiftSearcher/blob/master/README_assets/1.png" height="400">

([Source](https://developer.apple.com/design/human-interface-guidelines/ios/visual-design/typography/))

This means if you were to use Dynamic Type for your app, regardless of the font settings that the user imposes themselves, the font size, style, weight, and leading points are going to be exactly the same as other apps using Dynamic Type, providing a unified experience.  For example, let’s say you use the “Title 1” style for your title and “Body” for the content, you’ll have the same Regular weight, 28 size for the title, 17 for the body, and 34 leading points for the title and 22 for the body, and SF for the font style as other apps using Dynamic Type.  The amount of increase or decrease in size according to the user’s preference is going to be the same for all apps using Dynamic Type. 

It’s possible to use Dynamic Type with the custom font, but all the different sizes and weight have to be provided as well.

```
func makeAttributedString(title: String, subtitle: String) -> NSAttributedString {
    let titleAttributes = [NSAttributedString.Key.font: UIFont.preferredFont(forTextStyle: .headline), NSAttributedString.Key.foregroundColor: UIColor.purple]
    let subtitleAttributes = [NSAttributedString.Key.font: UIFont.preferredFont(forTextStyle: .subheadline)]
    
    let titleString = NSMutableAttributedString(string: "\(title)\n", attributes: titleAttributes)
    let subtitleString = NSMutableAttributedString(string: "\(subtitle)\n", attributes: subtitleAttributes)
    
    titleString.append(subtitleString)
    
    return titleString
}
```

To sum up, Dynamic Type is beneficial in two things:

1. Consistent experience for the user even, especially they change the font size. (If they don’t care to change the font size, they are going to be similar for all apps anyways.)
2. Conducive to the readability.


### SafariServices

SafariServices is similar to WKWebView except it provides an interface that’s similar to Safari which users are more familiar with than the custom web views. SafariServices also allows you to utilize the Safari capabilities like the extension or adding to the reading list. The form auto-complete or the sharing cookies is also possible.  The seamless transition to and fro the Safari web view from your app makes this framework such a powerful tool.  

<img src="https://github.com/igibliss00/SwiftSearcher/blob/master/README_assets/3.png" width="400">

```
func showTutorial(_ which: Int) {
    if let url = URL(string: "https://www.hackingwithswift.com/read/\(which + 1)") {
        let config = SFSafariViewController.Configuration()
        config.entersReaderIfAvailable = true
        
        let vc = SFSafariViewController(url: url, configuration: config)
        present(vc, animated: true)
    }
}
```

### Core Spotlight

Core Spotlight framework allows the user to search for the items and the activities within your app from outside your app which makes your app all the more accessible.  The key point to remember is that Apple monitors the interactions between your app’s spotlight search result and the user and if they deem the results to be unhelpful, then iOS may stop serving up the indexed results on on Core Spotlight. 

<img src="https://github.com/igibliss00/SwiftSearcher/blob/master/README_assets/4.png" width="400">

```
func application(_ application: UIApplication, continue userActivity: NSUserActivity, restorationHandler: @escaping ([UIUserActivityRestoring]?) -> Void) -> Bool {
    if userActivity.activityType == CSSearchableItemActionType {
        if let uniqueIdentifier = userActivity.userInfo?[CSSearchableItemActivityIdentifier] as? String {
            if let navigationController = window?.rootViewController as? UINavigationController {
                if let viewController = navigationController.topViewController as? ViewController {
                    viewController.showTutorial(Int(uniqueIdentifier)!)
                }
            }
        }
    }

    return true
}
```
