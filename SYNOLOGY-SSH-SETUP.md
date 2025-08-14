# Synology NAS SSH Setup

## Status
✅ **Unified SSH key deployed successfully**

## Security Configuration on Synology (Latest DSM)

### Auto Block Settings (Recommended)
Since the latest DSM doesn't offer "key-only" authentication, use Auto Block for security:

1. Log into DSM (http://192.168.1.10:5000)
2. Go to **Control Panel** → **Security** → **Protection**
3. Enable **Auto Block**
4. Configure:
   - **✅ Current Setting**: 10 attempts within 5 minutes
   - **Block duration**: 1 day
   - This prevents brute force attacks while keeping emergency access

### Why This Works Well
- SSH key authentication is tried first (instant access for you)
- Password remains as emergency backup
- Auto Block prevents brute force attacks
- After 10 failed attempts, IP is blocked for 24 hours
- Your key-based access is never affected

## Test Connection
```bash
# Test with shortcut (key-only)
ssh nas hostname

# Test explicitly without password
ssh -o PasswordAuthentication=no nas hostname

# Should fail if password auth is disabled
ssh -o PubkeyAuthentication=no nas hostname
```

## Quick Info
- **IP**: 192.168.1.10
- **User**: joseph
- **Shortcuts**: `ssh nas` or `ssh synology`
- **Model**: DS1520+ (Gemini Lake)
- **DSM Version**: 7.x

## Backup Access
If you lock yourself out:
1. Re-enable SSH with password in DSM web interface
2. Or use DSM web terminal
3. Physical access to NAS for password reset if needed

## Important Notes
- Synology may re-enable password auth after DSM updates
- Check after major DSM updates
- Keep unified key backed up in Bitwarden
- DSM user 'joseph' must be in administrators group for sudo