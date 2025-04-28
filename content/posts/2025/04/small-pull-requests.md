---
title: "Engineering Teams Should Embrace Small Pull Requests"
summary: "Advocating for small pull requests"
date: 2025-04-16
tags:
  - SoftwareEngineering
cover:
  image: "posts/2025/04/small-pull-requests.png"
  alt: "small pull requests"
  caption: "How is it possible to review everything?"
---
Have you ever had a Pull Request (PR) that was more than a few weeks old and had thousands of lines of code changes spanning a large number of files? 
I'm sure the code review process went like this: Hmmm, looks good to me.

Now, this is against the spirit of code reviews and definitely something that I would strongly discourage. 
There's a better way to do code reviews - small PRs.

## Why we should embrace small PRs

- Easier and faster to review:
  The smaller scope and more focused changes helps to avoid cognitive overload.
  It's easier for reviewers to understand the developer's intent and provide feedback.
  On the contrary, large PRs tend to require more clarifications which results in reduced productivity. 
- Reduces merge conflicts: By nature, the small changeset means less likelihood of conflicts with other PRs.
- It's easier to catch bugs (e.g. a misunderstood requirement which was incorrectly implemented).
- Better collaboration: Bite-sized chunks are easier to digest and allows developers to learn from each other.

And with small PRs, it should lead to faster development cycles, high quality of code and happier engineers.
But... how do we keep PRs small? Here are some considerations:
- Avoid addressing multiple issues/features in a single PR.
- Assess if we can break down an issues/feature into small ones.
- Avoid the mindset of "only fully completed features that delivers value are allowed to be merged". If a feature isn't complete, implement feature flags to disable it. In the meantime, we should incrementally merge changes. 

Keep your PRs small!