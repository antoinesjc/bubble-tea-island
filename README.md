# Bubble Tea Island

Tools and widgets for [bubbleteaisland.com](https://www.bubbleteaisland.com).

## Taipei events calendar (`/events`)

A curated calendar of networking and social events in Taipei, embedded on
https://www.bubbleteaisland.com/networking-social-events-in-taiwan

Live page: https://antoinesjc.github.io/bubble-tea-island/events/

### How it works

1. **Weekly search.** A scheduled Claude task searches Luma, Meetup, the chambers of
   commerce (ECCT, AmCham, CCIFT, BCCT…) and startup hubs every week and adds new
   events to the **Inbox** tab of the Google Sheet *Bubble Tea Island – Taipei Events*
   with status `Pending`.
2. **Approval.** Set each row's Status to `Approved` or `Rejected`.
3. **Website.** The **Published** tab lists approved events only (a `FILTER` formula).
   That tab is published to the web as CSV, and `events/index.html` reads it, so an
   approval shows on the site within a few minutes. Nothing else needs to be deployed.
4. **Google Calendar.** The weekly task also copies approved events to the
   *Bubble Tea Island* Google Calendar, which readers can subscribe to.
5. **Newsletter.** On the 1st and 15th, a scheduled task drafts a Taiwan Career Brief
   post from the approved upcoming events.

### Categories

Networking · Business & Trade · Startup & Tech · Talks & Conferences · Social · Culture

Featured events: tick **Featured** (Inbox column Q) on an approved event. It is pinned at the top of the list view and shown as a tile beside the month view until it has passed.

Multi-day events: put the first day in **Date** and the last day in **End date** (Inbox column P). The page shows them as a date range and spans them across every day in the month view.

Colors are set in `CATEGORIES` at the bottom of `events/index.html`. Adding a category
means adding it there and to the Category dropdown in the Sheet.

### Embedding in Odoo

Paste this into an Odoo *Embed Code* block:

```html
<iframe id="bti-events" src="https://antoinesjc.github.io/bubble-tea-island/events/"
        style="width:100%;border:0;height:760px" loading="lazy"
        title="Networking & social events in Taipei"></iframe>
<script>
  window.addEventListener("message", function (e) {
    if (e.data && e.data.type === "bti-events-height") {
      var f = document.getElementById("bti-events");
      f.style.minHeight = "0";
      f.style.height = e.data.height + "px";
    }
  });
</script>
```

The iframe resizes itself to fit the content.
