Security Assessment: example.py

Risk: Medium (26/100)
Prediction model: LSTM (2 layers, 128 hidden units), trained on 835 ordered attack paths from 34 real intrusions (MITRE CTID Attack Flow corpus)

 1. Observed evidence (website scan and network traffic)
- T1557 Adversary-in-the-Middle (confidence 0.6): Site not served over HTTPS
- T1566 Phishing (confidence 0.45): No SPF record found; No DMARC record found

 2. LSTM forecast: techniques likely to follow (predicted, not confirmed)
| Rank | Technique | LSTM probability |
|---|---|---|
| 1 | T1590 Gather Victim Network Information | 0.34 |
| 2 | T1204 User Execution | 0.17 |
| 3 | T1078 Valid Accounts | 0.10 |
| 4 | T1203 Exploitation for Client Execution | 0.05 |
| 5 | T1059 Command and Scripting Interpreter | 0.05 |

 3. Most likely attack path (LSTM step-by-step forecast)

T1590 Gather Victim Network Information -> T1560 Archive Collected Data -> T1070 Indicator Removal -> T1071 Application Layer Protocol

Each step is the model's top choice given the observed exposures plus the earlier forecast steps.

 4. Recommended defensive actions

Fix what was observed:
- Serve the site over HTTPS only, enable HSTS, and keep certificates valid.
- Add SPF and DMARC records for the domain.

Prepare for what the LSTM predicts:**
- T1590 Gather Victim Network Information: Reduce publicly exposed network information.
- T1204 User Execution: User-awareness training and attachment/link sandboxing.
- T1078 Valid Accounts: Enforce MFA, remove unused accounts, alert on unusual logins.
- T1203 Exploitation for Client Execution: Patch client software promptly; enable exploit-protection features.
- T1059 Command and Scripting Interpreter: Restrict and log script interpreters (PowerShell/shell); enable script-block logging.

 5. Confidence and limitations
- On intrusions never seen in training: correct technique in top-3 about 20% of the time, top-5 about 25% (see output/evaluation_results.txt).
- Observed items are weaknesses seen in the scan. They do not prove any attack happened.
- Predicted items are statistical associations learned from historical intrusions.
- The training set is small (34 intrusions), so treat rankings as hints for prioritising defences.