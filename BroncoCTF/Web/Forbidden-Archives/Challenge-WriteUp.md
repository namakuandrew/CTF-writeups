# CTF Writeup: The Forbidden Archives (BroncoCTF)
**Category**: Web Exploitation

**Vulnerability**: SQL Injection (SQLi) / WAF Bypass

**Flag**: bronco{you_d3f3@t3d_th3_h1gh_counc1l}
______________________________________________________________________________________________________________________________________________________________________
# Challenge Overview

"The Forbidden Archives" presented a web interface with a single search bar used to query a database of magical scrolls/books. The page included flavor text stating: "Every scroll tells a story, every silence hides a truth." and "No public scrolls bear that name. Perhaps it is... forbidden."

The objective was to extract a hidden "forbidden" scroll containing the flag from the backend database.

<img width="1360" height="590" alt="image" src="https://github.com/user-attachments/assets/31c6bdea-30d7-496d-86ee-311c05dc1d99" />

## Phase 1: Reconnaissance and Fingerprinting
Initial testing on the search input revealed it was vulnerable to SQL injection. When attempting standard boolean payloads like ' OR '1'='1, the application successfully returned the first book in the database, A Wizard of Earthsea.

This behavior revealed two key pieces of information about the backend:

- The database was likely using a LIKE operator (e.g., WHERE title LIKE '%[INPUT]%').

- The application was enforcing a limit (like fetchone() or LIMIT 1), preventing it from dumping the entire table at once.
<img width="1211" height="673" alt="Screenshot 2026-07-12 195619" src="https://github.com/user-attachments/assets/69a1986c-c9e5-4c81-bc40-8344116e552d" />

## Phase 2: The Row Exclusion Rabbit Hole
Because the application only displayed the first result, the initial strategy was to use Row Exclusion. By explicitly telling the database to ignore the books we had already found, we could theoretically page through the database to find the hidden scroll.

- Payload: Nope' OR title != 'A Wizard of Earthsea' OR '1'='0

- Expansion: Nope' OR title NOT IN ('A Wizard of Earthsea', 'The Name of the Wind', 'The Way of Kings', 'The Hobbit', 'The Color of Magic') OR '1'='0

While this successfully enumerated public books, it quickly became a rabbit hole. We verified up to rowid = 99 and found nothing but standard fantasy novels. The flag was either in a different table, or heavily filtered.

## Phase 3: Confronting the WAF
Attempting to map the database structure using standard UNION SELECT attacks revealed a Web Application Firewall (WAF) in place.

- Injecting UNION SELECT 1,2,3... resulted in a syntax error: near "SELECT": syntax error.

- Injecting ORDER BY 1 resulted in: near "DER": syntax error.

The WAF was actively stripping keywords like UNION, SELECT, ORDER, and OR.

Bypassing the Filter:
To get around the keyword stripping, we utilized SQLite's logical concatenation operator || instead of OR, and avoided explicitly banned keywords. While payloads like Nope'||(title LIKE '%bronco{%')||'1'='0 successfully bypassed the WAF, they returned blank results, indicating the flag was not stored in the standard title, author, or description columns of the public scrolls.
