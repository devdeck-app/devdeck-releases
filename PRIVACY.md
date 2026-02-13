# Privacy Policy — DevDeck Browser Companion

**Last updated:** February 13, 2026

## What DevDeck Browser Companion Does

DevDeck Browser Companion is a Chrome extension that connects to the DevDeck server running on your local machine. It enables automatic deck switching based on your active browser tab and browser automation (form filling, element clicking) triggered by user-initiated workflows.

## Data Collected

### Web History (Active Tab Only)

The extension reads the **current active tab's URL and title** when you switch tabs or navigate to a new page. This data is sent to the DevDeck server running on your local machine (`localhost`) for context-aware deck switching.

- Only the currently active tab is read — not your full browsing history
- Data is sent exclusively to your local machine via WebSocket (`localhost:4242`)
- No browsing data is stored persistently by the extension
- Internal browser pages (`chrome://`, `about:`, `edge://`) are filtered out and never sent

### Website Content (Automation Only)

When you trigger a workflow automation, the extension may read:

- **Form field metadata**: element roles, labels, placeholders, and attributes (accessibility tree) for intelligent form filling
- **Element text and attributes**: for data extraction commands

This data is:

- Collected only during user-initiated automation workflows
- Sent exclusively to your local DevDeck server
- Discarded after the automation step completes

### Local Storage

The extension stores a single value in `chrome.storage.local`:

- **Authentication token**: a DevDeck server token you manually enter to authenticate the WebSocket connection. This token never leaves your machine.

## Data NOT Collected

- Personally identifiable information (names, emails, addresses)
- Passwords or user credentials
- Financial or payment information
- Health information
- Location data
- Browsing history beyond the current active tab
- Keystrokes, mouse movements, or scroll behavior
- Analytics or telemetry of any kind

## Data Sharing

**No data is shared with third parties.** All data collected by the extension is sent exclusively to the DevDeck server running on your local machine. No data is transmitted to external servers, analytics services, or any other party.

## Data Security

- All communication occurs over localhost — no data leaves your machine
- The DevDeck server validates that connections originate from localhost
- WebSocket connections are authenticated via user-provided token

## Changes to This Policy

Updates to this policy will be reflected in this document with an updated date. Continued use of the extension after changes constitutes acceptance.

## Contact

For questions about this privacy policy, open an issue at [github.com/devdeck-app/devdeck-releases](https://github.com/devdeck-app/devdeck-releases/issues).
