# Google IP List

Google crawler IP ranges, generated from Google's published crawler IP range files.

## Usage

Use the raw IP list files directly:

```text
https://raw.githubusercontent.com/aminvakil/google-ip-list/refs/heads/main/ipv4.txt
https://raw.githubusercontent.com/aminvakil/google-ip-list/refs/heads/main/ipv6.txt
```

Or watch repository releases to be notified when `ipv4.txt` or `ipv6.txt` changes.

## Files

- `ipv4.txt`: All IPv4 prefixes from the Google crawler IP range files, one CIDR per line.
- `ipv6.txt`: All IPv6 prefixes from the Google crawler IP range files, one CIDR per line.
- `googlebot.json`: Google's common crawler IP ranges.
- `special-crawlers.json`: Google's special crawler IP ranges.
- `user-triggered-fetchers.json`: Google's user-triggered fetcher IP ranges.

## Sources

Data is fetched from Google:

```text
https://developers.google.com/static/crawling/ipranges/common-crawlers.json
https://developers.google.com/static/crawling/ipranges/special-crawlers.json
https://developers.google.com/static/crawling/ipranges/user-triggered-fetchers.json
```

## Generate

```bash
curl https://developers.google.com/static/crawling/ipranges/common-crawlers.json -o googlebot.json -L
curl https://developers.google.com/static/crawling/ipranges/special-crawlers.json -o special-crawlers.json -L
curl https://developers.google.com/static/crawling/ipranges/user-triggered-fetchers.json -o user-triggered-fetchers.json -L

jq -r '.prefixes[] | .ipv4Prefix' googlebot.json | grep -v null > ipv4.txt
jq -r '.prefixes[] | .ipv4Prefix' special-crawlers.json | grep -v null >> ipv4.txt
jq -r '.prefixes[] | .ipv4Prefix' user-triggered-fetchers.json | grep -v null >> ipv4.txt

jq -r '.prefixes[] | .ipv6Prefix' googlebot.json | grep -v null > ipv6.txt
jq -r '.prefixes[] | .ipv6Prefix' special-crawlers.json | grep -v null >> ipv6.txt
jq -r '.prefixes[] | .ipv6Prefix' user-triggered-fetchers.json | grep -v null >> ipv6.txt
```

## Updates

GitHub Actions checks Google's published files hourly and opens a pull request when files change.

JSON-only changes may be merged automatically. Releases are created when changes to `ipv4.txt` or `ipv6.txt` are merged into `main`.
