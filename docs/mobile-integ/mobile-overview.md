---
path: '/mobile-integration'
title: 'Interaction Studio Mobile Integrations'
tags: ['mobile']
---

Interaction Studio [web templates](/campaign-development/web-templates) are based on responsive HTML and can support web campaigns that are displayed on mobile devices. However, these "standard" web templates cannot leverage all of the features and capabilities available to native mobile apps. For companies and developers who want Interaction Studio to have access to native mobile app features and capabilities, Interaction Studio provides an Apple iOS SDK and Android SDK for mobile app integration. 

## Interaction Studio Mobile SDKs
Interaction Studio provides software development kits (SDKs) that developers can use to integrate Interaction Studio with native iOS and native Android mobile applications. The Interaction Studio mobile SDKs gather information about the user, device, and in-app behavior and sends it to Interaction Studio to build profiles, generate statistics, and feed machine learning. The SDKs also handle event responses containing personalization
[campaigns](https://doc.evergage.com/display/EKB/Mobile+Campaigns) for qualified mobile application users.

### Mobile SDK Developer Documentation
Complete instructions, SDK specifications and requirements for mobile app developers are documented in separate documentation websites: 
* **Apple iOS Mobile SDK:** https://developer.evergage.com/mobile-sdk/ios/latest/
* **Android Mobile SDK:** https://developer.evergage.com/mobile-sdk/android/latest/


### Native Mobile SDK Features
* Integration
    * iOS
    	* Direct framework, static or dynamic
    	* CocoaPods
    	* Supports iOS7+ (for now, likely to be iOS8+ soon)
    	* Supports Objective-C and Swift code
    	* Includes module map for Swift
    	* Optional swizzling to reduce integration points
	* Android
		* Maven Central aar
    	* Supports Android 14+ (4.0+)
    	* Supports Java and Kotlin
	* Configurable debug logging
* Tracking/Events
    * Automatic
        * App events (launch, install, upgrade, foreground, background, etc)
        * Misc SDK-handled: campaigns (In-App), view time accumulation, any APIs reduced by swizzling, etc
    * API-driven
        * User info
        * Catalog interactions
        * Custom events
        * Stats for custom app-handled campaigns
* Optional Visual Screen Mapping (iOS-only)
    * When enabled, user navigates through app and screen info and images are uploaded to Interaction Studio web console
    * User can then assign an action name for the screen, based on the sceen info (including hierarchy)
    * SDK receives screen mappings as part of the configuration from server
    * When user navigates to the screen in the app, the SDK automatically detects this and tracks the mapped action
* Real-Time Campaigns
    * In response to events tracked
    * All campaigns support dynamic content based on the user and catalog, including recommendations
    * In-App Message
        * Simple banner that floats over app with optional buttons and click links
        * SDK handles rendering and stats itself
    * Push Notifications
        * Apple Push Notification System and Firebase supported
        * Scheduling, including based on last-known user timezone
    * Data
        * JSON payload to a target/content-zone
        * APIs for stats based on app's handling of data
* Testing
    * Can control testing unpublished campaigns directly from the device
* Anonymous Identifiers
	* iOS: identifierForVendor
	* Android: generated UUID
* Configuration
    * Tiered: Defaults, client-provided, server-provided
    * SDK periodically obtains 'AppConfig' from CDN/server for the app
    * SDK will not send events until it receives a successful configuration
    * Configuration can disable the SDK. Default response for an unknown app disables the SDK.
    * Supports many settings to change SDK behavior.
* Robust Networking
    * Events queued during connectivity issues
    * Automatically re-attempts upon return of connectivity
    * Async non-main threads to keep app UI responsive
    * Detects when response no longer relevant, e.g. screen no longer exists due to user navigation
    * Dynamic headers for routing to user-affine node, etc
* Stability
    * Designed to avoid negatively impacting host app and user experience
    * Async network calls
    * Checkpoints to see if operations are no longer relevant and should be dropped
    * Safety-wrapping processing of dynamic data
    * Cleaning up screen-specific resources/handlers as user/navigation discards the screen

#### Identity Management

The Native Mobile SDKs handle anonymous user identifiers itself (details above). Named user identifiers can be 
provided by the app, to support the merging of profile identities across different devices and channels.
