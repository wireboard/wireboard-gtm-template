# WireBoard for Google Tag Manager

Official Google Tag Manager template for [WireBoard](https://wireboard.io) analytics.

Install WireBoard on any site through Google Tag Manager with no code changes. The template handles pageview tracking, custom events, and one-click automatic capture of any event you already push to `window.dataLayer` (GA4-compatible).

## Installing the template

### From the Community Template Gallery (recommended)

1. Open your Google Tag Manager workspace.
2. Go to **Templates** and click **Discover more tag types in the Community Template Gallery**.
3. Search for **WireBoard** and click **Add to workspace**.
4. Create a new tag of type **WireBoard**.

### Manual import (advanced)

If you cannot use the gallery, you can import the template file directly:

1. Download [`template.tpl`](./template.tpl) from this repository.
2. In Google Tag Manager, open **Templates** and click **New** in the **Tag Templates** section.
3. Open the menu (three vertical dots) in the top right of the Template Editor and choose **Import**.
4. Select the downloaded `template.tpl` file.

## Using the template

The template has two modes, selected by the **Tag Type** field on each tag.

### Initialization

Loads WireBoard on every page and starts tracking. Create one tag of this type and fire it on the **Initialization - All Pages** trigger.

| Field | Required | Notes |
| --- | --- | --- |
| Site ID | Yes | Copy from your WireBoard site settings. |
| Publisher ID | Yes | Copy from your WireBoard site settings. |
| Collector URL | Yes | Defaults to `pipeline-0.collector.wireboard.io`. Change only if directed by WireBoard support or self-hosting. |
| Tracking mode | No | `Cookie` (default) or `Cookieless`. |
| Auto-capture from dataLayer | No (default ON) | Automatically captures GA4-style events you push to `window.dataLayer`. |
| Track engagement | No (default ON) | Enables WireBoard's activity heartbeat. |

### Custom Event

Fires a single WireBoard custom event when the tag's trigger runs. Use this when you want to send events from a GTM tag rather than from `window.dataLayer`.

| Field | Required | Notes |
| --- | --- | --- |
| Category | Yes | e.g. `ecommerce`, `engagement`. |
| Action | Yes | e.g. `purchase`, `sign_up`. |
| Label | No | Optional identifier for the event. |
| Value | No | Numeric value, e.g. a price. |
| Custom properties | No | Key/value table. Up to 30 entries. |

Publisher ID is inherited from the Initialization tag automatically. No need to repeat it on each Custom Event tag.

## How auto-capture works

When **Auto-capture from dataLayer** is enabled on the Initialization tag, WireBoard listens to `window.dataLayer` and forwards relevant events automatically:

- **GA4 events** (`purchase`, `add_to_cart`, `view_item`, `sign_up`, `login`, `search`, `video_progress`, `file_download`, `form_submit`, and ~20 more) are mapped to clean WireBoard events with category, action, label, value, and properties.
- **Legacy Universal Analytics events** (`{event: 'gaEvent', eventCategory, eventAction, ...}`) are passed through.
- **Unknown events** are captured as `{category: 'custom', action: '<event-name>', props: {...}}`.
- **GTM internals** (`gtm.js`, `gtm.click`, `gtm.formSubmit`, etc.) are skipped.
- **Pageview events** are skipped because the WireBoard tracker already records pageviews directly.
- **PII keys** (`email`, `phone`, `first_name`, `address`, `credit_card`, etc.) are stripped before events are sent.

Sites that already push GA4-style events to `dataLayer` get full WireBoard coverage with zero per-event setup.

## Permissions

The template declares the following GTM sandbox permissions:

- `injectScript` for `https://static.wireboard.io/wireboard.js` and `https://static.wireboard.io/events.min.js`.
- `callInWindow` for `wireboard`, `wireboardEvent`, and `wireboardSetPublisher`.

No access to `dataLayer` is requested from within the template. The dataLayer listener is implemented in WireBoard's `events.min.js` and is activated via the `enableDataLayerListener` command.

## Documentation

- [WireBoard documentation](https://wireboard.io/docs)
- [WireBoard custom events](https://wireboard.io/docs/custom-events)
- [WireBoard cookies and cookieless mode](https://wireboard.io/docs/cookies)

## Support

Open an issue on this repository for template-specific bugs. For account or installation questions, contact [info@wireboard.io](mailto:info@wireboard.io).

## License

Apache 2.0. See [LICENSE](./LICENSE).
