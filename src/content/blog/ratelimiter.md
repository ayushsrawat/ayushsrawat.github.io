---
title: 'Implementing Rate Limiting'
description: 'Protect your API with a simple Token Bucket algorithm.'
pubDate: 'Dec 05 2025'
heroImage: ''
---

# Understanding Rate Limiting

Rate limiting is crucial for protecting your APIs from abuse and ensuring fair usage. In this post, we'll explore the **Token Bucket** algorithm, a popular approach for rate limiting.

## The Concept

Imagine a bucket that holds tokens.
1.  Requests need a token to be processed.
2.  Tokens are added to the bucket at a fixed rate.
3.  If the bucket is empty, the request is rejected.
4.  If the bucket is full, new tokens are discarded.

## Simple Implementation

Here's a basic implementation in JavaScript/TypeScript:

```typescript
class TokenBucket {
  private tokens: number;
  private readonly capacity: number;
  private readonly refillRate: number;
  private lastRefill: number;

  constructor(capacity: number, refillRate: number) {
    this.capacity = capacity;
    this.refillRate = refillRate;
    this.tokens = capacity;
    this.lastRefill = Date.now();
  }

  private refill() {
    const now = Date.now();
    const elapsedTime = (now - this.lastRefill) / 1000;
    const newTokens = elapsedTime * this.refillRate;
    
    this.tokens = Math.min(this.capacity, this.tokens + newTokens);
    this.lastRefill = now;
  }

  tryConsume(amount: number = 1): boolean {
    this.refill();
    
    if (this.tokens >= amount) {
        this.tokens -= amount;
        return true;
    }
    
    return false;
  }
}
```

## Usage

```typescript
const limiter = new TokenBucket(10, 1); // Capacity 10, refill 1 token/sec

if (limiter.tryConsume()) {
    console.log("Request allowed");
} else {
    console.log("Rate limit exceeded");
}
```

This algorithm allows for bursts of traffic (up to `capacity`) while enforcing a long-term rate limit. It's simple, efficient, and effective for many use cases.
