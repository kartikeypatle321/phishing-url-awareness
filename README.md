# EWB Phishing URL Awareness Checker

## Overview
Phishing is a trick where a criminal uses a fake link or message to make people share passwords, card numbers or OTPs. This command-line tool reads the *text* of a URL and points out a few visible warning signs, like a guard who checks a parcel's label without opening it. It never visits the link.

## Features
- Detects an @ symbol in the URL
- Flags unusually long URLs (over 100 characters)
- Flags IP-address hostnames
- Flags many hostname segments (5 or more)
- Flags punycode-style hostnames (xn--)
- Flags the HTTP scheme
- Loop mode: check many URLs without restarting; type `q` to quit
- "REVIEW CAREFULLY" summary with a warning count
- Safe handling of malformed input (no crashes)
- A "Why it matters" explanation under each warning
- Numbered output

## Requirements
Python 3.10 or later. No extra packages.

## How to Run
Windows: `python phishing_checker.py`
Mac/Linux: `python3 phishing_checker.py`

## Example Output
```
EWB Phishing URL Awareness Checker
Text checks only - this program does not open URLs.

Enter a sample URL (or type q to quit): http://192.0.2.10/account

Result:
1. Uses an IP address instead of a familiar domain.
   Why it matters: Honest services normally use readable names; a bare number hides who owns the site.
2. Uses HTTP. HTTPS alone does not prove trust.
   Why it matters: HTTP is not encrypted, so data can be seen on the way. HTTPS alone still does not prove trust.

REVIEW CAREFULLY: 2 warning sign(s) found.
Never enter passwords from unexpected links.
```

## Testing
| # | Input | Expected | Actual | Result |
|---|-------|----------|--------|--------|
| 1 | https://www.example.com/login | No warning signs | No listed warning signs found | PASS |
| 2 | http://192.0.2.10/account | IP address + HTTP | IP address + HTTP flagged | PASS |
| 3 | http://example.com@sample.invalid/login | @ symbol + HTTP | @ symbol + HTTP flagged | PASS |
| 4 | https://a.b.c.d.e.example.invalid/path | Many hostname segments | Flagged | PASS |
| 5 | https://xn--example.invalid/ | Punycode-style hostname | Flagged | PASS |

Full table: [tests/test_samples.md](tests/test_samples.md)

## Screenshots
![Sample run](screenshots/sample_run.png)
![Test results](screenshots/test_results.png)

## Improvements
1. **Loop with quit (A)** - the program keeps asking for URLs until `q`, `quit` or `exit`, so I do not restart it for every test. Checked by running all five samples in one session.
2. **Review carefully summary (B)** - a helper `has_warnings()` separates real warnings from information messages, then a count is printed. Checked that the empty input and the "no signs" case do not show it.
3. **Malformed input handling (C)** - `urlparse` is wrapped in `try/except ValueError`, so `http://[bad` prints a friendly message instead of crashing.
4. **Explanations (D)** - a `REASONS` dictionary prints why each warning matters.
5. **Better formatting (E)** - numbered results.

After every change I re-ran all five tests to make sure nothing broke.

## Limitations
This is an educational tool. It only reads URL text. It does not visit URLs, scan for malware or prove that a URL is safe. No warning signs found does NOT mean a URL is safe.

## Ethical Use
Use sample strings only. Do not collect credentials. Do not use this project to deceive anyone or to test websites without permission.

## Author
Kartikey Patle, BTech (CS/Cybersecurity), VIT Pune - EWB Courses
