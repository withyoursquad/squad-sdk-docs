# Push Notifications

The SDK includes push notification handlers for all platforms. You wire your app's notification service to the Squad handler, and the SDK routes notifications to the correct screen.

## Notification Types

| Type | Description | Navigation |
|------|-------------|------------|
| `message` | New message received | Messaging screen |
| `squad_invite` | Squad invite received | Home feed |
| `incoming_call` | Incoming voice call | Call overlay |
| `poll` | New poll available | Poll screen |
| `freestyle` | New freestyle in feed | Home feed |
| `reaction` | Reaction on your content | Home feed |

## Setup

### Step 1: Configure your notification service

=== "React Native (Expo)"

    ```tsx
    import * as Notifications from 'expo-notifications';
    import { PushNotificationHandler } from '@squad-sports/react-native';

    Notifications.addNotificationResponseReceivedListener(response => {
      const data = response.notification.request.content.data;
      const action = PushNotificationHandler.handleNotification(data);
      if (action) {
        // Navigate to the Squad screen
        navigation.navigate(action.screen, action.params);
      }
    });
    ```

=== "iOS"

    ```swift
    import UserNotifications
    import SquadSportsSDK

    class AppDelegate: UIResponder, UIApplicationDelegate, UNUserNotificationCenterDelegate {

        func userNotificationCenter(
            _ center: UNUserNotificationCenter,
            didReceive response: UNNotificationResponse
        ) async {
            let userInfo = response.notification.request.content.userInfo
            if let action = SquadPushHandler.handleNotification(userInfo: userInfo) {
                SquadSDK.shared?.router?.push(action.route)
            }
        }
    }
    ```

=== "Android"

    ```kotlin
    import com.google.firebase.messaging.FirebaseMessagingService
    import com.google.firebase.messaging.RemoteMessage
    import com.squadsports.sdk.realtime.SquadPushHandler

    class MyFirebaseService : FirebaseMessagingService() {
        override fun onMessageReceived(message: RemoteMessage) {
            // Let Squad handle call notifications
            if (SquadSportsSDK.shared.squadLine.handleMessage(message.data)) return

            // Route other notifications
            val route = SquadPushHandler.handleNotification(message.data)
            if (route != null) {
                // Start activity with route, or store for next app open
            }
        }
    }
    ```

### Step 2: Register device token

The simplest approach is to pass the push token directly in `setup()`:

=== "iOS"

    ```swift
    try await SquadSDK.setup(
        partnerId: "acme-sports",
        apiKey: "sqk_live_...",
        pushToken: deviceTokenString
    )
    ```

=== "Android"

    ```kotlin
    SquadSportsSDK.setup(
        context = this,
        partnerId = "acme-sports",
        apiKey = "sqk_live_...",
        pushToken = FirebaseMessaging.getInstance().token.await(),
    )
    ```

Alternatively, you can register the token at any time after setup via `updateDeviceInfo()`:

=== "iOS"

    ```swift
    func application(_ application: UIApplication,
                     didRegisterForRemoteNotificationsWithDeviceToken deviceToken: Data) {
        let token = deviceToken.map { String(format: "%02.2hhx", $0) }.joined()
        SquadSDK.shared?.apiClient.updateDeviceInfo(token: token, platform: "ios")
    }
    ```

=== "Android"

    ```kotlin
    override fun onNewToken(token: String) {
        SquadSportsSDK.shared?.apiClient?.updateDeviceInfo(token, "android")
    }
    ```

### Step 3: Request permission

=== "iOS"

    ```swift
    UNUserNotificationCenter.current().requestAuthorization(options: [.alert, .sound, .badge]) { granted, _ in
        if granted {
            DispatchQueue.main.async { UIApplication.shared.registerForRemoteNotifications() }
        }
    }
    ```

=== "Android"

    Android 13+ requires runtime permission:
    ```kotlin
    if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.TIRAMISU) {
        requestPermissions(arrayOf(Manifest.permission.POST_NOTIFICATIONS), 0)
    }
    ```

## Payload Format

Push notifications from Squad use this payload structure:

```json
{
  "type": "message",
  "title": "New message from Alex",
  "body": "Hey, are you going to the game?",
  "data": {
    "connectionId": "conn-123",
    "senderId": "user-456"
  }
}
```

The `type` field determines which Squad screen to navigate to. The `data` field contains context-specific parameters.
