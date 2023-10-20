# Queries to be run against your instance DB to restore modlog functionality. 

# See: https://github.com/LemmyNet/lemmy-ui/issues/1899

# Problem: Modlogs will freeze and refuse to load, and will eventually return a server error, because the date/time fields are out of range. When a mod bans someone for '999999999' or something similar, lemmy stalls and returns a server error. 

# Queries can be run to fix the problem but, because of the nature of federation, a modlog is easily corrupted again by another '99999999' entry. Thus, it's handy to have some queries ready to go. 

# Solution: Run these queries against your PGSQL DB to restore the modlog.

# Fixes mod_ban_from_community expires column.

UPDATE mod_ban_from_community SET expires = NULL WHERE expires > now() + INTERVAL '10 years';

# Fixes mod_ban expires column.

UPDATE mod_ban SET expires = NULL WHERE expires > now() + INTERVAL '10 years';

# Fixes community_person_ban expires column.

UPDATE community_person_ban SET expires = NULL WHERE expires > now() + INTERVAL '10 years';

