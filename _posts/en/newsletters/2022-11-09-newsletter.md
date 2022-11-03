---
title: 'Bitcoin Optech Newsletter #225'
permalink: /en/newsletters/2022/11/09/
name: 2022-11-09-newsletter
slug: 2022-11-09-newsletter
type: newsletter
layout: newsletter
lang: en
---
This week's newsletter FIXME

## News

- **Continued discussion about enabling full-RBF:** as mentioned in
  newsletters for the past several weeks---users, service providers, and
  Bitcoin Core developers have been evaluating the inclusion of the
  `mempoolfullrbf` configuration option into Bitcoin Core's development
  branch and current release candidates for version 24.0.  Those
  previous newsletters have summarized many previous
  arguments for and against this [full RBF][topic rbf] option
  ([1][news222 rbf], [2][news223 rbf], [3][news224 rbf]).  This week,
  Suhas Daftuar [posted][daftuar rbf] to the Bitcoin-Dev mailing list to
  "argue that we should continue to maintain a relay policy where
  replacements are rejected for transactions that don't opt-in to RBF
  (as described in BIP 125), and moreover, that we should remove the
  `-mempoolfullrbf` flag from Bitcoin Core's latest release candidate
  and not plan to release software with that flag, unless (or until)
  circumstances change on the network."  He notes:

    - *Opt-in RBF already available:* anyone who wants the benefits of
      RBF should be able to opt-in to it using the mechanism describe in
      [BIP125][].  Users would only be served by full RBF if there was
      some reason they couldn't use opt-in RBF.

    - *Full RBF doesn't fix anything that isn't broken in other ways:*
      a possible case where some users of a multiparty protocol could
      deny other users the ability to use opt-in RBF was previously
      [identified][riard funny games], but Daftuar notes that protocol
      is vulnerable to other cheap or free attacks which full RBF would
      not solve.

    - *Full RBF takes away options:* "absent any other examples [of
      problems fixed by fullrbf], it does not seem to me that fullrbf
      solves any problems for RBF users, who are already free to choose
      to subject their transactions to BIP 125's RBF policy.  From this
      perspective, "enabling fullrbf" is really just taking away user
      choice to opt a transaction into a non-replacement policy regime."

    - *Offering non-replacement doesn't introduce any issues for full nodes:*
      indeed, it simplifies the handling of long chains of transactions.

    - *We want other network policies that aren't perfectly incentive compatible:*
       Daftuar uses the proposal for v3 transaction relay (see
       [Newsletter #220][news220 v3tx]) as an example:

       > Suppose in a few years someone proposes that we add a
       > "-disable_v3_transaction_enforcement" flag to our software, to
       > let users decide to turn off those policy restrictions and
       > treat v3 transactions the same as v2, for all the same reasons
       > that could be argued today with fullrbf [...]
       >
       > [That] would be subversive to making the lightning use case for
       > v3 transactions work [...] we should not enable users to
       > disable this policy, because as long as that policy is just
       > optional and working for those who want it, it shouldn't harm
       > anyone that we offer a tighter set of rules for a particular
       > use case.  Adding a way to bypass those rules is just trying to
       > break someone else's use case, not trying to add a new one.  We
       > should not wield "incentive compatibility" as a bludgeon for
       > breaking things that appear to be working and not causing
       > others harm.
       >
       > I think this is exactly what is happening with fullrbf.

    Daftuar ends his email with three questions for those who still want
    the `mempoolfullrbf` option to be included in Bitcoin Core:

    1. "Does fullrbf offer any benefits other than breaking zeroconf
       business practices?  If so, what are they?"

    2. "Is it reasonable to enforce BIP 125's rbf rules on all
       transactions, if those rules themselves are not always incentive
       compatible?"

    3. "If someone were to propose a command line option that breaks v3
       transaction relay in the future, is there a logical basis for
       opposing that which is consistent with moving towards fullrbf
       now?"

    As of this writing, no one has answered Daftuar's questions on the
    mailing list, although two answers to the set of questions were
    posted to a Bitcoin Core [PR][bitcoin core #26438] that Daftuar
    opened to propose removing the `mempoolfullrbf` configuration
    option.

    Discussion about the topic was ongoing as of this writing.

- **Block parsing bug affecting multiple software:** as reported in
  [Newsletter #222][news222 bug], it appeared that a serious bug
  affecting the BTCD full node and LND LN node was accidentally
  triggered, putting users of the software at risk.  Updated software
  was quickly released.  Shortly after that bug was triggered, Anthony
  Towns [discovered][towns find] a second related bug that could only be
  triggered by miners.  Towns reported the bug to BTCD and LND lead
  maintainer Olaoluwa Osuntokun, who prepared a patch to include in the
  next general update of the software.  Including the security fix
  alongside other changes could hide that a vulnerability was being
  fixed and reduce the chance of it being exploited).  Both Towns and
  Osuntokun responsibly kept the vulnerability private until its fix
  could be deployed.

    Unfortunately, the second related bug was independently rediscovered by
    someone who partnered with a miner to trigger it.  This new bug
    affected BTCD and LND again, but it also affected at least [two
    other][liquid and rust bitcoin vulns] significant projects or
    services.  All users of affected systems should upgrade immediately.
    We repeat our advice from three weeks ago for anyone using any
    Bitcoin software sign up for security announcements from that
    software’s development team.

    With the release of this newsletter, Optech has also added a special
    topic page where we list the names of [the amazing people who
    responsibly disclosed a vulnerability][topic responsible
    disclosures] that we've summarized in an Optech newsletter.  We
    extend our most heartfelt thanks to everyone listed.  There are
    likely several other disclosures not listed because they haven't
    been made public yet.  We look forward to thanking those discoverers
    and adding them to the topic page of Bitcoin security disclosure
    heroes.  We of course also thank all the reviewers of proposals and
    pull requests whose diligent effort prevented innumerable
    security bugs from making it into released software.

## Bitcoin Core PR Review Club

*In this monthly section, we summarize a recent [Bitcoin Core PR Review Club][]
meeting, highlighting some of the important questions and answers.  Click on a
question below to see a summary of the answer from the meeting.*

FIXME:LarryRuane

{% include functions/details-list.md
  q0="FIXME"
  a0="FIXME"
  a0link="https://bitcoincore.reviews/26114#l-27FIXME"
%}

## Releases and release candidates

*New releases and release candidates for popular Bitcoin infrastructure
projects.  Please consider upgrading to new releases or helping to test
release candidates.*

- [Bitcoin Core 24.0 RC3][] is a release candidate for the
  next version of the network's most widely used full node
  implementation.  A [guide to testing][bcc testing] is available.

  **Warning:** this release candidate includes the `mempoolfullrbf`
  configuration option which several protocol and application developers
  believe could lead to problems for merchant services as described in
  this newsletter and previous newsletters [#222][news222 rbf] and
  [#223][news223 rbf].  It could also cause problems for transaction
  relay as described in [Newsletter #224][news224 rbf].  Optech
  encourages any services that might be affected to evaluate the RC and
  participate in the public discussion.

## Notable code and documentation changes

*Notable changes this week in [Bitcoin Core][bitcoin core repo], [Core
Lightning][core lightning repo], [Eclair][eclair repo], [LDK][ldk repo],
[LND][lnd repo], [libsecp256k1][libsecp256k1 repo], [Hardware Wallet
Interface (HWI)][hwi repo], [Rust Bitcoin][rust bitcoin repo], [BTCPay
Server][btcpay server repo], [BDK][bdk repo], [Bitcoin Improvement
Proposals (BIPs)][bips repo], and [Lightning BOLTs][bolts repo].*

FIXME:harding

{% include references.md %}
{% include linkers/issues.md v=2 issues="26438" %}
[bitcoin core 24.0 rc3]: https://bitcoincore.org/bin/bitcoin-core-24.0/
[bcc testing]: https://github.com/bitcoin-core/bitcoin-devwiki/wiki/24.0-Release-Candidate-Testing-Guide
[news222 rbf]: /en/newsletters/2022/10/19/#transaction-replacement-option
[news223 rbf]: /en/newsletters/2022/10/26/#continued-discussion-about-full-rbf
[news224 rbf]: /en/newsletters/2022/11/02/#mempool-consistency
[topic responsible disclosures]: https://example.com/#FIXME-delete-this-link-when-topic-page-added
[daftuar rbf]: https://lists.linuxfoundation.org/pipermail/bitcoin-dev/2022-October/021135.html
[riard funny games]: https://lists.linuxfoundation.org/pipermail/lightning-dev/2021-May/003033.html
[news220 v3tx]: /en/newsletters/2022/10/05/#proposed-new-transaction-relay-policies-designed-for-ln-penalty
[news222 bug]: /en/newsletters/2022/10/19/#block-parsing-bug-affecting-btcd-and-lnd
[liquid and rust bitcoin vulns]: https://twitter.com/Liquid_BTC/status/1587499305664913413
[towns find]: https://twitter.com/roasbeef/status/1587481219981508608
