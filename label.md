# Lables

## Thunderbird

Thunderbird uses the `$Label<number>` and maps the labels to names locally.

## Apple Mail

Apple Mail uses `$MailFlagBit<number>` and maps the values 0, 1, 2 to ???

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

## TODO

- Annotations / Metadata / Colors in METADATA