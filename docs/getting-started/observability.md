# Observability & Crash Reporting

The SDK provides hooks for integrating with your existing observability stack — Sentry, Crashlytics, Datadog, or any custom provider.

## Crash Reporting

### Sentry

=== "React Native"

    ```tsx
    import * as Sentry from '@sentry/react-native';
    import { Logger, AnalyticsTracker } from '@squad-sports/core';

    // Forward SDK errors to Sentry
    Logger.shared.configure({
      sink: (level, message, context) => {
        if (level === 'error') {
          Sentry.captureMessage(message, { level: 'error', extra: context });
        } else {
          Sentry.addBreadcrumb({ message, level, data: context });
        }
      },
    });

    // Forward analytics to Sentry performance
    AnalyticsTracker.shared.configure({
      customAdapter: (event) => {
        Sentry.addBreadcrumb({
          category: 'squad',
          message: event.name,
          data: event.properties,
        });
      },
    });
    ```

    Also wire the `onError` prop:

    ```tsx
    <SquadExperience
      partnerId="your-id"
      apiKey="your-key"
      onError={(error) => Sentry.captureException(error)}
    />
    ```

=== "iOS"

    ```swift
    import Sentry

    SquadLogger.shared.sink = SentryLogSink()

    class SentryLogSink: SquadLogSink {
        func log(level: SquadLogLevel, message: String, context: [String: Any]) {
            let breadcrumb = Breadcrumb(level: level == .error ? .error : .info, category: "squad")
            breadcrumb.message = message
            breadcrumb.data = context
            SentrySDK.addBreadcrumb(breadcrumb)

            if level == .error {
                SentrySDK.capture(message: message)
            }
        }
    }
    ```

=== "Android"

    ```kotlin
    import io.sentry.Sentry
    import io.sentry.Breadcrumb
    import io.sentry.SentryLevel

    SquadLogger.handler = { level, tag, message ->
        val breadcrumb = Breadcrumb().apply {
            this.category = "squad"
            this.message = "[$tag] $message"
            this.level = when (level) {
                SquadLogLevel.ERROR -> SentryLevel.ERROR
                SquadLogLevel.WARN -> SentryLevel.WARNING
                else -> SentryLevel.INFO
            }
        }
        Sentry.addBreadcrumb(breadcrumb)

        if (level == SquadLogLevel.ERROR) {
            Sentry.captureMessage("[$tag] $message", SentryLevel.ERROR)
        }
    }
    ```

### Firebase Crashlytics

=== "React Native"

    ```tsx
    import crashlytics from '@react-native-firebase/crashlytics';
    import { Logger } from '@squad-sports/core';

    Logger.shared.configure({
      sink: (level, message, context) => {
        crashlytics().log(`[Squad:${level}] ${message}`);
        if (level === 'error') {
          crashlytics().recordError(new Error(message));
        }
      },
    });
    ```

=== "iOS"

    ```swift
    import FirebaseCrashlytics

    SquadLogger.shared.sink = CrashlyticsLogSink()

    class CrashlyticsLogSink: SquadLogSink {
        func log(level: SquadLogLevel, message: String, context: [String: Any]) {
            Crashlytics.crashlytics().log("[Squad:\(level)] \(message)")
            if level == .error {
                Crashlytics.crashlytics().record(error: NSError(
                    domain: "SquadSDK", code: -1,
                    userInfo: [NSLocalizedDescriptionKey: message]
                ))
            }
        }
    }
    ```

=== "Android"

    ```kotlin
    import com.google.firebase.crashlytics.FirebaseCrashlytics

    SquadLogger.handler = { level, tag, message ->
        FirebaseCrashlytics.getInstance().log("[$tag:$level] $message")
        if (level == SquadLogLevel.ERROR) {
            FirebaseCrashlytics.getInstance().recordException(RuntimeException("[$tag] $message"))
        }
    }
    ```

## Datadog / Custom APM

```tsx
import { Logger, AnalyticsTracker } from '@squad-sports/core';

Logger.shared.configure({
  sink: (level, message, context) => {
    datadogRum.addAction('squad_log', { level, message, ...context });
  },
});

AnalyticsTracker.shared.configure({
  customAdapter: (event) => {
    datadogRum.addAction(event.name, event.properties);
  },
});
```

## Request Tracing

Every API response includes an `X-Request-ID` header. When reporting issues to Squad support, include this value along with:

- SDK version (`X-Squad-SDK-Version` header value)
- Timestamp of the issue
- Platform and OS version
- Steps to reproduce

This allows us to correlate your report with our server logs within seconds.

## Debug Mode

Enable verbose logging during development:

=== "React Native"

    ```tsx
    Logger.shared.configure({ minLevel: 'debug' });
    ```

=== "iOS"

    ```swift
    SquadLogger.shared.minLevel = .debug
    ```

=== "Android"

    ```kotlin
    SquadLogger.minLevel = SquadLogLevel.DEBUG
    ```

Debug mode logs all API requests/responses, SSE events, auth state changes, and analytics events. Disable in production builds.
