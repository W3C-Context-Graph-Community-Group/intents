# Ron to-do for Juan

- Provide a list of intents
- Provide predicates
- Documentation
- examples / reference implementations

## Definitions:
*(Ron's language)*
 - *intent*:

```json
{
        "trigger-url": "cgp:/r/triggers/pii-email.md",
        "predicates": [
          { "type": "regex", "value": "\\S+@\\S+\\.\\S+" }
        ],
        "handler": {}
}
```
- *intent-map*:
Description: In a particular moment, and instance, we have this group of intents applied to this html object

```json
{
  "program-intent": {
    "halt": [
      {
        "trigger-url": "cgp:/r/triggers/pii-email.md",
        "predicates": [
          { "type": "regex", "value": "\\S+@\\S+\\.\\S+" }
        ],
        "handler": {}
      }
    ],
    "ask":  [
      {
        "trigger-url": "cgp:/r/triggers/fiber-on-dark-spike.md",
        "predicates": [
          { "type": "fiber-anchored", "facet-empty": "/meaning" }
        ],
        "handler": [
          "cgp:/r/commands/ask-meaning.md"
        ]
      }
    ],
    "act":  [
      {
        "trigger-url": "cgp:/r/triggers/console-log-on-keyup.md",
        "predicates": [
          { "type": "event", "event": "keyup" }
        ],
        "handler": { "console-log": true }
      }
    ]
  },
  "user-intent":     { "halt": [], "ask": [], "act": [] },
  "business-intent": { "halt": [], "ask": [], "act": [] }
}

```
- *intent-registry*:

  Description: The global collection of intents across all systems available to the Global System

## Triggers

A trigger states a predicate (condition) for which to trigger an event (command chain).

```md
<!-- triggers/pii-email.md -->

# pii-email

**Reference URL:** `cgp:/r/triggers/pii-email.md`

Halt-gate trigger that fires when the textarea value contains an email address pattern. Declared under `program-intent/halt` in the textarea intent map.

## Predicates

```json
[{ "type": "regex", "value": "\\S+@\\S+\\.\\S+" }]
```

## Predicate evaluation

- **Textarea handler:** the regex evaluates against `target.value` (the full textarea content) on each act-gate event (keyup). If the halt fires, the spike still mints (Coupling Rule) but act processing is skipped.
- **Drop handler (runtime):** if a drop-side intent map declared this predicate type, the regex would evaluate against the joined header string (`headers.join(',')`), not per-cell. Alpha's drop intent map has an empty halt array, so this path does not fire.

## Effect

When the predicate matches:

1. A spike is emitted on `cgp:/r/events/intent-matched.md` with `gate: halt` in its `/context`.
2. A `cgp:/r/events/halted.md` event is emitted with `{ anchor, trigger, frame }`.
3. Act-gate processing (console.log, spike emission for act) is skipped for that crossing.

The handler has no UI behavior declaration — the halt is silent (console log only). Implementers may add UI feedback (error message, blocked submit) per the spec.


```
