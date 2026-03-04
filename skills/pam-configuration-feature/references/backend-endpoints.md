# Backend Endpoints

## Scope

Use this reference when the feature needs a read/write API path in Django.

## Typical Files

- `platform-source/src/django/pam/configuration/configuration_views.py`
- `platform-source/src/django/pam/configuration/configuration_urls.py`
- `platform-source/src/django/pam/initial_entries/constants.py`
- `platform-source/src/django/pam/models.py` or nearby config helpers only if the surrounding pattern requires it

## Standard Flow

1. Add or confirm the config key in `initial_entries/constants.py`.
2. Implement a dedicated getter in `configuration_views.py`.
3. Implement a dedicated change/save handler in `configuration_views.py`.
4. Register the routes in `configuration_urls.py`.
5. Ensure the response shape matches what Ember expects to render.

## Getter Guidance

- Return only the fields needed by the UI.
- Use existing helper functions for reading configuration values if nearby code already uses them.
- Normalize missing values to the same defaults used by the save path.
- Keep field names stable between GET and POST responses.

## Change Handler Guidance

- Validate the request before reading payload values.
- Persist all related config keys together.
- Preserve nearby audit logging, permission checks, and helper usage.
- If the feature toggles schedules or background work, perform those side effects in the same handler path.
- Return a response payload that allows the UI to refresh without guessing derived values.

## Query Style

If nearby code for the same area uses:

- `Serializer`
- `Criteria`
- `JoinTable`
- `QueryConstants`

follow that pattern instead of mixing direct ORM access into the same workflow.

## Common Mistakes

- Adding a POST endpoint without a matching GET endpoint
- Returning different key names from save and fetch handlers
- Saving only the enable flag but not the related interval or mode value
- Updating config values but forgetting URL registration
- Using a helper path that bypasses expected audit or schedule logic
