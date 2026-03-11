# Validation Checklist

1. Constants are added in `initial_entries/constants.py`.
2. Matching activity definitions exist in `initial_entries/activities.py` for both categories when required.
3. Every new audit call references an existing constant.
4. No try/except fallback is hiding missing ActivityTypes registration.
5. `register_user_activity` and `register_account_activity` payloads include required context fields.
6. `python -m py_compile` passes for all modified Python files.
7. Manual action test confirms audit rows exist in both logs.
