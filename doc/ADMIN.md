## Calendar feed

MoreLater publishes an ICS feed you can subscribe to from Apple Calendar, Google Calendar or any other calendar app. Calendar apps can't log in through YunoHost, so the feed is reachable without a login, but only at a URL containing a secret token:

{% if path == "/" %}`https://__DOMAIN__/api/calendar.ics?token=__ICS_TOKEN__`{% else %}`https://__DOMAIN____PATH__/api/calendar.ics?token=__ICS_TOKEN__`{% endif %}

Treat this URL like a password: anyone who has it can read your whole calendar. Requests without the right token get a 404.

To change the token (for instance if the URL has leaked), run this as root on the server, then re-subscribe your calendars with the new URL shown here:

```bash
yunohost app setting __APP__ ics_token -v "$(tr -dc 'A-Za-z0-9' < /dev/urandom | head -c 32)"
yunohost app upgrade __APP__ --force
```

If you'd rather the feed needed a YunoHost login, remove "Visitors" from the app's calendar-feed permission in the admin panel. Most calendar apps won't then be able to subscribe.
