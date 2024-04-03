---
layout: post
title: "Poetry issues with Certificates"
date: 2024-04-03 14:25:06 -0000
categories: python poetry venv
---

# Poetry Issues with Certificates
![Bug search](./full_experience_in_commercial_software_dev.md)

Today was not a good day. I tried to set up a new project in which I'll be committing for the next two weeks and finally got stuck with an error like this:
```
[SSL: CERTIFICATE_VERIFY_FAILED] certificate verify failed: unable to get local issuer certificate (_ssl.c:997)
```
My current Python environment configuration looks like this:
```
System Python -> PyEnv -> python venv (pip) -> Poetry
```
Poetry seems to be the culprit in this case, as I verified that `pip` is working properly. I even attempted to trace calls from inside a running instance using:
```bash
strace -e trace=open,openat,execve -f poetry install 2>&1 | tee strace_output.txt | grep "\.pem"
```
After an hour of googling and investigating, I found a solution:
```bash
sudo cp ~/Downloads/all.pem /etc/ca-certificates/trust-source/anchors/certificates.pem
sudo trust extract-compat
export REQUESTS_CA_BUNDLE=/etc/ssl/certs/ca-certificates.crt
poetry install
```