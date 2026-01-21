# id

## What it does
Displays user identity information such as UID, GID, and group memberships.

## Basic syntax
```bash
id
id username
```

## Common usage
```bash
id
id ubuntu
id jethalal
```

## DevOps use cases
- Verifying user and group configuration
- Debugging permission issues
- Confirming group changes after usermod or gpasswd

## Notes / Gotchas
- Shows both primary and secondary groups
- Very useful after changing user or group settings
