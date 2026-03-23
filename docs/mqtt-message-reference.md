# MQTT message reference

## Access message format when opening a door

**JSON schema:**

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "AccessEvent",
  "description": "Schema for an RFID access control event",
  "type": "object",
  "required": [
    "timestamp",
    "entrypoint_ip",
    "entrypoint_location",
    "transponder_uid",
    "user_id",
    "user_dn",
    "user_display_name",
    "status",
    "notification"
  ],
  "properties": {
    "timestamp": {
      "type": "string",
      "format": "date-time",
      "description": "ISO 8601 timestamp of when the access event occurred"
    },
    "entrypoint_ip": {
      "type": "string",
      "format": "ipv4",
      "description": "IP address of the RFID reader"
    },
    "entrypoint_location": {
      "type": "string",
      "minLength": 1,
      "maxLength": 200,
      "description": "Human-readable name of the entry point location"
    },
    "transponder_uid": {
      "type": "string",
      "pattern": "^[A-Fa-f0-9]{8,20}$",
      "description": "RFID transponder UID as a hexadecimal string (8–20 hex characters)"
    },
    "user_id": {
      "type": "string",
      "minLength": 1,
      "description": "Unique identifier of the user"
    },
    "user_dn": {
      "type": "string",
      "pattern": "^(UID|uid)=.+",
      "description": "LDAP distinguished name of the user object (e.g. UID=john.doe@example.com,OU=Users,DC=example,DC=com)"
    },
    "user_display_name": {
      "type": "string",
      "minLength": 1,
      "maxLength": 200,
      "description": "Human-readable display name of the user"
    },
    "status": {
      "type": "string",
      "enum": ["granted", "denied"],
      "description": "Whether access was granted or denied"
    },
    "notification": {
      "type": "boolean",
      "description": "Whether a notification should be sent for this event"
    }
  },
  "additionalProperties": false
}
```

**Description:**

* `entrypoint_ip` uses "`format`": "`ipv4`".
* `transponder_uid` uses a hex pattern (`^[A-Fa-f0-9]{8,20}$`) which covers common RFID UID lengths (4, 7, and 10 bytes represented as hex).
* `user_dn` has a loose pattern requiring it to start with `UID=` or `uid=`.
* `user_id` is kept as a plain string since the ID format wasn't specified.
* `notification` is a strict boolean, so string values like "true" would be rejected.

**Examples with placeholders:**

```json
{
  "timestamp": "<timestamp>",
  "entrypoint_ip": "<IP address of the RFID reader>",
  "entrypoint_location": "<Name of the entry point>",
  "transponder_uid": "<RFID transponder UID>",
  "user_id": "<user ID>",
  "user_dn": "<LDAP destinguished name of the user object>",
  "user_display_name": "<Name of the user>",
  "status": "<granted|denied>",
  "notification": true|false
}
```

## ACS Server status

**JSON schema:**

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "StatusEvent",
  "description": "Schema for a status event log entry",
  "type": "object",
  "required": ["timestamp", "severity", "status", "description"],
  "properties": {
    "timestamp": {
      "type": "string",
      "format": "date-time",
      "description": "ISO 8601 timestamp of when the event occurred"
    },
    "severity": {
      "type": "string",
      "enum": ["info", "warning", "error"],
      "description": "Severity level of the event"
    },
    "status": {
      "type": "string",
      "minLength": 1,
      "maxLength": 100,
      "description": "Short description of the status"
    },
    "description": {
      "type": "string",
      "minLength": 1,
      "description": "Detailed status message"
    }
  },
  "additionalProperties": false
}
```

**Description:**

* `timestamp` uses "format": "date-time" to enforce ISO 8601 (e.g. 2026-03-23T10:15:00Z).
* `severity` uses an enum to restrict values strictly to `info`, `warning`, and `error`.
* `status` has a maxLength: 100 to enforce the "short description" intent.
* `additionalProperties`: `false` rejects any keys not defined in the schema, keeping payloads clean.

**Example with placeholders:**

```json
{
  "timestamp": "<timestamp>",
  "severity": "<info|warning|error>",
  "status": "<status short description>",
  "description": "<status message>"
}
```
