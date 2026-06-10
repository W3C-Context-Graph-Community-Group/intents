# Ron to-do for Juan

- Provide a list of intents
- Provide predicates
- Documentation
- examples / reference implementations

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
