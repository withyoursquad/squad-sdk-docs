# Frequently Asked Questions

## General Questions

### What platforms does the Squad SDK support?

The Squad SDK currently supports iOS and Android platforms, with React Native support coming soon. Each platform has its own specific SDK version optimized for that environment.

### What are the minimum platform requirements?

- iOS: iOS 13.0 or later, Xcode 13.0+
- Android: API Level 21 (Android 5.0) or higher, Android Studio Arctic Fox or newer

### How do I get API credentials?

1. Create a developer account at [developer.squadforsports.com](https://developer.squadforsports.com)
2. Navigate to Organization Settings
3. Generate your organization ID and API key

### Is the SDK free to use?

Contact our [sales team](mailto:sales@squadforsports.com) for pricing information and to discuss your specific needs.

## Integration Questions

### How long does integration typically take?

Basic integration can be completed in 1-2 days. More complex integrations with custom features may take longer depending on requirements.

### Can I customize the UI?

The Squad SDK provides a standard WebView interface that maintains consistency across apps. Limited customization options are available through configuration parameters.

### How do I handle deep linking?

The SDK supports deep linking through platform-specific implementations. Refer to our [iOS](../ios/deep-linking.md) or [Android](../android/deep-linking.md) deep linking guides.

### Can I use the SDK in multiple apps?

Yes, you can use the SDK in multiple apps under the same organization. Each app should use its own API credentials.

## Technical Questions

### How does authentication work?

The SDK supports two authentication methods:

1. Email-based authentication
2. Token-based authentication

Refer to our [Authentication Guide](../user-auth.md) for details.

### What happens if the connection is lost?

The SDK automatically handles connection loss and attempts to reconnect. You can configure retry policies and implement custom error handling.

### How do I handle background state?

The SDK provides methods to handle background states:

- iOS: Background modes and state restoration
- Android: Service lifecycle and process management

### What about memory management?

The SDK includes built-in memory management features:

- Automatic cache clearing
- Memory warning handlers
- Resource cleanup utilities

## Voice Calling Questions

### What audio codecs are supported?

The SDK uses Opus codec for optimal voice quality and supports various bitrates and modes.

### How does the SDK handle poor network conditions?

The SDK includes adaptive bitrate handling and automatic quality adjustment based on network conditions.

### Can users send emojis during calls?

Yes, the SDK supports custom emojis and reactions during voice calls.

## Security Questions

### How is user data protected?

- End-to-end encryption for voice calls
- Secure storage for user data
- Certificate pinning for network requests
- Compliance with data protection regulations

### Does the SDK support certificate pinning?

Yes, certificate pinning is supported and recommended for production environments. See our [Security Guide](../guides/security.md).

## Performance Questions

### What is the impact on app size?

Approximate size impact:

- iOS: 2-3 MB
- Android: 3-4 MB

### How is battery usage optimized?

The SDK includes various optimizations:

- Efficient audio processing
- Smart background handling
- Resource management
- Power-aware features

## Troubleshooting

### Where can I find error logs?

- iOS: Console.app or Xcode console
- Android: Logcat with tag "SquadSDK"

### Common error codes:

- 1001: Authentication failed
- 1002: Network error
- 1003: WebView error
- 1004: Permission denied

For detailed error handling, see our [Troubleshooting Guide](../troubleshooting.md).

## Updates and Maintenance

### How often is the SDK updated?

We release updates monthly with bug fixes and quarterly with feature updates.

### How do I update the SDK?

- iOS: Update through SPM or CocoaPods
- Android: Update Gradle dependency

### Is there a changelog?

Yes, check our [Release Notes](https://github.com/withyoursquad/squad-sdk/releases) for detailed changelog.

## Support

### How do I get help?

1. Check our [documentation](https://docs.squadforsports.com)
2. Visit our [support portal](https://support.squadforsports.com)
3. Contact [support@squadforsports.com](mailto:support@squadforsports.com)

### What information should I provide when seeking support?

- SDK version
- Platform details
- Error logs
- Steps to reproduce
- Sample code (if applicable)

## Additional Resources

- [Documentation Home](../index.md)
- [Sample Projects](https://github.com/withyoursquad/samples)
- [Support Contact](support.md)
