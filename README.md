# challenges

## Introduction

Collection of programming challenges/puzzles, loosely following the CTF paradigm
and mostly inspired by the [Python Challenge](http://www.pythonchallenge.com/).

The general idea for these puzzles is to extract hidden information encoded
somehow in a file (whatever that file may be). That should usually require
figuring out where the information is hidden and applying some transformations
to get it out. Some challenges may feature algorithms commonly used in the IT
world, file formats or just some clever tricks to hide information.


## Difficulty

Compared to the Python Challenge, both the average and peak difficulty will be
lower. The non-technical *riddle* component will also be weaker here.

Early feedback suggests difficulty might be around *medium* or *medium-high*.
However, the perceived difficulty is very subjective and depends on several
factors like your experience and background, so your mileage may vary.


## Mechanics

Each challenge is represented by a self-contained file named `challengeNN`,
where `NN` is just an index according to the order challenges are published in.
Order does not necessarily align with difficulty (i.e., `challenge11` might be
easier than `challenge03`). It is not a requirement to solve challenge `NN` in
order to gain access to challenge `NN+1`; you can work on any of the challenges
currently published, in any order you like.

The goal is the same for every challenge: to find a hidden `key`. The `key` will
usually be a short string, which in many cases will be a recognizable word or
combination of words. However, in some challenges the `key` could still be a
random-looking sequence of characters. But it should always be a relatively
short printable sequence of characters.

Once you found the `key` for a challenge, you can generate and publish a proof
(details about this later).


## Approach

On any individual challenge, your first task will always be to figure out what
kind of file that is. Only then can you access its contents in the appropriate
way.

From that point on, challenges will vary wildly. In general, but depending on
the challenge, you might have to:

- examine the information you are given
- notice the presence of clues
- proactively explore data from different angles and notice patterns or things
  that break the patterns
- search for additional information online (e.g.: to learn about a file format
  or algorithm, or even to understand the meaning of clues sometimes)
- etc.

Ultimately, your goal is to figure out where information is hidden and how it is
encoded. The last step is, of course, to extract that information and get your
hands in that sweet `key`.

Throughout this whole process, you can use any tools you deem useful. That
includes: programming (any programming language), command line tools (Unix is
best, but everything is in principle doable in other OS types), external
applications or a browser and search engine.


## Proof generation

As a way of keeping track on progress, we can use a simple cryptographic proof
system. You can "prove" you have solved a challenge by generating an HMAC digest
based on your company email address and the `key` you got from the challenge.
We'll use SHA256 as hashing algorithm and, for conciseness, we'll just take the
first 6 hexadecimal digits from the result. We could summarize it conceptually
as:

```
PROOF = HMAC_SHA256(<KEY>, <EMAIL_ADDRESS>)[0..6]
```
and it can be easily computed, for example, with libgcrypt:

```shell
$ hmac256 '<KEY>' <(echo -n '<EMAIL>') | awk '{print substr($1, 0, 6)}'
c3f839
```

or Python 3:

```shell
$ python -c 'import hmac; print(hmac.new(b"<KEY>", b"<EMAIL>", "sha256").hexdigest()[:6])'
c3f839
```


### Reference proofs (for key validation)

These are the proofs generated for a fictitious `human@machine.tld` email
address. You can use them to verify you have actually found the correct `key` in
each one of the challenges:

| Challenge     | Proof    |
|---------------|----------|
| `challenge00` | `c42d5e` |
| `challenge01` | `145051` |
| `challenge02` | `2b71f3` |
| `challenge03` | `ce27a4` |
| `challenge04` | `5b3562` |


## Note for outsiders

I've created this set of challenges as a fun exercise to share with colleagues
at work. I saw no reason why they couldn't be made publicly available, so here
they are. Anyone can try and solve them.

That said, there are a couple of things you should be aware of, namely:

* The proof publishing part won't be relevant to you, as it was meant to keep
  track of progress internally. However, you can still use the reference proofs
  for key validation.
* Some pieces of information might be redacted, but that won't hinder your
  ability to solve the challenges.
* Although a rare exception, some of the challenges are meant to be executed as
  code in a machine. As a general rule, never trust random code downloaded from
  the Internet. Be safe; run them in a sandboxed environment, if at all.
* You'll notice `challenge00` is a bit of an oddball. It is a precursor to the
  idea of creating a set of challenges, so it deviates a tiny bit from the
  common pattern, but I decided to include it anyway.

That's all. Have fun :)
