# Scan Cache

NSFW Manager maintains a persistent scan cache so that files which have not changed since the last scan are not reanalysed. This dramatically reduces scan time on subsequent runs.

## How the Cache Works

<!-- Explain the hashing/fingerprinting strategy used to detect unchanged files. -->

## Cache Storage Location

<!-- Where the cache database is stored on disk. -->

## Invalidating the Cache

<!-- When the cache is automatically invalidated (file modification, move, rename) and how to manually clear it. -->

## Cache Size and Maintenance

<!-- How large the cache can grow, and how to prune old entries. -->

## Disabling the Cache

<!-- How to turn off the cache for a single scan or permanently via settings. -->
