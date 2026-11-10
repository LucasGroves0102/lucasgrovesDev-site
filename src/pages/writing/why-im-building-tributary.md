---
layout: ../../layouts/Post.astro
title: Why I'm building Tributary
date: 2026-11-10
summary: What I wanted from SEC data, what I've built so far, and where I think it should go next.
---

Every public company in the U.S. files its numbers with the SEC, and all of it is free. Ten-Ks, 10-Qs, earnings releases, the XBRL data behind every financial statement. In theory, anyone can do serious research on any company.

In practice, the data is hard to use. The same metric is tagged differently from company to company. Fiscal quarters don't line up with calendar quarters, some quarters run 16 weeks, and the fourth quarter usually isn't reported at all, so you derive it from the annual number. The tools that clean all this up cost thousands of dollars a year. I wanted something in between: the public data, cleaned up properly, with every number traceable back to the filing it came from.

That's [Tributary](https://tributary.lucasgroves.dev).

## What it does today

Tributary watches EDGAR and pulls every new filing in as it's accepted, usually within a minute. It runs the data through bronze, silver and gold layers in Postgres, with quality checks on every load, and keeps a record of where every row came from. On top of that sits a workspace for doing actual research on a company: its statements, earnings releases, what changed since the last filing, peers, a model, and a written thesis that can be checked against the filings.

There's also an AI assistant, and this is the part I care most about getting right. It can only answer from data the person asking is allowed to see, and every figure in its answers is checked against the source before it's shown. If it states a number that isn't in the data, the answer gets flagged. On my test set, 128 of 128 figures traced back to a filing or an XBRL fact.

All of it runs on a single server I manage myself, deployed from Git, with backups that get restored and checked every week.

## Why build it this way

Honestly, Tributary is more infrastructure than one person strictly needs. Kafka, change data capture, two layers of permissions: a simpler stack could carry today's traffic.

I built it this way on purpose. The things that make financial data trustworthy are the same things that make any data platform trustworthy: knowing where every number came from, checking it on the way in, controlling who can see what, and being able to rebuild it all from scratch. I wanted to get those right at the foundation, because they're very hard to bolt on later.

## Where I think it should go

The way I think about it now is something like Databricks for financial analysts: one governed place where filings, prices, transcripts and your own files go through the same pipeline and checks, and where an analyst, a quant writing SQL, and an AI agent can all work on the same trusted data.

A few directions I'm thinking through:

- **Opening the data up.** An API, bulk exports, and point-in-time queries that show what was known on a given date, so a backtest can't accidentally use numbers that were restated later.
- **Connecting your own tools.** Letting people point their own AI assistant at Tributary, with the same permissions and the same citation checks.
- **More sources through the same pipeline.** Market prices, earnings call transcripts, and your own models and notes, all governed the same way the SEC data is.

None of that is promised. It's where my thinking is, and I'll write about it as it develops, including the parts I get wrong.

If you're working on similar problems, I'd like to hear from you.
