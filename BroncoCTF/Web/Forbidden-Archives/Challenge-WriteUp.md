# CTF Writeup: The Forbidden Archives (BroncoCTF)
**Category**: Web Exploitation

**Vulnerability**: SQL Injection (SQLi) / WAF Bypass

**Flag**: bronco{you_d3f3@t3d_th3_h1gh_counc1l}
______________________________________________________________________________________________________________________________________________________________________
# Challenge Overview

"The Forbidden Archives" presented a web interface with a single search bar used to query a database of magical scrolls/books. The page included flavor text stating: "Every scroll tells a story, every silence hides a truth." and "No public scrolls bear that name. Perhaps it is... forbidden."

The objective was to extract a hidden "forbidden" scroll containing the flag from the backend database.

<img width="1360" height="590" alt="image" src="https://github.com/user-attachments/assets/31c6bdea-30d7-496d-86ee-311c05dc1d99" />
