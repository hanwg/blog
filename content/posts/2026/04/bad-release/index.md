---
title: "Why 100% Test Coverage Isn't Enough"
summary: "The 100% Code Coverage Fallacy"
date: 2026-04-07
tags:
- SoftwareEngineering
cover:
  image: "bad-release"
  alt: "Bad release"
---

## Oops, I Broke the Release 😅

It's the notification that every developer dreads - A bug report landing soon after a fresh release.

While implementing a feature enhancement, I accidentally introduced a critical regression that impacted new service onboarding.
Although existing services continued to run smoothly, a bug in the configuration flow prevented new services from completing the process.

## The Aftermath

The issue was first reported by a single user, but the response that followed was heartening.
Within a few hours, several others who encountered the same problem jumped in to help.
Instead of just vague "it doesn't work" comments, the community provided me with a wealth of valuable troubleshooting data such as:
- Environment and configuration details
- Screenshots
- Error messages and stack traces
- Reproduction steps

This collective effort enabled me to quickly pinpoint the root cause far quicker than I could have on my own.
While I began working on a fix, affected users were advised to roll back to the last stable version.
This also served as a reminder of the importance of having reliable backups and having a well-documented rollback process.

## The "Naughty" Parentheses

Fun-fact: The culprit was a sneaky pair of misplaced parentheses `()`.

In Python, parentheses are overloaded.
They are used for explicitly defining the **order of operations** (similar to Math where expressions within parentheses are evaluated first) and also for defining **tuples** (immutable collections).
There was a set of misplaced parentheses which unintentionally transformed my logic into a tuple.

Since the code was still syntactically valid, this bug was incredibly easy to overlook and slipped through the code review.

## The Post-Mortem: The 100% Code Coverage Fallacy

Part of the standard code review process was to verify that all code changes are covered by tests and a passing test suite.
So, how did the bug slip through tests with 100% code coverage?

**The Mock trap.**

The tests relied heavily on mocks for API requests.
The mocks didn't care whether the input was a single value or a tuple; it was programmed to return a success response.

The crucial piece was missing assertions for inputs.
If I had used `mock.assert_called_with(expected_input)`, the test would have failed and caught the issue before it made it to release.

## Key Takeaways

While code coverage is an important metric in testing, here are some limitations that let bugs slip through.

**The Assertion Gap.**
We often obsess over the *output* but neglect the *input*.
This case was a prime example: the code ran, but because I didn't verify the specific arguments passed to the mock via `mock.assert_called_with()`, the logic failed silently.

**Avoid shallow assertions.**
Checking just the HTTP status code is not enough.
You must verify the response headers and body to ensure the logic is working as intended.

**Missing logic.**
Code coverage only measures code that exists.
It has no way of flagging the edge cases or validation steps you *forgot* to implement.

---

PS: I’m incredibly grateful for a community that brings data instead of drama to a bug report.
The fix is out now, and we’re back on track now, with better tests and stricter assertions.
