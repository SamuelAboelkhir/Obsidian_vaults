---
tags: 
- MOC
MOC: Work
---
[[_0000 Home|Home]]
# All Projects
[[WR Projects index]]
# General notes
[[WR General Index]]

# TODO this week


- Remind Marc he wants to talk to me about something he wants to improve about him self
#### BIDIT
- [ ]  3. Add role to the seller object in listing responses (Important)

  ListingSeller already has role?: string defined in the frontend type, but if the backend isn't including it in the GET /listing  
  response payload, the seller type badge will never render.

  Needed: Ensure the seller relation in GET /listing and GET /listing/:id includes the seller's role field.

- [ ]  4. Add server-side filtering/sorting for pending listings (Nice to have)

  Currently the frontend fetches all listings and filters/sorts/paginates client-side. As data grows this will be slow.

  Needed: GET /listing?status=Pending+Approval&page=1&limit=10&sortBy=startTime&order=asc with proper server-side handling. 1. "My Listings" Dashboard Page  does not exist

  The /auction-listing route currently only shows the creation form — it's not a dashboard that lists the seller's existing listings.  
   There's no page that shows the seller their Draft/Live/Pending listings with actions. ---  
- [ ]  5. Route for editing a specific listing — does not exist The backend PATCH /:id/update has no status guard — a non-Draft listing can still be patched via API. No seller filtered listing endpoint