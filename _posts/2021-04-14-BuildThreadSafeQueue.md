---
layout: post
title:  "Build A Thread-safe Queue with Condition Variables"
date:   2021-04-14 18:12:21 -0500
categories: first try
---
state: empty(), size()
query the elements of queue: front(), back()
modification: push(), pop(), emplace()
try_pop(): try to pop the value from the queue but returns immediately if there wasm't a value to retrieve.
wait_and_pop(): waits until there's avalue to retrieve
