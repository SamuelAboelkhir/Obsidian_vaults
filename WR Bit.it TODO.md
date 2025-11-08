---
tags:
- WR/bidit/TODO
MOC: Work
---
[[_0000 Home|Home]] | [[_0002 Work MOC|Back to Work MOC]] | [[WR Projects index]]

- Currently, drafts use the create and update methods for listings
- Next step, when we have users that have lists of listings, the logic should be:
	- [ ] User goes to create a listing, and he either does it in one go "no listingID" or he saved the listing as a draft "listingID" and later he selects it from the list of listings to complete it
	- [x] Ask Marc if we can have each listing as a card with status "Draft", "Pending Approval", "Active", "Complete"
	- [x] onSubmit should check if a listing was fetched by its ID "it was a draft" and either create a new listing if it wasn't a draft, or just update the status from "Draft" to "Pending Approval", and reset the form as per the usual form submit behavior
	- [ ] Frontend tsconfig.json error: need to set baseUrl
	- [ ] Bid status in Bid entity: will need to constantly update after each new bid - is there a better way?