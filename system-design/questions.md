# System Design Questions

Categorized index of system design interview prompts. Use `structure.md` for the working framework and `estimate-facts.md` for back-of-envelope reference numbers.

## Finances

- Payment system (Stripe)
- Amazon Kindle payments
- Mint (personal finance aggregator)
- ATM machine
- Coinbase (crypto exchange)
- Vending machine
- Stock broker platform (Robinhood)

## E-Commerce & Marketplaces

- Online auction
- Ticket booking platform (BookMyShow, Ticketmaster)
  - Ref: *Design a Ticket Booking Site Like Ticketmaster* — Hello Interview
  - Ref: *Design Ticketmaster* — Ex-Meta Staff Engineer
- Yelp (local business reviews)
- Delivery platform (food ordering, Uber Eats, shipping)
- Ride-hailing (Uber, Lyft)
- Ref: *Design a Ride-Sharing Service Like Uber* — Hello Interview
  - Ref: *Design Uber* — Ex-Meta Staff Engineer
- Booking.com
- Hotel / short-term rental (Airbnb)
- Splitwise (group expense tracking)
- E-commerce cart
- Price tracking service
- Car rental system
- Flight booking system

## Audio / Video

- Global video streaming platform (Netflix, Amazon Prime Video)
- YouTube
- Spotify
- Top-K hitters (YouTube top videos, Spotify top songs, Meta heavy hitters)
- Type-ahead suggestions (Google Search, YouTube autocomplete)

## Social Media

- Ad click aggregator
- Facebook post search
- Facebook News Feed
  - Ref: *Design Facebook's News Feed*
  - Ref: *Design FB News Feed System Design Interview* — Ex-Meta Senior Manager
- Real-time commenting feature (Facebook, Instagram) — passive viewers see new comments live
- WhatsApp / Facebook Messenger
  - Ref: *Design WhatsApp* — Ex-Meta Senior Manager
  - Ref: *Design a Messaging App Like WhatsApp*
- LinkedIn
- Instagram
- Twitter / X
  - Ref: *System Design Interview Walkthrough — Design Twitter*
  - Ref: *Twitter / Newsfeed System Design Interview Question*
- Tinder
- TikTok
- News feed / timeline (Twitter, Facebook, Instagram)
- Mutual friends
- Quora
- Reddit

## Collaboration

- Google Docs / Drive, Dropbox
  - Ref: *Design Dropbox or Google Drive* — Ex-Meta Staff Engineer
  - Ref: *Design a File Storage Service Like Dropbox*
  - Ref: *Design Dropbox / Google Drive — Cloud File Sharing Service*
- Google Meet, Zoom (video conferencing)
- Real-time alerts / notification service
- Collaborative rich-document editor
- Calendar application
- Email server

## Tech / AI

- LLM chat product (ChatGPT, Claude, Gemini, Perplexity, Grok)
- Search platform (Google Search, Bing)
  - Crawling
  - Indexing
  - Ranking
  - Querying
- Coding evaluation system (LeetCode, HackerRank)
- Web crawler / search engine
  - Ref: *Web Crawler — System Design Interview Question*
- Cloud hosting solution
- URL shortener / key-value store / paste service (Bitly, Pastebin)
  - Ref: *Tiny URL — System Design Interview Question*
  - Ref: *Design a URL Shortener Like Bit.ly*
  - Ref: *Designing Pastebin* — CodeX / Medium
- Job scheduling platform
- Concurrent producer / consumer (thread safety, locks, blocking queues)
- Distributed LRU / FRU cache
- Rate limiter
  - Ref: *Rate Limiter — System Design Interview Question*
- Distributed messaging queue
- Simple rule engine
- Analytics platform
- Logging framework
- In-memory file system (tree structure, search, delete, path handling)
- In-memory message queue (pub-sub, ordering, retries, consumers)
- Notification push service (Firebase)
- Stack Overflow website
- Type-ahead search box

## Utilities

- Tic-Tac-Toe
- Chess
- Soccer goal-scoring tracking system
- Strava (fitness tracking)
- Google News aggregator

---

## Notes

- Class-design problems (Parking Lot, Elevator System, Access Management, Locker Allocation) are in [`../class-design/`](../class-design/) with full class skeletons.
- For interviews that blur the line (Vending Machine, Tic-Tac-Toe, Chess), decide framing based on the interviewer's prompt: scaled-out service → system design; OO modeling → class design.
