# Snooze and notify when replied/not replied

eM Client stores the addition information about snoozing and notification requests on the servers. We implement the snooze feature by additional snooze time attached to a message.

TODO: Decribe the notification times, what they mean, how it works for conversations

## Servers with ANNOTATE

If the server supports ANNOTATE IMAP extension we store the values in the annotations as follows:
- "/vendor/emclient/snooze/snoozed" => `true` / `false`
- "/vendor/emclient/snooze/until" => `YYYY-MM-DDThh:mm:ss` (eg. `2008-06-15T21:15:07`)
- "/vendor/emclient/reply/notifyWhenNotReplied" => `true` / `false`
- "/vendor/emclient/reply/notifyWhenNotRepliedUntil" => `YYYY-MM-DDThh:mm:ss`
- "/vendor/emclient/reply/notifyWhenReplied" => `true` / `false`

## Servers without ANNOTATE

For servers that don't support the ANNOTATE extension we store the value as IMAP keywords in the following format:
- `$SnoozedUntil_MIN` (MIN == minimum value, TODO: What does that mean?)
- `$SnoozedUntil_<unix time>`
- `$NotifyWhenNotReplied_MIN`
- `$NotifyWhenNotReplied_<unix time>`
- `$NotifyWhenReplied` => present / not present