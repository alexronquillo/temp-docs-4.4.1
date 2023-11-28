# Track sessions

The SDK provides automatic session tracking. This works by wrapping the `UIApplicationDelegate` set on `UIApplication.shared` and handling the events relevant for the SDK. If you did set a delegate there already your delegate will still get called as before. If you set another delegate after initializing the SDK you will overwrite the delegate set by the SDK and session tracking will no longer work.
