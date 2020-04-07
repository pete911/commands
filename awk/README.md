# awk

awk rules consist of actions and patterns (`{}`)

By default, field is a string surrounded by whitespace. Fields are identified
by a dollar sign ($) and a number.

 - `$0` represents the entire line of text
 - `$1` represents the first field
 - `$2` represents the second field
 - `$NF` stands for "number of fields" and represents the last field

```
who | awk '{print $1}'
who | awk '{print $1,$4}'
```

## output field separator - OFS

```
date | awk '{print$2,$3,$6}'
date | awk 'OFS="/" {print$2,$3,$6}'
date | awk 'OFS="/" {print$2,$3,$6}'
```

## the BEGIN and END rules

A `BEGIN` rule is executed once before awk even reads any text. An `END` rule
is executed after all processing has completed.

```
awk 'BEGIN {print "Content of some file"} {print $0}' <some-file>.txt
who | awk 'BEGIN {print "Active Sessions"} {print $1,$4}'
```

## input field separators

By default aws uses whitespace as field seaparator, this can be changed with
`-F` flag e.g. `-F:`.

```
# print user account and home folder
awk -F: '{print $1,$6}' /etc/passwd
```

## adding patterns

```
# print action only when the third field ($3) value is 1,000 or greater
awk -F: '$3 >= 1000 {print $1,$6}' /etc/passwd
```

Patterns are full-fledged regular expressions.

```
# search 'IdentityFile' in ssh config
awk '/IdentityFile/ {print $0}' ~/.ssh/config | uniq
```

## built in functions and awk scripts

[The GNU Awk User's Guide](https://www.gnu.org/software/gawk/manual/gawk.html)
