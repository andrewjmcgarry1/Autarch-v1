Security Policy
Supported versions
Only the latest release of Autarch-v1 receives fixes.

Version    Supported
1.0.x    Yes
Reporting a vulnerability
Please do not open a public issue for security problems.

Report them privately by emailing [aurevenai@gmail.com] with:

a description of the problem and how to reproduce it
the file or script affected
any suggested fix, if you have one
I will reply within [7] days to confirm receipt and discuss next steps.

What counts as a security issue
Code in this repository that could run unintended commands or access files on a user's machine (for example, unsafe loading of model files)
Anything in the released model files (model/autarch_v1_final.pt) that could execute code when loaded
Accidental inclusion of personal data, credentials, or API keys in the repository
What does not count
Errors in the model's predictions or accuracy. Report those as normal issues, since they are research findings, not security flaws.
Misuse of the model for clinical decisions. This project is for research only and is not validated for clinical use.
Notes for users
Load the model file safely. When loading autarch_v1_final.pt, use torch.load(path, weights_only=True). Only load weights files from sources you trust.
Do not use this model to make decisions about patients. It is a research prototype (see README).
Do not send real patient images to any service or script based on this code.
