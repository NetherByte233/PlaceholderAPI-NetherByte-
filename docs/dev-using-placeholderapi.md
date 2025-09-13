# Using PlaceholderAPI (for consumers)

This page shows how to use PlaceholderAPI from your plugin when you want to parse messages or fetch single values.

> Note: Only built-in placeholders and placeholders from installed providers will resolve.
> If a third-party plugin exposes a Provider, server admins must run `/papi download <id>` once to enable that plugin’s placeholders.

## Parse text with placeholders

```php
use NetherByte\PlaceholderAPI\PlaceholderAPI;
use pocketmine\player\Player;

/** @var Player|null $player */
$msg = "Welcome %player_name% to %server_name%! TPS: %server_tps%";
$out = PlaceholderAPI::parse($msg, $player);
```

Notes:
- Player can be `null` for server-only placeholders.
- Unknown placeholders remain unchanged.

## Get a single value

```php
$tps = PlaceholderAPI::get('server_tps');
$time = PlaceholderAPI::get('server_time:Y-m-d H:i'); // parameterized style
```

## Parameterized placeholders

You can pass a parameter with `identifier:param`.

Examples:
- `server_time:<Y-m-d H:i>`
- `server_online:world`

The plugin also accepts underscore-style for backwards compatibility:
- `server_time_<format>`
- `server_online_<world>`

## Debugging

- Use `/papi info <identifier[:param]> [player]` to see:
  - Which expansion handles it
  - The resolved value
  - Expansion metadata (author, version, description)

## Performance tips

- Parsing once and reusing the result is cheaper than parsing every tick.
- If you poll frequently (e.g., scoreboards), design your own cache where possible.
- The API supports light caching per expansion when the expansion declares an update interval.
