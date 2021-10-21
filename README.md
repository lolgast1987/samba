Copied to files from dperson/samba. His x64 image isn't updated for months at the moment, which, with all Samba security fixes going on wasn't giving me the best of feelings. So here is an updates one, with one major change. This image ONLY supports SMB3 and higher.

Base: alpine:latest \
Samba: 4.14.8-r0

Uses the same environment variables as the image made by dperson. In fact, you can just swap in this image and everything still works. Since this image only supports SMB3+, the SMB environment variable is stripped.

## Configuration

    sudo docker run -it --rm dperson/samba -h
    Usage: samba.sh [-opt] [command]
    Options (fields in '[]' are optional, '<>' are required):
        -h          This help
        -c "<from:to>" setup character mapping for file/directory names
                    required arg: "<from:to>" character mappings separated by ','
        -G "<section;parameter>" Provide generic section option for smb.conf
                    required arg: "<section>" - IE: "share"
                    required arg: "<parameter>" - IE: "log level = 2"
        -g "<parameter>" Provide global option for smb.conf
                    required arg: "<parameter>" - IE: "log level = 2"
        -i "<path>" Import smbpassword
                    required arg: "<path>" - full file path in container
        -n          Start the 'nmbd' daemon to advertise the shares
        -p          Set ownership and permissions on the shares
        -r          Disable recycle bin for shares
        -s "<name;/path>[;browse;readonly;guest;users;admins;writelist;comment]"
                    Configure a share
                    required arg: "<name>;</path>"
                    <name> is how it's called for clients
                    <path> path to share
                    NOTE: for the default values, just leave blank
                    [browsable] default:'yes' or 'no'
                    [readonly] default:'yes' or 'no'
                    [guest] allowed default:'yes' or 'no'
                    NOTE: for user lists below, usernames are separated by ','
                    [users] allowed default:'all' or list of allowed users
                    [admins] allowed default:'none' or list of admin users
                    [writelist] list of users that can write to a RO share
                    [comment] description of share
        -u "<username;password>[;ID;group;GID]"       Add a user
                    required arg: "<username>;<passwd>"
                    <username> for user
                    <password> for user
                    [ID] for user
                    [group] for user
                    [GID] for group
        -w "<workgroup>"       Configure the workgroup (domain) samba should use
                    required arg: "<workgroup>"
                    <workgroup> for samba
        -W          Allow access wide symbolic links
        -I          Add an include option at the end of the smb.conf
                    required arg: "<include file path>"
                    <include file path> in the container, e.g. a bind mount

    The 'command' (if provided and valid) will be run instead of samba

**Environment Variables**
 * `CHARMAP` - As above, configure character mapping
 * `GENERIC` - As above, configure a generic section option (See NOTE3 below)
 * `GLOBAL` - As above, configure a global option (See NOTE3 below)
 * `IMPORT` - As above, import a smbpassword file
 * `NMBD` - As above, enable nmbd
 * `PERMISSIONS` - As above, set file permissions on all shares
 * `RECYCLE` - As above, disable recycle bin
 * `SHARE` - As above, setup a share (See NOTE3 below)
 * `TZ` - Set a timezone, IE `EST5EDT`
 * `USER` - As above, setup a user (See NOTE3 below)
 * `WIDELINKS` - As above, allow access wide symbolic links
 * `WORKGROUP` - As above, set workgroup
 * `USERID` - Set the UID for the samba server's default user (smbuser)
 * `GROUPID` - Set the GID for the samba server's default user (smbuser)
 * `INCLUDE` - As above, add a smb.conf include
 
**Ports**\
137/udp 138/udp 139 445
