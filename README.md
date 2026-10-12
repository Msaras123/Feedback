# Feedback REST API

A Java 17 / Spring Boot REST API that accepts feedback fields and validates them against rules configured in `src/main/resources/application.yml`.

## Run

```bash
mvn spring-boot:run
```

The service uses an in-memory store: submitted feedback is cleared when the application restarts.

## API

### Submit feedback

`POST /api/feedback`

```json
{
  "fields": {
    "name": "Avery",
    "email": "avery@example.com",
    "rating": 5,
    "comments": "Very helpful",
    "category": "SERVICE"
  }
}
```

A successful request returns `201 Created` with a generated ID, the submitted fields, and a creation timestamp.

### List feedback

`GET /api/feedback`

Returns all feedback accepted since the current application process started.

Invalid requests return `400 Bad Request` with a `fieldErrors` object keyed by field name. Unknown fields are rejected.

## Dynamic validation rules

Configure each accepted field under `feedback.validation.fields` in `application.yml`. Supported types are `STRING`, `INTEGER`, `NUMBER`, and `BOOLEAN`. Rules can include:

- `required`
- `min-length` and `max-length` for strings
- `min` and `max` for numeric values
- `pattern` for full-string regular-expression matching
- `allowed-values` for an allowlist

For example:

```yaml
feedback:
  validation:
    fields:
      satisfaction:
        type: INTEGER
        required: true
        min: 1
        max: 10
```

Run tests with `mvn test`.
