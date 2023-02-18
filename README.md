# `pgen`(1) – Passphrase Generator

[![Crates.io](https://img.shields.io/crates/v/pgen?style=flat-square)](https://crates.io/crates/pgen)
[![Crates.io](https://img.shields.io/crates/d/pgen?style=flat-square)](https://crates.io/crates/pgen)
[![License](https://img.shields.io/badge/license-ISC-blue?style=flat-square)](LICENSE)
[![GitHub stars](https://img.shields.io/github/stars/ctsrc/Pgen?style=social)](https://github.com/ctsrc/Pgen#start-of-content)

Generate passphrases using the [wordlists for random passphrases][EFFWL]
made by the EFF.

The EFF wordlists consist of words that are easy to type and easy to remember.

By default, passphrases generated by `pgen` consist of twelve words
randomly selected from the autocomplete-optimized wordlist. Be sure to
[read the article][EFFWL] to learn about the difference between the
different wordlists provided by the EFF.

These are some examples of generated passphrases:

 * gimmick saffron nirvana superstore voicemail dedicate guacamole oftentimes dwindling kingdom shuttle upright
 * bobcat pulley yearbook nectar krypton pesticide relic sauna detergent amnesty dishcloth tapestry
 * porcupine identical occupation oxidize avalanche celery vaporizer dastardly vicinity enlarged hatchling urethane

## Table of Contents

* [Usage](#usage)
  - [Options](#options)
* [How many bits of entropy does your passphrase need?](#how-many-bits-of-entropy-does-your-passphrase-need)
* [Is a CSPRNG really needed here?](#is-a-csprng-really-needed-here)
* [Installation](#installation)

## Usage

```
pgen [--dice] [-l | -s] [-n <n>] [-k <k>] [-e]
pgen -h | --help
pgen -V | --version
```

### Options

`-l` Use long wordlist instead of autocomplete-optimized short wordlist.
     Recommended for the creation of memorable passphrases since the
     increased number of words as well as the greater effective word
     length allows for good entropy with a lower amount of words
     compared to the autocomplete-optimized short wordlist.
     Mutually exclusive with option `-s`.

`-s` Use non-optimized short wordlist instead of autocomplete-optimized
     short wordlist. Mutually exclusive with option `-l`.

`-n` Specify the number of words to use *n*. Default value:

  * Twelve (12) words if either of the short wordlists are being used
    (meaning that the `-l` option was **not** specified).
  * Ten (10) words if the large wordlist is being used (meaning that
    the `-l` option was specified.)

`-k` Specify the number of passphrases to generate *k*. Default value: 1.

`-e` Calculate and print the entropy for the passphrase(s) that would be
     generated with the given settings. What is password entropy?
     [Entropy is a measure of what the password could have been, so it relates to the selection process](https://crypto.stackexchange.com/a/376).

`--dice` Use physical six-sided dice instead of letting the computer pick
         words. Useful in case you distrust the ability or willingness of
	 your computer to generate "sufficiently random" numbers.

`-h`, `--help` Show help and exit.

`-V`, `--version` Print version information and exit.

## How many bits of entropy does your passphrase need?

How many bits of entropy should your passphrase consist of?

Looking at [the article about password strength on Wikipedia](https://en.wikipedia.org/wiki/Password_strength), you will find that the following is said:

> The minimum number of bits of entropy needed for a password depends
> on the threat model for the given application. If
> [key stretching](https://en.wikipedia.org/wiki/Key_stretching)
> is not used, passwords with more entropy are needed.
> [RFC 4086](https://tools.ietf.org/html/rfc4086), "Randomness Requirements
> for Security", presents some example threat models and how to calculate
> the entropy desired for each one. Their answers vary between 29 bits
> of entropy needed if only online attacks are expected, and up to 128 bits
> of entropy needed for important cryptographic keys used in applications
> like encryption where the password or key needs to be secure for a long
> period of time and stretching isn't applicable.

In the case of web services such as webmail, social networks, etc.,
given that historically we have seen password databases leaked, where
weak hashing algorithms such as MD5 were used, it is the opinion of the
author that the neighbourhood of 128 bits of entropy is in fact
an appropriate default for such use.

When calculating the entropy of a password or a passphrase,
[one must assume that the password generation procedure is known to the attacker](https://crypto.stackexchange.com/a/376).
Hence with 12 words from either of the short wordlists, each of which
consist of 1296 words, we get a password entropy of log2(1296^12) ~=
124.08 bits. Similarily, with 10 words from the long wordlist (7776 words),
we get a password entropy of log2(7776^10) ~= 129.25 bits.

## Is a CSPRNG really needed here?

Using a CSPRNG ensures uniform distribution of probability. This in turn
ensures that the password entropy calculations are correct. Hence it makes
sense to use a CSPRNG.

## See also

* `lastresort`(1) on [crates.io](https://crates.io/crates/base256) / [GitHub](https://github.com/ctsrc/Base256)

## Installation

1. [Install Rust](https://www.rust-lang.org/en-US/install.html).
2. Run `cargo install pgen`


[EFFWL]: https://www.eff.org/deeplinks/2016/07/new-wordlists-random-passphrases
