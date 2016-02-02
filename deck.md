title: 18F micro-purchase API
author:
  name: Alan deLevie & Alla Goldman Seiffert
  twitter: adelevie @allagoldman
  url: https://18f.gsa.gov
output: index.html
controls: true

--

# Micro-purchase API
## Government contracting via the command line

--

### What is a micropurchase?

In the FAR, the government can use a credit card to procure nearly anything for $3,500 or less.

This could be used for things like paper, pencils, and staplers.

This had us wondering ...

--

### ... Why not micropurchase custom software?

--

### First iteration

We used a Rube Goldberg contraption comprised of a Google Form and the GitHub API to accept bids.

- https://github.com/18f/calc/issues/255
- https://docs.google.com/a/gsa.gov/forms/d/1eRFX0hSTTXMc2FulK6kPP2P02ZApQqlYZL7oVDghJJo/closedform

It sort of worked, and we ended with a bid for $1.

--

### The micro-purchase platform

A web application for posting, bidding, and awarding very small government IT contracts.

All you need is a DUNS number and a SAM.gov registration. (See https://micropurchase.18f.gov/faq)

--

### Behind the scenes

- https://micropurchase.18f.gov/
- Standard Rails app
- Open Source (https://github.com/18f/micropurchase)
- Read and write API (yay!)

--

### Rails architecture

Rather than create separate controllers for API routes, we provide JSONic interfaces to the same routes accessible via the web UI.

For example, if you're viewing `micropurchase.18f.gov/auctions` in the browser, you can also access that same route via API.

--

### API Authentication

The app only uses GitHub OAuth. We don't store any passwords and we didn't want to start generating and storing API keys.

Instead, we accept GitHub Personal API Tokens as the API keys (https://pages.18f.gov/micropurchase-api-docs/quickstart/).

--

### Read methods

`GET /auctions`

`curl https://micropurchase.18f.gov/auctions -H "Accept: text/x-json" | jq '.'`

--

### Write methods

`POST /auctions/:id/bids/`

```bash
curl -i \
  -H "Accept: text/x-json" \
  -H "Api-Key: $MICROPURCHASE_API_KEY" \
  -H "Content-Type: application/json" \
  -X POST -d '{"bid": {"amount": 150}}' \
  https://micropurchase-staging.18f.gov/auctions/1/bids
```
