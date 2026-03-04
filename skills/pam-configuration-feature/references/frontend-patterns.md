# Frontend Patterns

## Scope

Use this reference when the feature touches Ember configuration UI.

## Typical Files

- `platform-source/src/ember/app/components/configuration/general-configuration/general-configuration-list.js`
- `platform-source/src/ember/app/templates/components/configuration/general-configuration/general-configuration-list.hbs`
- `platform-source/src/ember/app/components/configuration/`
- `platform-source/src/ember/app/templates/components/configuration/`

## Standard Flow

1. Load the current config state into the general configuration page or the relevant section component.
2. Render a card or row using the existing configuration UI pattern.
3. Open a focused edit component or modal for changes.
4. Save through the dedicated backend endpoint.
5. Update local state from the server response instead of guessing the persisted result.

## UI Consistency Rules

- Reuse nearby card markup and modal structure before creating a new presentation pattern.
- Keep labels, payload keys, and response keys semantically aligned.
- Prefer explicit tracked state for each editable field.
- Reset modal state correctly on cancel and reopen.
- Reflect backend defaults in the initial UI display.

## Save Behavior

- Use the existing request utility/service used by adjacent configuration components.
- Handle success by updating the card state immediately from the response.
- Surface validation errors in the same style as nearby configuration forms.
- If toggling a feature changes which fields matter, disable or hide dependent controls consistently.

## Common Mistakes

- Updating the save call but not the initial fetch path
- Rendering a card that is never wired into the configuration list
- Using frontend-only field names that diverge from the backend payload
- Not refreshing derived display text after save
- Leaving stale modal state when a previous edit failed validation
