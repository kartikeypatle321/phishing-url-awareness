# Test Results

Python version used: 3.14.3 | Operating system: windows | Date: 27-10-2026

(Re-run all five on YOUR computer, then fill the three blanks above with your own details.)

| # | Input | Expected result | Actual result | Pass/Fail |
|---|-------|-----------------|---------------|-----------|
| 1 | https://www.example.com/login | No listed warning signs | No listed warning signs found. This does NOT prove safety. | PASS |
| 2 | http://192.0.2.10/account | IP address + HTTP | Uses an IP address instead of a familiar domain. / Uses HTTP. HTTPS alone does not prove trust. (REVIEW CAREFULLY: 2) | PASS |
| 3 | http://example.com@sample.invalid/login | @ symbol + HTTP | Contains an @ symbol; inspect carefully. / Uses HTTP. HTTPS alone does not prove trust. (REVIEW CAREFULLY: 2) | PASS |
| 4 | https://a.b.c.d.e.example.invalid/path | Many hostname segments | Contains many hostname segments/subdomains. (REVIEW CAREFULLY: 1) | PASS |
| 5 | https://xn--example.invalid/ | Punycode-style hostname | Contains a punycode-style hostname. (REVIEW CAREFULLY: 1) | PASS |

## Bonus checks

| Input | Actual result |
|-------|---------------|
| (empty Enter) | No URL entered. |
| www.example.com | Uses HTTP. (missing scheme is treated as http://) |
| https://secure-login.example.invalid | No listed warning signs found. (a tricky name slips through) |
| http://[bad | This text is not a valid URL format. (no crash, thanks to Improvement C) |

## Notes

I learned that a phishing checker can only look for clues in the URL text, so it cannot say whether a link is really safe. The @ symbol test surprised me, because example.com@sample.invalid looks like example.com but the real hostname is sample.invalid. I also saw that https://secure-login.example.invalid showed no warning signs even though the name looks suspicious, and that http://[bad used to crash the program until I added try/except to handle it. All five tests passed, and I re-ran them after each improvement to make sure nothing broke.
