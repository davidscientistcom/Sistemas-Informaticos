### Obtener los usuarios del sistema.

```bash
awk -F: '$7 !~ /(false|nologin)/ {print $1}' /etc/passwd
```