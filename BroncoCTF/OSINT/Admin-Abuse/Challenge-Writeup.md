# CTF-Writeup: Admin Abuse

**Category**: OSINT

**Flag**: bronco{wh0_g4v3_th15_m4n_3d1t_pr1v1l3g35}
______________________________________________________________________________________________________________________________________________________________________
# Challenge Overview

Some administrator has been going around doing some weird things lately. All they left me were these two cryptic clues. Allegedly, if you follow their trail, they'll lead you to a flag. What gives?

1160888390661714032

<t:1739660340:R>
____________________________________________________________________________________________________________________________________________
## Initial Analysis

Two clues were given, both of which turned out to be **Discord metadata artifacts** rather than anything geographic or otherwise obscure:

### Clue 1 — `1160888390661714032`

This is a 19-digit integer in the exact range of a **Discord snowflake ID** (Discord's 64-bit unique identifier format used for users, messages, and channels). Snowflakes encode a creation timestamp in their upper bits, computed as:

```python
sid = 1160888390661714032
discord_epoch = 1420070400000  # Jan 1, 2015, Discord's custom epoch
ts_ms = (sid >> 22) + discord_epoch

import datetime
print(datetime.datetime.utcfromtimestamp(ts_ms / 1000))
# -> 2023-10-09 10:36:00 UTC
```

### Clue 2 — `<t:1739660340:R>`

This is **literally Discord's native timestamp markdown syntax** — `<t:UNIX_EPOCH:FORMAT>` — which Discord's client renders as a dynamic timestamp (e.g. "3 months ago") in any message. The `R` flag specifically means "relative" format.

```python
import datetime
print(datetime.datetime.utcfromtimestamp(1739660340))
# -> 2025-02-15 22:59:00 UTC (~Feb 16, depending on local timezone display)
```

Given the challenge references an "administrator" and mentions a trail to follow, and BroncoCTF runs an active Discord server (confirmed via the separate "Become a Discordian" challenge), both clues pointed toward **investigating the BroncoCTF Discord server** rather than any external tool.
________________________________________________________________________________________________________________________________________

## OSINT Trail

1. **Joined the BroncoCTF Discord** and enabled Developer Mode (User Settings → Advanced → Developer Mode) to allow copying/searching raw Discord IDs.

2. **Searched the raw snowflake ID** (`1160888390661714032`) directly in the server's search bar. This surfaced **21 results** — multiple messages across `#public-chat` and `#osint` referencing or mentioning the same ID, confirming it was a real, relevant account.
<p align="center">
  <img width="526" height="416" alt="image" src="https://github.com/user-attachments/assets/63167a7b-5b30-4447-b3ba-b01a6667b55f" />
</p>


3. **Attempted to open the user's profile** by clicking the mention. This returned:
   > *"You don't have access to this link. This link is to a user you don't have access to."*
<p align="center">
  <img width="498" height="207" alt="image" src="https://github.com/user-attachments/assets/cba0ef49-6e29-4fab-88fc-1a1e6bd4ad0d" />
</p>

Combined with the account displaying as `@unknown-user` in chat, this confirmed the account had been **kicked or banned** from the server — consistent with the "Admin Abuse" framing (the admin apparently removed this account after "doing weird things").

4. **Found the pivot** in the same search results: two independent messages in `#public-chat` pointed toward the same place:
   - *"check `#announcements` for the details"*
   - *"`#announcements`, 20ish hours?"* (a rough reference to Discord's relative-time rendering of the clue)

5. **Filtered `#announcements` by date**, narrowing the search to around the decoded timestamp (`2025-02-15`/`2025-02-16`).

6. **Located the flag directly in a message** posted by user **yoshie** (RHAB role) at **2/16/2025 5:59 AM**, titled *"Restarting"* — timestamp matching the decoded clue almost exactly (one minute before a related "Servers will restart" announcement):
<p align="center">
  <img width="450" height="750" alt="image" src="https://github.com/user-attachments/assets/eaa1f5b5-d2ed-4753-a321-9f7fc4492b9d" />
</p>

   ```
   bronco{wh0_g4v3_th15_m4n_3d1t_pr1v1l3g35}
   ```

## Flag

```
bronco{wh0_g4v3_th15_m4n_3d1t_pr1v1l3g35}
```
