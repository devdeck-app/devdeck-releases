# Privacy Policy — DevDeck iOS App

**Last updated:** February 18, 2026

## What DevDeck Does

DevDeck is an iOS app that turns your iPhone or iPad into a customizable shortcut deck for your Mac. It connects to the DevDeck Server running on your computer via WebSocket to execute commands, launch applications, and run workflows.

## Data Collected

### Connection Information (Stored on Device)

The app stores server connection details so it can reconnect automatically:

- **Server address and port**: stored in UserDefaults on your device
- **Authentication token**: stored securely in the iOS Keychain

This data never leaves your device — it is used solely to establish a connection to your DevDeck server.

### Camera (QR Code Scanning)

The app uses your device camera to scan QR codes for server pairing. Camera access is:

- Used only when you explicitly open the QR scanner
- Not recorded, stored, or transmitted
- Processed entirely on-device

### Local Network (Bonjour / mDNS)

The app uses local network access to auto-discover DevDeck servers on your Wi-Fi network via Bonjour (mDNS). No network traffic leaves your local network during discovery.

## Data NOT Collected

- Personally identifiable information (names, emails, addresses)
- Passwords or user credentials (beyond the optional DevDeck server token)
- Financial or payment information
- Health information
- Location data
- Usage analytics or telemetry
- Crash reports sent to third parties
- Advertising identifiers
- Browsing history
- Contacts, photos, or other device data

## Data Sharing

**No data is shared with third parties.** DevDeck does not transmit any data to external servers, analytics services, advertising networks, or any other party. All communication occurs directly between your device and the DevDeck server you configure.

## Data Security

- Server authentication tokens are stored in the iOS Keychain, protected by the device's hardware security
- WebSocket connections can be authenticated via user-provided token
- All communication stays within your local network unless you explicitly configure remote access (e.g., ngrok)

## Data Retention

Connection data is stored on your device until you disconnect or delete the app. Deleting the app removes all stored data.

## Children's Privacy

DevDeck does not knowingly collect information from children under 13. The app does not collect personal information from any user.

## Changes to This Policy

Updates to this policy will be reflected in this document with an updated date. Continued use of the app after changes constitutes acceptance.

## Contact

For questions about this privacy policy, open an issue at [github.com/devdeck-app/devdeck-releases](https://github.com/devdeck-app/devdeck-releases/issues) or visit [devdeck.app](https://devdeck.app).
