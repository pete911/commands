# DNS

## configuration

- mac `scutil --dns` print current DNS configuration
- *nix `/etc/resolv.conf` - client resolver configuration
- *nix `/etc/nsswitch` - how mapping should be performed, if the file does not
  exist, `hosts: dns [!UNAVAIL=return] files` is default

## nameserver taxonomy

- authoritative - officially represetns a zone
  - master/primare - master server for a zone; gets its data from a disk
  - slave/secondary - copies its data from the master
  - stub - like slave, but copies only name server data (no host data)
  - distribution - a server advertised only withing a domain (aka stealth server)
- nonauthoritative - answers a query from cache; doesn't know if the data is still valid
  - caching - caches data from previous queries; usually has no local zones
  - forwarder - performs queries on behalf of many clients; builds a large cache
- recursive - queries on your behalf until it returns either na answer or an error
- nonrecursive - refers you to another server if it can't answer a query

## query tools

- nameservers `dig -t ns <host>` or `dig <host> NS` or `dig <host> +nssearch`
- txt records, for large txt records add nameserver to avoid trimming `dig @8.8.8.8 -t txt <host>`
- only answer `dig -t ns <host> +noall +answer` or `dig <host> NS +noall +answer`
- specify nameserver `dig @<ns> <host> NS +noall +answer` e.g. `dig @1.1.1.1 cloudflare.com. NS +noall +answer`
- return SOA record `dig <host> soa`
- return all cached data `dig <host> any`

