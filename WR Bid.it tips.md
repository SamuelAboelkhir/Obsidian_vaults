---
tags:
- WR/bidit
MOC: Work
---
[[_0000 Home|Home]] | [[_0002 Work MOC|Back to Work MOC]] | [[WR Projects index]]

# Form
- Issues data type can be a boolean
- Look into extracting the form schema outside the component
- Check if client side parts can be extracted so that the entire component is not `use client`
- Repeated functions even if small should be extracted to avoid repeating yourself. "Normalization" should be done in a reusable function

For dealing with loading bids
```TS
// ❌ This loads ALL bids - could be slow for popular auctions 
const listing = await listingRepo.findOne({ where: { id: 1 }, relations: ['bids'] // Loads all bids at once 
}); 
// ✅ Better: Load listing without bids by default 
const listing = await listingRepo.findOne({ where: { id: 1 } }); 
// ✅ Load bids separately with pagination/limits 
const recentBids = await bidRepo.find({ where: { listing: { id: 1 } }, order: { bidAmount: 'DESC' }, take: 10 // Only get top 10 bids });
```