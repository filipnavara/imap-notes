# Lables

## Thunderbird

Thunderbird uses the `$Label<number>` and maps the labels to names locally.

Default mapping in Thunderbird (TODO):
- Important (red)
- Work (yellow/orange)
- Personal (green)
- To Do (blue)
- Later (purple)

## Apple Mail

Apple Mail uses `$MailFlagBit[012]`. A message with a `\Flagged` keyword is consider to be flagged. By default the flag is red but an additional color is specified by a combination of the `$MailFlagBit0`, `$MailFlagBit1` and `$MailFlagBit2` flags. Together they form a 3-bit number that maps to a color through a translation table.

TODO: Add the mapping table

## IceWarp WebMail (and eM Client w/ IceWarp Server)

Arbitrary labels are URI encoded, then `%20` is replaced with `+`, and `%` is replaced with `'`. Labels starting with `\` and `$` are not colliding because the characters would get encoded as `'5C` and `'24` respectively.

Example:
- `Vacation` => `Vacation`
- `naïve person` => `na'C3'AFve+person`

## eM Client with Dovecot, Surgemail, SmarterMail

Use the following transformation for each character:
- ASCII character > 32 and < 127 (TODO: should be <= 127??) except `'`, `\`, `%`, `_`, `*`, `+`, `=`, `~`, `"`, `(`, `)`, `[`, `]`, `{`, `}` => keep the character as is
- ` ` => `_`
- Anything else => `'{2-character hex value}`

Example:
- `Vacation` => `Vacation`
- `naïve person` => `na'C3'AFve_person`

## Gmail

Use `X-GM-LABELS` FETCH/STORE keyword.

### Automatic classification

Orthogonal but related thing is **automatic** classification done by some clients and servers, such as Gmail. Common categories are the following:
- Personal
- Social
- Promotions
- Updates
- Forums

Gmail used to expose them when the classification was still part of Google Labs. Once it graduated into production they were removed from most of the IMAP commands except for SEARCH.

## Other commonly observed $-prefixed keywords

- `$HasAttachment` / `$HasNoAttachment`
- `$IsMailingList`
- `$IsNotification`
- `$Junk` / `$NotJunk`
- `$HasTD` (any idea?)
- `$IsTrusted`
- `$X-ME-Annot-2` (iCloud?)
- `$purchases`

## TODO

- Define addition METADATA to specify a list of labels for UI selection, and their color mapping.