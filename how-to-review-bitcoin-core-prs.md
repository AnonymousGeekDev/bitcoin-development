## (WIP) How to Review Bitcoin Core PRs

Last updated: May 31, 2019


### BEFORE YOU BEGIN

These self-study notes build on the foundations laid by these 3 articles:

1. [A Gentle Introduction to Bitcoin Core Development](https://bitcointechtalk.com/a-gentle-introduction-to-bitcoin-core-development-fdc95eaee6b8)
by [Jimmy Song](https://twitter.com/jimmysong) (2017)

2. [Understanding the Technical Side of Bitcoin](https://medium.com/@pierre_rochard/understanding-the-technical-side-of-bitcoin-2c212dd65c09)
by [Pierre Rochard](https://twitter.com/pierre_rochard) (2018)

3. [Contributing to Bitcoin Core, a personal account](https://bitcointechtalk.com/contributing-to-bitcoin-core-a-personal-account-35f3a594340b)
by [John Newbery](https://twitter.com/jfnewbery) (2017)


### INTRODUCTION

Reviewing and testing can be the best ways to begin contributing to Bitcoin Core.

Experienced review and testing are regularly cited by long-term Bitcoin Core
developers as both

  - resource bottlenecks, and

  - the best and most helpful way to begin contributing and earning reputation
    in the community.

Yet the process and learning curve can seem intimidating and in practice very
few new contributors do it.

As a newcomer, this article represents my current understanding after a couple
months, a few dozen PR reviews (some more useful than others), a few issues
tested or handled, and a handful of commits.

Some of this understanding was gained from years of

  - contributing to a large open source project,
    [Ruby on Rails](https://github.com/rails/rails), and co-editing its blog and
    newsletter, [This Week in Rails](https://rails-weekly.ongoodbits.com/archive)

  - development and lead maintainership of
    [Ransack](https://github.com/activerecord-hackery/ransack),
    a popular Ruby search engine

  - daily discussions on this topic of mutual interest with my good friend and
    colleague, [Kasper Timm Hansen](https://twitter.com/kaspth), a developer at
    [Basecamp](https://basecamp.com/about/team) and since 2016 a member of the
    [Ruby on Rails Core Team](https://rubyonrails.org/community/).

Yet much of it is specific to Bitcoin Core and came only with time. I would have
preferred to know these things from the start. So here we are. This is foremost
a self-study exercise, but perhaps it can be useful for others.


### GENERAL

As a newcomer, the goal is to try to add value, with friendliness and
humility, while learning as much as possible.

A good approach is to make it not about you, but rather "How can I best serve?"

Remember that contributor and maintainer resources are limited; ask for them
carefully and respectfully. The goal is to try to give more than you take, to
help more than hinder while getting up to speed.

Be aware of what you don’t know; long-term contributors have years of experience
and context.

Follow the
[bitcoin-core-dev IRC channel](https://webchat.freenode.net/?channels=bitcoin-core-dev)
and
[mailing list](https://lists.linuxfoundation.org/mailman/listinfo/bitcoin-core-dev).

Before jumping in, take plenty of time to

  - understand the contribution process and guidelines, not only by reading
    everything in the repository, notably in
    [/doc](https://github.com/bitcoin/bitcoin/tree/master/doc) and
    [/test](https://github.com/bitcoin/bitcoin/tree/master/test),
    but also by observing interactions on the bitcoin-core-dev IRC channel and
    [code review in the repository](https://github.com/bitcoin/bitcoin/pulls)

  - get to know the maintainers and regular contributors: what they do, what
    they like and want

The big picture is much more important than nits, spelling, or code style.

Focus on quality over quantity and a balance between deep work and quick wins.

Focus on user problems, actual bugs, and "used, but untested" methods that
affect outcomes and need tests.

Documentation is important, e.g. whether a function has a good description and
Doxygen documentation for all its arguments, and high-level documentation of how
things work and interact.

Test coverage is essential; don't hesitate to write any missing unit or
functional tests.

Try to avoid overly commenting in PRs about code style issues and nits,
particularly with PRs labeled as WIP, or when a PR has just been filed and the
PR author is mainly looking for concept ACKs, e.g. general consensus, not
nitpicking. Long-term contributors report that activity like this repels them,
and it can diminish your social capital on the project. Try to understand what
kind of review is needed and when to do what.

Be a contributor. Help PRs move forward by reviewing, proposing tests or fixes
in a helpful way, proposing to rebase, or even offering to take over the PR
after months of silence. In short, help each other!

Keep in mind that no one is forced to take your review comments into account;
it's perfectly fine for the author to reply that they don't want to do something
if they feel it is outside the scope of the change, including/especially if your
comment is nitpicky.

The best time for any nit comments is after the concept acks and consensus on
the PR, and before the PR is finalised and has tested ACKs.

Give nits and style advice in an advisory way -- as in, feel free to ignore,
feel free to adjust if you happen to rebase, etc.

When you can, scale up the difficulty and priority of the PRs you review.

You can add more value by taking the time to do deep, quality review of the
high-priority and more difficult PRs, the ones that intimidate people and that
stagnate for months, killing their author's motivation with death by a thousand
cuts from nits/code style comments, lack of quality review, and rebase hell.

The process takes time; nothing can substitute for months and years invested in
gathering context and understanding from following the code, issues,
PRs/reviews, bitcoin-core-dev IRC and
[mailing list](https://lists.linuxfoundation.org/mailman/listinfo/bitcoin-core-dev).

Keep ego and hopes out of the process. Don't take things personally and keep
moving forward.

When in doubt, assume good intentions.

Be patient with people and outcomes.

Work on it every day.

These are all much easier said than done. Be forgiving with yourself and others.

jnewbery:

- A good rule of thumb is to review 5-15 PRs for each PR you make
- Start small
- Have a plan, don't spread yourself too thin
- Sharpen your tools
- Ask for and offer help, like rebasing for people or adding test cases
- People are generous with their time because they care
- Contribute by understanding what others want and being respectful

gmaxwell:

- Avoid doing review that is inconsistent or focused on minutia or code
  style, especially if it comes across as oppressive rather than enabling


### TECHNICAL SPECIFICS

Concept ACK means that the reviewer acknowledges and agrees with the concept of
the change, but is not (yet) confirming they've looked at the code or tested it.

This can be a valuable signal to a PR author to let them know that the PR has
merit and is headed in the right direction.

Manual testing of new features is always welcome.

While you're reviewing, adding tests yourself helps you understand the behaviour
and you can send them to the author who can add them to the PR too.

Contributing automated tests to the PR author is a really helpful way to start
contributing.

Authors really appreciate it when someone reviews the pr and provides additional
tests.

A comment that is really helpful in review: "here's what I tested and my methodology".

jnewbery/instagibbs:
- check out the branch locally
- run a difftool on each commit in turn using meld on linux and opendiff on mac
- ACK the commit hash of the HEAD commit from my local checkout of the branch,
  so unless my local tools are compromised, I know I'm ACKing the exact changes
  it's useful when a force push happens and links to old commits are lost on GH
- though unless you gpg sign the ACK, GH could just modify what you're saying;
  if you want to fully remove trust, you can go the full Marco Falke and
  sign/opentimestamp all of your review comments

aj:
- i find "gitk" useful for getting an overview of changes

fanquake:
- I have some core dev tools in https://github.com/fanquake/core-review
  It has guides on how to gitian build, review certain types of PRs, etc
  I still need to improve the docs and push up more stuff I have sitting locally
- There is also https://github.com/bitcoin-core/bitcoin-maintainer-tools
- Rough distro list for build-related changes:
  https://github.com/fanquake/core-review/blob/master/operating-systems.md

BIP 125 describes bitcoin core's mempool policy for allowing txs to be replaced:
https://github.com/bitcoin/bips/blob/master/bip-0125.mediawiki

harding:
- In case it's helpful to anyone, I took a quick look at the PR and made notes
  about what I'd test for it:
  https://gist.github.com/harding/168f82e7986a1befb0e785957fb600dd

harding's process:
1. Open PR in browser
2. Checkout PR in dev environment
3. Build
4. Run integration tests
5. During and after 2-4, make notes on what I need to review
6. Answer what questions I can by looking at the code
7. Start regtest node (or testnet or mainnet if necessary) and review actual
   operation matches my expectation from the code

jnewbery:

I always download the PR branch to my machine so I can build and review locally.
I don't use the github webpage to review, only to leave comments.

My git tools: https://gist.github.com/jnewbery/1d45a6b5e14b3f3fefe5942d0cc2608d

I've got a short script that checks out the PR branch and queries the github API
to add a description to that branch locally, that makes it easier when I have a
bunch of PRs checked out locally that I can run a `git branch` command and see
what they are

once I have the branch locally, I'll set off a build in a VM while I look
through the changes

first, I run something like `git log --oneline upstream/master..`
that gives me a list of all the commits in the PR branch, one per line

and then I use a one-liner, which I have bash aliased as git-review, that steps
through the commits one-by-one, printing the commit log to the console, then
opening my difftool program:

for commit in `git log master..HEAD --oneline | cut -d' ' -f1 | tac`; do git log -1 $commit; git difftool ${commit}{^,} --dir-diff; done

I'll look at the diff, and when I quit the difftool program, git-review will
step forward to the next commit

First run through, I'll just skim everything, reading the commit logs and
looking at the overall changes, so I get an idea of what the PR is doing

Then I'll go through again, but look at each commit in more detail,
reviewing every line in detail

I make extensive use of the functional tests by adding
`import pdb; pdb.set_trace()`
break points, and then manually running RPC commands on the nodes under test

I pull the PR description from the github API and then use it to label the branch

There's an attribute of a git branch called `description` that you can update
manually, my `gb` doesn't map onto `git branch` exactly, I do some formatting on
the output so it includes the description

I'm using vagrant, just the standard ubuntu bionic image.
I have a vagrant file that installs all the dependencies.

I run make check and the functional tests, not usually --extended because I'm
too impatient.

Here's my vagrant config (YMMV): https://github.com/jnewbery/btc-dev

A toolchain for bitcoind data files: https://github.com/jnewbery/bitcointools

MarcoFalke: to run functional tests in the gui, pass BITCOIND=bitcoin-qt

To turn on debug=net with the logging RPC: bitcoin-cli logging '["net"]'

If we need any more linters it should be to check for unexpected changes to
consensus code


### ABOUT MAKING PULL REQUESTS

When you do start contributing PRs, here are some guidelines:

Focus on user problems, actual bugs, and "used, but untested" methods that
affect outcomes and need tests.

Avoid making refactoring, fixup/cleanup, or trivial spelling PRs; these consume
valuable contributor and maintainer time for little gain. As mentioned above,
activity like this actively repels long term contributors and can diminish your
social capital on the project.

Do not begin by trying to change consensus code; it is difficult and
dangerous territory. The goal of bitcoin core is to maintain the correct
consensus on the bitcoin network, all else is secondary.

Documentation is important, e.g. whether a function has a good description and
Doxygen documentation for all its arguments, and high-level documentation of how
things work and interact.

Test coverage is essential; don't hesitate to write any missing unit or
functional tests.

In general, PRs that intelligently improve documentation and tests in a well
thought-out way tend to be well-received.

Don't rush: it's often better to work on code/tests/PRs without actually filing
them... give them time to mature and re-visit them with fresh eyes and
perspective. It's a good rule of thumb to write several unfiled PRs for each
one that you do file.

Scale up your PR contributions slowly in line with your ability and
understanding of the project, process, and codebase.

Never put GitHub usernames in commits; this can cause endless annoying
notifications for those concerned.

Any release notes should refer to the RPC help for details instead of
substituting for full documentation, for example: "Please refer to the RPC help
of `getbalances` for details."

Put the time in to make your PRs as easy to understand and review as possible:
code/tests/comments, a good commit message, concise references/explanations.

In your PR description, it's a smart habit to give reviewers tips on how to
review and test your changes, e.g. "Here's how to review this".

Set up Travis CI on your own GitHub Bitcoin repository so that when you push a
branch or commit the linter and CI tests run. It's a good idea to be sure they
are all green on your GitHub repository before filing a PR.

Only add PRs that are worth merging, both for Bitcoin and for your track record.
This way your PRs will receve attention from the maintainers and other
contributors and be merged more quickly.

Learn how to get people to review your work when needed. It's a skill.

Part of that is to remember to continue doing more reviewing and testing than
adding PRs to the backlog. 5 to 15 reviews for each PR you add is a good range.
If you are a good reviewer and begin building a reputation, the other
contributors might reciprocate by reviewing your work as well.


### CREDITS

A special thank you to John Newbery for launching and running the Bitcoin Core
PR Reviews Club (https://bitcoin-core-review-club.github.io/) and to the
long-term contributors who participated so far: Dave Harding, Anthony Towns,
Gregory Sanders, Michael Ford, Andrew Chow, and Marco Falke.

This article includes observed comments on GitHub and IRC by the following
Bitcoin Core contributors/maintainers who deserve to be credited:
Wladimir J. van der Laan, Marco Falke, Pieter Wuille, and Gregory Maxwell.

Over the years I had become disillusioned by the central influence of BDFLs in
programming languages and open source projects. Wladimir van der Laan's humble
service to Bitcoin sparked the possibility to me of perhaps doing the same.

Finally, a big thank you to the Core contributors for their patience with my
stumbling review attempts so far, notably John Newbery, Marco Falke, João
Barbosa, practicalswift, Gregory Sanders, Jonas Schnelli, Pieter Wuille and
Wladimir J. van der Laan, and to Adam Jonas and John Newbery for their guidance
and advice from the start.

Cheers,

[Jon Atack](https://twitter.com/jonatack)