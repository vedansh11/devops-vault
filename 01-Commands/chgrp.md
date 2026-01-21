# chgrp

## What it does
Changes the group ownership of a file or directory.

## Basic syntax
```bash
chgrp group file
```

## Common usage
```bash
chgrp devops app.log
chgrp -R devops /var/www
```

## Important flags
- -R → change group ownership recursively

## DevOps use cases
- Fixing permission issues for shared directories
- Managing group-based access

## Notes / Gotchas
- User must belong to the target group
- Often used with chown and chmod
