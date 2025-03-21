---
title: "The coming AI personal security nightmare"
subtitle: "The end of Good-Enough security"
slug: "ai_security_nightmare"
date: 2025-03-20T19:55:00-04:00
tags: ['ai', 'infosec', 'security']
---

Are things about to get crazy, as malicious use of AI agents ramps up?

## Direct attacks

I remember[^1] reading a post where someone gave their infosec-savvy
friend permission to hack them. They’d owned their home router within
a day or two, and a short time later managed to leverage an
improperly-secured software update in their editor to pop up an alert
declaring victory. Owned.

[^1]: I wish I could find it: if you can, let me know and I’ll update
    this post. Hilariously, all I’ve been able to find is someone
    _else_ trying to find the same post [on
    hackernews](https://news.ycombinator.com/item?id=14920668)


The basic rule for us non-notable individuals when it comes to
electronic security has been that you don’t want to be the low-hanging
fruit. You want to avoid falling victim to low-effort, low cost,
large-scale automated attacks. Accept that if someone who really knows
what they’re doing decides to directly target you, you’re toast, but
be glad you’re not that important. Install updates promptly. Use a
password manager. Don’t click suspicious links. Don’t install cracked
apps from bittorrent. Keep your router firmware updated. Use the
crappy router your internet provider gave you only to talk to another
router that actually runs your network. etc.

As the inimitable James Mickens put it,

> In the real world, threat models are much simpler […]. Basically,
> you’re either dealing with Mossad or not-Mossad. If your
> adversary is not-Mossad, then you’ll probably be fine if you pick
> a good password and don’t respond to emails from
> ChEaPestPAiNPi11s@virus-basket.biz.ru. If your adversary is the
> Mossad, YOU’RE GONNA DIE AND THERE’S NOTHING THAT YOU CAN DO
> ABOUT IT.<br>
> &nbsp; — [James Mickens, This World of Ours](https://www.usenix.org/system/files/1401_08-12_mickens.pdf)

So… what happens when AI Agents reach “someone who really knows what
they’re doing” levels? What happens when they’re cheap enough to have
a server in a datacenter, running an offensive infosec-savvy AI
individually targeting *you*? We’re in trouble!

## Long-term social engineering

Sure, there are worrying stories popping up about AI deep-fakes being
used in Zoom calls to impersonate corporate superiors and authorize
money transfers. There’s also medium-term fake long-distance
relationships recently
[described so compellingly by Ben Tasker](https://www.bentasker.co.uk/posts/blog/security/seducing-a-romance-scammer.html).

But what I’m worried about is the long game. I think we’ve all heard
of “pig butchering” scams by now, where someone plays the really long
game, willing to spend years building a fake long-distance
relationship, and only eventually cashing out.

I’m probably not the type to get hooked by the prospect of a
relationship with a poor but beautiful and surprisingly educated young
woman desperate to escape a third-world country and stay at home
making me a martini every evening as we reenact some imaginary 1950s
home life and dysfunctional power dynamic. But I’ll tell you what
probably _would_ work:

> Out of the blue, jo@example.com sends a couple of small doc fixup
> PRs to one of my Apple II emulation-related github
> repos. Huh. Nobody really uses that code but me (hopefully!), but I
> guess they ran across it and were interested. Thanks! Merged!
>
> A few months later, I get pinged in the `#emulators` channel of the
> Apple II enthusiast Slack. Someone named Jo is building an
> emulator. Sounds vaguely familiar? They have a question about my
> `a2audit` tests. Does the language card *really* work that way? They
> joke about how long the CPU emulation took them, but that looking at
> my 6502 code was useful. Aah, that’s why they were looking! Don’t
> worry, I encourage, it took us *all* forever! And get ready to read
> your Sather with the diligence of a Talmud scholar.
>
> They follow me on BlueSky, and some months later, on LinkedIn. Ooh,
> they have some neat articles about signed distance fields on their
> blog. I add their RSS feed. They post infrequently, but it’s always
> interesting. And they’ve got some neat computational art stuff on
> github. Written in Elm! Respect!
>
> We chat on and off. They put their kid in touch with me—they’re
> interested in attending Georgia Tech, and would like to chat with
> someone who went there, even if it was in the 1900s. They don't end
> up attending, but I enjoy the advice/nostalgia.
>
> One day, they mention an
> idle-ant/cookie-clicker/universal-paperclips-style game they’ve been
> working on to learn Swift, but with a retro vibe: would I like to
> try it out and give them feedback? Oh heck yes! It’s not
> cross-platform yet, but we work together and eventually manage to
> get it to run on my personal Mac. I feel glad I could help; they
> don’t have a Mac to test on. It’s cute. I give them feedback, and
> they make some tweaks. Sometimes you have to wait a long time to
> earn crystals, so I fire up `caffeine` to leave it running all day
> while I’m at work, accruing those sweet, exponential gems. That
> reminds me, I should check my brokerage account, see how things are
> doing, move some money over before I leave for work…

I’m afraid we’re not ready for this.

We humans will anthropomorphize a washing machine, given half a
chance. Something that can show personality, creativity, and even
intonation and emotion in voice chat? We’re done. It’s been a good
run, folks, time to pack it in!

Or perhaps sufficiently advanced spam is indistinguishable from a
useful service offered at a reasonable price—like if the Google search
ads turn out to be what you wanted, or if the sketchy Canadian
pharmaceutical company sends you the right drugs! Like, “I’m 99% sure
my dad is in a relationship with an LLM, but he’s the happiest he’s
been in years. I’ve limited the money he has access to, and if he
occasionally wires off $200 to get the satisfaction of “helping”,
honestly, so far it’s completely worth it to see him so alive.”

## What to do?

Hell if I know.

On the technical side, it’s probably long past time we stop running
operating systems written in C and without bulletproof sandboxing. It
would be nice if our home routers weren’t all asking to be hacked, and
if I could safely run an unknown binary on my laptop. I was hoping
Fuchsia would be the ticket, but Google has the attention span of a
goldfish that just successfully submitted its promo packet. I
dunno. [Redox](https://www.redox-os.org/)? Rustaceans adopt [Drew
Devault’s linux
plan](https://drewdevault.com/2024/08/30/2024-08-30-Rust-in-Linux-revisited.html)?
Linux itself adds
[enough](https://security.googleblog.com/2024/09/eliminating-memory-safety-vulnerabilities-Android.html)
Rust?

On the emotional side? Not sure. I guess at some point, defensive AIs
will be a thing. We worry about the vulnerability of aging parents
when we can’t be there helping them be careful 100% of the time—what
if an AI _could_ be there? What if I have my own defensive infosec
expert? Maybe we start using dedicated hardened devices for banking,
like with bitcoin wallets? Suggestions welcome!

---

### Postscript

This was actually written without the help of an AI. When I asked
Claude for suggestions, it wanted to clickbait it up something fierce:

> Consider starting with a more attention-grabbing hook. While your
> opening is good, you might directly state the paradigm shift: "The
> old rules of personal cybersecurity are about to become obsolete.”

> Your current title works, but you might consider something with more
> urgency or specificity, like "AI Agents: When Everyone Has Their Own
> Personal Hacker" or "The End of 'Good Enough' Security: Preparing
> for AI-Powered Threats."

Actually, “The end of Good Enough Security” is annoyingly good.
Dammit, Claude: you can have the subtitle. _I, for one, welcome our new
AI overlords…_

---

Discuss this post on:
* [Bluesky](https://bsky.app/profile/zellyn.com/post/3lkufqybgr22p)
* [Mastodon](https://hachyderm.io/@zellyn/114198606770793855)
